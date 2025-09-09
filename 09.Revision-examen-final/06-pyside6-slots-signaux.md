# Exercice 6 - PySide6 Signaux et Slots

## Objectifs
- Maitriser le mecanisme signaux/slots de Qt
- Comprendre la communication entre composants
- Implementer des interactions complexes entre widgets

## Exercice 6.1 - Systeme de Chat en Temps Reel

### Enonce
Developpez un simulateur de chat avec plusieurs utilisateurs :

**Fonctionnalites :**
- Plusieurs fenetres de chat (utilisateurs)
- Messages diffuses en temps reel
- Notifications de connexion/deconnexion
- Historique des messages
- Statuts utilisateurs (en ligne, absent, occupe)

**Signaux personnalises a implementer :**
- `message_envoye(str, str)` : nom utilisateur et message
- `utilisateur_connecte(str)` : nom utilisateur
- `utilisateur_deconnecte(str)` : nom utilisateur
- `statut_change(str, str)` : nom et nouveau statut

### Code de Base
```python
import sys
from datetime import datetime
from PySide6.QtWidgets import (QApplication, QMainWindow, QWidget, QVBoxLayout,
                              QHBoxLayout, QTextEdit, QLineEdit, QPushButton,
                              QComboBox, QLabel, QListWidget, QSplitter)
from PySide6.QtCore import Qt, Signal, QObject, QTimer

class ServeurChat(QObject):
    """Serveur central pour gerer les communications"""
    
    # Signaux du serveur
    message_recu = Signal(str, str, str)  # expediteur, message, timestamp
    utilisateur_connecte = Signal(str)
    utilisateur_deconnecte = Signal(str)
    statut_utilisateur_change = Signal(str, str)
    
    def __init__(self):
        super().__init__()
        self.utilisateurs_connectes = {}
        self.historique_messages = []
    
    def connecter_utilisateur(self, nom_utilisateur, client_chat):
        """Connecte un utilisateur au serveur"""
        if nom_utilisateur not in self.utilisateurs_connectes:
            self.utilisateurs_connectes[nom_utilisateur] = {
                'client': client_chat,
                'statut': 'en ligne'
            }
            
            # Connecter les signaux du client
            client_chat.message_envoye.connect(self.diffuser_message)
            client_chat.statut_change.connect(self.changer_statut_utilisateur)
            
            # Notifier la connexion
            self.utilisateur_connecte.emit(nom_utilisateur)
            
            # Envoyer l'historique au nouveau client
            for msg in self.historique_messages:
                client_chat.recevoir_message(msg['expediteur'], msg['message'], msg['timestamp'])
    
    def deconnecter_utilisateur(self, nom_utilisateur):
        """Deconnecte un utilisateur"""
        pass
    
    def diffuser_message(self, expediteur, message):
        """Diffuse un message a tous les utilisateurs connectes"""
        pass
    
    def changer_statut_utilisateur(self, nom_utilisateur, nouveau_statut):
        """Change le statut d'un utilisateur"""
        pass

class FenetreChat(QMainWindow):
    """Fenetre de chat pour un utilisateur"""
    
    # Signaux personnalises
    message_envoye = Signal(str, str)  # nom_utilisateur, message
    statut_change = Signal(str, str)   # nom_utilisateur, statut
    
    def __init__(self, nom_utilisateur, serveur_chat):
        super().__init__()
        self.nom_utilisateur = nom_utilisateur
        self.serveur_chat = serveur_chat
        
        self.setWindowTitle(f"Chat - {nom_utilisateur}")
        self.setGeometry(100, 100, 600, 500)
        
        self.init_ui()
        self.connecter_signaux()
        
        # Se connecter au serveur
        self.serveur_chat.connecter_utilisateur(nom_utilisateur, self)
    
    def init_ui(self):
        """Interface de chat"""
        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        
        layout = QVBoxLayout(central_widget)
        
        # Zone principale avec splitter
        splitter = QSplitter(Qt.Horizontal)
        
        # Zone de chat
        chat_widget = QWidget()
        chat_layout = QVBoxLayout(chat_widget)
        
        # Affichage des messages
        self.zone_messages = QTextEdit()
        self.zone_messages.setReadOnly(True)
        chat_layout.addWidget(self.zone_messages)
        
        # Zone de saisie
        saisie_layout = QHBoxLayout()
        
        self.ligne_message = QLineEdit()
        self.ligne_message.setPlaceholderText("Tapez votre message...")
        saisie_layout.addWidget(self.ligne_message)
        
        self.bouton_envoyer = QPushButton("Envoyer")
        saisie_layout.addWidget(self.bouton_envoyer)
        
        chat_layout.addLayout(saisie_layout)
        
        # Zone de statut
        statut_layout = QHBoxLayout()
        statut_layout.addWidget(QLabel("Statut:"))
        
        self.combo_statut = QComboBox()
        self.combo_statut.addItems(["en ligne", "absent", "occupe"])
        statut_layout.addWidget(self.combo_statut)
        
        chat_layout.addLayout(statut_layout)
        
        splitter.addWidget(chat_widget)
        
        # Liste des utilisateurs connectes
        self.liste_utilisateurs = QListWidget()
        self.liste_utilisateurs.setMaximumWidth(200)
        splitter.addWidget(self.liste_utilisateurs)
        
        layout.addWidget(splitter)
    
    def connecter_signaux(self):
        """Connecte les signaux et slots"""
        # Signaux internes
        self.bouton_envoyer.clicked.connect(self.envoyer_message)
        self.ligne_message.returnPressed.connect(self.envoyer_message)
        self.combo_statut.currentTextChanged.connect(self.changer_statut)
        
        # Signaux du serveur
        self.serveur_chat.message_recu.connect(self.recevoir_message)
        self.serveur_chat.utilisateur_connecte.connect(self.utilisateur_connecte)
        self.serveur_chat.utilisateur_deconnecte.connect(self.utilisateur_deconnecte)
        self.serveur_chat.statut_utilisateur_change.connect(self.statut_utilisateur_change)
    
    def envoyer_message(self):
        """Envoie un message"""
        message = self.ligne_message.text().strip()
        if message:
            self.message_envoye.emit(self.nom_utilisateur, message)
            self.ligne_message.clear()
    
    def recevoir_message(self, expediteur, message, timestamp):
        """Recoit et affiche un message"""
        pass
    
    def changer_statut(self, nouveau_statut):
        """Change le statut de l'utilisateur"""
        pass
    
    def utilisateur_connecte(self, nom_utilisateur):
        """Gere la connexion d'un utilisateur"""
        pass
    
    def utilisateur_deconnecte(self, nom_utilisateur):
        """Gere la deconnexion d'un utilisateur"""
        pass
    
    def statut_utilisateur_change(self, nom_utilisateur, statut):
        """Met a jour le statut d'un utilisateur"""
        pass
    
    def closeEvent(self, event):
        """Gere la fermeture de la fenetre"""
        self.serveur_chat.deconnecter_utilisateur(self.nom_utilisateur)
        event.accept()

class ApplicationChat(QApplication):
    """Application principale du chat"""
    
    def __init__(self):
        super().__init__(sys.argv)
        
        self.serveur = ServeurChat()
        self.fenetres_chat = []
        
        # Creer plusieurs utilisateurs pour la demonstration
        self.creer_utilisateurs()
    
    def creer_utilisateurs(self):
        """Cree plusieurs fenetres de chat"""
        utilisateurs = ["Alice", "Bob", "Charlie"]
        
        for i, nom in enumerate(utilisateurs):
            fenetre = FenetreChat(nom, self.serveur)
            fenetre.move(100 + i * 50, 100 + i * 50)
            fenetre.show()
            self.fenetres_chat.append(fenetre)

if __name__ == "__main__":
    app = ApplicationChat()
    sys.exit(app.exec())
```

### Criteres de Validation
- [ ] Signaux personnalises fonctionnels
- [ ] Communication entre fenetres
- [ ] Diffusion des messages en temps reel
- [ ] Gestion des statuts utilisateurs
- [ ] Liste des utilisateurs connectes
- [ ] Historique des messages

## Exercice 6.2 - Systeme de Notifications Avance

### Enonce
Creez un systeme de notifications avec differents types et niveaux de priorite :

**Types de notifications :**
- Information (bleue)
- Avertissement (orange)
- Erreur (rouge)
- Succes (verte)

**Fonctionnalites :**
- Notifications temporaires qui disparaissent
- File d'attente des notifications
- Compteur de notifications non lues
- Historique complet
- Filtrage par type et priorite

### Code de Depart
```python
import sys
from datetime import datetime
from enum import Enum
from PySide6.QtWidgets import (QApplication, QMainWindow, QWidget, QVBoxLayout,
                              QHBoxLayout, QPushButton, QLabel, QListWidget,
                              QComboBox, QSpinBox, QTextEdit, QFrame,
                              QScrollArea, QSystemTrayIcon, QMenu)
from PySide6.QtCore import Qt, Signal, QTimer, QPropertyAnimation, QRect
from PySide6.QtGui import QIcon

class TypeNotification(Enum):
    INFO = "info"
    AVERTISSEMENT = "warning"
    ERREUR = "error"
    SUCCES = "success"

class Notification(QFrame):
    """Widget de notification individuelle"""
    
    notification_fermee = Signal(object)  # Signal emis quand la notification est fermee
    
    def __init__(self, type_notif, titre, message, duree=5000):
        super().__init__()
        self.type_notif = type_notif
        self.titre = titre
        self.message = message
        self.duree = duree
        self.timestamp = datetime.now()
        
        self.init_ui()
        self.setup_timer()
        self.setup_animation()
    
    def init_ui(self):
        """Interface de la notification"""
        pass
    
    def setup_timer(self):
        """Configure le timer pour la disparition automatique"""
        pass
    
    def setup_animation(self):
        """Configure l'animation d'apparition/disparition"""
        pass
    
    def fermer_notification(self):
        """Ferme la notification avec animation"""
        pass

class GestionnaireNotifications(QObject):
    """Gestionnaire central des notifications"""
    
    nouvelle_notification = Signal(object)
    notification_lue = Signal(object)
    compteur_change = Signal(int)
    
    def __init__(self):
        super().__init__()
        self.notifications = []
        self.notifications_non_lues = []
        self.file_attente = []
        self.max_notifications_visibles = 5
    
    def ajouter_notification(self, type_notif, titre, message, priorite=1):
        """Ajoute une nouvelle notification"""
        pass
    
    def marquer_comme_lue(self, notification):
        """Marque une notification comme lue"""
        pass
    
    def obtenir_notifications_filtrees(self, type_filtre=None, priorite_min=1):
        """Retourne les notifications filtrees"""
        pass
    
    def vider_historique(self):
        """Vide l'historique des notifications"""
        pass

class ZoneNotifications(QWidget):
    """Zone d'affichage des notifications flottantes"""
    
    def __init__(self, parent=None):
        super().__init__(parent)
        self.setWindowFlags(Qt.FramelessWindowHint | Qt.WindowStaysOnTopHint)
        self.setAttribute(Qt.WA_TranslucentBackground)
        
        self.notifications_actives = []
        self.init_ui()
    
    def init_ui(self):
        """Interface de la zone de notifications"""
        pass
    
    def ajouter_notification(self, notification):
        """Ajoute une notification a la zone"""
        pass
    
    def retirer_notification(self, notification):
        """Retire une notification de la zone"""
        pass
    
    def repositionner_notifications(self):
        """Repositionne les notifications apres suppression"""
        pass

class FenetrePrincipale(QMainWindow):
    """Fenetre principale de test du systeme de notifications"""
    
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Systeme de Notifications")
        self.setGeometry(100, 100, 800, 600)
        
        self.gestionnaire = GestionnaireNotifications()
        self.zone_notifications = ZoneNotifications()
        
        self.init_ui()
        self.connecter_signaux()
        self.setup_system_tray()
    
    def init_ui(self):
        """Interface principale"""
        pass
    
    def connecter_signaux(self):
        """Connecte les signaux du gestionnaire"""
        pass
    
    def setup_system_tray(self):
        """Configure l'icone de la barre systeme"""
        pass
    
    def creer_notification_test(self):
        """Cree une notification de test"""
        pass
    
    def afficher_historique(self):
        """Affiche l'historique des notifications"""
        pass
    
    def mettre_a_jour_compteur(self, nombre):
        """Met a jour le compteur de notifications"""
        pass
```

### Criteres de Validation
- [ ] Systeme de notifications fonctionnel
- [ ] Animations fluides
- [ ] File d'attente operationnelle
- [ ] Filtrage et recherche
- [ ] Integration avec la barre systeme
- [ ] Gestion des priorites

## Exercice 6.3 - Editeur Collaboratif en Temps Reel

### Enonce
Developpez un editeur de texte ou plusieurs utilisateurs peuvent editer simultanement :

**Fonctionnalites :**
- Edition simultanee par plusieurs utilisateurs
- Curseurs multiples visibles
- Synchronisation en temps reel
- Historique des modifications
- Resolution de conflits simple

**Signaux a implementer :**
- `texte_modifie(int, int, str)` : position, longueur, nouveau texte
- `curseur_deplace(str, int)` : utilisateur, position
- `utilisateur_connecte(str)` : nom utilisateur
- `selection_changee(str, int, int)` : utilisateur, debut, fin

### Structure de Base
```python
import sys
from PySide6.QtWidgets import (QApplication, QMainWindow, QTextEdit, 
                              QVBoxLayout, QWidget, QLabel, QHBoxLayout)
from PySide6.QtCore import Qt, Signal, QObject, QTimer
from PySide6.QtGui import QTextCursor, QTextCharFormat, QColor

class ServeurEdition(QObject):
    """Serveur pour synchroniser les modifications"""
    
    texte_synchronise = Signal(str, int, int, str)  # utilisateur, pos, longueur, texte
    curseur_synchronise = Signal(str, int)
    selection_synchronisee = Signal(str, int, int)
    
    def __init__(self):
        super().__init__()
        self.document_partage = ""
        self.editeurs_connectes = {}
        self.historique_modifications = []
    
    def connecter_editeur(self, nom_utilisateur, editeur):
        """Connecte un editeur au serveur"""
        pass
    
    def synchroniser_modification(self, utilisateur, position, longueur, nouveau_texte):
        """Synchronise une modification entre tous les editeurs"""
        pass
    
    def synchroniser_curseur(self, utilisateur, position):
        """Synchronise la position du curseur"""
        pass

class EditeurCollaboratif(QMainWindow):
    """Editeur de texte collaboratif"""
    
    # Signaux pour la synchronisation
    modification_locale = Signal(str, int, int, str)
    curseur_local = Signal(str, int)
    selection_locale = Signal(str, int, int)
    
    def __init__(self, nom_utilisateur, serveur, couleur_curseur):
        super().__init__()
        self.nom_utilisateur = nom_utilisateur
        self.serveur = serveur
        self.couleur_curseur = couleur_curseur
        
        self.setWindowTitle(f"Editeur Collaboratif - {nom_utilisateur}")
        self.setGeometry(100, 100, 600, 500)
        
        self.curseurs_distants = {}
        self.en_synchronisation = False
        
        self.init_ui()
        self.connecter_signaux()
        
        # Se connecter au serveur
        self.serveur.connecter_editeur(nom_utilisateur, self)
    
    def init_ui(self):
        """Interface de l'editeur"""
        pass
    
    def connecter_signaux(self):
        """Connecte les signaux de synchronisation"""
        pass
    
    def on_texte_modifie(self):
        """Gere les modifications locales du texte"""
        pass
    
    def on_curseur_change(self):
        """Gere les changements de position du curseur"""
        pass
    
    def appliquer_modification_distante(self, utilisateur, position, longueur, nouveau_texte):
        """Applique une modification venant d'un autre utilisateur"""
        pass
    
    def mettre_a_jour_curseur_distant(self, utilisateur, position):
        """Met a jour la position d'un curseur distant"""
        pass
    
    def afficher_curseurs_distants(self):
        """Affiche les curseurs des autres utilisateurs"""
        pass

class ApplicationEdition(QApplication):
    """Application de demonstration"""
    
    def __init__(self):
        super().__init__(sys.argv)
        
        self.serveur = ServeurEdition()
        self.editeurs = []
        
        # Creer plusieurs editeurs
        couleurs = [QColor(255, 0, 0), QColor(0, 255, 0), QColor(0, 0, 255)]
        utilisateurs = ["Alice", "Bob", "Charlie"]
        
        for i, (nom, couleur) in enumerate(zip(utilisateurs, couleurs)):
            editeur = EditeurCollaboratif(nom, self.serveur, couleur)
            editeur.move(100 + i * 50, 100 + i * 50)
            editeur.show()
            self.editeurs.append(editeur)
```

### Criteres de Validation
- [ ] Synchronisation en temps reel
- [ ] Curseurs multiples visibles
- [ ] Gestion des conflits
- [ ] Historique des modifications
- [ ] Interface utilisateur claire
- [ ] Performance acceptable

## Questions de Reflexion

1. **Signaux personnalises** : Quand est-il preferable de creer ses propres signaux ?

2. **Performance** : Comment optimiser les performances avec de nombreux signaux ?

3. **Architecture** : Comment organiser le code avec des interactions complexes ?

4. **Debugging** : Comment debugger les problemes de signaux/slots ?

## Ressources Complementaires

- **Cours de reference** : `17. Les slots dans Qt-PySide6`
- **Exemples** : `24-(EXAMEN-1)- Exemple minimal de signal-slot avec PySide6`
- **Documentation Qt** : Signals and Slots in Qt

## Conseils Avances

1. **Nommage** : Utilisez des noms explicites pour vos signaux
2. **Deconnexion** : N'oubliez pas de deconnecter les signaux si necessaire
3. **Thread safety** : Attention aux signaux entre threads
4. **Performance** : Evitez les boucles infinies de signaux
5. **Documentation** : Documentez vos signaux personnalises

Excellente maitrise des signaux et slots !
