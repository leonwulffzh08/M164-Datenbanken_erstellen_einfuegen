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

**Mindmap der 10 wichtigsten DB-Engines:**
(Erstellt und im Lernportfolio abgelegt.)

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

