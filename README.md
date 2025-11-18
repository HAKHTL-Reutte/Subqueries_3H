# 🎮 Subqueries_3H  
*SQL-Projekt für die 3. Klasse – Einführung in Subqueries*

Dieses Projekt hilft dir dabei, **Subqueries in SQL** zu verstehen und zu üben.  
Dafür verwenden wir eine kleine, realistische **Videospiel-Datenbank**, die aus mehreren Tabellen besteht (Games, Publisher, Plattformen, Genres, Regionen usw.).

Du kannst alle SQL-Dateien selbst importieren und die Datenbank anschließend im Unterricht oder zuhause ausprobieren.

---

## 📦 Projektstruktur

Subqueries_3H/
│
├── README.md → Dieses Dokument
├── er_diagramm.png → ER-Modell der Datenbank
│
├── 01_reference_data.sql → Stammdaten (Genres, Publisher, Plattformen, Regionen)
├── 02_game.sql → Tabelle „game“
├── 03_game_publisher.sql → Tabelle „game_publisher“ (Zuordnung Game ↔ Publisher)
├── 04_game_platform.sql → Tabelle „game_platform“ (Zuordnung Game ↔ Plattform)
└── 05_region_sales.sql → Verkäufe pro Region und Plattform


---

## 🧠 Worum geht’s hier?

In diesem Projekt lernst du:

- was **Subqueries** sind  
- wie man **verschachtelte SQL-Abfragen** schreibt  
- wie man Daten aus mehreren Tabellen kombiniert  
- wie man Probleme logisch löst, auch wenn die Daten verteilt sind  
- wie reale Datenbanken aufgebaut sind  

Die Videospiel-Datenbank hilft dir, SQL besser zu verstehen, weil sie nah an der Lebenswelt ist und viele klare Beziehungen hat.

---

## 🏗️ Datenbankstruktur (ER-Modell)

Das folgende Bild zeigt alle Tabellen und wie sie miteinander verbunden sind:

![ER Diagramm](er_diagramm.png)

---

## 📚 Erklärung der SQL-Dateien

### **01_reference_data.sql**  
Enthält alle Stammdaten, z. B.:

- Genres (Action, Adventure, …)  
- Publisher (Nintendo, EA, Ubisoft, …)  
- Plattformen (PS4, Switch, PC, …)  
- Regionen (Europe, North America, Japan, …)

Diese Daten werden einmalig angelegt.

---

### **02_game.sql**  
Erstellt die Tabelle:

| Spalte      | Bedeutung |
|-------------|-----------|
| id          | Spiel-ID |
| genre_id    | Verknüpfung zu Genre |
| game_name   | Name des Spiels |

Beispiel: „The Legend of Zelda“, Genre „Adventure“.

---

### **03_game_publisher.sql**  
Ein Spiel kann mehrere Publisher haben → **N:M-Beziehung**.

| Spalte        | Bedeutung |
|---------------|-----------|
| game_id       | Verknüpfung zum Spiel |
| publisher_id  | Verknüpfung zum Publisher |

---

### **04_game_platform.sql**  
Ein Spiel kann auf vielen Plattformen erscheinen.

| Spalte             | Bedeutung |
|-------------------|-----------|
| game_publisher_id | Publisher-Version des Spiels |
| platform_id       | Auf welcher Plattform |
| release_year      | Erscheinungsjahr |

Hier kannst du später z. B. Subqueries mit **release_year** üben.

---

### **05_region_sales.sql**  
Zeigt die Verkaufszahlen pro Region.

| Spalte            | Bedeutung |
|------------------|-----------|
| region_id         | Region |
| game_platform_id  | Spiel-Plattform-Kombination |
| num_sales         | Anzahl Verkäufe |

Perfekt für Subqueries wie:  
„Welches Spiel hat sich in Europa am besten verkauft?“

---

## 🛠️ Installation / Import (XAMPP)

### **1. Starte XAMPP**  
MySQL und Apache einschalten.

### **2. Öffne phpMyAdmin**  
http://localhost/phpmyadmin

### **3. Neue Datenbank erstellen**  
Name: **videogames**  
→ „Erstellen“

### **4. SQL-Dateien in der richtigen Reihenfolge importieren**

1. 01_reference_data.sql  
2. 02_game.sql  
3. 03_game_publisher.sql  
4. 04_game_platform.sql  
5. 05_region_sales.sql  

phpMyAdmin → „Importieren“ → Datei auswählen.

---

## 🎯 Beispiel-Queries für den Start

### 🔍 1. Subquery – meistverkauftes Spiel weltweit  
```sql
SELECT game_name
FROM game
WHERE id = (
    SELECT game_id
    FROM game_publisher gp
    JOIN game_platform gpl ON gp.id = gpl.game_publisher_id
    JOIN region_sales rs ON gpl.id = rs.game_platform_id
    GROUP BY game_id
    ORDER BY SUM(num_sales) DESC
    LIMIT 1
);
