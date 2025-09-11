Voici la version corrigée en français.

# Quiz 25 – Révision générale

## Questions à choix multiples

### Question 1

Quelle est la principale différence entre Tkinter et PySide6 ?

a) Tkinter est plus moderne que PySide6
b) PySide6 offre plus de widgets et de fonctionnalités avancées
c) Tkinter est plus rapide que PySide6
d) Il n’y a pas de différence significative

**Réponse correcte :** b) PySide6 offre plus de widgets et de fonctionnalités avancées

---

### Question 2

Quel est l’avantage principal d’utiliser un ORM comme SQLAlchemy ?

a) Il est plus rapide que le SQL natif
b) Il abstrait les différences entre les bases de données
c) Il consomme moins de mémoire
d) Il n’y a pas d’avantage particulier

**Réponse correcte :** b) Il abstrait les différences entre les bases de données

---

### Question 3

Dans une API REST, que signifie l’acronyme CRUD ?

a) Create, Read, Update, Delete
b) Connect, Retrieve, Upload, Download
c) Copy, Rename, Undo, Delete
d) Create, Retrieve, Upgrade, Deploy

**Réponse correcte :** a) Create, Read, Update, Delete

---

### Question 4

Quel code HTTP est renvoyé pour une création réussie dans une API REST ?

a) 200 OK
b) 201 Created
c) 204 No Content
d) 301 Moved Permanently

**Réponse correcte :** b) 201 Created

---

### Question 5

Quelle est la meilleure pratique pour gérer les mots de passe dans une application ?

a) Les stocker en texte clair
b) Les chiffrer avec une clé secrète
c) Les hacher avec un algorithme sécurisé (bcrypt, Argon2)
d) Les encoder en base64

**Réponse correcte :** c) Les hacher avec un algorithme sécurisé (bcrypt, Argon2)

---

### Question 6

Dans FastAPI, quel décorateur utilise-t-on pour créer un endpoint GET ?

a) `@app.get()`
b) `@app.route()`
c) `@app.endpoint()`
d) `@app.method()`

**Réponse correcte :** a) `@app.get()`

---

### Question 7

Quelle est la fonction de Pydantic dans FastAPI ?

a) Gérer la base de données
b) Valider et sérialiser les données automatiquement
c) Créer l’interface utilisateur
d) Gérer l’authentification

**Réponse correcte :** b) Valider et sérialiser les données automatiquement

---

### Question 8

Dans SQLAlchemy, que signifie une relation `lazy='select'` ?

a) La relation n’est jamais chargée
b) La relation est chargée immédiatement
c) La relation est chargée à la demande par une requête séparée
d) La relation est mise en cache

**Réponse correcte :** c) La relation est chargée à la demande par une requête séparée

---

### Question 9

Quel est l’objectif principal d’utiliser des migrations de base de données ?

a) Accélérer les requêtes
b) Gérer l’évolution du schéma de manière contrôlée
c) Réduire la taille de la base
d) Améliorer la sécurité

**Réponse correcte :** b) Gérer l’évolution du schéma de manière contrôlée

---

### Question 10

Dans le contexte des interfaces graphiques, qu’est-ce qu’un « callback » ?

a) Une fonction qui sauvegarde les données
b) Une fonction appelée en réponse à un événement
c) Une fonction qui charge les données
d) Une fonction qui valide les entrées

**Réponse correcte :** b) Une fonction appelée en réponse à un événement

---

## Questions de synthèse

### Question 11

Comparez les architectures MVC et MVP dans le contexte d’une application desktop. Donnez un exemple concret pour chacune.

**Réponse attendue :**

**Architecture MVC (Model–View–Controller) :**

* **Model** : Gère les données et la logique métier
* **View** : Affiche les données à l’utilisateur
* **Controller** : Gère les interactions utilisateur et coordonne Model/View

**Architecture MVP (Model–View–Presenter) :**

* **Model** : Gère les données et la logique métier
* **View** : Interface passive, délègue tout au Presenter
* **Presenter** : Contient toute la logique de présentation

**Exemple MVC avec PySide6 :**

```python
# Model
class StudentModel:
    def __init__(self):
        self.students = []
    
    def add_student(self, name, age):
        self.students.append({"name": name, "age": age})
    
    def get_students(self):
        return self.students

# View
class StudentView(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setup_ui()
    
    def setup_ui(self):
        self.table = QTableWidget()
        # ... configuration de l’interface

# Controller
class StudentController:
    def __init__(self, model, view):
        self.model = model
        self.view = view
        self.connect_signals()
    
    def add_student(self):
        name = self.view.name_input.text()
        age = self.view.age_input.value()
        self.model.add_student(name, age)
        self.update_view()
```

**Exemple MVP :**

```python
# Presenter
class StudentPresenter:
    def __init__(self, model, view):
        self.model = model
        self.view = view
        self.view.set_presenter(self)  # La View connaît le Presenter
    
    def on_add_student_clicked(self, name, age):
        if self.validate_input(name, age):
            self.model.add_student(name, age)
            self.view.update_student_list(self.model.get_students())
        else:
            self.view.show_error("Données invalides")

# View (plus passive)
class StudentView(QMainWindow):
    def set_presenter(self, presenter):
        self.presenter = presenter
        self.add_button.clicked.connect(
            lambda: self.presenter.on_add_student_clicked(
                self.name_input.text(),
                self.age_input.value()
            )
        )
```

**Différences principales :**

* Dans MVC, la View peut accéder directement au Model.
* Dans MVP, la View ne connaît que le Presenter.
* MVP facilite les tests unitaires (View plus passive).

---

### Question 12

Expliquez le concept de « séparation des responsabilités » dans une application client–serveur. Comment l’implémenter concrètement ?

**Réponse attendue :**

**Principe de séparation des responsabilités :**
Chaque composant doit avoir une responsabilité unique et bien définie, ce qui facilite la maintenance, les tests et l’évolution.

**Dans une architecture client–serveur :**

**Côté serveur (API) :**

```python
# 1. Modèles de données (responsabilité : structure des données)
class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    email = Column(String, unique=True)

# 2. Services métier (responsabilité : logique métier)
class UserService:
    def __init__(self, db_session):
        self.db = db_session
    
    def create_user(self, email, password):
        # Validation métier
        if self.email_exists(email):
            raise ValueError("Email déjà utilisé")
        
        # Hachage du mot de passe
        hashed_password = hash_password(password)
        
        # Création
        user = User(email=email, password=hashed_password)
        self.db.add(user)
        self.db.commit()
        return user

# 3. Couche API (responsabilité : exposition HTTP)
@app.post("/users/")
async def create_user_endpoint(user_data: UserCreate):
    try:
        user = user_service.create_user(user_data.email, user_data.password)
        return UserResponse.from_orm(user)
    except ValueError as e:
        raise HTTPException(status_code=400, detail=str(e))

# 4. Schémas de validation (responsabilité : validation des données)
class UserCreate(BaseModel):
    email: EmailStr
    password: str = Field(min_length=8)

class UserResponse(BaseModel):
    id: int
    email: str
    
    class Config:
        from_attributes = True
```

**Côté client (interface) :**

```python
# 1. Services de communication (responsabilité : communication avec l’API)
class UserApiService:
    def __init__(self, base_url):
        self.base_url = base_url
    
    async def create_user(self, email, password):
        async with httpx.AsyncClient() as client:
            response = await client.post(
                f"{self.base_url}/users/",
                json={"email": email, "password": password}
            )
            response.raise_for_status()
            return response.json()

# 2. Modèles client (responsabilité : représentation locale des données)
class ClientUser:
    def __init__(self, id, email):
        self.id = id
        self.email = email

# 3. Contrôleurs/Presenters (responsabilité : logique de présentation)
class UserController:
    def __init__(self, api_service, view):
        self.api_service = api_service
        self.view = view
    
    async def create_user(self, email, password):
        try:
            # Validation côté client
            if not self.validate_email(email):
                self.view.show_error("Email invalide")
                return
            
            # Appel API
            user_data = await self.api_service.create_user(email, password)
            user = ClientUser(user_data["id"], user_data["email"])
            
            # Mise à jour de l’interface
            self.view.add_user_to_list(user)
            self.view.show_success("Utilisateur créé")
            
        except httpx.HTTPStatusError as e:
            self.view.show_error(f"Erreur : {e.response.json()['detail']}")

# 4. Vues (responsabilité : affichage et interaction utilisateur)
class UserView(QMainWindow):
    def __init__(self, controller):
        super().__init__()
        self.controller = controller
        self.setup_ui()
    
    def on_create_user_clicked(self):
        email = self.email_input.text()
        password = self.password_input.text()
        
        # Déléguer au contrôleur
        asyncio.create_task(
            self.controller.create_user(email, password)
        )
```

**Avantages de cette séparation :**

* **Testabilité** : chaque couche peut être testée indépendamment
* **Maintenabilité** : modifications localisées
* **Réutilisabilité** : services réutilisables
* **Scalabilité** : possibilité de remplacer/faire évoluer chaque couche

---

### Question 13

Décrivez une stratégie complète de gestion d’erreurs dans une application full-stack (client PySide6 + API FastAPI).

**Réponse attendue :**

**Stratégie de gestion d’erreurs multi-niveaux :**

**1. Niveau base de données / ORM :**

```python
# Exceptions personnalisées
class DatabaseError(Exception):
    pass

class UserNotFoundError(DatabaseError):
    pass

class DuplicateEmailError(DatabaseError):
    pass

# Service avec gestion d’erreurs
class UserService:
    def get_user(self, user_id):
        try:
            user = self.db.query(User).filter(User.id == user_id).first()
            if not user:
                raise UserNotFoundError(f"Utilisateur {user_id} non trouvé")
            return user
        except SQLAlchemyError as e:
            logger.error(f"Erreur base de données : {e}")
            raise DatabaseError("Erreur d’accès aux données")
```

**2. Niveau API (FastAPI) :**

```python
from fastapi import HTTPException, status
from fastapi.responses import JSONResponse
from fastapi.exceptions import RequestValidationError

# Gestionnaires d’exceptions globaux
@app.exception_handler(UserNotFoundError)
async def user_not_found_handler(request, exc):
    return JSONResponse(
        status_code=status.HTTP_404_NOT_FOUND,
        content={"error": "USER_NOT_FOUND", "message": str(exc)}
    )

@app.exception_handler(DuplicateEmailError)
async def duplicate_email_handler(request, exc):
    return JSONResponse(
        status_code=status.HTTP_409_CONFLICT,
        content={"error": "DUPLICATE_EMAIL", "message": str(exc)}
    )

@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request, exc):
    return JSONResponse(
        status_code=status.HTTP_422_UNPROCESSABLE_ENTITY,
        content={"error": "VALIDATION_ERROR", "details": exc.errors()}
    )

@app.exception_handler(DatabaseError)
async def database_error_handler(request, exc):
    return JSONResponse(
        status_code=status.HTTP_500_INTERNAL_SERVER_ERROR,
        content={"error": "DATABASE_ERROR", "message": "Erreur interne"}
    )

# Endpoint avec gestion d’erreurs
@app.get("/users/{user_id}")
async def get_user(user_id: int):
    try:
        user = user_service.get_user(user_id)
        return UserResponse.from_orm(user)
    except UserNotFoundError:
        raise  # Gérée par le gestionnaire global
    except Exception as e:
        logger.error(f"Erreur inattendue : {e}")
        raise HTTPException(
            status_code=500,
            detail="Erreur interne du serveur"
        )
```

**3. Niveau client (service de communication) :**

```python
import httpx
from typing import Optional
import logging

class ApiError(Exception):
    def __init__(self, status_code: int, error_type: str, message: str):
        self.status_code = status_code
        self.error_type = error_type
        self.message = message
        super().__init__(message)

class ApiService:
    def __init__(self, base_url: str):
        self.base_url = base_url
        self.logger = logging.getLogger(__name__)
    
    async def get_user(self, user_id: int) -> dict:
        try:
            async with httpx.AsyncClient() as client:
                response = await client.get(
                    f"{self.base_url}/users/{user_id}",
                    timeout=10.0
                )
                
                if response.status_code == 200:
                    return response.json()
                else:
                    # Gestion des erreurs HTTP
                    error_data = response.json()
                    raise ApiError(
                        status_code=response.status_code,
                        error_type=error_data.get("error", "UNKNOWN_ERROR"),
                        message=error_data.get("message", "Erreur inconnue")
                    )
                    
        except httpx.TimeoutException:
            raise ApiError(0, "TIMEOUT", "Timeout de connexion")
        except httpx.ConnectError:
            raise ApiError(0, "CONNECTION_ERROR", "Impossible de se connecter au serveur")
        except httpx.HTTPStatusError as e:
            self.logger.error(f"Erreur HTTP : {e}")
            raise ApiError(e.response.status_code, "HTTP_ERROR", str(e))
```

**4. Niveau interface utilisateur (PySide6) :**

```python
from PySide6.QtWidgets import QMessageBox
import asyncio
import logging

class UserController:
    def __init__(self, api_service, view):
        self.api_service = api_service
        self.view = view
        self.logger = logging.getLogger(__name__)
    
    async def load_user(self, user_id: int):
        try:
            # Afficher l’indicateur de chargement
            self.view.show_loading(True)
            
            user_data = await self.api_service.get_user(user_id)
            self.view.display_user(user_data)
            
        except ApiError as e:
            self.handle_api_error(e)
        except Exception as e:
            self.logger.error(f"Erreur inattendue : {e}")
            self.view.show_error("Une erreur inattendue s’est produite")
        finally:
            self.view.show_loading(False)
    
    def handle_api_error(self, error: ApiError):
        """Gestion centralisée des erreurs API"""
        error_messages = {
            "USER_NOT_FOUND": "Utilisateur non trouvé",
            "VALIDATION_ERROR": "Données invalides",
            "DUPLICATE_EMAIL": "Cet email est déjà utilisé",
            "DATABASE_ERROR": "Erreur de base de données",
            "TIMEOUT": "Timeout de connexion. Vérifiez votre connexion internet.",
            "CONNECTION_ERROR": "Impossible de se connecter au serveur"
        }
        
        message = error_messages.get(error.error_type, error.message)
        
        if error.status_code == 404:
            self.view.show_warning(message)
        elif error.status_code in [400, 422]:
            self.view.show_validation_error(message)
        elif error.status_code >= 500:
            self.view.show_error("Erreur serveur. Réessayez plus tard.")
        else:
            self.view.show_error(message)

class UserView(QMainWindow):
    def show_error(self, message: str):
        QMessageBox.critical(self, "Erreur", message)
    
    def show_warning(self, message: str):
        QMessageBox.warning(self, "Attention", message)
    
    def show_validation_error(self, message: str):
        QMessageBox.information(self, "Validation", message)
    
    def show_loading(self, loading: bool):
        # Afficher/masquer l’indicateur de chargement
        self.loading_indicator.setVisible(loading)
        self.content_widget.setEnabled(not loading)
```

**5. Journalisation et monitoring :**

```python
import logging
from datetime import datetime

# Configuration du logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('app.log'),
        logging.StreamHandler()
    ]
)

# Logger personnalisé pour les erreurs utilisateur
class UserActivityLogger:
    def __init__(self):
        self.logger = logging.getLogger("user_activity")
    
    def log_error(self, user_id: Optional[int], action: str, error: str):
        self.logger.error(
            f"User {user_id} - Action : {action} - Error : {error}"
        )
    
    def log_success(self, user_id: Optional[int], action: str):
        self.logger.info(
            f"User {user_id} - Action : {action} - Success"
        )
```

**Principes clés :**

* **Propagation contrôlée** : les erreurs remontent avec des informations adaptées à chaque niveau
* **Messages utilisateur** : erreurs techniques traduites en messages compréhensibles
* **Traçabilité** : journalisation complète pour le débogage
* **Dégradation maîtrisée** : l’application continue de fonctionner même en cas d’erreur
* **Retour utilisateur** : indicateurs visuels (chargement, erreurs)

---

### Question 14

Proposez une architecture complète pour une application de gestion de projets avec authentification, incluant la structure des fichiers et les technologies utilisées.

**Réponse attendue :**

**Architecture proposée :**

```
projet_management/
│
├── backend/                          # API FastAPI
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py                   # Point d’entrée FastAPI
│   │   ├── config.py                 # Configuration
│   │   ├── database.py               # Configuration base de données
│   │   │
│   │   ├── models/                   # Modèles SQLAlchemy
│   │   │   ├── __init__.py
│   │   │   ├── base.py
│   │   │   ├── user.py
│   │   │   ├── project.py
│   │   │   ├── task.py
│   │   │   └── team.py
│   │   │
│   │   ├── schemas/                  # Schémas Pydantic
│   │   │   ├── __init__.py
│   │   │   ├── user.py
│   │   │   ├── project.py
│   │   │   ├── task.py
│   │   │   └── auth.py
│   │   │
│   │   ├── services/                 # Logique métier
│   │   │   ├── __init__.py
│   │   │   ├── auth_service.py
│   │   │   ├── user_service.py
│   │   │   ├── project_service.py
│   │   │   └── task_service.py
│   │   │
│   │   ├── routers/                  # Endpoints API
│   │   │   ├── __init__.py
│   │   │   ├── auth.py
│   │   │   ├── users.py
│   │   │   ├── projects.py
│   │   │   └── tasks.py
│   │   │
│   │   ├── dependencies/             # Dépendances FastAPI
│   │   │   ├── __init__.py
│   │   │   ├── auth.py
│   │   │   └── database.py
│   │   │
│   │   └── utils/                    # Utilitaires
│   │       ├── __init__.py
│   │       ├── security.py
│   │       ├── email.py
│   │       └── validators.py
│   │
│   ├── alembic/                      # Migrations
│   │   ├── versions/
│   │   ├── env.py
│   │   └── alembic.ini
│   │
│   ├── tests/                        # Tests backend
│   │   ├── __init__.py
│   │   ├── conftest.py
│   │   ├── test_auth.py
│   │   ├── test_projects.py
│   │   └── test_tasks.py
│   │
│   ├── requirements.txt
│   └── .env
│
├── frontend/                         # Application PySide6
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py                   # Point d’entrée
│   │   ├── config.py                 # Configuration client
│   │   │
│   │   ├── models/                   # Modèles client
│   │   │   ├── __init__.py
│   │   │   ├── user.py
│   │   │   ├── project.py
│   │   │   └── task.py
│   │   │
│   │   ├── services/                 # Services API
│   │   │   ├── __init__.py
│   │   │   ├── base_service.py
│   │   │   ├── auth_service.py
│   │   │   ├── project_service.py
│   │   │   └── task_service.py
│   │   │
│   │   ├── controllers/              # Logique de contrôle
│   │   │   ├── __init__.py
│   │   │   ├── auth_controller.py
│   │   │   ├── main_controller.py
│   │   │   ├── project_controller.py
│   │   │   └── task_controller.py
│   │   │
│   │   ├── views/                    # Interfaces utilisateur
│   │   │   ├── __init__.py
│   │   │   ├── base_view.py
│   │   │   ├── login_view.py
│   │   │   ├── main_view.py
│   │   │   ├── project_view.py
│   │   │   ├── task_view.py
│   │   │   └── dialogs/
│   │   │       ├── __init__.py
│   │   │       ├── project_dialog.py
│   │   │       └── task_dialog.py
│   │   │
│   │   ├── widgets/                  # Widgets personnalisés
│   │   │   ├── __init__.py
│   │   │   ├── task_widget.py
│   │   │   ├── project_widget.py
│   │   │   └── user_widget.py
│   │   │
│   │   └── utils/                    # Utilitaires client
│   │       ├── __init__.py
│   │       ├── validators.py
│   │       ├── formatters.py
│   │       └── helpers.py
│   │
│   ├── resources/                    # Ressources
│   │   ├── icons/
│   │   ├── styles/
│   │   │   └── main.qss
│   │   └── ui/                       # Fichiers Qt Designer
│   │       ├── login.ui
│   │       ├── main_window.ui
│   │       └── dialogs/
│   │
│   ├── tests/                        # Tests frontend
│   │   ├── __init__.py
│   │   ├── test_controllers.py
│   │   ├── test_services.py
│   │   └── test_views.py
│   │
│   └── requirements.txt
│
├── shared/                           # Code partagé
│   ├── __init__.py
│   ├── constants.py
│   ├── enums.py
│   └── exceptions.py
│
├── docs/                             # Documentation
│   ├── api.md
│   ├── installation.md
│   ├── user_guide.md
│   └── developer_guide.md
│
├── docker-compose.yml                # Docker pour le dev
├── Dockerfile.backend
├── Dockerfile.frontend
├── README.md
└── .gitignore
```

**Technologies utilisées :**

**Backend :**

```python
# requirements.txt
fastapi==0.104.1
uvicorn[standard]==0.24.0
sqlalchemy==2.0.23
alembic==1.12.1
pydantic[email]==2.5.0
python-jose[cryptography]==3.3.0
passlib[bcrypt]==1.7.4
python-multipart==0.0.6
python-decouple==3.8
httpx==0.25.2
pytest==7.4.3
pytest-asyncio==0.21.1
```

**Frontend :**

```python
# requirements.txt
PySide6==6.6.0
httpx==0.25.2
python-decouple==3.8
pytest==7.4.3
pytest-qt==4.2.0
```

**Implémentation des composants clés :**

**1. Authentification backend :**

```python
# app/services/auth_service.py
from passlib.context import CryptContext
from jose import JWTError, jwt
from datetime import datetime, timedelta

class AuthService:
    def __init__(self):
        self.pwd_context = CryptContext(schemes=["bcrypt"])
        self.SECRET_KEY = "your-secret-key"
        self.ALGORITHM = "HS256"
    
    def verify_password(self, plain_password, hashed_password):
        return self.pwd_context.verify(plain_password, hashed_password)
    
    def get_password_hash(self, password):
        return self.pwd_context.hash(password)
    
    def create_access_token(self, data: dict):
        to_encode = data.copy()
        expire = datetime.utcnow() + timedelta(hours=24)
        to_encode.update({"exp": expire})
        return jwt.encode(to_encode, self.SECRET_KEY, algorithm=self.ALGORITHM)
    
    def verify_token(self, token: str):
        try:
            payload = jwt.decode(token, self.SECRET_KEY, algorithms=[self.ALGORITHM])
            return payload
        except JWTError:
            return None

# app/dependencies/auth.py
from fastapi import Depends, HTTPException, status
from fastapi.security import HTTPBearer

security = HTTPBearer()

async def get_current_user(token: str = Depends(security)):
    auth_service = AuthService()
    payload = auth_service.verify_token(token.credentials)
    if payload is None:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Token invalide"
        )
    return payload
```

**2. Service client avec authentification :**

```python
# app/services/base_service.py
import httpx
from typing import Optional
from app.config import settings

class BaseApiService:
    def __init__(self):
        self.base_url = settings.API_BASE_URL
        self.token: Optional[str] = None
    
    def set_token(self, token: str):
        self.token = token
    
    def get_headers(self):
        headers = {"Content-Type": "application/json"}
        if self.token:
            headers["Authorization"] = f"Bearer {self.token}"
        return headers
    
    async def request(self, method: str, endpoint: str, **kwargs):
        async with httpx.AsyncClient() as client:
            response = await client.request(
                method=method,
                url=f"{self.base_url}{endpoint}",
                headers=self.get_headers(),
                **kwargs
            )
            
            if response.status_code == 401:
                # Token expiré : rediriger vers la connexion
                from app.controllers.auth_controller import AuthController
                AuthController.logout()
                return None
            
            response.raise_for_status()
            return response.json()

# app/services/auth_service.py
class AuthApiService(BaseApiService):
    async def login(self, email: str, password: str):
        response = await self.request(
            "POST",
            "/auth/login",
            json={"email": email, "password": password}
        )
        if response:
            self.set_token(response["access_token"])
        return response
```

**3. Contrôleur principal :**

```python
# app/controllers/main_controller.py
from PySide6.QtCore import QObject, Signal
from app.services.auth_service import AuthApiService
from app.services.project_service import ProjectApiService

class MainController(QObject):
    # Signaux pour la communication avec les vues
    login_successful = Signal()
    login_failed = Signal(str)
    projects_loaded = Signal(list)
    
    def __init__(self):
        super().__init__()
        self.auth_service = AuthApiService()
        self.project_service = ProjectApiService()
        self.current_user = None
    
    async def login(self, email: str, password: str):
        try:
            result = await self.auth_service.login(email, password)
            if result:
                self.current_user = result["user"]
                # Partager le token avec les autres services
                self.project_service.set_token(result["access_token"])
                self.login_successful.emit()
            else:
                self.login_failed.emit("Identifiants invalides")
        except Exception as e:
            self.login_failed.emit(str(e))
    
    async def load_projects(self):
        try:
            projects = await self.project_service.get_all_projects()
            self.projects_loaded.emit(projects)
        except Exception:
            # Gérer l’erreur si besoin
            pass
```

**Avantages de cette architecture :**

* **Séparation claire** des responsabilités
* **Scalabilité** : ajout de fonctionnalités facilité
* **Testabilité** : chaque composant testable indépendamment
* **Maintenabilité** : code organisé et modulaire
* **Sécurité** : authentification JWT robuste
* **Flexibilité** : adaptation aisée

---

### Question 15

Concevez un plan de tests complet pour une application combinant une interface PySide6 et une API FastAPI. Incluez les différents types de tests et des exemples de code.

**Réponse attendue :**

**Stratégie de tests complète :**

**1. Tests backend (API FastAPI) :**

**Tests unitaires des services :**

```python
# tests/test_services/test_auth_service.py
import pytest
from app.services.auth_service import AuthService

class TestAuthService:
    def setup_method(self):
        self.auth_service = AuthService()
    
    def test_password_hashing(self):
        """Test du hachage de mot de passe"""
        password = "test_password_123"
        hashed = self.auth_service.get_password_hash(password)
        
        assert hashed != password
        assert self.auth_service.verify_password(password, hashed)
        assert not self.auth_service.verify_password("wrong_password", hashed)
    
    def test_token_creation_and_verification(self):
        """Test de création et vérification de token"""
        data = {"user_id": 1, "email": "test@example.com"}
        token = self.auth_service.create_access_token(data)
        
        assert token is not None
        
        payload = self.auth_service.verify_token(token)
        assert payload["user_id"] == 1
        assert payload["email"] == "test@example.com"
    
    def test_invalid_token_verification(self):
        """Test de vérification d’un token invalide"""
        invalid_token = "invalid.token.here"
        payload = self.auth_service.verify_token(invalid_token)
        assert payload is None
```

**Tests d’intégration API :**

```python
# tests/test_api/test_auth_endpoints.py
import pytest
from fastapi.testclient import TestClient
from app.main import app
from app.database import get_db
from tests.conftest import override_get_db, test_db

app.dependency_overrides[get_db] = override_get_db
client = TestClient(app)

class TestAuthEndpoints:
    def test_register_user_success(self):
        """Test d’inscription réussie"""
        user_data = {
            "email": "test@example.com",
            "password": "secure_password_123",
            "full_name": "Test User"
        }
        
        response = client.post("/auth/register", json=user_data)
        
        assert response.status_code == 201
        data = response.json()
        assert data["email"] == user_data["email"]
        assert data["full_name"] == user_data["full_name"]
        assert "id" in data
        assert "password" not in data  # Le mot de passe ne doit pas être renvoyé
    
    def test_register_duplicate_email(self):
        """Test d’inscription avec email dupliqué"""
        user_data = {
            "email": "duplicate@example.com",
            "password": "password123",
            "full_name": "User 1"
        }
        
        # Premier utilisateur
        response1 = client.post("/auth/register", json=user_data)
        assert response1.status_code == 201
        
        # Deuxième utilisateur avec le même email
        user_data["full_name"] = "User 2"
        response2 = client.post("/auth/register", json=user_data)
        assert response2.status_code == 400
        assert "email" in response2.json()["detail"].lower()
    
    def test_login_success(self):
        """Test de connexion réussie"""
        # Créer un utilisateur d’abord
        user_data = {
            "email": "login_test@example.com",
            "password": "test_password_123",
            "full_name": "Login Test"
        }
        client.post("/auth/register", json=user_data)
        
        # Tenter la connexion
        login_data = {
            "email": user_data["email"],
            "password": user_data["password"]
        }
        
        response = client.post("/auth/login", json=login_data)
        
        assert response.status_code == 200
        data = response.json()
        assert "access_token" in data
        assert "user" in data
        assert data["user"]["email"] == user_data["email"]
    
    def test_login_invalid_credentials(self):
        """Test de connexion avec identifiants invalides"""
        login_data = {
            "email": "nonexistent@example.com",
            "password": "wrong_password"
        }
        
        response = client.post("/auth/login", json=login_data)
        
        assert response.status_code == 401
        assert "invalid" in response.json()["detail"].lower()
    
    def test_protected_endpoint_without_token(self):
        """Test d’accès à un endpoint protégé sans token"""
        response = client.get("/users/me")
        assert response.status_code == 401
    
    def test_protected_endpoint_with_valid_token(self):
        """Test d’accès à un endpoint protégé avec token valide"""
        # Créer et connecter un utilisateur
        user_data = {
            "email": "protected_test@example.com",
            "password": "test_password_123",
            "full_name": "Protected Test"
        }
        client.post("/auth/register", json=user_data)
        
        login_response = client.post("/auth/login", json={
            "email": user_data["email"],
            "password": user_data["password"]
        })
        token = login_response.json()["access_token"]
        
        # Accéder à l’endpoint protégé
        headers = {"Authorization": f"Bearer {token}"}
        response = client.get("/users/me", headers=headers)
        
        assert response.status_code == 200
        data = response.json()
        assert data["email"] == user_data["email"]
```

**Tests de performance et de charge :**

```python
# tests/test_performance/test_load.py
import asyncio
import time
import httpx
import pytest

class TestAPIPerformance:
    @pytest.mark.asyncio
    async def test_concurrent_requests(self):
        """Test de requêtes concurrentes"""
        base_url = "http://localhost:8000"
        
        async def make_request(session, endpoint):
            start_time = time.time()
            response = await session.get(f"{base_url}{endpoint}")
            end_time = time.time()
            return {
                "status_code": response.status_code,
                "response_time": end_time - start_time
            }
        
        async with httpx.AsyncClient() as client:
            # 100 requêtes concurrentes
            tasks = [
                make_request(client, "/health")
                for _ in range(100)
            ]
            
            results = await asyncio.gather(*tasks)
            
            # Vérifications
            success_count = sum(1 for r in results if r["status_code"] == 200)
            avg_response_time = sum(r["response_time"] for r in results) / len(results)
            
            assert success_count >= 95  # Au moins 95 % de succès
            assert avg_response_time < 1.0  # Temps de réponse moyen < 1 s
```

**2. Tests frontend (PySide6) :**

**Tests unitaires des services client :**

```python
# tests/test_services/test_auth_service_client.py
import pytest
import httpx
from unittest.mock import AsyncMock, patch
from app.services.auth_service import AuthApiService

class TestAuthApiService:
    def setup_method(self):
        self.auth_service = AuthApiService()
    
    @pytest.mark.asyncio
    @patch('httpx.AsyncClient')
    async def test_login_success(self, mock_client):
        """Test de connexion réussie côté client"""
        # Mock de la réponse
        mock_response = AsyncMock()
        mock_response.status_code = 200
        mock_response.json.return_value = {
            "access_token": "fake_token_123",
            "user": {"id": 1, "email": "test@example.com"}
        }
        mock_response.raise_for_status.return_value = None
        
        mock_client_instance = AsyncMock()
        mock_client_instance.request.return_value = mock_response
        mock_client.return_value.__aenter__.return_value = mock_client_instance
        
        # Test
        result = await self.auth_service.login("test@example.com", "password123")
        
        # Vérifications
        assert result["access_token"] == "fake_token_123"
        assert result["user"]["email"] == "test@example.com"
        assert self.auth_service.token == "fake_token_123"
    
    @pytest.mark.asyncio
    @patch('httpx.AsyncClient')
    async def test_login_network_error(self, mock_client):
        """Test de gestion d’erreur réseau"""
        mock_client_instance = AsyncMock()
        mock_client_instance.request.side_effect = httpx.ConnectError("Connection failed")
        mock_client.return_value.__aenter__.return_value = mock_client_instance
        
        with pytest.raises(httpx.ConnectError):
            await self.auth_service.login("test@example.com", "password123")
```

**Tests des contrôleurs :**

```python
# tests/test_controllers/test_auth_controller.py
import pytest
from unittest.mock import AsyncMock, MagicMock
from PySide6.QtCore import QSignalSpy
from app.controllers.auth_controller import AuthController

class TestAuthController:
    def setup_method(self):
        self.controller = AuthController()
        self.controller.auth_service = AsyncMock()
    
    @pytest.mark.asyncio
    async def test_login_success_emits_signal(self):
        """Test que la connexion réussie émet le bon signal"""
        # Mock du service
        self.controller.auth_service.login.return_value = {
            "access_token": "token123",
            "user": {"id": 1, "email": "test@example.com"}
        }
        
        # Spy sur le signal
        spy = QSignalSpy(self.controller.login_successful)
        
        # Action
        await self.controller.login("test@example.com", "password123")
        
        # Vérification
        assert len(spy) == 1  # Signal émis une fois
        assert self.controller.current_user["email"] == "test@example.com"
    
    @pytest.mark.asyncio
    async def test_login_failure_emits_error_signal(self):
        """Test que l’échec de connexion émet le signal d’erreur"""
        # Mock du service pour lever une exception
        self.controller.auth_service.login.side_effect = Exception("Invalid credentials")
        
        # Spy sur le signal d’erreur
        spy = QSignalSpy(self.controller.login_failed)
        
        # Action
        await self.controller.login("test@example.com", "wrongpassword")
        
        # Vérification
        assert len(spy) == 1
        assert "Invalid credentials" in spy[0][0]  # Message d’erreur
```

**Tests d’interface utilisateur :**

```python
# tests/test_views/test_login_view.py
import pytest
from PySide6.QtWidgets import QApplication
from PySide6.QtTest import QTest
from PySide6.QtCore import Qt
from unittest.mock import AsyncMock, MagicMock
from app.views.login_view import LoginView
from app.controllers.auth_controller import AuthController

@pytest.fixture
def app():
    """Fixture pour l’application Qt"""
    return QApplication.instance() or QApplication([])

class TestLoginView:
    def setup_method(self):
        self.controller = MagicMock(spec=AuthController)
        self.view = LoginView(self.controller)
    
    def test_initial_state(self, app):
        """Test de l’état initial de la vue"""
        assert self.view.email_input.text() == ""
        assert self.view.password_input.text() == ""
        assert self.view.login_button.isEnabled()
        assert not self.view.loading_indicator.isVisible()
    
    def test_login_button_click_calls_controller(self, app):
        """Test que le clic sur le bouton appelle le contrôleur"""
        # Saisir des données
        self.view.email_input.setText("test@example.com")
        self.view.password_input.setText("password123")
        
        # Cliquer sur le bouton
        QTest.mouseClick(self.view.login_button, Qt.LeftButton)
        
        # Vérification
        self.controller.login.assert_called_once_with("test@example.com", "password123")
    
    def test_empty_fields_shows_validation_error(self, app):
        """Test de validation des champs vides"""
        # Laisser les champs vides et cliquer
        QTest.mouseClick(self.view.login_button, Qt.LeftButton)
        
        # Vérifier que le contrôleur n’est pas appelé
        self.controller.login.assert_not_called()
        
        # Vérifier qu’un message d’erreur est affiché
        assert self.view.error_label.isVisible()
        assert "requis" in self.view.error_label.text().lower()
    
    def test_login_success_hides_view(self, app):
        """Test que le succès de connexion masque la vue"""
        # Simuler un succès de connexion
        self.view.on_login_successful()
        
        # Vérification
        assert not self.view.isVisible()
    
    def test_login_error_shows_message(self, app):
        """Test que l’erreur de connexion affiche un message"""
        error_message = "Identifiants invalides"
        
        # Simuler une erreur
        self.view.on_login_failed(error_message)
        
        # Vérification
        assert self.view.error_label.isVisible()
        assert error_message in self.view.error_label.text()
        assert not self.view.loading_indicator.isVisible()
```

**3. Tests d’intégration de bout en bout :**

```python
# tests/test_e2e/test_user_workflow.py
import pytest
import asyncio
import time
from PySide6.QtWidgets import QApplication
from PySide6.QtTest import QTest
from PySide6.QtCore import Qt
from app.main import create_app
from tests.conftest import start_test_server, stop_test_server

class TestUserWorkflow:
    @pytest.fixture(autouse=True)
    def setup_and_teardown(self):
        """Setup et teardown pour les tests E2E"""
        # Démarrer le serveur de test
        self.server_process = start_test_server()
        time.sleep(2)  # Attendre que le serveur démarre
        
        yield
        
        # Arrêter le serveur
        stop_test_server(self.server_process)
    
    def test_complete_login_workflow(self):
        """Test du parcours complet de connexion"""
        app = QApplication.instance() or QApplication([])
        
        # Créer l’application
        main_app = create_app()
        
        # 1. Affichage de l’écran de connexion
        login_view = main_app.login_view
        assert login_view.isVisible()
        
        # 2. Saisir des identifiants valides
        login_view.email_input.setText("test@example.com")
        login_view.password_input.setText("password123")
        
        # 3. Cliquer sur « Connexion »
        QTest.mouseClick(login_view.login_button, Qt.LeftButton)
        
        # 4. Attendre la réponse (avec timeout)
        timeout = 5000  # 5 secondes
        start_time = time.time()
        
        while login_view.isVisible() and (time.time() - start_time) * 1000 < timeout:
            app.processEvents()
            time.sleep(0.1)
        
        # 5. Vérifier que l’écran principal est affiché
        assert not login_view.isVisible()
        assert main_app.main_view.isVisible()
        
        # 6. Vérifier que les données utilisateur sont chargées
        assert main_app.controller.current_user is not None
        assert main_app.controller.current_user["email"] == "test@example.com"
```

**4. Configuration des tests :**

```python
# tests/conftest.py
import pytest
import asyncio
import subprocess
import sqlite3
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from app.database import Base, get_db
from app.models import User
from app.services.auth_service import AuthService

# Base de données de test
TEST_DATABASE_URL = "sqlite:///./test.db"
test_engine = create_engine(TEST_DATABASE_URL, connect_args={"check_same_thread": False})
TestingSessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=test_engine)

@pytest.fixture
def test_db():
    """Fixture pour la base de données de test"""
    Base.metadata.create_all(bind=test_engine)
    yield
    Base.metadata.drop_all(bind=test_engine)

def override_get_db():
    """Override de la dépendance base de données"""
    try:
        db = TestingSessionLocal()
        yield db
    finally:
        db.close()

@pytest.fixture
def auth_service():
    """Fixture pour le service d’authentification"""
    return AuthService()

@pytest.fixture
def test_user(test_db, auth_service):
    """Fixture pour un utilisateur de test"""
    db = TestingSessionLocal()
    
    user = User(
        email="test@example.com",
        full_name="Test User",
        password_hash=auth_service.get_password_hash("password123")
    )
    
    db.add(user)
    db.commit()
    db.refresh(user)
    db.close()
    
    return user

def start_test_server():
    """Démarre le serveur de test"""
    return subprocess.Popen([
        "python", "-m", "uvicorn", 
        "app.main:app", 
        "--host", "127.0.0.1", 
        "--port", "8001"
    ])

def stop_test_server(process):
    """Arrête le serveur de test"""
    process.terminate()
    process.wait()
```

**5. Commandes de test :**

```bash
# pytest.ini
[tool:pytest]
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*
addopts = 
    -v
    --tb=short
    --strict-markers
    --disable-warnings
    --cov=app
    --cov-report=html
    --cov-report=term-missing

# Commandes pour exécuter les tests
pytest                              # Tous les tests
pytest tests/test_backend/          # Tests backend uniquement
pytest tests/test_frontend/         # Tests frontend uniquement
pytest tests/test_e2e/              # Tests E2E uniquement
pytest -k "test_auth"               # Tests contenant "auth"
pytest --cov=app --cov-report=html  # Avec couverture de code
```

**Avantages de cette stratégie :**

* **Couverture complète** : tests à tous les niveaux
* **Automatisation** : exécution en CI/CD
* **Isolation** : tests indépendants avec fixtures
* **Réalisme** : E2E simulant l’usage réel
* **Maintenance** : structure claire et réutilisable

---

## Barème de correction

|                         Section |     Points     | Détails                |
| ------------------------------: | :------------: | :--------------------- |
|          QCM (questions 1 à 10) |    20 points   | 2 points par question  |
| Questions de synthèse (11 à 15) |    80 points   | 16 points par question |
|                       **Total** | **100 points** |                        |

### Critères d’évaluation pour les questions de synthèse

* **Compréhension globale** (4 pts) : maîtrise des concepts
* **Complétude** (4 pts) : tous les aspects abordés
* **Exemples concrets** (4 pts) : code et implémentations
* **Bonnes pratiques** (2 pts) : respect des standards
* **Clarté et organisation** (2 pts) : présentation structurée

### Notes de passage

* **Excellent** : 90–100 points (90–100 %)
* **Bien** : 80–89 points (80–89 %)
* **Satisfaisant** : 70–79 points (70–79 %)
* **Insuffisant** : < 70 points (< 70 %)

Ce quiz de révision générale évalue la maîtrise complète du cours et la capacité à synthétiser les connaissances acquises. **Bonne chance !**
