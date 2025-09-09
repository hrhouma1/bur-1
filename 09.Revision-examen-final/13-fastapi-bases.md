# Exercice 13 - FastAPI Bases

## Objectifs
- Maitriser la creation d'APIs REST avec FastAPI
- Comprendre la validation automatique avec Pydantic
- Implementer des endpoints CRUD complets

## Exercice 13.1 - API de Gestion de Bibliotheque

### Enonce
Developpez une API complete pour la gestion d'une bibliotheque :

**Entites :**
- **Livre** : titre, auteur, isbn, annee_publication, genre, disponible
- **Membre** : nom, prenom, email, date_inscription, actif
- **Emprunt** : membre_id, livre_id, date_emprunt, date_retour_prevue, date_retour_reelle

**Endpoints requis :**
- CRUD complet pour les livres
- CRUD complet pour les membres
- Gestion des emprunts (emprunter, retourner)
- Recherche de livres par criteres
- Historique des emprunts

### Code de Base
```python
from fastapi import FastAPI, HTTPException, Depends, Query
from pydantic import BaseModel, EmailStr, Field, validator
from typing import Optional, List
from datetime import datetime, date, timedelta
from enum import Enum
import sqlite3
from contextlib import contextmanager

app = FastAPI(title="API Bibliotheque", version="1.0.0")

# Enums
class GenreLivre(str, Enum):
    FICTION = "fiction"
    NON_FICTION = "non-fiction"
    SCIENCE = "science"
    HISTOIRE = "histoire"
    BIOGRAPHIE = "biographie"
    POESIE = "poesie"

# Modeles Pydantic
class LivreBase(BaseModel):
    titre: str = Field(..., min_length=1, max_length=200)
    auteur: str = Field(..., min_length=1, max_length=100)
    isbn: str = Field(..., regex=r'^\d{13}$')
    annee_publication: int = Field(..., ge=1000, le=datetime.now().year)
    genre: GenreLivre
    disponible: bool = True

class LivreCreate(LivreBase):
    pass

class LivreUpdate(BaseModel):
    titre: Optional[str] = Field(None, min_length=1, max_length=200)
    auteur: Optional[str] = Field(None, min_length=1, max_length=100)
    isbn: Optional[str] = Field(None, regex=r'^\d{13}$')
    annee_publication: Optional[int] = Field(None, ge=1000, le=datetime.now().year)
    genre: Optional[GenreLivre] = None
    disponible: Optional[bool] = None

class Livre(LivreBase):
    id: int
    
    class Config:
        from_attributes = True

class MembreBase(BaseModel):
    nom: str = Field(..., min_length=1, max_length=50)
    prenom: str = Field(..., min_length=1, max_length=50)
    email: EmailStr
    actif: bool = True

class MembreCreate(MembreBase):
    pass

class MembreUpdate(BaseModel):
    nom: Optional[str] = Field(None, min_length=1, max_length=50)
    prenom: Optional[str] = Field(None, min_length=1, max_length=50)
    email: Optional[EmailStr] = None
    actif: Optional[bool] = None

class Membre(MembreBase):
    id: int
    date_inscription: datetime
    
    class Config:
        from_attributes = True

class EmpruntBase(BaseModel):
    membre_id: int
    livre_id: int

class EmpruntCreate(EmpruntBase):
    pass

class Emprunt(EmpruntBase):
    id: int
    date_emprunt: datetime
    date_retour_prevue: date
    date_retour_reelle: Optional[datetime] = None
    
    class Config:
        from_attributes = True

# Gestionnaire de base de donnees
class DatabaseManager:
    def __init__(self, db_path: str = "bibliotheque.db"):
        self.db_path = db_path
        self.init_database()
    
    @contextmanager
    def get_connection(self):
        conn = sqlite3.connect(self.db_path)
        conn.row_factory = sqlite3.Row
        try:
            yield conn
        finally:
            conn.close()
    
    def init_database(self):
        """Initialise la base de donnees avec les tables necessaires"""
        with self.get_connection() as conn:
            # Table livres
            conn.execute('''
                CREATE TABLE IF NOT EXISTS livres (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    titre TEXT NOT NULL,
                    auteur TEXT NOT NULL,
                    isbn TEXT UNIQUE NOT NULL,
                    annee_publication INTEGER NOT NULL,
                    genre TEXT NOT NULL,
                    disponible BOOLEAN DEFAULT TRUE
                )
            ''')
            
            # Table membres
            conn.execute('''
                CREATE TABLE IF NOT EXISTS membres (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    nom TEXT NOT NULL,
                    prenom TEXT NOT NULL,
                    email TEXT UNIQUE NOT NULL,
                    date_inscription DATETIME DEFAULT CURRENT_TIMESTAMP,
                    actif BOOLEAN DEFAULT TRUE
                )
            ''')
            
            # Table emprunts
            conn.execute('''
                CREATE TABLE IF NOT EXISTS emprunts (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    membre_id INTEGER NOT NULL,
                    livre_id INTEGER NOT NULL,
                    date_emprunt DATETIME DEFAULT CURRENT_TIMESTAMP,
                    date_retour_prevue DATE NOT NULL,
                    date_retour_reelle DATETIME,
                    FOREIGN KEY (membre_id) REFERENCES membres (id),
                    FOREIGN KEY (livre_id) REFERENCES livres (id)
                )
            ''')
            conn.commit()

# Instance du gestionnaire
db_manager = DatabaseManager()

# Dependance pour obtenir le gestionnaire de BD
def get_db_manager():
    return db_manager

# Endpoints pour les livres
@app.post("/livres/", response_model=Livre, status_code=201)
async def creer_livre(livre: LivreCreate, db: DatabaseManager = Depends(get_db_manager)):
    """Cree un nouveau livre"""
    with db.get_connection() as conn:
        try:
            cursor = conn.execute(
                '''INSERT INTO livres (titre, auteur, isbn, annee_publication, genre, disponible)
                   VALUES (?, ?, ?, ?, ?, ?)''',
                (livre.titre, livre.auteur, livre.isbn, livre.annee_publication, 
                 livre.genre.value, livre.disponible)
            )
            conn.commit()
            livre_id = cursor.lastrowid
            
            # Recuperer le livre cree
            row = conn.execute('SELECT * FROM livres WHERE id = ?', (livre_id,)).fetchone()
            return dict(row)
        except sqlite3.IntegrityError:
            raise HTTPException(status_code=400, detail="Un livre avec cet ISBN existe deja")

@app.get("/livres/", response_model=List[Livre])
async def lister_livres(
    skip: int = Query(0, ge=0),
    limit: int = Query(100, ge=1, le=1000),
    genre: Optional[GenreLivre] = None,
    auteur: Optional[str] = None,
    disponible: Optional[bool] = None,
    db: DatabaseManager = Depends(get_db_manager)
):
    """Liste les livres avec filtrage optionnel"""
    # Votre implementation ici
    pass

@app.get("/livres/{livre_id}", response_model=Livre)
async def obtenir_livre(livre_id: int, db: DatabaseManager = Depends(get_db_manager)):
    """Obtient un livre par son ID"""
    # Votre implementation ici
    pass

@app.put("/livres/{livre_id}", response_model=Livre)
async def modifier_livre(livre_id: int, livre_update: LivreUpdate, db: DatabaseManager = Depends(get_db_manager)):
    """Modifie un livre existant"""
    # Votre implementation ici
    pass

@app.delete("/livres/{livre_id}", status_code=204)
async def supprimer_livre(livre_id: int, db: DatabaseManager = Depends(get_db_manager)):
    """Supprime un livre"""
    # Votre implementation ici
    pass

# Endpoints pour les membres
@app.post("/membres/", response_model=Membre, status_code=201)
async def creer_membre(membre: MembreCreate, db: DatabaseManager = Depends(get_db_manager)):
    """Cree un nouveau membre"""
    # Votre implementation ici
    pass

@app.get("/membres/", response_model=List[Membre])
async def lister_membres(
    skip: int = Query(0, ge=0),
    limit: int = Query(100, ge=1, le=1000),
    actif: Optional[bool] = None,
    db: DatabaseManager = Depends(get_db_manager)
):
    """Liste les membres"""
    # Votre implementation ici
    pass

@app.get("/membres/{membre_id}", response_model=Membre)
async def obtenir_membre(membre_id: int, db: DatabaseManager = Depends(get_db_manager)):
    """Obtient un membre par son ID"""
    # Votre implementation ici
    pass

# Endpoints pour les emprunts
@app.post("/emprunts/", response_model=Emprunt, status_code=201)
async def emprunter_livre(emprunt: EmpruntCreate, db: DatabaseManager = Depends(get_db_manager)):
    """Emprunte un livre"""
    # Verifier que le livre est disponible
    # Verifier que le membre est actif
    # Creer l'emprunt avec date de retour prevue (ex: 2 semaines)
    # Marquer le livre comme non disponible
    pass

@app.put("/emprunts/{emprunt_id}/retour", response_model=Emprunt)
async def retourner_livre(emprunt_id: int, db: DatabaseManager = Depends(get_db_manager)):
    """Retourne un livre emprunte"""
    # Mettre a jour la date de retour reelle
    # Marquer le livre comme disponible
    pass

@app.get("/emprunts/", response_model=List[Emprunt])
async def lister_emprunts(
    membre_id: Optional[int] = None,
    livre_id: Optional[int] = None,
    en_cours: Optional[bool] = None,
    db: DatabaseManager = Depends(get_db_manager)
):
    """Liste les emprunts avec filtrage"""
    # Votre implementation ici
    pass

@app.get("/emprunts/retards", response_model=List[Emprunt])
async def obtenir_emprunts_en_retard(db: DatabaseManager = Depends(get_db_manager)):
    """Obtient les emprunts en retard"""
    # Votre implementation ici
    pass

# Endpoints de recherche
@app.get("/recherche/livres", response_model=List[Livre])
async def rechercher_livres(
    q: str = Query(..., min_length=1, description="Terme de recherche"),
    db: DatabaseManager = Depends(get_db_manager)
):
    """Recherche de livres par titre ou auteur"""
    # Votre implementation ici
    pass

# Endpoints de statistiques
@app.get("/statistiques/emprunts")
async def statistiques_emprunts(db: DatabaseManager = Depends(get_db_manager)):
    """Statistiques sur les emprunts"""
    # Nombre total d'emprunts
    # Nombre d'emprunts en cours
    # Nombre d'emprunts en retard
    # Livre le plus emprunte
    pass

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

### Criteres de Validation
- [ ] Tous les endpoints CRUD implementes
- [ ] Validation Pydantic fonctionnelle
- [ ] Gestion des erreurs appropriee
- [ ] Filtrage et pagination
- [ ] Logique business correcte (emprunts)
- [ ] Documentation automatique generee

## Exercice 13.2 - API de Commerce Electronique

### Enonce
Developpez une API pour un site de commerce electronique :

**Entites :**
- **Produit** : nom, description, prix, stock, categorie, actif
- **Categorie** : nom, description, parent_id (categories hierarchiques)
- **Commande** : utilisateur_email, date_commande, statut, total
- **LigneCommande** : commande_id, produit_id, quantite, prix_unitaire
- **Utilisateur** : nom, email, date_creation, actif

**Fonctionnalites avancees :**
- Gestion du stock avec verification
- Calcul automatique des totaux
- Statuts de commande (en_attente, confirmee, expediee, livree)
- Categories hierarchiques

### Structure Recommandee
```python
from fastapi import FastAPI, HTTPException, Depends, BackgroundTasks
from pydantic import BaseModel, EmailStr, Field, validator
from typing import Optional, List, Dict
from datetime import datetime
from enum import Enum
from decimal import Decimal

app = FastAPI(title="API E-Commerce", version="1.0.0")

# Enums
class StatutCommande(str, Enum):
    EN_ATTENTE = "en_attente"
    CONFIRMEE = "confirmee"
    EXPEDIEE = "expediee"
    LIVREE = "livree"
    ANNULEE = "annulee"

# Modeles Pydantic
class ProduitBase(BaseModel):
    nom: str = Field(..., min_length=1, max_length=200)
    description: Optional[str] = Field(None, max_length=1000)
    prix: Decimal = Field(..., gt=0, decimal_places=2)
    stock: int = Field(..., ge=0)
    categorie_id: int
    actif: bool = True

class ProduitCreate(ProduitBase):
    pass

class ProduitUpdate(BaseModel):
    nom: Optional[str] = Field(None, min_length=1, max_length=200)
    description: Optional[str] = Field(None, max_length=1000)
    prix: Optional[Decimal] = Field(None, gt=0, decimal_places=2)
    stock: Optional[int] = Field(None, ge=0)
    categorie_id: Optional[int] = None
    actif: Optional[bool] = None

class Produit(ProduitBase):
    id: int
    
    class Config:
        from_attributes = True

class CategorieBase(BaseModel):
    nom: str = Field(..., min_length=1, max_length=100)
    description: Optional[str] = Field(None, max_length=500)
    parent_id: Optional[int] = None

class CategorieCreate(CategorieBase):
    pass

class Categorie(CategorieBase):
    id: int
    enfants: Optional[List['Categorie']] = []
    
    class Config:
        from_attributes = True

# Mise a jour du modele pour les references circulaires
Categorie.model_rebuild()

class LigneCommandeBase(BaseModel):
    produit_id: int
    quantite: int = Field(..., gt=0)

class LigneCommandeCreate(LigneCommandeBase):
    pass

class LigneCommande(LigneCommandeBase):
    id: int
    prix_unitaire: Decimal
    sous_total: Decimal
    
    class Config:
        from_attributes = True

class CommandeBase(BaseModel):
    utilisateur_email: EmailStr

class CommandeCreate(CommandeBase):
    lignes: List[LigneCommandeCreate] = Field(..., min_items=1)

class Commande(CommandeBase):
    id: int
    date_commande: datetime
    statut: StatutCommande
    total: Decimal
    lignes: List[LigneCommande] = []
    
    class Config:
        from_attributes = True

# Service de gestion des produits
class ProduitService:
    def __init__(self, db_manager):
        self.db = db_manager
    
    async def verifier_stock(self, produit_id: int, quantite_demandee: int) -> bool:
        """Verifie si le stock est suffisant"""
        pass
    
    async def reduire_stock(self, produit_id: int, quantite: int):
        """Reduit le stock d'un produit"""
        pass
    
    async def restaurer_stock(self, produit_id: int, quantite: int):
        """Restaure le stock (en cas d'annulation)"""
        pass

# Service de gestion des commandes
class CommandeService:
    def __init__(self, db_manager, produit_service):
        self.db = db_manager
        self.produit_service = produit_service
    
    async def creer_commande(self, commande_data: CommandeCreate) -> Commande:
        """Cree une nouvelle commande avec verification du stock"""
        # 1. Verifier le stock pour tous les produits
        # 2. Calculer les prix et totaux
        # 3. Creer la commande
        # 4. Reduire le stock
        pass
    
    async def calculer_total_commande(self, lignes: List[LigneCommandeCreate]) -> Decimal:
        """Calcule le total d'une commande"""
        pass
    
    async def changer_statut_commande(self, commande_id: int, nouveau_statut: StatutCommande):
        """Change le statut d'une commande"""
        pass

# Endpoints produits
@app.post("/produits/", response_model=Produit, status_code=201)
async def creer_produit(produit: ProduitCreate):
    """Cree un nouveau produit"""
    pass

@app.get("/produits/", response_model=List[Produit])
async def lister_produits(
    skip: int = 0,
    limit: int = 100,
    categorie_id: Optional[int] = None,
    prix_min: Optional[Decimal] = None,
    prix_max: Optional[Decimal] = None,
    en_stock: Optional[bool] = None
):
    """Liste les produits avec filtres"""
    pass

@app.get("/produits/{produit_id}", response_model=Produit)
async def obtenir_produit(produit_id: int):
    """Obtient un produit par ID"""
    pass

# Endpoints categories
@app.post("/categories/", response_model=Categorie, status_code=201)
async def creer_categorie(categorie: CategorieCreate):
    """Cree une nouvelle categorie"""
    pass

@app.get("/categories/", response_model=List[Categorie])
async def lister_categories():
    """Liste les categories avec leur hierarchie"""
    pass

@app.get("/categories/{categorie_id}/produits", response_model=List[Produit])
async def obtenir_produits_categorie(categorie_id: int, inclure_sous_categories: bool = True):
    """Obtient les produits d'une categorie"""
    pass

# Endpoints commandes
@app.post("/commandes/", response_model=Commande, status_code=201)
async def creer_commande(commande: CommandeCreate, background_tasks: BackgroundTasks):
    """Cree une nouvelle commande"""
    pass

@app.get("/commandes/", response_model=List[Commande])
async def lister_commandes(
    utilisateur_email: Optional[EmailStr] = None,
    statut: Optional[StatutCommande] = None
):
    """Liste les commandes"""
    pass

@app.put("/commandes/{commande_id}/statut")
async def changer_statut_commande(commande_id: int, nouveau_statut: StatutCommande):
    """Change le statut d'une commande"""
    pass

# Endpoints de recherche et statistiques
@app.get("/recherche/produits", response_model=List[Produit])
async def rechercher_produits(q: str, prix_max: Optional[Decimal] = None):
    """Recherche de produits"""
    pass

@app.get("/statistiques/ventes")
async def statistiques_ventes(date_debut: Optional[datetime] = None, date_fin: Optional[datetime] = None):
    """Statistiques de ventes"""
    pass
```

### Criteres de Validation
- [ ] Gestion du stock fonctionnelle
- [ ] Calculs de prix automatiques
- [ ] Categories hierarchiques
- [ ] Workflow de commandes complet
- [ ] Validation business appropriee
- [ ] Endpoints de recherche et stats

## Exercice 13.3 - API de Gestion de Projets

### Enonce
Creez une API pour la gestion de projets avec :

**Entites :**
- **Projet** : nom, description, date_debut, date_fin, statut, budget
- **Tache** : titre, description, projet_id, assignee_email, statut, priorite, date_echeance
- **Utilisateur** : nom, email, role, actif
- **Commentaire** : tache_id, auteur_email, contenu, date_creation
- **PieceJointe** : tache_id, nom_fichier, taille, type_mime, chemin_fichier

**Fonctionnalites :**
- Gestion des roles (admin, chef_projet, developpeur)
- Upload de fichiers
- Notifications par email
- Tableau de bord avec statistiques
- Filtrage avance des taches

### Code de Depart
```python
from fastapi import FastAPI, HTTPException, Depends, UploadFile, File, BackgroundTasks
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
from pydantic import BaseModel, EmailStr, Field
from typing import Optional, List
from datetime import datetime, date
from enum import Enum
import os
from pathlib import Path

app = FastAPI(title="API Gestion de Projets", version="1.0.0")

# Configuration
UPLOAD_DIR = Path("uploads")
UPLOAD_DIR.mkdir(exist_ok=True)

# Enums
class StatutProjet(str, Enum):
    PLANIFICATION = "planification"
    EN_COURS = "en_cours"
    TERMINE = "termine"
    ANNULE = "annule"

class StatutTache(str, Enum):
    A_FAIRE = "a_faire"
    EN_COURS = "en_cours"
    EN_REVISION = "en_revision"
    TERMINEE = "terminee"

class PrioriteTache(str, Enum):
    BASSE = "basse"
    MOYENNE = "moyenne"
    HAUTE = "haute"
    CRITIQUE = "critique"

class RoleUtilisateur(str, Enum):
    ADMIN = "admin"
    CHEF_PROJET = "chef_projet"
    DEVELOPPEUR = "developpeur"

# Modeles Pydantic
class ProjetBase(BaseModel):
    nom: str = Field(..., min_length=1, max_length=200)
    description: Optional[str] = Field(None, max_length=1000)
    date_debut: date
    date_fin: Optional[date] = None
    budget: Optional[float] = Field(None, ge=0)

class ProjetCreate(ProjetBase):
    pass

class Projet(ProjetBase):
    id: int
    statut: StatutProjet
    chef_projet_email: Optional[EmailStr] = None
    
    class Config:
        from_attributes = True

# Votre implementation complete ici...

# Systeme d'authentification simple
security = HTTPBearer()

async def get_current_user(credentials: HTTPAuthorizationCredentials = Depends(security)):
    """Obtient l'utilisateur actuel (simulation)"""
    # Dans un vrai projet, vous valideriez le token JWT ici
    # Pour cet exercice, on simule avec un utilisateur fixe
    return {"email": "admin@example.com", "role": "admin"}

# Endpoints avec authentification
@app.post("/projets/", response_model=Projet, status_code=201)
async def creer_projet(projet: ProjetCreate, current_user=Depends(get_current_user)):
    """Cree un nouveau projet"""
    pass

@app.post("/taches/{tache_id}/fichiers/", status_code=201)
async def uploader_fichier(
    tache_id: int,
    file: UploadFile = File(...),
    current_user=Depends(get_current_user)
):
    """Upload un fichier pour une tache"""
    # Verifier la taille et le type de fichier
    # Sauvegarder le fichier
    # Enregistrer en base
    pass

@app.get("/tableau-bord/")
async def tableau_de_bord(current_user=Depends(get_current_user)):
    """Tableau de bord avec statistiques"""
    # Nombre de projets par statut
    # Taches assignees a l'utilisateur
    # Taches en retard
    # Charge de travail
    pass
```

### Criteres de Validation
- [ ] Systeme d'authentification fonctionnel
- [ ] Upload de fichiers securise
- [ ] Gestion des roles et permissions
- [ ] Tableau de bord informatif
- [ ] Notifications par email
- [ ] Filtrage avance des donnees

## Questions de Reflexion

1. **Validation Pydantic** : Quels sont les avantages de la validation automatique ?

2. **Documentation** : Comment FastAPI genere-t-il automatiquement la documentation ?

3. **Performance** : Quelles strategies utiliser pour optimiser les performances d'une API ?

4. **Securite** : Quelles sont les bonnes pratiques de securite pour les APIs ?

## Ressources Complementaires

- **Cours de reference** : `45. FastAPI (Initialisation) - Partie 1`, `46. FastAPI (Initialisation) - Partie 2`
- **Tests** : `54. Tester une API FastAPI avec Postman`
- **Documentation** : FastAPI Official Documentation

## Conseils

1. **Validation** : Exploitez au maximum les validateurs Pydantic
2. **Documentation** : Utilisez les docstrings pour enrichir la doc auto
3. **Gestion d'erreurs** : Implementez des gestionnaires d'erreurs personnalises
4. **Tests** : Ecrivez des tests automatises pour vos endpoints
5. **Securite** : Implementez l'authentification et l'autorisation appropriees

Excellente maitrise de FastAPI !
