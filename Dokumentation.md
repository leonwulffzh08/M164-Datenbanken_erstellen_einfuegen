# Tag 1

## Grundlagen Datenmodellierung

### Einführung
Datenmodellierung ist ein essenzieller Bestandteil der Entwicklung von Datenbanken. Sie dient dazu, die Struktur der zu speichernden und zu verarbeitenden Daten festzulegen. Dabei wird sichergestellt, dass die Daten effizient gespeichert und verwaltet werden können. Die Datenmodellierung besteht aus mehreren Phasen, die aufeinander aufbauen und unterschiedliche Abstraktionsebenen abbilden.

### Die drei Ebenen der Datenmodellierung

#### 1. Konzeptionelles Datenmodell (ERM - Entity Relationship Model)
- Rein fachliche Sichtweise auf die Daten
- Nicht an eine konkrete Implementierung gebunden
- Entwickelt anhand der Anforderungen der Fachabteilung
- Stellt Entitäten (Objekte) und deren Beziehungen zueinander dar
- Beispiel: Ein Unternehmen hat Mitarbeiter, und jeder Mitarbeiter gehört zu einer Abteilung

#### 2. Logisches Datenmodell (ERD - Entity Relationship Diagram)
- Leitet sich aus dem konzeptionellen Modell ab
- Wird an das Ziel-Datenbanksystem angepasst (relationales Modell, NoSQL, etc.)
- Enthält bereits Primär- und Fremdschlüssel
- Attribute und Relationen sind detaillierter beschrieben
- Beispiel: Ein relationales Datenmodell mit Tabellen für Mitarbeiter und Abteilungen mit entsprechenden Schlüsseln

#### 3. Physisches Datenmodell
- Konkrete Implementierung des logischen Modells in einem Datenbanksystem
- Enthält DDL-Anweisungen (Data Definition Language) wie `CREATE TABLE`
- Berücksichtigt technische Aspekte wie Indizes, Partitionierung, Speicherstrukturen
- Beispiel: SQL-Skripte zur Erstellung von Tabellen und zur Definition von Constraints

### Arten der Datenmodellierung
- **3NF (Dritte Normalform)**: Häufig in operativen Systemen (z.B. ERP, Core Data Warehouse) zur Sicherstellung der Datenintegrität und Vermeidung von Redundanzen
- **Star Schema**: Wird vorrangig in Data-Marts und Reporting-Systemen eingesetzt, um schnelle Abfragen zu ermöglichen
- **Data Vault Modellierung**: Moderne Modellierungsart für leicht erweiterbare und automatisiert erstellbare Ladeprozesse in Data Warehouses


## Normalisierungsschritte (M162)
Die Normalisierung ist ein Prozess zur Optimierung des Datenmodells, indem Redundanzen und Anomalien vermieden werden. Die Normalisierungsstufen bauen aufeinander auf und stellen sicher, dass die Daten in einer strukturierten und konsistenten Weise gespeichert werden.

### 1. Normalform (1NF)
- Jedes Attribut enthält nur atomare Werte (keine Listen oder mehrfachen Werte in einer Zelle)
- Jede Zeile ist eindeutig identifizierbar (meist durch einen Primärschlüssel)
- Beispiel: Eine Tabelle mit `Mitarbeiter_ID`, `Name`, `Adresse`, aber keine mehrfach gespeicherten Telefonnummern pro Zeile

### 2. Normalform (2NF)
- Erfüllt alle Bedingungen der 1NF
- Alle Nicht-Schlüsselattribute sind vollständig funktional vom gesamten Primärschlüssel abhängig
- Vermeidung partieller Abhängigkeiten
- Beispiel: Falls eine Tabelle einen zusammengesetzten Schlüssel hat, müssen alle Attribute von der gesamten Schlüsselkombination abhängen, nicht nur von einem Teil davon

### 3. Normalform (3NF)
- Erfüllt alle Bedingungen der 2NF
- Keine transitiven Abhängigkeiten zwischen Nicht-Schlüsselattributen
- Jedes Attribut hängt nur vom Primärschlüssel ab und nicht von anderen Nicht-Schlüsselattributen
- Beispiel: Eine Tabelle mit `Mitarbeiter_ID`, `Abteilungs_ID` und `Abteilungsname` sollte aufgeteilt werden, damit `Abteilungsname` nicht direkt von `Mitarbeiter_ID` abhängt, sondern in eine separate Abteilungstabelle verschoben wird

### Boyce-Codd-Normalform (BCNF)
- Stärkere Form der 3NF
- Jedes funktional abhängige Attribut muss eine Determinante sein
- Wird notwendig, wenn in der 3NF noch Abhängigkeiten bestehen, die zu Anomalien führen könnten

### 4. Normalform (4NF)
- Vermeidung mehrwertiger Abhängigkeiten
- Falls eine Tabelle mehrwertige Abhängigkeiten enthält, wird sie in separate Tabellen zerlegt
- Beispiel: Falls ein Student mehrere Telefonnummern und mehrere Kurse hat, sollten Telefonnummern und Kurse in separate Tabellen ausgelagert werden

### 5. Normalform (5NF, PJ/NF)
- Zerlegung der Tabelle in noch kleinere Teile, ohne dass durch Joins Informationen verloren gehen
- Ziel ist es, sicherzustellen, dass alle Datenbeziehungen optimal normalisiert sind

### 6. Normalform (6NF, selten verwendet)
- Wird für zeitabhängige Daten (temporal databases) verwendet
- Behandelt komplexe Zeitstempel- und Verlaufsinformationen

---

# Tag 2

#### **Generalisierung / Spezialisierung**
**Theorie:**
- Redundanzvermeidung durch Zusammenfassung gemeinsamer Attribute in einem generalisierten Entitätstyp.
- Spezialisierte Entitäten enthalten spezifische Attribute und verweisen per Fremdschlüssel auf die generalisierte Entität.
- Konzept ist vergleichbar mit Vererbung in der objektorientierten Programmierung.

**Beispiel:**
```sql
CREATE TABLE Person (
    person_id INT PRIMARY KEY,
    name VARCHAR(50),
    geburtsdatum DATE
);

CREATE TABLE Mitarbeiter (
    mitarbeiter_id INT PRIMARY KEY,
    person_id INT,
    abteilung VARCHAR(50),
    FOREIGN KEY (person_id) REFERENCES Person(person_id)
);

CREATE TABLE Kunde (
    kunde_id INT PRIMARY KEY,
    person_id INT,
    treuepunkte INT,
    FOREIGN KEY (person_id) REFERENCES Person(person_id)
);
```

**Zusätzliche Beispiele aus anderen Quellen:**
- Ein Fahrzeug kann als allgemeine Entität erfasst werden, während Autos und Motorräder spezialisierte Entitäten sind.
- Ein Benutzerkonto kann generalisiert sein, mit Spezialisierungen für Administratoren und Standardnutzer.

#### **Identifying / Non-Identifying Relationship**
**Theorie:**
- Identifying Relationship: Fremdschlüssel ist Teil des Primärschlüssels der Child-Tabelle.
- Non-Identifying Relationship: Fremdschlüssel ist kein Bestandteil des Primärschlüssels.

**Beispiele:**
```sql
-- Identifying Relationship
CREATE TABLE Bestellung (
    bestellung_id INT,
    produkt_id INT,
    PRIMARY KEY (bestellung_id, produkt_id),
    FOREIGN KEY (produkt_id) REFERENCES Produkt(produkt_id)
);

-- Non-Identifying Relationship
CREATE TABLE Mitarbeiter (
    mitarbeiter_id INT PRIMARY KEY,
    name VARCHAR(50),
    abteilung_id INT,
    FOREIGN KEY (abteilung_id) REFERENCES Abteilung(abteilung_id)
);
```

#### **DBMS Analyse**
**Vergleich mit DB-Engine Ranking:**
1. Oracle
2. MySQL
3. Microsoft SQL Server
4. PostgreSQL
5. MongoDB
6. Redis
7. Elasticsearch
8. IBM Db2
9. SQLite
10. Cassandra


#### **SQL-Datenbank-Erstellung**
```sql
CREATE SCHEMA SchulDB;
USE SchulDB;

CREATE TABLE Lehrer (
    lehrer_id INT PRIMARY KEY,
    name VARCHAR(50),
    fach VARCHAR(50)
);

CREATE TABLE Schueler (
    schueler_id INT PRIMARY KEY,
    name VARCHAR(50),
    klasse VARCHAR(10)
);

DROP TABLE Schueler;
ALTER TABLE Lehrer ADD COLUMN telefonnummer VARCHAR(20);
```

#### **Forward Engineering**
- Modell mit verschiedenen Relationen wurde erstellt und analysiert.
- SQL-Skript aus Forward Engineering generiert und mit Datenbank synchronisiert.

#### **Fortgeschrittene Themen**
- **Partitionierung:** Datenaufteilung zur Performance-Optimierung.
- **Storage Engine:** Steuert, wie Daten gespeichert und abgerufen werden (InnoDB, MyISAM).
- **Tablespace in InnoDB:** Speichert Tabellen und Indizes, ermöglicht flexible Speicherverwaltung.

#### **Zusätzliche Aufgaben (Lösung)**
1. Warum Generalisierung/Spezialisierung?
   - Vermeidung von Redundanz, bessere Datenstruktur.
2. Warum Identifying Beziehung?
   - Stärkere Integritätskontrolle, strukturierte Hierarchie.
3. Wie wird eine Identifying Beziehung implementiert?
   ```sql
   CREATE TABLE Projekt (
       projekt_id INT PRIMARY KEY,
       name VARCHAR(50)
   );
   
   CREATE TABLE Aufgabe (
       aufgabe_id INT,
       projekt_id INT,
       PRIMARY KEY (aufgabe_id, projekt_id),
       FOREIGN KEY (projekt_id) REFERENCES Projekt(projekt_id)
   );
   ```
4. DDL-Befehle:
   - `CREATE`, `ALTER`, `DROP` für Schema- und Tabellenverwaltung.

---
# Tag 3

## Datentypen in MySQL / MariaDB

| Datentyp                    | Beispiel                    | Beschreibung / Hinweis                                          |
|----------------------------|-----------------------------|------------------------------------------------------------------|
| `INT`                      | `42`                        | Ganze Zahlen                                                    |
| `TINYINT`, `SMALLINT`, ... | `255`                       | Natürliche Zahlen (positiv), abhängig vom Bereich               |
| `DECIMAL(M,D)`             | `DECIMAL(6,2)` → `1234.56`  | Feste Nachkommastellen (z. B. Geldbeträge)                      |
| `ENUM('A', 'B')`           | `'A'`                        | Aufzählung von Werten                                           |
| `BOOLEAN` (`TINYINT(1)`)   | `1` oder `0`                | Logische Werte (`TRUE`, `FALSE`)                                |
| `CHAR(1)`                  | `'A'`                        | Einzelnes Zeichen                                               |
| `FLOAT`, `DOUBLE`          | `1.23`, `3.1415`            | Gleitkommazahlen                                                |
| `CHAR(n)`                  | `'Hallo '`                  | Feste Zeichenkette (z. B. `CHAR(10)`)                            |
| `VARCHAR(n)`               | `'Hallo Welt'`              | Zeichenkette variabler Länge                                    |
| `DATE`                     | `'2024-03-25'`              | Datum                                                            |
| `DATETIME`, `TIMESTAMP`   | `'2024-03-25 12:34:56'`     | Datum und Zeit                                                  |
| `BLOB`, `LONGBLOB`         | Binärdaten                  | z. B. Bilder, Dateien                                            |
| `JSON`                     | `{"key": "value"}`          | JSON-Dokumente                                                  |



## Mehrfachbeziehungen

- Zwei Tabellen können **mehrere unabhängige Beziehungen** zueinander haben.
- Jede Beziehung repräsentiert einen anderen Sachverhalt.
- Beispiel: `tbl_Fahrten` → `tbl_Orte` (Startort, Zielort, Via)
- Falls `mc:mc`, dann ist eine **Transformationstabelle** erforderlich.


## Rekursion (strenge Hierarchie)

- Tabelle hat eine Beziehung **zu sich selbst**.
- Beispiel: Mitarbeiter haben einen Vorgesetzten.
- Fremdschlüssel zeigt auf Primärschlüssel **der gleichen Tabelle**.
- Beziehungstyp: `c:mc`
- Die „oberste Person“ hat `NULL` als Vorgesetzten.


## Einfache Hierarchie mit Zwischentabelle

- Wenn mehrere Vorgesetzte möglich sind (Netzwerk), braucht es `mc:mc`.
- Lösung: **Zwischentabelle** mit 2 FK:
  - `FK_ist_Vorgesetzter_von`
  - `FK_ist_Mitarbeiter_von`
- Dient zur Darstellung komplexer Organisationsstrukturen.



## Stücklistenproblem (rekursive Struktur in Produktion)

- Beispiel: Möbelteile → Tisch = Beine + Platte + Schrauben
- **Tabelle Produkte** + **Tabelle Zusammensetzungen**
- Jede Komponente verweist auf eine andere (rekursive Beziehung über Zwischentabelle)
- ➕ [SQL-Beispiel von Sybase](https://infocenter.sybase.com/help/index.jsp?topic=/com.sybase.help.sqlanywhere.12.0.1/dbusage/parts-explosion-cte-sqlug.html)



## Auftrag – Tourenplaner erweitern

1. **Generalisierung**: `tbl_Mitarbeiter` mit Vererbung umsetzen
2. **Mehrfachbeziehungen**: zwischen `tbl_Fahrten` und `tbl_Orte` einbauen
3. **Rekursion + Hierarchie**: beide Varianten abbilden
4. Struktur in DB per **Forward Engineering** umsetzen
5. **Stücklistenproblem** reflektieren



## Datenbearbeitung mit SQL (Repetition DML)

- `INSERT INTO`, `UPDATE`, `DELETE`, `ALTER`, `DROP`




## Daten auslesen (Repetition SELECT / DQL)

- `SELECT *` → Alle Spalten
- `SELECT spalten` → Spaltenauswahl
- `WHERE` → Bedingungen
- `IF()`, `ROUND()`, `ABS()`, `PI()` → Funktionen
- `JOIN`, `GROUP BY`, `ORDER BY` → für spätere Aufgaben




## 👤 Hierarchie & Orgagramm einpflegen

### Tabelle `tbl_Mitarbeiter`

| ID | Vorname | Name       | Telefon             |
|----|---------|------------|---------------------|
| 1  | Hans    | Muster     | +41 76 764 23 23    |
| 2  | Theo    | Dohr       | +41 79 324 55 78    |
| 3  | Justin  | Biber      | +41 79 872 12 32    |
| 4  | Johann  | Fluss      | +41 79 298 98 76    |
| 5  | Diana   | Knecht     | +41 78 323 77 00    |
| 6  | Anna    | Schöni     | +41 76 569 67 80    |
| 8  | Lucy    | Schmidt    | +49 420 232 2232    |
| 9  | Ardit   | Azubi      |                     |

### Organigramm-Beziehungen

- Chef: Justin Biber
- Unterchefs: Schöni (Dispo), Dohr (Fahrer)
- Schöni führt: Schmidt, Muster
- Dohr führt: Fluss, Knecht


```sql
-- Beispiel UPDATE
UPDATE tbl_Mitarbeiter
SET FS_Vorgesetzter = 3
WHERE ID_Mitarbeiter IN (2, 6); -- Dohr, Schöni unter Biber
```
---
# Tag 4

## Beziehungen mit Einschränkungen (Constraints)

### Beziehungstypen im physischen Modell

| Beziehungstyp       | Umsetzung im DBMS        | Notwendige Constraints       |
|---------------------|--------------------------|------------------------------|
| 1:1                 | 1:c oder c:c             | `NN`, `UQ`                   |
| 1:n                 | 1:mc oder c:mc           | `NN`                         |
| n:m (viele: viele)  | über Zwischentabelle     | `NN` auf beiden Seiten       |

> **Hinweis:** Einige gewünschte Beziehungstypen lassen sich im RDBMS nicht exakt umsetzen. Einschränkungen müssen ggf. durch Anwendungslogik abgesichert werden.

### Forward Engineering in Workbench

- Beziehungen werden im ER-Diagramm definiert.
- Constraints wie `NOT NULL`, `UNIQUE`, `FOREIGN KEY` werden automatisch erstellt.
- Jeder korrekt gesetzte FK erzeugt einen eigenen `CONSTRAINT`, der die referentielle Integrität sicherstellt.
- Beziehungen können beschriftet werden (`Caption` in der Beziehungseigenschaft).

### Nachträgliche Erstellung von Constraints

#### Fremdschlüssel hinzufügen

```sql
ALTER TABLE Detailtabelle
  ADD CONSTRAINT FK_Detail_Master FOREIGN KEY (Fremdschlüssel)
  REFERENCES Mastertabelle (Primärschlüssel);
```

---
# Tag 5

## Löschen in professionellen Datenbanken

### Warum `DELETE`-Befehle problematisch sind

- **Informationsverlust**: Gelöschte Datensätze lassen sich nicht mehr nachvollziehen (z. B. Aktionen eines ausgeschiedenen Mitarbeiters).
- **Rechtliche Probleme**: In sensiblen Bereichen (z. B. Bankwesen, Protokollierung) müssen Daten historisiert werden.
- **Manipulationsgefahr**: Bei Kassensystemen könnten Löschungen zu Missbrauch führen.
- **Lösung**: Keine Löschung → stattdessen:
  - **Austrittsdatum setzen**
  - **Status ändern (z. B. aktiv/inaktiv)**
  - **Stornierungen statt Löschung**
  - **Historisierung von Änderungen**

> Ziel: Vergangenheitsauswertungen und Revisionssicherheit erhalten!



## Datenintegrität

### Was bedeutet Datenintegrität?

Datenintegrität sichert:
- **Richtigkeit**
- **Konsistenz**
- **Vollständigkeit**
der gespeicherten Daten.

### Aspekte der Datenintegrität

1. **Eindeutigkeit & Konsistenz**
   - Jeder Datensatz eindeutig identifizierbar.
   - Keine Redundanzen.

2. **Referenzielle Integrität**
   - Beziehungen zwischen Tabellen müssen konsistent bleiben.
   - FK darf nur auf existierende PKs verweisen.

3. **Datentypen**
   - Richtig definierte Typen (z. B. Telefonnummer = `VARCHAR`, nicht `INT`).

4. **Datenbeschränkungen (Constraints)**
   - Nur gültige Werte erlaubt (z. B. positive Zahlen, gültige Formate).

5. **Validierung**
   - Daten werden vor dem Einfügen geprüft.



## Fremdschlüssel-Regeln beim Löschen (`ON DELETE`)

| Regel        | Bedeutung |
|--------------|-----------|
| **NO ACTION** / **RESTRICT** | Verhindert Löschung, wenn abhängige Datensätze existieren. (Standardverhalten) |
| **CASCADE** | Löscht automatisch alle verknüpften Datensätze in der Detailtabelle. **Gefährlich!** |
| **SET NULL** / **DEFAULT** | Setzt FK auf `NULL` oder den definierten Standardwert. Nur möglich, wenn `NULL` erlaubt ist. |

> `ON UPDATE`-Regeln sind meist irrelevant, da PKs in der Regel nicht geändert werden (Auto-Increment).



## SELECT ALIAS

- **Aliase für Spaltennamen**
  - Werden mit `AS` vergeben.
  - Nur temporär für die Abfrage gültig.
  
  SELECT vorname AS "Vorname", nachname AS "Nachname"
  FROM person;

---

# Tag 6

## Subqueries (Unterabfragen)

### Was ist eine Subquery?

Eine **Subquery** (auch Subselect oder Unterabfrage) ist eine SQL-Abfrage **innerhalb einer anderen**. Sie wird verwendet, um dynamisch Bedingungen zu ermitteln oder Daten zu filtern, basierend auf anderen Tabellen oder Werten.

### Einsatzorte

- In `WHERE`, `FROM`, `HAVING`, `SELECT`
- In `UPDATE`, `DELETE`, `INSERT`

### **Wichtige Regeln**
- Immer in **Klammern** setzen.
- Der **Operator** muss zur Rückgabeart passen:
  - *Skalar*: `=`, `<`, `>`, ...
  - *Nicht-skalar*: `IN`, `NOT IN`, `EXISTS`, `ANY`, `ALL`, ...



## 🔹 Skalare Subquery

- Gibt **eine Zeile, eine Spalte** zurück.
- Beispiel:
  ```sql
  SELECT city_destination, ticket_price
  FROM one_way_ticket
  WHERE ticket_price < (
    SELECT ticket_price
    FROM one_way_ticket
    WHERE city_destination = 'Bariloche' AND city_origin = 'Paris'
  )
  AND city_origin = 'Paris';
---
# Tag 7

## DB Backup & Datensicherung

### Warum sind Backups wichtig?

- Datenverlust führt zu:
  - Funktionsstörungen von Webseiten & Anwendungen
  - Verlust sensibler Daten
  - Vertrauensverlust bei Kunden
- Ursachen:
  - Technische Defekte
  - Benutzerfehler
  - Selten externe Angriffe



## Backup-Arten

| Typ              | Beschreibung                                                                 |
|------------------|-------------------------------------------------------------------------------|
| **Voll-Backup**     | Sichert gesamte Datenbank (alle Tabellen & Inhalte)                         |
| **Differentiell**   | Sichert nur Änderungen **seit dem letzten Voll-Backup**                     |
| **Inkrementell**    | Sichert nur Änderungen **seit dem letzten Backup** (Voll oder Inkrementell) |

> Für Restore bei differenziell: Voll + letztes Differenziell  
> Für Restore bei inkrementell: Voll + **alle** Inkrementellen bis zum gewünschten Stand



## Backup-Verfahren

| Verfahren     | Beschreibung |
|---------------|--------------|
| **Online-Backup** | Sicherung ohne Stillstand der DB – Änderungen werden nachgetragen |
| **Offline-Backup** | DB wird gestoppt – einfacher, aber Ausfallzeit!               |



## Tools zur Datensicherung

| Tool         | Merkmale                                                                 |
|--------------|--------------------------------------------------------------------------|
| `mysqldump`  | Standard-Tool für logische Backups (SQL-Script). Schnell & einfach.     |
| phpMyAdmin   | GUI-Export. Einfach, aber begrenzte Größe (~2MB).                        |
| BigDump      | Ergänzung zu phpMyAdmin für grosse Backups. Kein eigener Export.         |
| HeidiSQL     | Windows-Tool. Kann große Backups, aber keine Automatisierung.            |
| mariabackup  | Physisches Online-Backup-Tool für MariaDB. Unterstützt differenziell.    |

> **Backup-Benutzer anlegen:**
```sql
GRANT RELOAD, PROCESS, LOCK TABLES, REPLICATION CLIENT ON *.* TO 'backupuser'@'localhost' IDENTIFIED BY 'backup123';
```
---
# Tag 8

## Weiterarbeit: DB „Freifächer“ (von Tag 7)

> Vorbereitung auf **LB2** – vollständige Bearbeitung empfohlen!

### Aufgabenüberblick

1. **1. Normalform** aus Excel erstellen, Daten als CSV exportieren.
2. **Logisches ERD** mit mind. 5 Tabellen (mind. 2. NF), Kardinalitäten festlegen.
3. **Physisches ERD** mit NN-/UQ-Constraints erstellen. Forward Engineering → DDL-Script.
4. **CSV-Import** mittels `LOAD DATA LOCAL INFILE`.
5. **Daten bereinigen** (Dubletten, NULL-Werte, Inkonsistenzen).
6. **Vergleich**: CSV-Daten vs. SELECT-Ergebnisse.
7. **Testdatengenerator**: 290 zusätzliche Schüler erzeugen und importieren.
8. **Analyse-Aufgaben**:
   - Anzahl Teilnehmer bei *Inge Sommer* im Freifach.
   - Klassenliste mit Schüleranzahl je Klasse in Freifächern.
   - Alle Schüler, die „Chor“ oder „Elektronik“ besuchen.
9. Ergebnisse mit `SELECT INTO OUTFILE` exportieren.
10. **Backup** der Datenbank *Freifaecher* erstellen.

### Tipps & Ressourcen

- [1. NF Beispiel](../7.Tag/media/1NF.png)
- [Logisches ERD](../7.Tag/media/logERD_2NF.png)
- [Physisches ERD](../7.Tag/media/physERD_2NF.png)
- [290 CSV-Datensätze](../7.Tag/media/290_SchuelerInnen.csv)



## Opendata-Projekt (Einzelarbeit)

> Angewandte Datenmodellierung & Datenanalyse mit öffentlichen Daten

Wählen Sie **eine** der drei folgenden Quellen und bearbeiten Sie die Aufgaben.



### Option 1: Steuerdaten Stadt Zürich

**Datenquelle**:  
https://data.stadt-zuerich.ch/dataset/fd_median_einkommen_quartier_od1003

#### Aufgaben:

1. CSV-Datei analysieren und **normalisieren**.
2. **Attribute & Datentypen** bestimmen, Datei bereinigen.
3. **Physisches ERD + DDL-Script**, Bulkimport vorbereiten.
4. **Felder analysieren**: Bedeutung von `_p25`, `_p50`, `_p75` (→ Perzentile).
5. **SQL-Fragen beantworten**:
   - Quartier mit **max. `_p75`**
   - Quartier mit **min. `_p50`**
   - Quartier mit **max. `_p50`**
6. **Backup** der Datenbank erstellen.



### Option 2: Bildungsdaten vom Bundesamt für Statistik (BFS)

**Datenquelle**:  
https://www.bfs.admin.ch/bfs/de/home/statistiken/bildung-wissenschaft.assetdetail.14879800.html

#### Aufgaben:

1. Excel analysieren, **normalisieren**, export in CSV.
2. **Attribute & Datentypen** bestimmen, 1. NF sicherstellen.
3. **Physisches ERD + DDL-Script**, Bulkimport vorbereiten.
4. Daten analysieren, Bedeutung klären.
5. **SQL-Fragen beantworten**:
   - Länder unterhalb OECD-Mittel bei Schulbesuchsquote (15–19)
   - Land mit **höchstem tertiären Ausbildungsniveau**
   - Land mit **grösster Zunahme obligatorische Schulabschlüsse** pro Jahr
6. **Backup** der Datenbank erstellen.



### Option 3: Eigene Opendata-Quelle

#### Aufgaben:

1. **Quelle suchen**, CSV oder Excel laden.
2. **Normalisieren**, **ERD + DDL-Script** erstellen.
3. **Import** + DML-Script schreiben.
4. Eigene **Analysen & Abfragen** formulieren.
5. **Backup** der Datenbank erstellen.



## Checkpoint-Fragen

- Wie bringt man Daten aus Excel in eine **normalisierte DB**?
- Unterschied: **logisches vs. physisches Backup**?
- Wie funktioniert der **Restore** bei:
  - Voll-Backup?
  - Inkrementellem Backup?
  - Differentiellem Backup?
- Nenne **drei Backup-Möglichkeiten** + konkrete Tools/Befehle.
- Was macht `SELECT INTO OUTFILE`?



## Nützliche Quellen

- [OpenData Zürich](https://data.stadt-zuerich.ch)
- [OpenData Swiss](https://opendata.swiss)
- [BFS Statistikportal](https://www.bfs.admin.ch)
- [Open Science Swiss](https://www.uzh.ch/de/researchinnovation/ethics/openscience)
- [Open Access Network](https://open-access.network/startseite)



## Referenzen

- [Statistik-Grundlagen zu Perzentilen](https://wissenschafts-thurm.de/grundlagen-der-statistik-lagemasse-median-quartile-perzentile-und-modus/)
- [SELECT INTO OUTFILE](https://mariadb.com/kb/en/select-into-outfile/)

