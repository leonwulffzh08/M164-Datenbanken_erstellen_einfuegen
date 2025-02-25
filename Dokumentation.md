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

