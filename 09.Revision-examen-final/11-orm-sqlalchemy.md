# Exercice 11 - ORM avec SQLAlchemy

## Objectifs
- Maitriser SQLAlchemy ORM pour la gestion des bases de donnees
- Comprendre les relations entre entites
- Implementer des operations CRUD avancees avec ORM

## Exercice 11.1 - Systeme de Gestion d'Ecole

### Enonce
Developpez un systeme complet de gestion d'ecole avec les entites suivantes :

**Modeles requis :**
- **Etudiant** : nom, prenom, date_naissance, email, numero_etudiant
- **Professeur** : nom, prenom, email, specialite, date_embauche
- **Cours** : nom, code, credits, description
- **Inscription** : etudiant_id, cours_id, date_inscription, note (optionnel)
- **Departement** : nom, budget, chef_departement_id

**Relations :**
- Un departement a plusieurs professeurs (1-N)
- Un professeur peut enseigner plusieurs cours (N-N)
- Un etudiant peut s'inscrire a plusieurs cours (N-N via Inscription)
- Un departement a un chef (professeur) (1-1)

### Code de Base
```python
from sqlalchemy import create_engine, Column, Integer, String, DateTime, Float, ForeignKey, Table, Text
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship
from datetime import datetime

Base = declarative_base()

# Table d'association pour la relation N-N entre Professeur et Cours
professeur_cours_association = Table(
    'professeur_cours', Base.metadata,
    Column('professeur_id', Integer, ForeignKey('professeurs.id')),
    Column('cours_id', Integer, ForeignKey('cours.id'))
)

class Departement(Base):
    __tablename__ = 'departements'
    
    id = Column(Integer, primary_key=True)
    nom = Column(String(100), nullable=False, unique=True)
    budget = Column(Float, default=0.0)
    chef_departement_id = Column(Integer, ForeignKey('professeurs.id'))
    
    # Relations
    professeurs = relationship("Professeur", back_populates="departement", foreign_keys="Professeur.departement_id")
    chef = relationship("Professeur", foreign_keys=[chef_departement_id])
    
    def __repr__(self):
        return f"<Departement(nom='{self.nom}', budget={self.budget})>"

class Professeur(Base):
    __tablename__ = 'professeurs'
    
    id = Column(Integer, primary_key=True)
    nom = Column(String(50), nullable=False)
    prenom = Column(String(50), nullable=False)
    email = Column(String(100), nullable=False, unique=True)
    specialite = Column(String(100))
    date_embauche = Column(DateTime, default=datetime.utcnow)
    departement_id = Column(Integer, ForeignKey('departements.id'))
    
    # Relations
    departement = relationship("Departement", back_populates="professeurs", foreign_keys=[departement_id])
    cours = relationship("Cours", secondary=professeur_cours_association, back_populates="professeurs")
    
    def __repr__(self):
        return f"<Professeur(nom='{self.nom}', prenom='{self.prenom}', specialite='{self.specialite}')>"

class Etudiant(Base):
    __tablename__ = 'etudiants'
    
    id = Column(Integer, primary_key=True)
    nom = Column(String(50), nullable=False)
    prenom = Column(String(50), nullable=False)
    date_naissance = Column(DateTime, nullable=False)
    email = Column(String(100), nullable=False, unique=True)
    numero_etudiant = Column(String(20), nullable=False, unique=True)
    
    # Relations
    inscriptions = relationship("Inscription", back_populates="etudiant")
    
    def __repr__(self):
        return f"<Etudiant(nom='{self.nom}', prenom='{self.prenom}', numero='{self.numero_etudiant}')>"

class Cours(Base):
    __tablename__ = 'cours'
    
    id = Column(Integer, primary_key=True)
    nom = Column(String(100), nullable=False)
    code = Column(String(10), nullable=False, unique=True)
    credits = Column(Integer, default=3)
    description = Column(Text)
    
    # Relations
    professeurs = relationship("Professeur", secondary=professeur_cours_association, back_populates="cours")
    inscriptions = relationship("Inscription", back_populates="cours")
    
    def __repr__(self):
        return f"<Cours(nom='{self.nom}', code='{self.code}', credits={self.credits})>"

class Inscription(Base):
    __tablename__ = 'inscriptions'
    
    id = Column(Integer, primary_key=True)
    etudiant_id = Column(Integer, ForeignKey('etudiants.id'), nullable=False)
    cours_id = Column(Integer, ForeignKey('cours.id'), nullable=False)
    date_inscription = Column(DateTime, default=datetime.utcnow)
    note = Column(Float, nullable=True)  # Note finale (optionnel)
    
    # Relations
    etudiant = relationship("Etudiant", back_populates="inscriptions")
    cours = relationship("Cours", back_populates="inscriptions")
    
    def __repr__(self):
        return f"<Inscription(etudiant_id={self.etudiant_id}, cours_id={self.cours_id}, note={self.note})>"

class GestionnaireEcole:
    """Gestionnaire pour les operations CRUD"""
    
    def __init__(self, database_url="sqlite:///ecole.db"):
        self.engine = create_engine(database_url, echo=True)
        Base.metadata.create_all(self.engine)
        Session = sessionmaker(bind=self.engine)
        self.session = Session()
    
    # Methodes pour Departement
    def creer_departement(self, nom, budget=0.0):
        """Cree un nouveau departement"""
        pass
    
    def obtenir_departement(self, departement_id):
        """Obtient un departement par ID"""
        pass
    
    def lister_departements(self):
        """Liste tous les departements"""
        pass
    
    def mettre_a_jour_departement(self, departement_id, **kwargs):
        """Met a jour un departement"""
        pass
    
    def supprimer_departement(self, departement_id):
        """Supprime un departement"""
        pass
    
    # Methodes pour Professeur
    def creer_professeur(self, nom, prenom, email, specialite, departement_id=None):
        """Cree un nouveau professeur"""
        pass
    
    def obtenir_professeur(self, professeur_id):
        """Obtient un professeur par ID"""
        pass
    
    def lister_professeurs_par_departement(self, departement_id):
        """Liste les professeurs d'un departement"""
        pass
    
    def assigner_cours_professeur(self, professeur_id, cours_id):
        """Assigne un cours a un professeur"""
        pass
    
    # Methodes pour Etudiant
    def creer_etudiant(self, nom, prenom, date_naissance, email, numero_etudiant):
        """Cree un nouveau etudiant"""
        pass
    
    def obtenir_etudiant(self, etudiant_id):
        """Obtient un etudiant par ID"""
        pass
    
    def rechercher_etudiants(self, terme_recherche):
        """Recherche des etudiants par nom/prenom"""
        pass
    
    # Methodes pour Cours
    def creer_cours(self, nom, code, credits=3, description=None):
        """Cree un nouveau cours"""
        pass
    
    def obtenir_cours(self, cours_id):
        """Obtient un cours par ID"""
        pass
    
    def lister_cours_par_professeur(self, professeur_id):
        """Liste les cours d'un professeur"""
        pass
    
    # Methodes pour Inscription
    def inscrire_etudiant(self, etudiant_id, cours_id):
        """Inscrit un etudiant a un cours"""
        pass
    
    def attribuer_note(self, etudiant_id, cours_id, note):
        """Attribue une note a un etudiant pour un cours"""
        pass
    
    def obtenir_notes_etudiant(self, etudiant_id):
        """Obtient toutes les notes d'un etudiant"""
        pass
    
    def calculer_moyenne_etudiant(self, etudiant_id):
        """Calcule la moyenne d'un etudiant"""
        pass
    
    # Methodes de requetes complexes
    def obtenir_statistiques_departement(self, departement_id):
        """Obtient les statistiques d'un departement"""
        pass
    
    def obtenir_cours_populaires(self, limite=5):
        """Obtient les cours avec le plus d'inscriptions"""
        pass
    
    def obtenir_etudiants_excellents(self, seuil_moyenne=85):
        """Obtient les etudiants avec une moyenne superieure au seuil"""
        pass
    
    def fermer_session(self):
        """Ferme la session de base de donnees"""
        self.session.close()

# Script de demonstration
if __name__ == "__main__":
    gestionnaire = GestionnaireEcole()
    
    try:
        # Creer des donnees de test
        print("Creation des donnees de test...")
        
        # Creer des departements
        dept_info = gestionnaire.creer_departement("Informatique", 100000)
        dept_math = gestionnaire.creer_departement("Mathematiques", 80000)
        
        # Creer des professeurs
        prof1 = gestionnaire.creer_professeur("Dupont", "Jean", "j.dupont@ecole.com", "Programmation", dept_info.id)
        prof2 = gestionnaire.creer_professeur("Martin", "Marie", "m.martin@ecole.com", "Algebre", dept_math.id)
        
        # Creer des cours
        cours1 = gestionnaire.creer_cours("Python Avance", "INF301", 4, "Programmation Python avancee")
        cours2 = gestionnaire.creer_cours("Algebre Lineaire", "MAT201", 3, "Mathematiques appliquees")
        
        # Creer des etudiants
        etudiant1 = gestionnaire.creer_etudiant("Tremblay", "Alice", datetime(2000, 5, 15), "a.tremblay@etudiant.com", "E20001")
        etudiant2 = gestionnaire.creer_etudiant("Gagnon", "Bob", datetime(1999, 8, 22), "b.gagnon@etudiant.com", "E20002")
        
        # Inscriptions et notes
        gestionnaire.inscrire_etudiant(etudiant1.id, cours1.id)
        gestionnaire.inscrire_etudiant(etudiant1.id, cours2.id)
        gestionnaire.attribuer_note(etudiant1.id, cours1.id, 87.5)
        gestionnaire.attribuer_note(etudiant1.id, cours2.id, 92.0)
        
        # Afficher des statistiques
        print(f"Moyenne d'Alice: {gestionnaire.calculer_moyenne_etudiant(etudiant1.id)}")
        
    finally:
        gestionnaire.fermer_session()
```

### Criteres de Validation
- [ ] Tous les modeles correctement definis
- [ ] Relations entre entites fonctionnelles
- [ ] Operations CRUD completes
- [ ] Requetes complexes implementees
- [ ] Gestion des erreurs appropriee
- [ ] Transactions correctement gerees

## Exercice 11.2 - Systeme de Blog avec Commentaires

### Enonce
Creez un systeme de blog avec les fonctionnalites suivantes :

**Modeles :**
- **Utilisateur** : nom_utilisateur, email, mot_de_passe_hash, date_creation
- **Article** : titre, contenu, date_creation, date_modification, auteur_id, statut
- **Commentaire** : contenu, date_creation, article_id, auteur_id, parent_id (pour reponses)
- **Tag** : nom, description
- **ArticleTag** : article_id, tag_id (table d'association)

**Fonctionnalites avancees :**
- Commentaires imbriqu\u00e9s (reponses aux commentaires)
- Systeme de tags pour les articles
- Statuts d'articles (brouillon, publie, archive)
- Recherche full-text dans les articles

### Structure Recommandee
```python
from sqlalchemy import create_engine, Column, Integer, String, DateTime, Text, ForeignKey, Table, Enum
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship
from enum import Enum as PyEnum
import hashlib
from datetime import datetime

Base = declarative_base()

class StatutArticle(PyEnum):
    BROUILLON = "brouillon"
    PUBLIE = "publie"
    ARCHIVE = "archive"

# Table d'association pour les tags
article_tag_association = Table(
    'article_tags', Base.metadata,
    Column('article_id', Integer, ForeignKey('articles.id')),
    Column('tag_id', Integer, ForeignKey('tags.id'))
)

class Utilisateur(Base):
    __tablename__ = 'utilisateurs'
    
    # Votre implementation ici
    pass

class Article(Base):
    __tablename__ = 'articles'
    
    # Votre implementation ici
    pass

class Commentaire(Base):
    __tablename__ = 'commentaires'
    
    # Implementation avec support des commentaires imbriqu\u00e9s
    pass

class Tag(Base):
    __tablename__ = 'tags'
    
    # Votre implementation ici
    pass

class GestionnaireBlog:
    """Gestionnaire pour le systeme de blog"""
    
    def __init__(self, database_url="sqlite:///blog.db"):
        # Votre implementation ici
        pass
    
    # Methodes pour utilisateurs
    def creer_utilisateur(self, nom_utilisateur, email, mot_de_passe):
        """Cree un utilisateur avec mot de passe hache"""
        pass
    
    def authentifier_utilisateur(self, nom_utilisateur, mot_de_passe):
        """Authentifie un utilisateur"""
        pass
    
    # Methodes pour articles
    def creer_article(self, titre, contenu, auteur_id, tags=None):
        """Cree un nouvel article"""
        pass
    
    def publier_article(self, article_id):
        """Change le statut d'un article en publie"""
        pass
    
    def rechercher_articles(self, terme_recherche):
        """Recherche dans les titres et contenus"""
        pass
    
    def obtenir_articles_par_tag(self, tag_nom):
        """Obtient les articles avec un tag specifique"""
        pass
    
    # Methodes pour commentaires
    def ajouter_commentaire(self, article_id, auteur_id, contenu, parent_id=None):
        """Ajoute un commentaire (ou une reponse)"""
        pass
    
    def obtenir_commentaires_article(self, article_id):
        """Obtient tous les commentaires d'un article avec leur hierarchie"""
        pass
    
    # Methodes pour tags
    def creer_tag(self, nom, description=None):
        """Cree un nouveau tag"""
        pass
    
    def obtenir_tags_populaires(self, limite=10):
        """Obtient les tags les plus utilises"""
        pass
```

### Criteres de Validation
- [ ] Modeles avec relations complexes
- [ ] Commentaires imbriqu\u00e9s fonctionnels
- [ ] Systeme de tags operationnel
- [ ] Recherche full-text
- [ ] Authentification securisee
- [ ] Gestion des statuts

## Exercice 11.3 - Systeme de Reservation d'Hotel

### Enonce
Developpez un systeme de reservation d'hotel complet :

**Entites :**
- **Hotel** : nom, adresse, nombre_etoiles, telephone
- **TypeChambre** : nom, description, prix_base, capacite_max
- **Chambre** : numero, hotel_id, type_chambre_id, etage
- **Client** : nom, prenom, email, telephone, date_naissance
- **Reservation** : client_id, chambre_id, date_debut, date_fin, prix_total, statut
- **Service** : nom, description, prix
- **ReservationService** : reservation_id, service_id, quantite

**Contraintes business :**
- Une chambre ne peut pas etre reservee deux fois pour des dates qui se chevauchent
- Le prix total doit etre calcule automatiquement
- Gestion des annulations avec politiques de remboursement

### Code de Depart
```python
from sqlalchemy import create_engine, Column, Integer, String, DateTime, Float, ForeignKey, Boolean, CheckConstraint
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship, validates
from datetime import datetime, timedelta
from enum import Enum as PyEnum

Base = declarative_base()

class StatutReservation(PyEnum):
    CONFIRMEE = "confirmee"
    ANNULEE = "annulee"
    TERMINEE = "terminee"

class Hotel(Base):
    __tablename__ = 'hotels'
    
    # Votre implementation ici
    pass

class TypeChambre(Base):
    __tablename__ = 'types_chambres'
    
    # Votre implementation ici
    pass

class Chambre(Base):
    __tablename__ = 'chambres'
    
    # Votre implementation ici
    pass

class Client(Base):
    __tablename__ = 'clients'
    
    # Votre implementation ici
    pass

class Reservation(Base):
    __tablename__ = 'reservations'
    
    # Implementation avec validations
    
    @validates('date_debut', 'date_fin')
    def valider_dates(self, key, value):
        """Valide les dates de reservation"""
        pass
    
    def calculer_prix_total(self):
        """Calcule le prix total de la reservation"""
        pass

class Service(Base):
    __tablename__ = 'services'
    
    # Votre implementation ici
    pass

class ReservationService(Base):
    __tablename__ = 'reservation_services'
    
    # Votre implementation ici
    pass

class GestionnaireReservation:
    """Gestionnaire pour le systeme de reservation"""
    
    def __init__(self, database_url="sqlite:///hotel.db"):
        # Votre implementation ici
        pass
    
    def verifier_disponibilite(self, chambre_id, date_debut, date_fin):
        """Verifie si une chambre est disponible pour les dates donnees"""
        pass
    
    def creer_reservation(self, client_id, chambre_id, date_debut, date_fin, services=None):
        """Cree une nouvelle reservation avec verification de disponibilite"""
        pass
    
    def annuler_reservation(self, reservation_id, raison=None):
        """Annule une reservation"""
        pass
    
    def obtenir_chambres_disponibles(self, hotel_id, date_debut, date_fin, type_chambre=None):
        """Obtient les chambres disponibles pour des dates donnees"""
        pass
    
    def calculer_chiffre_affaires(self, hotel_id, date_debut, date_fin):
        """Calcule le chiffre d'affaires pour une periode"""
        pass
    
    def obtenir_taux_occupation(self, hotel_id, date_debut, date_fin):
        """Calcule le taux d'occupation pour une periode"""
        pass
```

### Criteres de Validation
- [ ] Verification de disponibilite fonctionnelle
- [ ] Calcul automatique des prix
- [ ] Gestion des conflits de reservation
- [ ] Validations business implementees
- [ ] Requetes de reporting
- [ ] Gestion des services additionnels

## Questions de Reflexion

1. **Relations complexes** : Comment gerer efficacement les relations N-N avec des donnees additionnelles ?

2. **Performance** : Quelles strategies utiliser pour optimiser les requetes ORM ?

3. **Migrations** : Comment gerer l'evolution du schema de base de donnees ?

4. **Transactions** : Quand et comment utiliser les transactions avec SQLAlchemy ?

## Ressources Complementaires

- **Cours de reference** : `26. ORM vs SQL natif - introduction a SQLAlchemy`
- **Relations** : `32. Definir des modeles ORM relationnels`
- **Migrations** : `31. Migrations avec SQLAlchemy et Alembic`

## Conseils Avances

1. **Lazy loading** : Comprenez les strategies de chargement des relations
2. **Validations** : Utilisez les validateurs SQLAlchemy pour les contraintes business
3. **Sessions** : Gerez correctement le cycle de vie des sessions
4. **Performances** : Utilisez eager loading quand necessaire
5. **Tests** : Testez vos modeles avec une base de donnees en memoire

Excellente maitrise de l'ORM !
