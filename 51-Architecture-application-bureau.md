# Exercice 26 - Architecture d'Application Bureau

## Objectifs
- Comprendre les architectures d'applications bureau modernes
- Maitriser les connexions aux bases de donnees
- Comprendre le role des services web (Flask/FastAPI)
- Maitriser SQLAlchemy et son integration
- Concevoir une architecture complete et evolutive

## Partie 1 - Architectures d'Applications Bureau

### Question 1.1 - Patterns Architecturaux
Expliquez les differents patterns architecturaux pour les applications bureau et leurs avantages/inconvenients :

#### A. Architecture Monolithique
**Description :**
```
┌─────────────────────────────────────┐
│        APPLICATION BUREAU           │
├─────────────────────────────────────┤
│  Interface Graphique (Tkinter/Qt)   │
├─────────────────────────────────────┤
│       Logique Metier               │
├─────────────────────────────────────┤
│    Acces aux Donnees (SQL)         │
├─────────────────────────────────────┤
│      Base de Donnees Locale        │
│        (SQLite/Access)             │
└─────────────────────────────────────┘
```

**Avantages :**
- Simplicite de developpement
- Pas de dependance reseau
- Performance locale optimale
- Facilite de distribution

**Inconvenients :**
- Difficulte de partage des donnees
- Maintenance complexe sur plusieurs postes
- Pas de centralisation
- Sauvegardes multiples

#### B. Architecture Client-Serveur Classique
```
┌─────────────────┐    Network    ┌──────────────────┐
│  CLIENT LOURD   │◄──────────────►│   SERVEUR BDD    │
│                 │               │                  │
│ ┌─────────────┐ │               │ ┌──────────────┐ │
│ │Interface GUI│ │               │ │ MySQL/       │ │
│ └─────────────┘ │               │ │ PostgreSQL   │ │
│ ┌─────────────┐ │               │ └──────────────┘ │
│ │Logique      │ │               │                  │
│ │Metier       │ │               │                  │
│ └─────────────┘ │               │                  │
│ ┌─────────────┐ │               │                  │
│ │Connecteur   │ │               │                  │
│ │Base Donnees │ │               │                  │
│ └─────────────┘ │               │                  │
└─────────────────┘               └──────────────────┘
```

**Avantages :**
- Donnees centralisees
- Sauvegardes centralisees
- Controle d'acces unifie
- Coherence des donnees

**Inconvenients :**
- Dependance reseau
- Point de defaillance unique
- Charge reseau importante
- Complexite de gestion des connexions

#### C. Architecture 3-Tiers avec Services Web
```
┌─────────────────┐    HTTP/REST   ┌──────────────────┐    SQL    ┌─────────────┐
│  CLIENT LEGER   │◄──────────────►│  SERVEUR WEB     │◄─────────►│   BASE DE   │
│                 │               │   (Flask/FastAPI) │           │   DONNEES   │
│ ┌─────────────┐ │               │                  │           │             │
│ │Interface GUI│ │               │ ┌──────────────┐ │           │ ┌─────────┐ │
│ │(PySide6)    │ │               │ │ API REST     │ │           │ │MySQL/   │ │
│ └─────────────┘ │               │ │ Endpoints    │ │           │ │Postgres │ │
│ ┌─────────────┐ │               │ └──────────────┘ │           │ └─────────┘ │
│ │Services API │ │               │ ┌──────────────┐ │           │             │
│ │(httpx/      │ │               │ │ Logique      │ │           │             │
│ │requests)    │ │               │ │ Metier       │ │           │             │
│ └─────────────┘ │               │ └──────────────┘ │           │             │
│ ┌─────────────┐ │               │ ┌──────────────┐ │           │             │
│ │Gestion      │ │               │ │ SQLAlchemy   │ │           │             │
│ │Etat Local   │ │               │ │ ORM          │ │           │             │
│ └─────────────┘ │               │ └──────────────┘ │           │             │
└─────────────────┘               └──────────────────┘           └─────────────┘
```

**Avantages :**
- Separation claire des responsabilites
- Scalabilite horizontale
- Maintenance independante des couches
- Reutilisation de l'API
- Support multi-plateforme

**Inconvenients :**
- Complexite accrue
- Latence reseau
- Gestion des erreurs complexe
- Securite multi-niveaux

### Question 1.2 - Architecture Microservices pour Applications Bureau
```
┌─────────────────────────────────────────────────────────────────┐
│                    CLIENT BUREAU (PySide6)                     │
├─────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐ │
│  │Auth Service │ │User Service │ │Order Service│ │...Service │ │
│  │Client       │ │Client       │ │Client       │ │Client     │ │
│  └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │ HTTP/REST
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      API GATEWAY                               │
│                   (Nginx/Kong/Traefik)                        │
└─────────────────────────────────────────────────────────────────┘
                              │
            ┌─────────────────┼─────────────────┐
            │                 │                 │
            ▼                 ▼                 ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│  AUTH SERVICE   │ │  USER SERVICE   │ │ ORDER SERVICE   │
│   (FastAPI)     │ │   (FastAPI)     │ │   (FastAPI)     │
├─────────────────┤ ├─────────────────┤ ├─────────────────┤
│ ┌─────────────┐ │ │ ┌─────────────┐ │ │ ┌─────────────┐ │
│ │SQLAlchemy   │ │ │ │SQLAlchemy   │ │ │ │SQLAlchemy   │ │
│ │Auth Models  │ │ │ │User Models  │ │ │ │Order Models │ │
│ └─────────────┘ │ │ └─────────────┘ │ │ └─────────────┘ │
└─────────────────┘ └─────────────────┘ └─────────────────┘
         │                   │                   │
         ▼                   ▼                   ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│   Auth DB       │ │   Users DB      │ │   Orders DB     │
│  (PostgreSQL)   │ │  (PostgreSQL)   │ │  (PostgreSQL)   │
└─────────────────┘ └─────────────────┘ └─────────────────┘
```

## Partie 2 - Connexions aux Bases de Donnees

### Question 2.1 - Types de Connexions et Leurs Utilisations

#### A. Connexion SQLite (Base Locale)
```python
import sqlite3
from contextlib import contextmanager

class SQLiteManager:
    def __init__(self, db_path="app.db"):
        self.db_path = db_path
        self.init_database()
    
    @contextmanager
    def get_connection(self):
        """Gestionnaire de contexte pour les connexions"""
        conn = sqlite3.connect(self.db_path)
        conn.row_factory = sqlite3.Row  # Acces par nom de colonne
        try:
            yield conn
        except Exception as e:
            conn.rollback()
            raise e
        else:
            conn.commit()
        finally:
            conn.close()
    
    def init_database(self):
        """Initialise la base de donnees"""
        with self.get_connection() as conn:
            conn.execute('''
                CREATE TABLE IF NOT EXISTS users (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    username TEXT UNIQUE NOT NULL,
                    email TEXT UNIQUE NOT NULL,
                    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
                )
            ''')
    
    def create_user(self, username, email):
        """Cree un utilisateur"""
        with self.get_connection() as conn:
            cursor = conn.execute(
                'INSERT INTO users (username, email) VALUES (?, ?)',
                (username, email)
            )
            return cursor.lastrowid
    
    def get_user(self, user_id):
        """Recupere un utilisateur"""
        with self.get_connection() as conn:
            cursor = conn.execute(
                'SELECT * FROM users WHERE id = ?',
                (user_id,)
            )
            return cursor.fetchone()

# Utilisation dans une application Tkinter
class UserApp:
    def __init__(self):
        self.db = SQLiteManager()
        self.setup_ui()
    
    def create_user_action(self):
        username = self.username_entry.get()
        email = self.email_entry.get()
        
        try:
            user_id = self.db.create_user(username, email)
            messagebox.showinfo("Succes", f"Utilisateur cree avec ID: {user_id}")
        except sqlite3.IntegrityError as e:
            messagebox.showerror("Erreur", "Nom d'utilisateur ou email deja utilise")
```

**Avantages SQLite :**
- Pas de serveur requis
- Fichier unique portable
- Parfait pour applications mono-utilisateur
- Transactions ACID completes

**Inconvenients SQLite :**
- Pas de concurrence multi-utilisateur
- Limitations sur les gros volumes
- Pas de replication native

#### B. Connexion MySQL/PostgreSQL (Base Distante)
```python
import mysql.connector
from mysql.connector import pooling
import logging

class MySQLManager:
    def __init__(self, host="localhost", user="root", password="", database="myapp"):
        # Pool de connexions pour optimiser les performances
        self.pool_config = {
            'pool_name': 'myapp_pool',
            'pool_size': 10,
            'pool_reset_session': True,
            'host': host,
            'user': user,
            'password': password,
            'database': database,
            'autocommit': False
        }
        
        try:
            self.pool = mysql.connector.pooling.MySQLConnectionPool(**self.pool_config)
            self.test_connection()
        except mysql.connector.Error as e:
            logging.error(f"Erreur de connexion MySQL: {e}")
            raise
    
    @contextmanager
    def get_connection(self):
        """Gestionnaire de contexte avec pool de connexions"""
        connection = None
        try:
            connection = self.pool.get_connection()
            yield connection
        except mysql.connector.Error as e:
            if connection:
                connection.rollback()
            logging.error(f"Erreur base de donnees: {e}")
            raise
        else:
            if connection:
                connection.commit()
        finally:
            if connection:
                connection.close()
    
    def test_connection(self):
        """Test de la connexion"""
        with self.get_connection() as conn:
            cursor = conn.cursor()
            cursor.execute("SELECT 1")
            result = cursor.fetchone()
            if result[0] != 1:
                raise Exception("Test de connexion echoue")
    
    def execute_query(self, query, params=None):
        """Execute une requete SELECT"""
        with self.get_connection() as conn:
            cursor = conn.cursor(dictionary=True)
            cursor.execute(query, params or ())
            return cursor.fetchall()
    
    def execute_update(self, query, params=None):
        """Execute INSERT/UPDATE/DELETE"""
        with self.get_connection() as conn:
            cursor = conn.cursor()
            cursor.execute(query, params or ())
            return cursor.rowcount

# Gestion des erreurs et reconnexion automatique
class RobustDatabaseManager:
    def __init__(self, config):
        self.config = config
        self.connection = None
        self.reconnect_attempts = 3
        self.reconnect_delay = 5  # secondes
    
    def connect_with_retry(self):
        """Connexion avec retry automatique"""
        for attempt in range(self.reconnect_attempts):
            try:
                self.connection = mysql.connector.connect(**self.config)
                return True
            except mysql.connector.Error as e:
                logging.warning(f"Tentative {attempt + 1} echouee: {e}")
                if attempt < self.reconnect_attempts - 1:
                    time.sleep(self.reconnect_delay)
        return False
    
    def execute_with_retry(self, query, params=None):
        """Execute avec gestion des deconnexions"""
        max_retries = 2
        for retry in range(max_retries):
            try:
                if not self.connection or not self.connection.is_connected():
                    if not self.connect_with_retry():
                        raise Exception("Impossible de se reconnecter")
                
                cursor = self.connection.cursor(dictionary=True)
                cursor.execute(query, params or ())
                
                if query.strip().upper().startswith('SELECT'):
                    return cursor.fetchall()
                else:
                    self.connection.commit()
                    return cursor.rowcount
                    
            except mysql.connector.Error as e:
                if retry < max_retries - 1:
                    logging.warning(f"Erreur, retry: {e}")
                    self.connection = None
                    continue
                else:
                    raise
```

**Avantages MySQL/PostgreSQL :**
- Multi-utilisateur concurrent
- Gros volumes de donnees
- Replication et haute disponibilite
- Fonctionnalites avancees

**Inconvenients :**
- Serveur requis
- Configuration complexe
- Dependance reseau
- Gestion des connexions

### Question 2.2 - Strategies de Connexion Avancees

#### A. Pattern Repository avec Abstraction
```python
from abc import ABC, abstractmethod
from typing import List, Optional, Dict, Any

class UserRepository(ABC):
    """Interface abstraite pour l'acces aux donnees utilisateur"""
    
    @abstractmethod
    def create(self, user_data: Dict[str, Any]) -> int:
        pass
    
    @abstractmethod
    def get_by_id(self, user_id: int) -> Optional[Dict[str, Any]]:
        pass
    
    @abstractmethod
    def get_by_email(self, email: str) -> Optional[Dict[str, Any]]:
        pass
    
    @abstractmethod
    def update(self, user_id: int, user_data: Dict[str, Any]) -> bool:
        pass
    
    @abstractmethod
    def delete(self, user_id: int) -> bool:
        pass
    
    @abstractmethod
    def list_all(self, limit: int = 100, offset: int = 0) -> List[Dict[str, Any]]:
        pass

class SQLiteUserRepository(UserRepository):
    """Implementation SQLite du repository"""
    
    def __init__(self, db_manager: SQLiteManager):
        self.db = db_manager
    
    def create(self, user_data: Dict[str, Any]) -> int:
        with self.db.get_connection() as conn:
            cursor = conn.execute(
                'INSERT INTO users (username, email, full_name) VALUES (?, ?, ?)',
                (user_data['username'], user_data['email'], user_data.get('full_name', ''))
            )
            return cursor.lastrowid
    
    def get_by_id(self, user_id: int) -> Optional[Dict[str, Any]]:
        with self.db.get_connection() as conn:
            cursor = conn.execute('SELECT * FROM users WHERE id = ?', (user_id,))
            row = cursor.fetchone()
            return dict(row) if row else None
    
    # ... autres methodes

class MySQLUserRepository(UserRepository):
    """Implementation MySQL du repository"""
    
    def __init__(self, db_manager: MySQLManager):
        self.db = db_manager
    
    def create(self, user_data: Dict[str, Any]) -> int:
        query = 'INSERT INTO users (username, email, full_name) VALUES (%s, %s, %s)'
        params = (user_data['username'], user_data['email'], user_data.get('full_name', ''))
        
        with self.db.get_connection() as conn:
            cursor = conn.cursor()
            cursor.execute(query, params)
            return cursor.lastrowid
    
    # ... autres methodes

# Factory pour creer le bon repository
class RepositoryFactory:
    @staticmethod
    def create_user_repository(db_type: str, **config) -> UserRepository:
        if db_type == 'sqlite':
            db_manager = SQLiteManager(config.get('db_path', 'app.db'))
            return SQLiteUserRepository(db_manager)
        elif db_type == 'mysql':
            db_manager = MySQLManager(**config)
            return MySQLUserRepository(db_manager)
        else:
            raise ValueError(f"Type de base de donnees non supporte: {db_type}")

# Utilisation dans l'application
class UserService:
    def __init__(self, repository: UserRepository):
        self.repository = repository
    
    def create_user(self, username: str, email: str, full_name: str = "") -> int:
        # Validation business
        if not username or not email:
            raise ValueError("Username et email sont requis")
        
        if '@' not in email:
            raise ValueError("Format email invalide")
        
        # Verification unicite
        existing_user = self.repository.get_by_email(email)
        if existing_user:
            raise ValueError("Email deja utilise")
        
        # Creation
        user_data = {
            'username': username,
            'email': email,
            'full_name': full_name
        }
        
        return self.repository.create(user_data)
    
    def get_user(self, user_id: int) -> Optional[Dict[str, Any]]:
        return self.repository.get_by_id(user_id)
    
    # ... autres methodes business

# Configuration dans l'application principale
class Application:
    def __init__(self, config):
        # Creer le repository selon la configuration
        self.user_repository = RepositoryFactory.create_user_repository(
            db_type=config['database']['type'],
            **config['database']['params']
        )
        
        # Creer les services
        self.user_service = UserService(self.user_repository)
    
    def switch_database(self, new_config):
        """Permet de changer de base de donnees a l'execution"""
        self.user_repository = RepositoryFactory.create_user_repository(
            db_type=new_config['type'],
            **new_config['params']
        )
        self.user_service = UserService(self.user_repository)
```

## Partie 3 - Role des Services Web (Flask/FastAPI)

### Question 3.1 - Flask comme Couche de Service

#### A. Architecture Flask pour Application Bureau
```python
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate
from flask_jwt_extended import JWTManager, jwt_required, create_access_token, get_jwt_identity
from werkzeug.security import generate_password_hash, check_password_hash
import logging

# Configuration Flask
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://user:password@localhost/myapp'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
app.config['JWT_SECRET_KEY'] = 'your-secret-key'

# Extensions
db = SQLAlchemy(app)
migrate = Migrate(app, db)
jwt = JWTManager(app)

# Modeles SQLAlchemy
class User(db.Model):
    __tablename__ = 'users'
    
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    password_hash = db.Column(db.String(255), nullable=False)
    full_name = db.Column(db.String(200))
    is_active = db.Column(db.Boolean, default=True)
    created_at = db.Column(db.DateTime, default=db.func.current_timestamp())
    
    # Relations
    orders = db.relationship('Order', backref='user', lazy='dynamic')
    
    def set_password(self, password):
        self.password_hash = generate_password_hash(password)
    
    def check_password(self, password):
        return check_password_hash(self.password_hash, password)
    
    def to_dict(self):
        return {
            'id': self.id,
            'username': self.username,
            'email': self.email,
            'full_name': self.full_name,
            'is_active': self.is_active,
            'created_at': self.created_at.isoformat() if self.created_at else None
        }

class Order(db.Model):
    __tablename__ = 'orders'
    
    id = db.Column(db.Integer, primary_key=True)
    user_id = db.Column(db.Integer, db.ForeignKey('users.id'), nullable=False)
    total_amount = db.Column(db.Numeric(10, 2), nullable=False)
    status = db.Column(db.String(20), default='pending')
    created_at = db.Column(db.DateTime, default=db.func.current_timestamp())
    
    def to_dict(self):
        return {
            'id': self.id,
            'user_id': self.user_id,
            'total_amount': float(self.total_amount),
            'status': self.status,
            'created_at': self.created_at.isoformat() if self.created_at else None
        }

# Services Business
class UserService:
    @staticmethod
    def create_user(username, email, password, full_name=None):
        """Cree un nouvel utilisateur"""
        # Validation
        if User.query.filter_by(username=username).first():
            raise ValueError("Nom d'utilisateur deja utilise")
        
        if User.query.filter_by(email=email).first():
            raise ValueError("Email deja utilise")
        
        # Creation
        user = User(
            username=username,
            email=email,
            full_name=full_name
        )
        user.set_password(password)
        
        db.session.add(user)
        db.session.commit()
        
        return user
    
    @staticmethod
    def authenticate_user(username, password):
        """Authentifie un utilisateur"""
        user = User.query.filter_by(username=username).first()
        
        if user and user.check_password(password):
            return user
        
        return None
    
    @staticmethod
    def get_user_orders(user_id):
        """Recupere les commandes d'un utilisateur"""
        user = User.query.get_or_404(user_id)
        return user.orders.all()

# API Endpoints
@app.route('/api/auth/login', methods=['POST'])
def login():
    """Endpoint de connexion"""
    data = request.get_json()
    
    if not data or not data.get('username') or not data.get('password'):
        return jsonify({'error': 'Username et password requis'}), 400
    
    user = UserService.authenticate_user(data['username'], data['password'])
    
    if user:
        access_token = create_access_token(identity=user.id)
        return jsonify({
            'access_token': access_token,
            'user': user.to_dict()
        }), 200
    else:
        return jsonify({'error': 'Identifiants invalides'}), 401

@app.route('/api/auth/register', methods=['POST'])
def register():
    """Endpoint d'inscription"""
    data = request.get_json()
    
    required_fields = ['username', 'email', 'password']
    if not all(field in data for field in required_fields):
        return jsonify({'error': 'Champs requis manquants'}), 400
    
    try:
        user = UserService.create_user(
            username=data['username'],
            email=data['email'],
            password=data['password'],
            full_name=data.get('full_name')
        )
        
        return jsonify({
            'message': 'Utilisateur cree avec succes',
            'user': user.to_dict()
        }), 201
        
    except ValueError as e:
        return jsonify({'error': str(e)}), 400
    except Exception as e:
        logging.error(f"Erreur creation utilisateur: {e}")
        return jsonify({'error': 'Erreur interne'}), 500

@app.route('/api/users/me', methods=['GET'])
@jwt_required()
def get_current_user():
    """Recupere les informations de l'utilisateur connecte"""
    user_id = get_jwt_identity()
    user = User.query.get_or_404(user_id)
    return jsonify(user.to_dict())

@app.route('/api/users/<int:user_id>/orders', methods=['GET'])
@jwt_required()
def get_user_orders(user_id):
    """Recupere les commandes d'un utilisateur"""
    current_user_id = get_jwt_identity()
    
    # Verification des droits
    if current_user_id != user_id:
        return jsonify({'error': 'Acces refuse'}), 403
    
    orders = UserService.get_user_orders(user_id)
    return jsonify([order.to_dict() for order in orders])

@app.route('/api/orders', methods=['POST'])
@jwt_required()
def create_order():
    """Cree une nouvelle commande"""
    user_id = get_jwt_identity()
    data = request.get_json()
    
    if not data or 'total_amount' not in data:
        return jsonify({'error': 'Montant total requis'}), 400
    
    try:
        order = Order(
            user_id=user_id,
            total_amount=data['total_amount'],
            status=data.get('status', 'pending')
        )
        
        db.session.add(order)
        db.session.commit()
        
        return jsonify(order.to_dict()), 201
        
    except Exception as e:
        logging.error(f"Erreur creation commande: {e}")
        return jsonify({'error': 'Erreur interne'}), 500

# Gestion des erreurs globales
@app.errorhandler(404)
def not_found(error):
    return jsonify({'error': 'Ressource non trouvee'}), 404

@app.errorhandler(500)
def internal_error(error):
    db.session.rollback()
    return jsonify({'error': 'Erreur interne du serveur'}), 500

# Initialisation
if __name__ == '__main__':
    with app.app_context():
        db.create_all()
    
    app.run(debug=True, host='0.0.0.0', port=5000)
```

#### B. Client Bureau Connecte a Flask
```python
import tkinter as tk
from tkinter import messagebox, ttk
import requests
import json
from typing import Optional, Dict, Any

class FlaskApiClient:
    """Client pour communiquer avec l'API Flask"""
    
    def __init__(self, base_url: str = "http://localhost:5000/api"):
        self.base_url = base_url
        self.access_token: Optional[str] = None
        self.session = requests.Session()
    
    def set_token(self, token: str):
        """Definit le token d'authentification"""
        self.access_token = token
        self.session.headers.update({
            'Authorization': f'Bearer {token}',
            'Content-Type': 'application/json'
        })
    
    def login(self, username: str, password: str) -> Dict[str, Any]:
        """Connexion utilisateur"""
        response = self.session.post(
            f"{self.base_url}/auth/login",
            json={'username': username, 'password': password}
        )
        
        if response.status_code == 200:
            data = response.json()
            self.set_token(data['access_token'])
            return data
        else:
            raise Exception(f"Erreur de connexion: {response.json().get('error', 'Erreur inconnue')}")
    
    def register(self, username: str, email: str, password: str, full_name: str = "") -> Dict[str, Any]:
        """Inscription utilisateur"""
        response = self.session.post(
            f"{self.base_url}/auth/register",
            json={
                'username': username,
                'email': email,
                'password': password,
                'full_name': full_name
            }
        )
        
        if response.status_code == 201:
            return response.json()
        else:
            raise Exception(f"Erreur d'inscription: {response.json().get('error', 'Erreur inconnue')}")
    
    def get_current_user(self) -> Dict[str, Any]:
        """Recupere les informations de l'utilisateur connecte"""
        if not self.access_token:
            raise Exception("Non authentifie")
        
        response = self.session.get(f"{self.base_url}/users/me")
        
        if response.status_code == 200:
            return response.json()
        else:
            raise Exception(f"Erreur: {response.json().get('error', 'Erreur inconnue')}")
    
    def get_user_orders(self, user_id: int) -> list:
        """Recupere les commandes d'un utilisateur"""
        if not self.access_token:
            raise Exception("Non authentifie")
        
        response = self.session.get(f"{self.base_url}/users/{user_id}/orders")
        
        if response.status_code == 200:
            return response.json()
        else:
            raise Exception(f"Erreur: {response.json().get('error', 'Erreur inconnue')}")
    
    def create_order(self, total_amount: float, status: str = "pending") -> Dict[str, Any]:
        """Cree une nouvelle commande"""
        if not self.access_token:
            raise Exception("Non authentifie")
        
        response = self.session.post(
            f"{self.base_url}/orders",
            json={'total_amount': total_amount, 'status': status}
        )
        
        if response.status_code == 201:
            return response.json()
        else:
            raise Exception(f"Erreur: {response.json().get('error', 'Erreur inconnue')}")

class LoginWindow:
    """Fenetre de connexion"""
    
    def __init__(self, api_client: FlaskApiClient, on_success_callback):
        self.api_client = api_client
        self.on_success_callback = on_success_callback
        
        self.window = tk.Toplevel()
        self.window.title("Connexion")
        self.window.geometry("300x200")
        self.window.transient()
        self.window.grab_set()
        
        self.create_widgets()
    
    def create_widgets(self):
        """Cree l'interface de connexion"""
        # Username
        tk.Label(self.window, text="Nom d'utilisateur:").pack(pady=5)
        self.username_entry = tk.Entry(self.window, width=30)
        self.username_entry.pack(pady=5)
        
        # Password
        tk.Label(self.window, text="Mot de passe:").pack(pady=5)
        self.password_entry = tk.Entry(self.window, width=30, show="*")
        self.password_entry.pack(pady=5)
        
        # Boutons
        button_frame = tk.Frame(self.window)
        button_frame.pack(pady=20)
        
        tk.Button(button_frame, text="Se connecter", command=self.login).pack(side=tk.LEFT, padx=5)
        tk.Button(button_frame, text="S'inscrire", command=self.register).pack(side=tk.LEFT, padx=5)
        tk.Button(button_frame, text="Annuler", command=self.window.destroy).pack(side=tk.LEFT, padx=5)
        
        # Focus sur le premier champ
        self.username_entry.focus()
        
        # Bind Enter key
        self.window.bind('<Return>', lambda e: self.login())
    
    def login(self):
        """Tentative de connexion"""
        username = self.username_entry.get().strip()
        password = self.password_entry.get().strip()
        
        if not username or not password:
            messagebox.showerror("Erreur", "Veuillez saisir le nom d'utilisateur et le mot de passe")
            return
        
        try:
            result = self.api_client.login(username, password)
            messagebox.showinfo("Succes", f"Connexion reussie ! Bienvenue {result['user']['username']}")
            self.window.destroy()
            self.on_success_callback(result['user'])
            
        except Exception as e:
            messagebox.showerror("Erreur de connexion", str(e))
    
    def register(self):
        """Ouvre la fenetre d'inscription"""
        RegisterWindow(self.api_client, self.on_register_success)
    
    def on_register_success(self, user_data):
        """Callback apres inscription reussie"""
        messagebox.showinfo("Succes", "Inscription reussie ! Vous pouvez maintenant vous connecter.")

class RegisterWindow:
    """Fenetre d'inscription"""
    
    def __init__(self, api_client: FlaskApiClient, on_success_callback):
        self.api_client = api_client
        self.on_success_callback = on_success_callback
        
        self.window = tk.Toplevel()
        self.window.title("Inscription")
        self.window.geometry("350x300")
        self.window.transient()
        self.window.grab_set()
        
        self.create_widgets()
    
    def create_widgets(self):
        """Cree l'interface d'inscription"""
        # Username
        tk.Label(self.window, text="Nom d'utilisateur:").pack(pady=5)
        self.username_entry = tk.Entry(self.window, width=30)
        self.username_entry.pack(pady=5)
        
        # Email
        tk.Label(self.window, text="Email:").pack(pady=5)
        self.email_entry = tk.Entry(self.window, width=30)
        self.email_entry.pack(pady=5)
        
        # Full name
        tk.Label(self.window, text="Nom complet:").pack(pady=5)
        self.fullname_entry = tk.Entry(self.window, width=30)
        self.fullname_entry.pack(pady=5)
        
        # Password
        tk.Label(self.window, text="Mot de passe:").pack(pady=5)
        self.password_entry = tk.Entry(self.window, width=30, show="*")
        self.password_entry.pack(pady=5)
        
        # Confirm password
        tk.Label(self.window, text="Confirmer mot de passe:").pack(pady=5)
        self.confirm_password_entry = tk.Entry(self.window, width=30, show="*")
        self.confirm_password_entry.pack(pady=5)
        
        # Boutons
        button_frame = tk.Frame(self.window)
        button_frame.pack(pady=20)
        
        tk.Button(button_frame, text="S'inscrire", command=self.register).pack(side=tk.LEFT, padx=5)
        tk.Button(button_frame, text="Annuler", command=self.window.destroy).pack(side=tk.LEFT, padx=5)
    
    def register(self):
        """Tentative d'inscription"""
        username = self.username_entry.get().strip()
        email = self.email_entry.get().strip()
        full_name = self.fullname_entry.get().strip()
        password = self.password_entry.get().strip()
        confirm_password = self.confirm_password_entry.get().strip()
        
        # Validation
        if not all([username, email, password, confirm_password]):
            messagebox.showerror("Erreur", "Veuillez remplir tous les champs obligatoires")
            return
        
        if password != confirm_password:
            messagebox.showerror("Erreur", "Les mots de passe ne correspondent pas")
            return
        
        if '@' not in email:
            messagebox.showerror("Erreur", "Format d'email invalide")
            return
        
        try:
            result = self.api_client.register(username, email, password, full_name)
            self.window.destroy()
            self.on_success_callback(result['user'])
            
        except Exception as e:
            messagebox.showerror("Erreur d'inscription", str(e))

class MainApplication:
    """Application principale"""
    
    def __init__(self):
        self.api_client = FlaskApiClient()
        self.current_user = None
        
        self.root = tk.Tk()
        self.root.title("Application Bureau - Flask API")
        self.root.geometry("800x600")
        
        self.create_widgets()
        self.show_login()
    
    def create_widgets(self):
        """Cree l'interface principale"""
        # Menu
        menubar = tk.Menu(self.root)
        self.root.config(menu=menubar)
        
        file_menu = tk.Menu(menubar, tearoff=0)
        menubar.add_cascade(label="Fichier", menu=file_menu)
        file_menu.add_command(label="Se connecter", command=self.show_login)
        file_menu.add_command(label="Se deconnecter", command=self.logout)
        file_menu.add_separator()
        file_menu.add_command(label="Quitter", command=self.root.quit)
        
        # Frame principal
        self.main_frame = tk.Frame(self.root)
        self.main_frame.pack(fill=tk.BOTH, expand=True, padx=10, pady=10)
        
        # Zone d'informations utilisateur
        self.user_info_frame = tk.LabelFrame(self.main_frame, text="Informations Utilisateur")
        self.user_info_frame.pack(fill=tk.X, pady=5)
        
        self.user_info_label = tk.Label(self.user_info_frame, text="Non connecte", font=("Arial", 12))
        self.user_info_label.pack(pady=10)
        
        # Zone des commandes
        self.orders_frame = tk.LabelFrame(self.main_frame, text="Mes Commandes")
        self.orders_frame.pack(fill=tk.BOTH, expand=True, pady=5)
        
        # Tableau des commandes
        columns = ('ID', 'Montant', 'Statut', 'Date')
        self.orders_tree = ttk.Treeview(self.orders_frame, columns=columns, show='headings')
        
        for col in columns:
            self.orders_tree.heading(col, text=col)
            self.orders_tree.column(col, width=100)
        
        self.orders_tree.pack(fill=tk.BOTH, expand=True, padx=5, pady=5)
        
        # Boutons d'action
        action_frame = tk.Frame(self.orders_frame)
        action_frame.pack(fill=tk.X, padx=5, pady=5)
        
        tk.Button(action_frame, text="Rafraichir", command=self.load_orders).pack(side=tk.LEFT, padx=5)
        tk.Button(action_frame, text="Nouvelle Commande", command=self.create_new_order).pack(side=tk.LEFT, padx=5)
        
        # Desactiver les elements au debut
        self.set_authenticated_state(False)
    
    def show_login(self):
        """Affiche la fenetre de connexion"""
        LoginWindow(self.api_client, self.on_login_success)
    
    def on_login_success(self, user_data):
        """Callback apres connexion reussie"""
        self.current_user = user_data
        self.user_info_label.config(
            text=f"Connecte: {user_data['username']} ({user_data['email']})"
        )
        self.set_authenticated_state(True)
        self.load_orders()
    
    def logout(self):
        """Deconnexion"""
        self.api_client.access_token = None
        self.api_client.session.headers.pop('Authorization', None)
        self.current_user = None
        self.user_info_label.config(text="Non connecte")
        self.set_authenticated_state(False)
        
        # Vider le tableau des commandes
        for item in self.orders_tree.get_children():
            self.orders_tree.delete(item)
    
    def set_authenticated_state(self, authenticated: bool):
        """Active/desactive les elements selon l'etat d'authentification"""
        state = tk.NORMAL if authenticated else tk.DISABLED
        
        for child in self.orders_frame.winfo_children():
            if isinstance(child, tk.Frame):
                for button in child.winfo_children():
                    if isinstance(button, tk.Button):
                        button.config(state=state)
    
    def load_orders(self):
        """Charge les commandes de l'utilisateur"""
        if not self.current_user:
            return
        
        try:
            orders = self.api_client.get_user_orders(self.current_user['id'])
            
            # Vider le tableau
            for item in self.orders_tree.get_children():
                self.orders_tree.delete(item)
            
            # Ajouter les commandes
            for order in orders:
                self.orders_tree.insert('', 'end', values=(
                    order['id'],
                    f"{order['total_amount']:.2f}€",
                    order['status'],
                    order['created_at'][:10] if order['created_at'] else ''
                ))
                
        except Exception as e:
            messagebox.showerror("Erreur", f"Impossible de charger les commandes: {e}")
    
    def create_new_order(self):
        """Cree une nouvelle commande"""
        if not self.current_user:
            return
        
        # Dialogue simple pour saisir le montant
        amount_str = tk.simpledialog.askstring("Nouvelle Commande", "Montant de la commande:")
        
        if amount_str:
            try:
                amount = float(amount_str)
                if amount <= 0:
                    messagebox.showerror("Erreur", "Le montant doit etre positif")
                    return
                
                order = self.api_client.create_order(amount)
                messagebox.showinfo("Succes", f"Commande creee avec l'ID: {order['id']}")
                self.load_orders()
                
            except ValueError:
                messagebox.showerror("Erreur", "Montant invalide")
            except Exception as e:
                messagebox.showerror("Erreur", f"Impossible de creer la commande: {e}")
    
    def run(self):
        """Lance l'application"""
        self.root.mainloop()

if __name__ == "__main__":
    app = MainApplication()
    app.run()
```

### Question 3.2 - FastAPI comme Alternative Moderne

#### A. Avantages de FastAPI vs Flask
```python
from fastapi import FastAPI, HTTPException, Depends, status
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
from pydantic import BaseModel, EmailStr, Field
from sqlalchemy.orm import Session
from typing import Optional, List
import asyncio

# Modeles Pydantic pour validation automatique
class UserCreate(BaseModel):
    username: str = Field(..., min_length=3, max_length=50)
    email: EmailStr
    password: str = Field(..., min_length=8)
    full_name: Optional[str] = Field(None, max_length=200)

class UserResponse(BaseModel):
    id: int
    username: str
    email: str
    full_name: Optional[str]
    is_active: bool
    created_at: Optional[str]
    
    class Config:
        from_attributes = True

class OrderCreate(BaseModel):
    total_amount: float = Field(..., gt=0, le=999999.99)
    status: Optional[str] = Field("pending", regex="^(pending|confirmed|shipped|delivered|cancelled)$")

class OrderResponse(BaseModel):
    id: int
    user_id: int
    total_amount: float
    status: str
    created_at: Optional[str]
    
    class Config:
        from_attributes = True

class LoginRequest(BaseModel):
    username: str
    password: str

class LoginResponse(BaseModel):
    access_token: str
    token_type: str = "bearer"
    user: UserResponse

# FastAPI app avec documentation automatique
app = FastAPI(
    title="Application Bureau API",
    description="API moderne pour applications bureau avec FastAPI",
    version="2.0.0",
    docs_url="/docs",
    redoc_url="/redoc"
)

# Middleware pour CORS (si necessaire)
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # En production: specifier les domaines autorises
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Endpoints avec validation automatique et documentation
@app.post("/api/auth/register", response_model=UserResponse, status_code=status.HTTP_201_CREATED)
async def register_user(user_data: UserCreate, db: Session = Depends(get_db)):
    """
    Inscription d'un nouvel utilisateur
    
    - **username**: Nom d'utilisateur unique (3-50 caracteres)
    - **email**: Adresse email valide
    - **password**: Mot de passe (minimum 8 caracteres)
    - **full_name**: Nom complet optionnel
    """
    # Verification unicite
    existing_user = db.query(User).filter(
        (User.username == user_data.username) | (User.email == user_data.email)
    ).first()
    
    if existing_user:
        if existing_user.username == user_data.username:
            raise HTTPException(
                status_code=status.HTTP_400_BAD_REQUEST,
                detail="Nom d'utilisateur deja utilise"
            )
        else:
            raise HTTPException(
                status_code=status.HTTP_400_BAD_REQUEST,
                detail="Email deja utilise"
            )
    
    # Creation
    user = User(
        username=user_data.username,
        email=user_data.email,
        full_name=user_data.full_name
    )
    user.set_password(user_data.password)
    
    db.add(user)
    db.commit()
    db.refresh(user)
    
    return user

@app.post("/api/auth/login", response_model=LoginResponse)
async def login_user(login_data: LoginRequest, db: Session = Depends(get_db)):
    """
    Connexion utilisateur
    
    Retourne un token JWT pour l'authentification des requetes suivantes
    """
    user = authenticate_user(db, login_data.username, login_data.password)
    
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Identifiants invalides",
            headers={"WWW-Authenticate": "Bearer"},
        )
    
    access_token = create_access_token(data={"sub": str(user.id)})
    
    return LoginResponse(
        access_token=access_token,
        user=UserResponse.from_orm(user)
    )

@app.get("/api/users/me", response_model=UserResponse)
async def get_current_user_info(current_user: User = Depends(get_current_active_user)):
    """Recupere les informations de l'utilisateur connecte"""
    return current_user

@app.get("/api/users/{user_id}/orders", response_model=List[OrderResponse])
async def get_user_orders(
    user_id: int,
    skip: int = 0,
    limit: int = 100,
    current_user: User = Depends(get_current_active_user),
    db: Session = Depends(get_db)
):
    """
    Recupere les commandes d'un utilisateur
    
    - **user_id**: ID de l'utilisateur
    - **skip**: Nombre d'elements a ignorer (pagination)
    - **limit**: Nombre maximum d'elements a retourner
    """
    # Verification des droits
    if current_user.id != user_id:
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="Acces refuse"
        )
    
    orders = db.query(Order).filter(Order.user_id == user_id).offset(skip).limit(limit).all()
    return orders

@app.post("/api/orders", response_model=OrderResponse, status_code=status.HTTP_201_CREATED)
async def create_order(
    order_data: OrderCreate,
    current_user: User = Depends(get_current_active_user),
    db: Session = Depends(get_db)
):
    """
    Cree une nouvelle commande
    
    - **total_amount**: Montant total (doit etre positif)
    - **status**: Statut de la commande (optionnel, defaut: pending)
    """
    order = Order(
        user_id=current_user.id,
        total_amount=order_data.total_amount,
        status=order_data.status
    )
    
    db.add(order)
    db.commit()
    db.refresh(order)
    
    return order

# Endpoints de monitoring et sante
@app.get("/health")
async def health_check():
    """Verification de l'etat de l'API"""
    return {"status": "healthy", "timestamp": datetime.utcnow().isoformat()}

@app.get("/api/stats")
async def get_api_stats(current_user: User = Depends(get_current_active_user), db: Session = Depends(get_db)):
    """Statistiques de l'API (pour les utilisateurs connectes)"""
    total_users = db.query(User).count()
    total_orders = db.query(Order).count()
    user_orders = db.query(Order).filter(Order.user_id == current_user.id).count()
    
    return {
        "total_users": total_users,
        "total_orders": total_orders,
        "your_orders": user_orders
    }
```

**Avantages de FastAPI :**
- **Validation automatique** avec Pydantic
- **Documentation automatique** (Swagger/OpenAPI)
- **Performance superieure** (asynchrone)
- **Type hints natifs** Python
- **Gestion d'erreurs standardisee**
- **Support moderne** (async/await)

## Partie 4 - Role de SQLAlchemy

### Question 4.1 - SQLAlchemy comme Couche d'Abstraction

#### A. Architecture avec SQLAlchemy
```python
from sqlalchemy import create_engine, Column, Integer, String, DateTime, ForeignKey, Table, Text, Boolean, Numeric
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship, Session
from sqlalchemy.pool import QueuePool
from contextlib import contextmanager
import logging

# Configuration de la base
Base = declarative_base()

# Tables d'association pour relations N-N
user_roles = Table('user_roles', Base.metadata,
    Column('user_id', Integer, ForeignKey('users.id'), primary_key=True),
    Column('role_id', Integer, ForeignKey('roles.id'), primary_key=True)
)

order_products = Table('order_products', Base.metadata,
    Column('order_id', Integer, ForeignKey('orders.id'), primary_key=True),
    Column('product_id', Integer, ForeignKey('products.id'), primary_key=True),
    Column('quantity', Integer, default=1),
    Column('unit_price', Numeric(10, 2))
)

# Modeles SQLAlchemy
class User(Base):
    __tablename__ = 'users'
    
    id = Column(Integer, primary_key=True)
    username = Column(String(80), unique=True, nullable=False, index=True)
    email = Column(String(120), unique=True, nullable=False, index=True)
    password_hash = Column(String(255), nullable=False)
    full_name = Column(String(200))
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime, default=func.current_timestamp())
    updated_at = Column(DateTime, default=func.current_timestamp(), onupdate=func.current_timestamp())
    
    # Relations
    orders = relationship("Order", back_populates="user", lazy="dynamic")
    roles = relationship("Role", secondary=user_roles, back_populates="users")
    profile = relationship("UserProfile", back_populates="user", uselist=False)
    
    def __repr__(self):
        return f"<User(username='{self.username}', email='{self.email}')>"
    
    def has_role(self, role_name):
        """Verifie si l'utilisateur a un role specifique"""
        return any(role.name == role_name for role in self.roles)
    
    def get_order_count(self):
        """Compte le nombre de commandes"""
        return self.orders.count()
    
    def get_total_spent(self):
        """Calcule le montant total depense"""
        return sum(order.total_amount for order in self.orders if order.status == 'completed')

class Role(Base):
    __tablename__ = 'roles'
    
    id = Column(Integer, primary_key=True)
    name = Column(String(50), unique=True, nullable=False)
    description = Column(Text)
    
    # Relations
    users = relationship("User", secondary=user_roles, back_populates="roles")

class UserProfile(Base):
    __tablename__ = 'user_profiles'
    
    id = Column(Integer, primary_key=True)
    user_id = Column(Integer, ForeignKey('users.id'), unique=True, nullable=False)
    phone = Column(String(20))
    address = Column(Text)
    birth_date = Column(DateTime)
    preferences = Column(Text)  # JSON string
    
    # Relations
    user = relationship("User", back_populates="profile")

class Product(Base):
    __tablename__ = 'products'
    
    id = Column(Integer, primary_key=True)
    name = Column(String(200), nullable=False, index=True)
    description = Column(Text)
    price = Column(Numeric(10, 2), nullable=False)
    stock_quantity = Column(Integer, default=0)
    category_id = Column(Integer, ForeignKey('categories.id'))
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime, default=func.current_timestamp())
    
    # Relations
    category = relationship("Category", back_populates="products")
    orders = relationship("Order", secondary=order_products, back_populates="products")

class Category(Base):
    __tablename__ = 'categories'
    
    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False, unique=True)
    description = Column(Text)
    parent_id = Column(Integer, ForeignKey('categories.id'))
    
    # Relations
    parent = relationship("Category", remote_side=[id], backref="children")
    products = relationship("Product", back_populates="category")

class Order(Base):
    __tablename__ = 'orders'
    
    id = Column(Integer, primary_key=True)
    user_id = Column(Integer, ForeignKey('users.id'), nullable=False)
    total_amount = Column(Numeric(10, 2), nullable=False)
    status = Column(String(20), default='pending', index=True)
    created_at = Column(DateTime, default=func.current_timestamp())
    updated_at = Column(DateTime, default=func.current_timestamp(), onupdate=func.current_timestamp())
    
    # Relations
    user = relationship("User", back_populates="orders")
    products = relationship("Product", secondary=order_products, back_populates="orders")
    
    @property
    def is_completed(self):
        return self.status == 'completed'
    
    def add_product(self, product, quantity=1):
        """Ajoute un produit a la commande"""
        # Implementation avec la table d'association
        pass

# Gestionnaire de base de donnees avec SQLAlchemy
class DatabaseManager:
    """Gestionnaire centralisé pour SQLAlchemy"""
    
    def __init__(self, database_url: str, echo: bool = False):
        # Configuration du moteur avec pool de connexions
        self.engine = create_engine(
            database_url,
            echo=echo,
            poolclass=QueuePool,
            pool_size=10,
            max_overflow=20,
            pool_pre_ping=True,  # Verification des connexions
            pool_recycle=3600    # Recyclage toutes les heures
        )
        
        # Session factory
        self.SessionLocal = sessionmaker(
            bind=self.engine,
            autocommit=False,
            autoflush=False,
            expire_on_commit=False
        )
        
        # Creation des tables
        Base.metadata.create_all(bind=self.engine)
        
        # Logger
        self.logger = logging.getLogger(__name__)
    
    @contextmanager
    def get_session(self):
        """Gestionnaire de contexte pour les sessions"""
        session = self.SessionLocal()
        try:
            yield session
        except Exception as e:
            session.rollback()
            self.logger.error(f"Erreur base de donnees: {e}")
            raise
        else:
            session.commit()
        finally:
            session.close()
    
    def get_session_dependency(self):
        """Dependance pour FastAPI/Flask"""
        session = self.SessionLocal()
        try:
            yield session
        finally:
            session.close()

# Services business avec SQLAlchemy
class UserService:
    """Service pour la gestion des utilisateurs"""
    
    def __init__(self, db_manager: DatabaseManager):
        self.db = db_manager
    
    def create_user(self, username: str, email: str, password: str, full_name: str = None) -> User:
        """Cree un nouvel utilisateur"""
        with self.db.get_session() as session:
            # Verification unicite
            existing_user = session.query(User).filter(
                (User.username == username) | (User.email == email)
            ).first()
            
            if existing_user:
                if existing_user.username == username:
                    raise ValueError("Nom d'utilisateur deja utilise")
                else:
                    raise ValueError("Email deja utilise")
            
            # Creation
            user = User(
                username=username,
                email=email,
                full_name=full_name
            )
            user.set_password(password)
            
            session.add(user)
            session.flush()  # Pour obtenir l'ID
            
            # Assigner le role par defaut
            default_role = session.query(Role).filter(Role.name == 'user').first()
            if default_role:
                user.roles.append(default_role)
            
            return user
    
    def get_user_by_id(self, user_id: int) -> Optional[User]:
        """Recupere un utilisateur par ID"""
        with self.db.get_session() as session:
            return session.query(User).filter(User.id == user_id).first()
    
    def get_user_by_username(self, username: str) -> Optional[User]:
        """Recupere un utilisateur par nom d'utilisateur"""
        with self.db.get_session() as session:
            return session.query(User).filter(User.username == username).first()
    
    def update_user(self, user_id: int, **kwargs) -> User:
        """Met a jour un utilisateur"""
        with self.db.get_session() as session:
            user = session.query(User).filter(User.id == user_id).first()
            
            if not user:
                raise ValueError("Utilisateur non trouve")
            
            # Mise a jour des attributs
            for key, value in kwargs.items():
                if hasattr(user, key) and key not in ['id', 'created_at']:
                    setattr(user, key, value)
            
            return user
    
    def delete_user(self, user_id: int) -> bool:
        """Supprime un utilisateur"""
        with self.db.get_session() as session:
            user = session.query(User).filter(User.id == user_id).first()
            
            if not user:
                return False
            
            # Soft delete
            user.is_active = False
            return True
    
    def search_users(self, query: str, limit: int = 50) -> List[User]:
        """Recherche d'utilisateurs"""
        with self.db.get_session() as session:
            return session.query(User).filter(
                (User.username.ilike(f"%{query}%")) |
                (User.email.ilike(f"%{query}%")) |
                (User.full_name.ilike(f"%{query}%"))
            ).filter(User.is_active == True).limit(limit).all()
    
    def get_user_statistics(self, user_id: int) -> Dict[str, Any]:
        """Statistiques d'un utilisateur"""
        with self.db.get_session() as session:
            user = session.query(User).filter(User.id == user_id).first()
            
            if not user:
                raise ValueError("Utilisateur non trouve")
            
            # Statistiques avec requetes optimisees
            stats = {
                'total_orders': user.orders.count(),
                'completed_orders': user.orders.filter(Order.status == 'completed').count(),
                'total_spent': user.get_total_spent(),
                'avg_order_value': 0,
                'favorite_category': None
            }
            
            # Calcul de la valeur moyenne
            if stats['completed_orders'] > 0:
                stats['avg_order_value'] = stats['total_spent'] / stats['completed_orders']
            
            # Categorie favorite (requete complexe)
            favorite_category = session.query(Category.name, func.count(Order.id).label('order_count'))\
                .join(Product, Category.id == Product.category_id)\
                .join(order_products, Product.id == order_products.c.product_id)\
                .join(Order, order_products.c.order_id == Order.id)\
                .filter(Order.user_id == user_id)\
                .group_by(Category.name)\
                .order_by(func.count(Order.id).desc())\
                .first()
            
            if favorite_category:
                stats['favorite_category'] = favorite_category.name
            
            return stats

class OrderService:
    """Service pour la gestion des commandes"""
    
    def __init__(self, db_manager: DatabaseManager):
        self.db = db_manager
    
    def create_order(self, user_id: int, products_data: List[Dict]) -> Order:
        """Cree une nouvelle commande"""
        with self.db.get_session() as session:
            # Verification utilisateur
            user = session.query(User).filter(User.id == user_id).first()
            if not user:
                raise ValueError("Utilisateur non trouve")
            
            # Creation de la commande
            order = Order(user_id=user_id, total_amount=0)
            session.add(order)
            session.flush()  # Pour obtenir l'ID
            
            total_amount = 0
            
            # Ajout des produits
            for product_data in products_data:
                product_id = product_data['product_id']
                quantity = product_data['quantity']
                
                product = session.query(Product).filter(Product.id == product_id).first()
                if not product:
                    raise ValueError(f"Produit {product_id} non trouve")
                
                if product.stock_quantity < quantity:
                    raise ValueError(f"Stock insuffisant pour {product.name}")
                
                # Ajout a la table d'association avec prix unitaire
                stmt = order_products.insert().values(
                    order_id=order.id,
                    product_id=product.id,
                    quantity=quantity,
                    unit_price=product.price
                )
                session.execute(stmt)
                
                # Mise a jour du stock
                product.stock_quantity -= quantity
                
                # Calcul du total
                total_amount += float(product.price) * quantity
            
            # Mise a jour du total
            order.total_amount = total_amount
            
            return order
    
    def get_order_details(self, order_id: int) -> Optional[Dict]:
        """Recupere les details d'une commande"""
        with self.db.get_session() as session:
            order = session.query(Order).filter(Order.id == order_id).first()
            
            if not order:
                return None
            
            # Requete pour les produits avec quantites et prix
            products_query = session.query(
                Product.name,
                Product.price,
                order_products.c.quantity,
                order_products.c.unit_price
            ).join(
                order_products, Product.id == order_products.c.product_id
            ).filter(
                order_products.c.order_id == order_id
            ).all()
            
            return {
                'order': order,
                'products': [
                    {
                        'name': p.name,
                        'current_price': float(p.price),
                        'order_price': float(p.unit_price),
                        'quantity': p.quantity,
                        'subtotal': float(p.unit_price) * p.quantity
                    }
                    for p in products_query
                ]
            }
    
    def update_order_status(self, order_id: int, new_status: str) -> Order:
        """Met a jour le statut d'une commande"""
        valid_statuses = ['pending', 'confirmed', 'shipped', 'delivered', 'cancelled']
        
        if new_status not in valid_statuses:
            raise ValueError(f"Statut invalide. Statuts valides: {valid_statuses}")
        
        with self.db.get_session() as session:
            order = session.query(Order).filter(Order.id == order_id).first()
            
            if not order:
                raise ValueError("Commande non trouvee")
            
            old_status = order.status
            order.status = new_status
            
            # Logique business selon le changement de statut
            if old_status != 'cancelled' and new_status == 'cancelled':
                self._restore_stock_for_cancelled_order(session, order)
            
            return order
    
    def _restore_stock_for_cancelled_order(self, session: Session, order: Order):
        """Restaure le stock pour une commande annulee"""
        products_in_order = session.query(
            order_products.c.product_id,
            order_products.c.quantity
        ).filter(order_products.c.order_id == order.id).all()
        
        for product_id, quantity in products_in_order:
            product = session.query(Product).filter(Product.id == product_id).first()
            if product:
                product.stock_quantity += quantity

# Integration avec l'interface graphique
class SQLAlchemyIntegratedApp:
    """Application integrant SQLAlchemy"""
    
    def __init__(self, database_url: str):
        # Initialisation de la base de donnees
        self.db_manager = DatabaseManager(database_url)
        
        # Services
        self.user_service = UserService(self.db_manager)
        self.order_service = OrderService(self.db_manager)
        
        # Interface graphique
        self.root = tk.Tk()
        self.root.title("Application avec SQLAlchemy")
        
        self.create_widgets()
    
    def create_widgets(self):
        """Cree l'interface utilisateur"""
        # Notebook pour les onglets
        notebook = ttk.Notebook(self.root)
        notebook.pack(fill=tk.BOTH, expand=True, padx=10, pady=10)
        
        # Onglet utilisateurs
        user_frame = ttk.Frame(notebook)
        notebook.add(user_frame, text="Utilisateurs")
        self.create_user_tab(user_frame)
        
        # Onglet commandes
        order_frame = ttk.Frame(notebook)
        notebook.add(order_frame, text="Commandes")
        self.create_order_tab(order_frame)
    
    def create_user_tab(self, parent):
        """Cree l'onglet de gestion des utilisateurs"""
        # Zone de recherche
        search_frame = tk.Frame(parent)
        search_frame.pack(fill=tk.X, padx=5, pady=5)
        
        tk.Label(search_frame, text="Recherche:").pack(side=tk.LEFT)
        self.search_entry = tk.Entry(search_frame)
        self.search_entry.pack(side=tk.LEFT, fill=tk.X, expand=True, padx=5)
        tk.Button(search_frame, text="Rechercher", command=self.search_users).pack(side=tk.LEFT)
        
        # Tableau des utilisateurs
        columns = ('ID', 'Username', 'Email', 'Nom Complet', 'Actif', 'Commandes')
        self.user_tree = ttk.Treeview(parent, columns=columns, show='headings')
        
        for col in columns:
            self.user_tree.heading(col, text=col)
            self.user_tree.column(col, width=100)
        
        self.user_tree.pack(fill=tk.BOTH, expand=True, padx=5, pady=5)
        
        # Boutons d'action
        button_frame = tk.Frame(parent)
        button_frame.pack(fill=tk.X, padx=5, pady=5)
        
        tk.Button(button_frame, text="Nouveau", command=self.create_new_user).pack(side=tk.LEFT, padx=5)
        tk.Button(button_frame, text="Modifier", command=self.edit_selected_user).pack(side=tk.LEFT, padx=5)
        tk.Button(button_frame, text="Statistiques", command=self.show_user_stats).pack(side=tk.LEFT, padx=5)
        
        # Charger les utilisateurs au demarrage
        self.load_users()
    
    def create_order_tab(self, parent):
        """Cree l'onglet de gestion des commandes"""
        # Tableau des commandes
        columns = ('ID', 'Utilisateur', 'Montant', 'Statut', 'Date')
        self.order_tree = ttk.Treeview(parent, columns=columns, show='headings')
        
        for col in columns:
            self.order_tree.heading(col, text=col)
            self.order_tree.column(col, width=100)
        
        self.order_tree.pack(fill=tk.BOTH, expand=True, padx=5, pady=5)
        
        # Boutons d'action
        button_frame = tk.Frame(parent)
        button_frame.pack(fill=tk.X, padx=5, pady=5)
        
        tk.Button(button_frame, text="Nouvelle Commande", command=self.create_new_order).pack(side=tk.LEFT, padx=5)
        tk.Button(button_frame, text="Details", command=self.show_order_details).pack(side=tk.LEFT, padx=5)
        tk.Button(button_frame, text="Changer Statut", command=self.change_order_status).pack(side=tk.LEFT, padx=5)
        
        # Charger les commandes
        self.load_orders()
    
    def load_users(self, search_query: str = ""):
        """Charge la liste des utilisateurs"""
        try:
            if search_query:
                users = self.user_service.search_users(search_query)
            else:
                with self.db_manager.get_session() as session:
                    users = session.query(User).filter(User.is_active == True).limit(100).all()
            
            # Vider le tableau
            for item in self.user_tree.get_children():
                self.user_tree.delete(item)
            
            # Ajouter les utilisateurs
            for user in users:
                self.user_tree.insert('', 'end', values=(
                    user.id,
                    user.username,
                    user.email,
                    user.full_name or '',
                    'Oui' if user.is_active else 'Non',
                    user.get_order_count()
                ))
                
        except Exception as e:
            messagebox.showerror("Erreur", f"Impossible de charger les utilisateurs: {e}")
    
    def search_users(self):
        """Recherche d'utilisateurs"""
        query = self.search_entry.get().strip()
        self.load_users(query)
    
    def show_user_stats(self):
        """Affiche les statistiques d'un utilisateur"""
        selected = self.user_tree.selection()
        if not selected:
            messagebox.showwarning("Attention", "Veuillez selectionner un utilisateur")
            return
        
        user_id = self.user_tree.item(selected[0])['values'][0]
        
        try:
            stats = self.user_service.get_user_statistics(user_id)
            
            stats_text = f"""
Statistiques Utilisateur:

Nombre total de commandes: {stats['total_orders']}
Commandes completees: {stats['completed_orders']}
Montant total depense: {stats['total_spent']:.2f}€
Valeur moyenne par commande: {stats['avg_order_value']:.2f}€
Categorie favorite: {stats['favorite_category'] or 'Aucune'}
            """
            
            messagebox.showinfo("Statistiques", stats_text)
            
        except Exception as e:
            messagebox.showerror("Erreur", f"Impossible de recuperer les statistiques: {e}")
    
    def load_orders(self):
        """Charge la liste des commandes"""
        try:
            with self.db_manager.get_session() as session:
                orders = session.query(Order, User.username)\
                    .join(User, Order.user_id == User.id)\
                    .order_by(Order.created_at.desc())\
                    .limit(100).all()
            
            # Vider le tableau
            for item in self.order_tree.get_children():
                self.order_tree.delete(item)
            
            # Ajouter les commandes
            for order, username in orders:
                self.order_tree.insert('', 'end', values=(
                    order.id,
                    username,
                    f"{order.total_amount:.2f}€",
                    order.status,
                    order.created_at.strftime('%Y-%m-%d %H:%M') if order.created_at else ''
                ))
                
        except Exception as e:
            messagebox.showerror("Erreur", f"Impossible de charger les commandes: {e}")
    
    def show_order_details(self):
        """Affiche les details d'une commande"""
        selected = self.order_tree.selection()
        if not selected:
            messagebox.showwarning("Attention", "Veuillez selectionner une commande")
            return
        
        order_id = self.order_tree.item(selected[0])['values'][0]
        
        try:
            details = self.order_service.get_order_details(order_id)
            
            if not details:
                messagebox.showerror("Erreur", "Commande non trouvee")
                return
            
            # Fenetre de details
            detail_window = tk.Toplevel(self.root)
            detail_window.title(f"Details Commande #{order_id}")
            detail_window.geometry("600x400")
            
            # Informations generales
            info_frame = tk.LabelFrame(detail_window, text="Informations Generales")
            info_frame.pack(fill=tk.X, padx=10, pady=5)
            
            order = details['order']
            tk.Label(info_frame, text=f"ID: {order.id}").pack(anchor=tk.W)
            tk.Label(info_frame, text=f"Utilisateur: {order.user.username}").pack(anchor=tk.W)
            tk.Label(info_frame, text=f"Statut: {order.status}").pack(anchor=tk.W)
            tk.Label(info_frame, text=f"Total: {order.total_amount:.2f}€").pack(anchor=tk.W)
            tk.Label(info_frame, text=f"Date: {order.created_at}").pack(anchor=tk.W)
            
            # Produits
            products_frame = tk.LabelFrame(detail_window, text="Produits")
            products_frame.pack(fill=tk.BOTH, expand=True, padx=10, pady=5)
            
            columns = ('Produit', 'Prix Unitaire', 'Quantite', 'Sous-total')
            products_tree = ttk.Treeview(products_frame, columns=columns, show='headings')
            
            for col in columns:
                products_tree.heading(col, text=col)
                products_tree.column(col, width=120)
            
            products_tree.pack(fill=tk.BOTH, expand=True)
            
            for product in details['products']:
                products_tree.insert('', 'end', values=(
                    product['name'],
                    f"{product['order_price']:.2f}€",
                    product['quantity'],
                    f"{product['subtotal']:.2f}€"
                ))
            
        except Exception as e:
            messagebox.showerror("Erreur", f"Impossible d'afficher les details: {e}")
    
    def run(self):
        """Lance l'application"""
        self.root.mainloop()
```

### Question 4.2 - Roles et Responsabilites de SQLAlchemy

#### A. Abstraction de la Base de Donnees
**Role :** SQLAlchemy abstrait les differences entre les systemes de bases de donnees

**Avantages :**
- **Portabilite** : Meme code pour MySQL, PostgreSQL, SQLite
- **Syntaxe unifiee** : Pas besoin d'apprendre le SQL specifique
- **Gestion automatique** : Types, connexions, transactions

**Exemple :**
```python
# Meme code fonctionne avec toutes les bases
user = session.query(User).filter(User.email == 'test@example.com').first()

# SQLAlchemy genere le bon SQL selon la base :
# MySQL: SELECT * FROM users WHERE users.email = %s
# PostgreSQL: SELECT * FROM users WHERE users.email = %(email_1)s  
# SQLite: SELECT * FROM users WHERE users.email = ?
```

#### B. Gestion des Relations
**Role :** Gestion automatique des relations entre entites

**Types de relations :**
- **One-to-One** : User ↔ UserProfile
- **One-to-Many** : User → Orders
- **Many-to-Many** : Users ↔ Roles

**Avantages :**
- **Lazy loading** : Chargement a la demande
- **Eager loading** : Chargement optimise
- **Cascade operations** : Suppression automatique

#### C. Optimisation des Performances
**Role :** Optimisation automatique et manuelle des requetes

**Techniques :**
```python
# Lazy loading (defaut)
user = session.query(User).first()
orders = user.orders  # Requete separee

# Eager loading
user = session.query(User).options(joinedload(User.orders)).first()
orders = user.orders  # Deja charge

# Requetes optimisees
users_with_order_count = session.query(
    User.username,
    func.count(Order.id).label('order_count')
).outerjoin(Order).group_by(User.id).all()
```

#### D. Gestion des Transactions
**Role :** Gestion automatique des transactions ACID

**Avantages :**
- **Atomicite** : Tout ou rien
- **Consistance** : Contraintes respectees  
- **Isolation** : Transactions independantes
- **Durabilite** : Donnees persistantes

## Questions de Synthese

### Question 5.1 - Architecture Complete
Concevez une architecture complete pour une application de gestion de bibliotheque incluant :

**Composants requis :**
- Interface PySide6
- API FastAPI  
- Base de donnees avec SQLAlchemy
- Authentification JWT
- Gestion des roles

**Diagramme d'architecture :**
```
┌─────────────────────────────────────────────────────────────────┐
│                    CLIENT PYSIDE6                               │
├─────────────────────────────────────────────────────────────────┤
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐ │
│ │Login View   │ │Books View   │ │Users View   │ │Stats View │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘ │
├─────────────────────────────────────────────────────────────────┤
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐ │
│ │Auth         │ │Book         │ │User         │ │Stats      │ │
│ │Controller   │ │Controller   │ │Controller   │ │Controller │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘ │
├─────────────────────────────────────────────────────────────────┤
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐ │
│ │Auth API     │ │Book API     │ │User API     │ │Stats API  │ │
│ │Service      │ │Service      │ │Service      │ │Service    │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │ HTTP/REST + JWT
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      FASTAPI SERVER                            │
├─────────────────────────────────────────────────────────────────┤
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐ │
│ │Auth         │ │Books        │ │Users        │ │Reports    │ │
│ │Endpoints    │ │Endpoints    │ │Endpoints    │ │Endpoints  │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘ │
├─────────────────────────────────────────────────────────────────┤
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐ │
│ │Auth         │ │Book         │ │User         │ │Report     │ │
│ │Service      │ │Service      │ │Service      │ │Service    │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘ │
├─────────────────────────────────────────────────────────────────┤
│ ┌─────────────────────────────────────────────────────────────┐ │
│ │                 SQLALCHEMY ORM                             │ │
│ └─────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │ SQL
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    POSTGRESQL DATABASE                         │
├─────────────────────────────────────────────────────────────────┤
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐ │
│ │Users        │ │Books        │ │Loans        │ │Categories │ │
│ │Table        │ │Table        │ │Table        │ │Table      │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘ │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐ │
│ │Roles        │ │User_Roles   │ │Reservations │ │Authors    │ │
│ │Table        │ │Table        │ │Table        │ │Table      │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### Question 5.2 - Comparaison des Approches
Comparez les avantages et inconvenients de chaque approche :

| Aspect | SQLite Direct | MySQL + ORM | API + Client |
|--------|---------------|--------------|--------------|
| **Simplicite** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| **Performance** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Scalabilite** | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Maintenance** | ⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Securite** | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Partage** | ⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Complexite** | ⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

### Question 5.3 - Bonnes Pratiques
Listez les bonnes pratiques pour chaque composant :

**SQLAlchemy :**
- Utiliser des sessions avec gestionnaire de contexte
- Optimiser les requetes avec joinedload/selectinload
- Implementer des validations au niveau modele
- Gerer les migrations avec Alembic

**FastAPI :**
- Valider avec Pydantic
- Implementer l'authentification JWT
- Documenter automatiquement
- Gerer les erreurs globalement

**PySide6 :**
- Separer la logique de l'interface
- Utiliser le pattern MVC/MVP
- Gerer les erreurs utilisateur
- Optimiser les performances UI

Cette architecture complete permet de creer des applications robustes, evolutives et maintenables !
