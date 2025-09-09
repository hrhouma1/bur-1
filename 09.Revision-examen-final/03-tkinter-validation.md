# Exercice 3 - Validation des Entrees et Expressions Regulieres

## Objectifs
- Maitriser la validation des entrees avec Tkinter
- Comprendre et utiliser les expressions regulieres
- Implementer des validations robustes et user-friendly

## Exercice 3.1 - Formulaire de Contact avec Validation Complete

### Enonce
Developpez un formulaire de contact avec validation en temps reel :

**Champs a valider :**
- Nom (2-50 caracteres, lettres uniquement)
- Prenom (2-50 caracteres, lettres uniquement)
- Email (format valide)
- Telephone (format canadien : (XXX) XXX-XXXX)
- Code postal (format canadien : A1A 1A1)
- Age (18-120 ans, chiffres uniquement)

**Types de validation :**
- Validation en temps reel (pendant la saisie)
- Validation lors de la perte de focus
- Validation finale lors de la soumission

### Code de Base
```python
import tkinter as tk
from tkinter import messagebox
import re

class FormulaireContactAvecValidation:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Formulaire avec Validation")
        self.root.geometry("500x700")
        
        # Patterns regex
        self.patterns = {
            'nom': r'^[A-Za-zÀ-ÿ\s]{2,50}$',
            'email': r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$',
            'telephone': r'^\(\d{3}\) \d{3}-\d{4}$',
            'code_postal': r'^[A-Za-z]\d[A-Za-z] \d[A-Za-z]\d$',
            'age': r'^(1[8-9]|[2-9]\d|1[01]\d|120)$'
        }
        
        # Variables pour les champs
        self.nom_var = tk.StringVar()
        self.prenom_var = tk.StringVar()
        self.email_var = tk.StringVar()
        self.telephone_var = tk.StringVar()
        self.code_postal_var = tk.StringVar()
        self.age_var = tk.StringVar()
        
        # Variables pour les messages d'erreur
        self.erreurs = {}
        
        self.create_widgets()
        self.setup_validation()
    
    def create_widgets(self):
        # Interface avec labels d'erreur pour chaque champ
        pass
    
    def setup_validation(self):
        # Configuration des validations
        pass
    
    def valider_champ(self, champ, valeur):
        """Valide un champ specifique"""
        if champ in self.patterns:
            return bool(re.match(self.patterns[champ], valeur))
        return False
    
    def valider_en_temps_reel(self, champ):
        """Validation pendant la saisie"""
        pass
    
    def valider_focus_out(self, champ, event):
        """Validation lors de la perte de focus"""
        pass
    
    def afficher_erreur(self, champ, message):
        """Affiche un message d'erreur pour un champ"""
        pass
    
    def masquer_erreur(self, champ):
        """Masque le message d'erreur pour un champ"""
        pass
    
    def valider_formulaire_complet(self):
        """Validation complete avant soumission"""
        pass
    
    def soumettre(self):
        """Soumission du formulaire"""
        pass
```

### Criteres de Validation
- [ ] Validation en temps reel fonctionnelle
- [ ] Validation sur perte de focus
- [ ] Messages d'erreur clairs et specifiques
- [ ] Expressions regulieres correctes
- [ ] Interface utilisateur intuitive
- [ ] Gestion des cas limites

## Exercice 3.2 - Validateur de Mots de Passe Avance

### Enonce
Creez un systeme de validation de mots de passe avec indicateur de force :

**Criteres de validation :**
- Longueur minimale : 8 caracteres
- Au moins une lettre minuscule
- Au moins une lettre majuscule
- Au moins un chiffre
- Au moins un caractere special (!@#$%^&*)
- Pas de mots courants (dictionnaire simple)

**Indicateur de force :**
- Tres faible (0-1 critere)
- Faible (2 criteres)
- Moyen (3-4 criteres)
- Fort (5-6 criteres)
- Tres fort (tous les criteres + longueur > 12)

### Structure Recommandee
```python
import tkinter as tk
from tkinter import ttk
import re

class ValidateurMotDePasse:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Validateur de Mot de Passe")
        self.root.geometry("500x400")
        
        # Mots courants a eviter
        self.mots_courants = [
            'password', '123456', 'azerty', 'qwerty',
            'admin', 'root', 'user', 'test'
        ]
        
        self.mot_de_passe_var = tk.StringVar()
        self.confirmation_var = tk.StringVar()
        
        self.create_widgets()
        self.setup_validation()
    
    def create_widgets(self):
        # Interface avec barre de progression pour la force
        pass
    
    def analyser_mot_de_passe(self, mot_de_passe):
        """Analyse la force du mot de passe"""
        criteres = {
            'longueur': len(mot_de_passe) >= 8,
            'minuscule': bool(re.search(r'[a-z]', mot_de_passe)),
            'majuscule': bool(re.search(r'[A-Z]', mot_de_passe)),
            'chiffre': bool(re.search(r'\d', mot_de_passe)),
            'special': bool(re.search(r'[!@#$%^&*(),.?\":{}|<>]', mot_de_passe)),
            'pas_courant': mot_de_passe.lower() not in self.mots_courants
        }
        
        score = sum(criteres.values())
        return criteres, score
    
    def calculer_force(self, score, longueur):
        """Calcule le niveau de force"""
        pass
    
    def mettre_a_jour_indicateurs(self):
        """Met a jour l'affichage des indicateurs"""
        pass
    
    def verifier_confirmation(self):
        """Verifie que les mots de passe correspondent"""
        pass
```

### Criteres de Validation
- [ ] Analyse correcte de tous les criteres
- [ ] Indicateur de force visuel
- [ ] Verification de la confirmation
- [ ] Messages explicatifs pour chaque critere
- [ ] Interface claire et informative

## Exercice 3.3 - Formulaire Bancaire Securise

### Enonce
Developpez un formulaire de saisie d'informations bancaires :

**Champs a valider :**
- Numero de carte (format XXXX-XXXX-XXXX-XXXX, validation Luhn)
- Date d'expiration (MM/AA, future)
- Code CVV (3 ou 4 chiffres selon le type de carte)
- Nom sur la carte (lettres et espaces uniquement)
- Montant (decimal positif, max 2 decimales)

**Validations speciales :**
- Algorithme de Luhn pour le numero de carte
- Detection du type de carte (Visa, MasterCard, Amex)
- Validation de date future
- Masquage des donnees sensibles

### Code de Depart
```python
import tkinter as tk
from tkinter import messagebox
import re
from datetime import datetime, date

class FormulaireBancaire:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Formulaire Bancaire Securise")
        self.root.geometry("450x500")
        
        # Patterns pour les types de cartes
        self.types_cartes = {
            'Visa': r'^4[0-9]{12}(?:[0-9]{3})?$',
            'MasterCard': r'^5[1-5][0-9]{14}$',
            'American Express': r'^3[47][0-9]{13}$'
        }
        
        self.create_widgets()
    
    def valider_luhn(self, numero):
        """Validation avec l'algorithme de Luhn"""
        def luhn_checksum(card_num):
            def digits_of(n):
                return [int(d) for d in str(n)]
            digits = digits_of(card_num)
            odd_digits = digits[-1::-2]
            even_digits = digits[-2::-2]
            checksum = sum(odd_digits)
            for d in even_digits:
                checksum += sum(digits_of(d*2))
            return checksum % 10
        return luhn_checksum(numero) == 0
    
    def detecter_type_carte(self, numero):
        """Detecte le type de carte"""
        pass
    
    def valider_date_expiration(self, date_str):
        """Valide que la date est future"""
        pass
    
    def valider_cvv(self, cvv, type_carte):
        """Valide le CVV selon le type de carte"""
        pass
    
    def masquer_numero_carte(self, numero):
        """Masque le numero de carte pour l'affichage"""
        pass
    
    def formater_numero_carte(self, numero):
        """Formate le numero avec des tirets"""
        pass
```

### Criteres de Validation
- [ ] Validation Luhn correcte
- [ ] Detection du type de carte
- [ ] Validation de date d'expiration
- [ ] Validation CVV selon le type
- [ ] Masquage des donnees sensibles
- [ ] Interface securisee

## Exercice 3.4 - Validateur d'Adresses Complexes

### Enonce
Creez un validateur d'adresses avec support international :

**Formats supportes :**
- Canada : 123 Rue Example, Montreal, QC, H1A 1A1
- France : 123 Rue de la Paix, 75001 Paris
- USA : 123 Main St, New York, NY 10001
- Belgique : Rue de la Loi 123, 1000 Bruxelles

**Validations :**
- Format selon le pays selectionne
- Code postal valide pour le pays
- Ville existante (liste predefinie)
- Numero de rue valide

### Structure
```python
class ValidateurAdresse:
    def __init__(self):
        self.patterns_pays = {
            'Canada': {
                'adresse': r'^\d+\s+[A-Za-z\s]+$',
                'code_postal': r'^[A-Za-z]\d[A-Za-z]\s\d[A-Za-z]\d$',
                'province': ['QC', 'ON', 'BC', 'AB', 'MB', 'SK', 'NS', 'NB', 'PE', 'NL', 'YT', 'NT', 'NU']
            },
            'France': {
                'adresse': r'^\d+\s+[A-Za-z\s]+$',
                'code_postal': r'^[0-9]{5}$',
                'regions': ['Ile-de-France', 'Provence-Alpes-Cote d\'Azur', 'Auvergne-Rhone-Alpes']
            }
        }
    
    def valider_adresse_complete(self, pays, adresse, ville, region, code_postal):
        """Validation complete d'une adresse"""
        pass
```

### Criteres de Validation
- [ ] Support multi-pays fonctionnel
- [ ] Validation selon le format du pays
- [ ] Interface adaptive selon le pays
- [ ] Messages d'erreur localises
- [ ] Validation des codes postaux

## Questions de Reflexion

1. **Expressions regulieres** : Comment equilibrer precision et flexibilite dans les regex ?

2. **UX de validation** : Quand faut-il valider : pendant la saisie, sur focus out, ou a la soumission ?

3. **Securite** : Comment proteger les donnees sensibles dans l'interface ?

4. **Internationalisation** : Comment gerer les differences culturelles dans la validation ?

## Ressources Complementaires

- **Cours de reference** : `06. Tkinter - Les expressions regulieres/`
- **Validation** : `07. Tkinter - La validation des entrees/`
- **Documentation** : Python re module, Tkinter validation

## Conseils Avances

1. **Regex lisibles** : Commentez vos expressions regulieres complexes
2. **Feedback utilisateur** : Donnez des messages d'erreur constructifs
3. **Performance** : Optimisez les validations en temps reel
4. **Accessibilite** : Pensez aux utilisateurs avec des handicaps
5. **Tests** : Testez tous les cas limites de validation

Bonne validation !
