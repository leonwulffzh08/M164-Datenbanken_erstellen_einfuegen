## Grundlagen Datenmodellierung

### Stichwortartige Zusammenfassung:
- **Datenmodellierung** als Teil der Datenbankentwicklung
- Ziel: Struktur der zu verarbeitenden und speichernden Daten festlegen
- Drei aufeinander aufbauende Datenmodelle:
  1. **Konzeptionelles Datenmodell (ERM)**: Fachlich orientiert, nicht implementierungsabhängig
  2. **Logisches Datenmodell (ERD)**: Relationales Modell, mit Primär- und Fremdschlüsseln
  3. **Physisches Datenmodell**: Enthält DDL-Anweisungen und technische Details
- **3NF (Dritte Normalform)** in operativen Systemen (z.B. ERP, Core DWH)
- **Star Schema** für Reporting-Systeme (Data Mart)
- **Data Vault Modellierung** für flexible, automatisierte Datenintegration (Core DWH, Integration Layer)

---

## Normalisierungsschritte (M162):

### 1. Normalform (1NF)
- Eliminierung von Wiederholungsgruppen
- Atomare Attributwerte

### 2. Normalform (2NF)
- 1NF erfüllt
- Jedes Nicht-Schlüsselattribut ist voll funktional abhängig vom Primärschlüssel

### 3. Normalform (3NF)
- 2NF erfüllt
- Keine transitiven Abhängigkeiten zwischen Nicht-Schlüsselattributen

### Boyce-Codd-Normalform (BCNF) (optional, falls nötig)
- Stärkere Form der 3NF
- Jedes funktional abhängige Attribut muss eine Determinante sein

### 4. Normalform (4NF)
- Keine mehrwertigen Abhängigkeiten

### 5. Normalform (5NF, PJ/NF)
- Zerlegung in kleinere Tabellen ohne Informationsverlust (Join-Abhängigkeit)

### 6. Normalform (6NF, selten verwendet)
- Umgang mit zeitabhängigen Daten (temporal databases)

