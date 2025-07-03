### Arborescence complète du mini-projet **Students\_Info\_System**

```
Students_Info_System/                 # dossier racine
│
├── main.py                           # point d’entrée PySide6
├── main.ui                           # maquetté sous Qt Designer
├── main_ui.py                        # code auto-généré depuis main.ui
├── connect_database.py               # classe utilitaire MySQL
├── requirements.txt                  # dépendances Python
└── icons/                            # ressources SVG
    ├── add.svg
    ├── clear.svg
    ├── delete.svg
    ├── expand_more.svg
    ├── export.svg
    ├── import.svg
    ├── reset.svg
    ├── search.svg
    ├── select.svg
    └── update.svg
```







```text
git clone https://github.com/hrhouma1/Pyside6_Students_Information_Management_System.git

python3.13 --version
python3.13 -m venv env
.\env\Scripts\activate
pip install pyside6
pip install mysql-connector-python
python connect_database.py
python add_mysql_test_data.py
python main.py
```








--------------------------------------------
# ANNEXE 1 - Prérequis 1
--------------------------------------------

- Avoir installé MysqlWorkbench (client + serveur) 
- Référence : Document Word partagé avec vous

--------------------------------------------
# ANNEXE 2 - Prérequis 2
--------------------------------------------

- Création de la base de données + tables et colonnes
- 2 méthodes (méthode 1 manuelle + méthode 2 via les scripts)
mysql


---------------------------------------
# Création de la base de données
---------------------------------------
```text
-- 1. Créer la base
CREATE DATABASE db_students;
GRANT ALL PRIVILEGES ON db_students.* TO 'root'@'localhost';
FLUSH PRIVILEGES;


-- 2. Vérifier qu'elle est bien là
SHOW DATABASES;

-- 3. Se placer dans la base
USE db_students;

-- 4. Créer la table
CREATE TABLE students_info (
  studentId INT NOT NULL,
  firstName VARCHAR(45) NOT NULL,
  lastName VARCHAR(45),
  state VARCHAR(45),
  city VARCHAR(45),
  emailAddress VARCHAR(45),
  PRIMARY KEY (studentId),
  UNIQUE INDEX studentId_UNIQUE (studentId ASC) VISIBLE
);

-- 5. Vérifier les tables
SHOW TABLES;
```


--------------------------------------------
# ANNEXE 3 - Workflow du développement
--------------------------------------------

```text
python3.13 --version
python3.13 -m venv env
.\env\Scripts\activate
pip install pyside6
pyside6-designer
//====> Effectuez votre design - drag & drop (youtube)
//====> main.ui

pyside6-uic.exe  main.ui  -o  main_ui.py

//====>deux fichiers : (main.ui,main_ui.py)

main.py ==> Nous allons charger main.ui grâce à main_ui.py

//====>Introduction de la base de données

Ajout de connect_database.py
Mise à jour de main.py pour ajouter la connexion à la BD
Ajout de 1 script optionnel pour tester la conenxion à la BD 
- add_mysql_test_data.py


pip install mysql-connector-python
python connect_database.py
python add_mysql_test_data.py
python main.py


connect_database.py
main.py
```


# Snippets de code

## 1. `requirements.txt`

```text
PySide6==6.9.1
mysql-connector-python
```



## 2. `connect_database.py`

```python
import mysql.connector
from mysql.connector import Error


class ConnectDatabase:
    """Gestion basique d’une connexion MySQL."""

    def __init__(self,
                 host: str = "127.0.0.1",
                 port: int = 3306,
                 user: str = "testuser",
                 password: str = "codequestions",
                 database: str = "db_students") -> None:
        self._host = host
        self._port = port
        self._user = user
        self._password = password
        self._database = database
        self.con = None
        self.cursor = None

    # ------------------------------------------------------------------ #
    # API publique
    # ------------------------------------------------------------------ #
    def connect_db(self) -> None:
        """Établit la connexion et prépare le curseur."""
        try:
            self.con = mysql.connector.connect(
                host=self._host,
                port=self._port,
                database=self._database,
                user=self._user,
                password=self._password,
            )
            self.cursor = self.con.cursor(buffered=True)
        except Error as exc:
            raise RuntimeError(f"Connexion MySQL impossible : {exc}") from exc

    def execute(self, sql: str, params: tuple | None = None) -> None:
        """Exécute une requête (INSERT/UPDATE/DELETE)."""
        self.cursor.execute(sql, params)
        self.con.commit()

    def fetch(self, sql: str, params: tuple | None = None) -> list[tuple]:
        """Exécute une requête SELECT et retourne les lignes."""
        self.cursor.execute(sql, params)
        return self.cursor.fetchall()

    def close(self) -> None:
        if self.cursor:
            self.cursor.close()
        if self.con:
            self.con.close()
```


## 3. `main.py`

```python
import sys

from PySide6.QtGui      import QIntValidator
from PySide6.QtWidgets  import (
    QApplication, QWidget, QMessageBox,
    QTableWidgetItem
)

from main_ui            import Ui_Form
from connect_database   import ConnectDatabase


class MainWindow(QWidget):
    """Fenêtre principale – gestion étudiants."""

    def __init__(self) -> None:
        super().__init__()

        # --- UI -------------------------------------------------------- #
        self.ui = Ui_Form()
        self.ui.setupUi(self)

        # --- Validators ------------------------------------------------ #
        self.ui.id_lineEdit.setValidator(QIntValidator(0, 99999, self))

        # --- Connexion base ------------------------------------------- #
        self.db = ConnectDatabase()
        try:
            self.db.connect_db()
        except RuntimeError as exc:
            QMessageBox.critical(self, "Erreur DB", str(exc))
            sys.exit(1)

        # --- Connexions signaux/slots --------------------------------- #
        self.ui.add_btn.clicked.connect(self.add_student)
        self.ui.update_btn.clicked.connect(self.update_student)
        self.ui.delete_btn.clicked.connect(self.delete_student)
        self.ui.search_btn.clicked.connect(self.search_students)
        self.ui.reset_btn.clicked.connect(self.fill_table)

        # --- Chargement initial --------------------------------------- #
        self.fill_table()

    # ------------------------------------------------------------------ #
    # Slots
    # ------------------------------------------------------------------ #
    def add_student(self) -> None:
        data = self._collect_inputs(require_id=False)
        sql  = "INSERT INTO students(name, age, email) VALUES (%s, %s, %s)"
        self.db.execute(sql, data[1:])
        self.fill_table()

    def update_student(self) -> None:
        data = self._collect_inputs(require_id=True)
        sql  = ("UPDATE students SET name=%s, age=%s, email=%s "
                "WHERE id=%s")
        self.db.execute(sql, (*data[1:], data[0]))
        self.fill_table()

    def delete_student(self) -> None:
        sid = self.ui.id_lineEdit.text()
        if not sid:
            return
        self.db.execute("DELETE FROM students WHERE id=%s", (sid,))
        self.fill_table()

    def search_students(self) -> None:
        keyword = f"%{self.ui.search_lineEdit.text()}%"
        rows = self.db.fetch(
            "SELECT id, name, age, email FROM students "
            "WHERE name LIKE %s OR email LIKE %s",
            (keyword, keyword)
        )
        self._populate_table(rows)

    def fill_table(self) -> None:
        rows = self.db.fetch(
            "SELECT id, name, age, email FROM students ORDER BY id DESC"
        )
        self._populate_table(rows)

    # ------------------------------------------------------------------ #
    # Helpers
    # ------------------------------------------------------------------ #
    def _collect_inputs(self, require_id: bool) -> tuple[str, str, str, str]:
        sid   = self.ui.id_lineEdit.text()
        name  = self.ui.name_lineEdit.text()
        age   = self.ui.age_lineEdit.text()
        email = self.ui.email_lineEdit.text()

        if require_id and not sid:
            QMessageBox.warning(self, "Validation", "ID requis.")
            raise ValueError

        return sid, name, age, email

    def _populate_table(self, rows: list[tuple]) -> None:
        table = self.ui.students_table
        table.setRowCount(len(rows))
        for r, (sid, name, age, email) in enumerate(rows):
            table.setItem(r, 0, QTableWidgetItem(str(sid)))
            table.setItem(r, 1, QTableWidgetItem(name))
            table.setItem(r, 2, QTableWidgetItem(str(age)))
            table.setItem(r, 3, QTableWidgetItem(email))


# ---------------------------------------------------------------------- #
# Application
# ---------------------------------------------------------------------- #
if __name__ == "__main__":
    app     = QApplication(sys.argv)
    window  = MainWindow()
    window.show()
    sys.exit(app.exec())
```



## 4. `main.ui` (extrait minimal)

> Ce fichier XML est généré par Qt Designer ; voici un **exemple réduit** pour trois champs, la table et quelques boutons. Adapte-le au besoin puis régénère `main_ui.py` avec :
> `pyside6-uic main.ui -o main_ui.py`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ui version="4.0">
 <class>Form</class>
 <widget class="QWidget" name="Form">
  <layout class="QVBoxLayout" name="verticalLayout">
   <item>
    <layout class="QFormLayout" name="formLayout">
     <item row="0" column="0">
      <widget class="QLabel" name="id_label">
       <property name="text"><string>ID :</string></property>
      </widget>
     </item>
     <item row="0" column="1">
      <widget class="QLineEdit" name="id_lineEdit"/>
     </item>
     <!-- 2 autres lignes : Name & Age -->
    </layout>
   </item>

   <item>
    <widget class="QTableWidget" name="students_table">
     <column>
      <property name="text"><string>ID</string></property>
     </column>
     <column>
      <property name="text"><string>Name</string></property>
     </column>
     <column>
      <property name="text"><string>Age</string></property>
     </column>
     <column>
      <property name="text"><string>Email</string></property>
     </column>
    </widget>
   </item>

   <item>
    <layout class="QHBoxLayout">
     <item><widget class="QPushButton" name="add_btn"><property name="text"><string>Add</string></property></widget></item>
     <item><widget class="QPushButton" name="update_btn"><property name="text"><string>Update</string></property></widget></item>
     <item><widget class="QPushButton" name="delete_btn"><property name="text"><string>Delete</string></property></widget></item>
     <item><widget class="QPushButton" name="reset_btn"><property name="text"><string>Reset</string></property></widget></item>
    </layout>
   </item>
  </layout>
 </widget>
 <resources/>
 <connections/>
</ui>
```



## 5. `main_ui.py`

* Ce fichier est **auto-généré**.
* Après chaque modification de `main.ui`, exécute :

  ```bash
  pyside6-uic main.ui -o main_ui.py
  ```



### Comment lancer l’application ?

```bash
python -m venv env
env\Scripts\activate
pip install -r requirements.txt
pyside6-uic main.ui -o main_ui.py   # si pas encore généré
python main.py
```

> Pensez à créer la table `students` dans MySQL :
>
> ```sql
> CREATE DATABASE db_students;
> USE db_students;
> CREATE TABLE students(
>     id INT AUTO_INCREMENT PRIMARY KEY,
>     name VARCHAR(100),
>     age INT,
>     email VARCHAR(100)
> );
> ```

