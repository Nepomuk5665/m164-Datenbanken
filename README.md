# 18.02.2024

Heute habe ich mich mit Datenbanken beschäftigt. Zu Beginn haben wir wichtige Begriffe wiederholt und aufgefrischt. Wir haben auch über das Thema Normalisierung gesprochen.

Dann sollte ich MySQL auf meinem Mac installieren. Das hat leider nicht so gut geklappt - ich hatte Probleme damit, den MySQL-Dienst zu starten. Auch nach mehreren Versuchen läuft es noch nicht richtig. Das muss ich mir morgen nochmal genauer anschauen.

Wir haben auch über verschiedene Möglichkeiten gesprochen, wie man MySQL einrichten kann - zum Beispiel mit Xampp, Docker oder in der AWS Cloud. Außerdem wurde uns MySQLWorkbench als Programm vorgestellt.

Am Ende haben wir uns mit Datenbankmodellen beschäftigt. Wir haben an einem Beispiel eines "Tourenplaners" gelernt, wie man Daten richtig strukturiert (erste, zweite und dritte Normalform).

Auch wenn es mit MySQL noch nicht klappt, habe ich heute viel Neues über Datenbanken gelernt.

# 25.02.2025

Heute habe ich mich intensiv mit fortgeschrittenen Konzepten der Datenbankmodellierung beschäftigt. Wir haben das Thema Generalisierung/Spezialisierung behandelt, was eine elegante Lösung für das Problem redundanter Attribute in verschiedenen Entitätstypen bietet. Dieses Konzept ist vergleichbar mit der Vererbung in der objektorientierten Programmierung.

Am Beispiel eines Tourenplaner-Systems haben wir gesehen, wie Mitarbeiter als Fahrer oder Disponenten agieren können. Statt alle Attribute in jeder Tabelle zu wiederholen, werden gemeinsame Attribute (Name, Vorname, etc.) in eine übergeordnete Tabelle "Person" oder "Mitarbeiter" ausgelagert. Die spezifischen Attribute (Führerscheinnummer für Fahrer, Büronummer für Disponenten) bleiben in den spezialisierten Tabellen. Diese "is_a"-Beziehung wird durch Fremdschlüssel realisiert.

Ein weiteres wichtiges Konzept, das wir behandelt haben, sind Identifying vs. Non-Identifying Relationships:
- Bei einer Identifying Relationship ist der Fremdschlüssel Teil des Primärschlüssels der Child-Tabelle. Beispiel: Die Raum-ID enthält zwingend die Gebäude-ID.
- Bei einer Non-Identifying Relationship bleibt der Fremdschlüssel separat vom Primärschlüssel. Beispiel: Ein Mitarbeiter hat eine Abteilungs-ID als Fremdschlüssel, aber seine eigene eindeutige MA-ID als Primärschlüssel.

Praktisch haben wir diese Konzepte mit SQL-DDL-Befehlen umgesetzt:
```sql
CREATE SCHEMA tourenplaner DEFAULT CHARACTER SET utf8mb4;
CREATE TABLE tbl_mitarbeiter (
    MA_ID INT PRIMARY KEY,
    Name VARCHAR(50),
    Vorname VARCHAR(30),
    Geburtsdatum DATETIME,
    Telefonnummer VARCHAR(12),
    Einkommen FLOAT(10,2)
);
CREATE TABLE tbl_fahrer (
    Fahrer_ID INT PRIMARY KEY,
    MA_ID INT,
    Fuehrerschein VARCHAR(20),
    FOREIGN KEY (MA_ID) REFERENCES tbl_mitarbeiter(MA_ID)
);
```

Interessant war auch das Forward Engineering mit MySQL Workbench, das uns ermöglicht, direkt aus einem visuellen Modell heraus SQL-Code zu generieren. Diese Funktion ist besonders nützlich bei komplexen Datenbankstrukturen, da sie Fehler reduziert und die Konsistenz zwischen Modell und Implementierung sicherstellt.

Wir haben außerdem über die Bedeutung der Zeichensatz-Wahl (Charset) gesprochen. UTF-8, genauer utf8mb4, ist heutzutage Standard, da er sämtliche Unicode-Zeichen einschließlich Emojis unterstützt.

Zum Schluss haben wir noch fortgeschrittene Datenbankkonzepte wie Partitioning, Storage Engines und Tablespaces angeschnitten, die für die Performance und Skalierung größerer Systeme relevant sind.

# 04.03.2025

Heute haben wir uns intensiv mit fortgeschrittenen Datenbankkonzepten und der praktischen Datenbearbeitung beschäftigt. Der Tag begann mit einer Wiederholung der Themen Generalisierung/Spezialisierung und Identifying Relationships, die wir letzte Woche behandelt hatten.

Anschließend haben wir uns mit Datentypen in MySQL/MariaDB befasst. Ich habe eine Übersicht verschiedener Datentypen erstellt, darunter Zahlentypen wie DECIMAL, Zeichenketten (VARCHAR, CHAR), Datumsformate, Boolean-Werte und binäre Objekte. Die richtige Auswahl des Datentyps ist entscheidend für die Effizienz und Integrität der Datenbank.

Wir haben drei wichtige Beziehungsarten in Datenbanken kennengelernt:
1. **Mehrfachbeziehungen**: Wenn zwei Tabellen mehrere unabhängige Beziehungen haben, die verschiedene Sachverhalte darstellen (z.B. im Tourenplaner die verschiedenen Beziehungen zwischen Fahrten und Orten).
2. **Rekursion (strenge Hierarchie)**: Eine Beziehung innerhalb derselben Tabelle, wobei ein Datensatz auf einen anderen der gleichen Tabelle verweist (z.B. Vorgesetzte/Mitarbeiter-Beziehung).
3. **Einfache Hierarchie mit Zwischentabelle**: Für komplexere Strukturen, in denen z.B. ein Mitarbeiter mehrere Vorgesetzte haben kann.

Besonders interessant fand ich das Stücklistenproblem, bei dem ein Produkt aus mehreren anderen Produkten bestehen kann, die wiederum aus weiteren Komponenten zusammengesetzt sein können.

Wir haben dann unser Tourenplaner-Schema mit diesen Beziehungsarten erweitert und die Struktur mit Forward Engineering in eine echte Datenbank übertragen.

Der nächste Teil des Tages konzentrierte sich auf die praktische Datenbearbeitung (DML - Data Manipulation Language):
- INSERT-Befehle zum Hinzufügen neuer Datensätze
- UPDATE zum Ändern bestehender Daten
- DELETE zum Entfernen von Datensätzen
- ALTER TABLE zum Ändern der Tabellenstruktur

Anschließend haben wir uns mit dem SELECT-Befehl befasst, um Daten aus der Datenbank abzufragen. Wir haben verschiedene Anwendungsfälle geübt, wie das Auswählen bestimmter Spalten, das Filtern mit WHERE-Klauseln und die Verwendung von Funktionen innerhalb von SELECT-Anweisungen.

Die Hauptaufgabe des Tages bestand darin, unseren erweiterten Tourenplaner mit Testdaten zu füllen. Wir haben Mitarbeiterdaten eingepflegt, Hierarchiebeziehungen definiert und Touren mit Start-, Ziel- und Via-Orten angelegt. Dabei habe ich gelernt, wie man komplexe UPDATE-Befehle mit Unterabfragen formuliert, um Daten effizient zu aktualisieren.

Bei den Hierarchie-Beziehungen war es wichtig zu verstehen, dass NULL-Werte in der Transformationstabelle nicht zulässig sind, da jeder Eintrag eine konkrete Beziehung darstellt.

Zum Abschluss haben wir unsere Daten mit SELECT- und JOIN-Befehlen überprüft und uns mit Herausforderungen wie referentieller Integrität bei Fremdschlüsseln beschäftigt.

Ich schreibe gerne die Fortsetzung deines Arbeitsjournals für heute, basierend auf dem, was du in den vorherigen Einträgen gelernt hast und dem aktuellen Unterrichtsmaterial.

# 11.03.2025

Heute haben wir uns mit fortgeschrittenen Datenbankbeziehungen und deren Implementierung in MySQL beschäftigt. Wir haben besonders die verschiedenen Typen von Constraints und deren Bedeutung für die referentielle Integrität untersucht.

Zunächst haben wir die verschiedenen Arten von Beziehungen wiederholt und analysiert, welche davon in relationalen Datenbanksystemen realisierbar sind:
- 1:1 Beziehungen werden in der Praxis als 1:c Beziehungen mit UNIQUE Constraint implementiert
- 1:m und c:m Beziehungen werden als 1:mc bzw. c:mc umgesetzt
- m:m Beziehungen können nur über Transformationstabellen realisiert werden

Besonders wichtig war das Verständnis der Constraints NOT NULL (NN) und UNIQUE (UQ), die bei Fremdschlüsseln eingesetzt werden, um die gewünschten Beziehungstypen zu realisieren. Wir haben mit MySQL Workbench verschiedene Beziehungen modelliert und per Forward Engineering die entsprechenden SQL-Befehle erzeugt.

In der Analyse des erzeugten SQL-Codes haben wir untersucht:
- Wie NOT NULL Constraints für Fremdschlüssel definiert werden
- Warum automatisch Indizes für Fremdschlüssel erstellt werden (Performance-Optimierung bei Joins)
- Wie UNIQUE Constraints für Fremdschlüssel implementiert werden
- Wie die referentielle Integrität durch CONSTRAINT-Anweisungen sichergestellt wird

Wir haben auch gelernt, wie man Fremdschlüssel-Constraints nachträglich mit ALTER TABLE hinzufügen kann:
```sql
ALTER TABLE DetailTabelle
  ADD CONSTRAINT FK_Detail_Master FOREIGN KEY (FremdschluesselSpalte)
  REFERENCES MasterTabelle (PrimaerschluesselSpalte);
```

Im praktischen Teil haben wir Testdaten in verknüpfte Tabellen eingefügt und die Auswirkungen der Constraints getestet. Dabei wurde besonders deutlich, wie wichtig die referentielle Integrität ist: Bei definierten Constraints ist es nicht möglich, in einer Detail-Tabelle einen Fremdschlüsselwert einzufügen, der als Primärschlüssel in der Master-Tabelle nicht existiert.

Im zweiten Teil des Tages haben wir uns mit der Mengenlehre beschäftigt und deren Anwendung auf Datenbankabfragen. Die verschiedenen Mengenoperationen (Vereinigung, Schnittmenge, Differenz) bilden die theoretische Grundlage für JOIN-Operationen in SQL.

Wir haben verschiedene JOIN-Typen wiederholt und praktisch angewandt:
- INNER JOIN für die Schnittmenge zweier Tabellen
- LEFT JOIN und RIGHT JOIN für asymmetrische Verknüpfungen
- FULL JOIN für die vollständige Vereinigung (in MySQL über UNION realisiert)

Besonders interessant war für mich die Umsetzung komplexer Abfragen mit mehreren JOINs und die Erkenntnis, dass die Reihenfolge der Tabellen und der gewählte JOIN-Typ großen Einfluss auf das Ergebnis haben.

Zum Abschluss haben wir noch Checkpoints zu wichtigen Konzepten durchgearbeitet:
- Referentielle Integrität: Sicherstellung der Konsistenz zwischen verknüpften Tabellen
- Constraints bei Beziehungen (NN, UQ, Fremdschlüssel-Constraints)
- Unterschiede zwischen verschiedenen JOIN-Arten
- Umsetzung von 1:1 und c:m Beziehungen
- Konsequenzen fehlender Constraint-Anweisungen

Auch wenn einige der fortgeschrittenen Konzepte anfangs komplex erschienen, hat mir die praktische Arbeit mit MySQL Workbench und die Analyse der generierten SQL-Befehle geholfen, die theoretischen Grundlagen besser zu verstehen. Ich sehe jetzt klarer, wie wichtig ein sorgfältiges Datenbankdesign und die korrekte Implementierung von Beziehungen für die Datenintegrität und -konsistenz sind.

# 18.03.2025

Heute haben wir uns intensiv mit der praktischen Anwendung von komplexen SELECT-Abfragen in MySQL beschäftigt. Besonderes Augenmerk haben wir auf die korrekte Reihenfolge der SQL-Klauseln gelegt, da dies entscheidend für die Verarbeitung und das Ergebnis einer Abfrage ist.

Ein wichtiger Merksatz, den wir für die Reihenfolge der SELECT-Klauseln gelernt haben, ist "WARUM GEHT HERBERT OFT LAUFEN". Dieser Merksatz hilft uns, die zwingende Reihenfolge in MySQL zu behalten:

- **W**HERE: Filtert die Daten vor der Gruppierung
- **G**ROUP BY: Fasst Daten in Gruppen zusammen
- **H**AVING: Filtert die Daten nach der Gruppierung
- **O**RDER BY: Sortiert das Ergebnis
- **L**AUFEN (LIMIT): Begrenzt die Anzahl der Ergebniszeilen

Was ich besonders interessant fand, ist dass diese Reihenfolge nicht nur eine syntaktische Anforderung ist, sondern auch die tatsächliche Verarbeitungsreihenfolge der Abfrage im Datenbanksystem widerspiegelt. Das erklärt, warum HAVING-Bedingungen erst nach GROUP BY angewendet werden können - sie operieren auf den bereits gruppierten Ergebnissen.

Wir haben diese Konzepte mit praktischen Beispielen vertieft und komplexe Abfragen formuliert, die mehrere dieser Klauseln kombinieren. Zum Beispiel:

```sql
SELECT 
    abteilung, 
    COUNT(mitarbeiter_id) AS anzahl_mitarbeiter,
    AVG(gehalt) AS durchschnittsgehalt
FROM 
    tbl_mitarbeiter
WHERE 
    einstellungsdatum > '2020-01-01'
GROUP BY 
    abteilung
HAVING 
    COUNT(mitarbeiter_id) > 5
ORDER BY 
    durchschnittsgehalt DESC
LIMIT 3;
```

Diese Abfrage zeigt die drei Abteilungen mit dem höchsten Durchschnittsgehalt, in denen mehr als 5 Mitarbeiter nach 2020 eingestellt wurden.

Wir haben auch den Unterschied zwischen WHERE und HAVING genauer betrachtet:
- WHERE filtert einzelne Datensätze vor der Gruppierung
- HAVING filtert Gruppen basierend auf Aggregatfunktionen nach der Gruppierung

Ein weiteres wichtiges Konzept, das wir heute behandelt haben, ist die Verwendung von Aliassen (AS) sowohl für Tabellen als auch für Spalten, um die Lesbarkeit von Abfragen zu verbessern und komplexe Joins zu vereinfachen.

In praktischen Übungen haben wir verschiedene Aggregatfunktionen (COUNT, SUM, AVG, MIN, MAX) mit GROUP BY und HAVING kombiniert, um aussagekräftige Analysen unserer Datensätze zu erstellen.

Ich habe auch Notizen zu den Aspekten der Datenintegrität gemacht:
1. Eindeutigkeit und Datenkonsistenz
2. Referenzielle Integrität
3. Korrekte Datentypen
4. Sinnvolle Datenbeschränkungen
5. Datenvalidierung vor dem Einfügen

Besonders wichtig erscheint mir nach dem heutigen Tag das Verständnis dafür, wie professionelle Datenbanken mit Löschoperationen umgehen - nämlich dass echtes Löschen (DELETE) in vielen professionellen Umgebungen vermieden wird, um historische Daten zu bewahren und die Nachvollziehbarkeit zu gewährleisten.

Für die nächste Woche plane ich, die Übungen zu den ON DELETE-Optionen bei Fremdschlüsseln (CASCADE, SET NULL, NO ACTION/RESTRICT) zu vertiefen und mehr über die praktischen Implikationen dieser Constraints zu lernen.


# 25.03.2025

Heute haben wir uns mit fortgeschrittenen Abfragetechniken und Datenimport-Methoden in MySQL beschäftigt. Der Schwerpunkt lag auf Subqueries und dem effizienten Import größerer Datenmengen mittels LOAD DATA INFILE.

Wir begannen mit Subqueries (Unterabfragen), die eine mächtige Methode darstellen, um komplexe Abfragen zu formulieren. Ich habe gelernt, dass es zwei Haupttypen von Subqueries gibt:

1. **Skalare Unterabfragen**: Diese liefern genau einen Wert zurück (eine Zeile, eine Spalte) und können mit Vergleichsoperatoren wie =, >, <, >= und <= verwendet werden.

2. **Nicht-skalare Unterabfragen**: Diese liefern mehrere Werte zurück und werden typischerweise mit Operatoren wie IN, NOT IN, EXISTS oder NOT EXISTS kombiniert.

Ein Beispiel für eine skalare Unterabfrage, das wir behandelt haben:

```sql
SELECT city_destination, ticket_price, travel_time, transportation 
FROM one_way_ticket
WHERE ticket_price < (
    SELECT ticket_price FROM one_way_ticket
    WHERE city_destination = 'Bariloche' AND city_origin = 'Paris'
) AND city_origin = 'Paris';
```

Hier wird zuerst die Unterabfrage ausgeführt, um den Ticketpreis für die Strecke Paris-Bariloche zu ermitteln, und dann werden alle Ziele von Paris aus gesucht, die günstiger sind.

Für nicht-skalare Unterabfragen haben wir folgendes Beispiel betrachtet:

```sql
SELECT name, age, country
FROM users
WHERE country IN (
   SELECT name FROM country WHERE region = 'Europa'
);
```

Diese Abfrage liefert alle Benutzer, die in europäischen Ländern leben, indem sie zunächst alle europäischen Länder ermittelt und dann die Benutzer filtert.

Im zweiten Teil des Tages haben wir uns mit dem Bulkimport von Daten beschäftigt, speziell mit dem LOAD DATA INFILE Befehl in MySQL. Dies ist besonders nützlich für den Import großer Datenmengen aus CSV-Dateien.

Wir haben den Unterschied zwischen dem Import vom Server und vom Client gelernt:
- `LOAD DATA INFILE` sucht die Datei im Dateisystem des Servers
- `LOAD DATA LOCAL INFILE` liest die Datei vom Client-Rechner

Für den Client-Import waren einige Konfigurationsänderungen notwendig:
```sql
SET GLOBAL local_infile=1;
```

Außerdem mussten wir in der Workbench-Verbindung den Parameter `OPT_LOCAL_INFILE=1` setzen.

Besonders interessant fand ich die vielfältigen Anpassungsmöglichkeiten beim Datenimport:

1. **Datumsformate anpassen**:
```sql
LOAD DATA LOCAL INFILE 'C:/M164/import.csv'
INTO TABLE meine_tabelle
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\r\n'
IGNORE 1 ROWS
(id, name, @datum, wert)
SET geburtsdatum = STR_TO_DATE(@datum, '%d.%m.%Y');
```

2. **Spaltenreihenfolge ändern**:
Man kann die Reihenfolge der Spalten in der CSV-Datei an die Tabellenstruktur anpassen, indem man die Spalten in der richtigen Reihenfolge auflistet.

3. **Spalten überspringen**:
Mit Hilfe von Platzhalter-Variablen können unerwünschte Spalten übersprungen werden:
```sql
(id, name, @skip_spalte, alter, @skip_spalte2)
```

4. **Werte transformieren**:
Mit der SET-Klausel können Werte beim Import transformiert werden, z.B. mit CASE-Statements:
```sql
(id, name, @ort)
SET ort_id = CASE
    WHEN @ort = 'Zürich' THEN 1
    WHEN @ort = 'Basel' THEN 2
    ELSE NULL
END;
```

In einer praktischen Übung haben wir die Normalisierung einer Adressdatenbank implementiert:
1. Import von Adressdaten in eine temporäre Tabelle `tbl_Adr`
2. Erstellung eines normalisierten Schemas mit Tabellen für Personen, Straßen und Orte
3. Übertragen der Daten mit INSERT INTO ... SELECT
4. Überprüfung der Datenintegrität durch JOINs
5. Direkter Import in die normalisierten Tabellen ohne Zwischenstation

Eine wichtige Erkenntnis war die Methode zum Finden und Bereinigen redundanter Datensätze:
```sql
SELECT Ortsbezeichnung, COUNT(*) as Anzahl FROM tbl_Ort
GROUP BY Ortsbezeichnung
HAVING COUNT(*) > 1;
```

Zum Abschluss haben wir gelernt, wie man Daten aus einer Tabelle in eine andere einfügt:
```sql
INSERT INTO customers (name, email, address)
SELECT name, email, address FROM new_customers;
```

Diese Technik ist besonders nützlich, um Daten aus temporären Import-Tabellen in eine normalisierte Datenbankstruktur zu überführen.

Insgesamt war heute ein sehr praxisorientierter Tag, der mir gezeigt hat, wie man große Datenmengen effizient verarbeiten und in eine strukturierte Datenbank importieren kann. Diese Kenntnisse werden mir bei der Arbeit mit realen Datensätzen sehr nützlich sein.


# 01.04.2025

Heute haben wir uns intensiv mit Datenbankbackups und verschiedenen Sicherungsstrategien beschäftigt. Dieses Thema ist für den professionellen Umgang mit Datenbanken äußerst wichtig, da der Schutz von Daten vor Verlust essenziell ist.

Wir begannen mit einer Einführung in die theoretischen Grundlagen von Datenbankbackups. Dabei haben wir verschiedene Backup-Typen kennengelernt:

- **Vollbackup**: Sicherung der gesamten Datenbank
- **Differentielles Backup**: Sicherung aller Änderungen seit dem letzten Vollbackup
- **Inkrementelles Backup**: Sicherung aller Änderungen seit dem letzten Backup (egal welcher Art)

Außerdem haben wir die Unterschiede zwischen logischen und physischen Backups verstanden:
- **Logische Backups**: Exportieren die Datenbank in SQL-Befehle (DDL und DML), die beim Restore ausgeführt werden
- **Physische Backups**: Kopieren die tatsächlichen Datenbankdateien des Dateisystems

Im praktischen Teil haben wir zunächst mit mysqldump gearbeitet, einem Tool für logische Backups. Ich konnte erfolgreich ein Backup meiner Tourenplaner-Datenbank erstellen:

```bash
mysqldump -u root -p --port=3306 tourenplaner > C:\BACKUP\tp_dump.sql
```

Besonders interessant war die Analyse des erzeugten Backup-Files. Es enthält nicht nur die INSERT-Statements für meine Daten, sondern auch alle DDL-Befehle, um die Tabellenstruktur, Indizes und Constraints wieder herzustellen. Außerdem werden zu Beginn des Backups alle DROP TABLE-Befehle ausgeführt, um eine saubere Neuinstallation zu gewährleisten.

Um das Backup zu verifizieren, habe ich einen Restore-Test durchgeführt, indem ich die Datenbank gelöscht und aus dem Backup wiederhergestellt habe:

```sql
DROP DATABASE tourenplaner;
CREATE DATABASE tourenplaner;
USE tourenplaner;
SOURCE C:\BACKUP\tp_dump.sql;
```

Die Datenbank wurde erfolgreich wiederhergestellt, was die Integrität des Backups bestätigt hat.

Danach haben wir uns mit fortgeschrittenen Backup-Strategien beschäftigt. Besonders wichtig war das Verständnis des Unterschieds zwischen Online- und Offline-Backups:
- **Online-Backups**: Werden während des laufenden Datenbankbetriebs erstellt
- **Offline-Backups**: Erfordern ein Herunterfahren der Datenbank

Wir haben auch gelernt, was Snapshot-Backups sind - eine spezielle Form des physischen Backups, bei dem ein konsistenter "Schnappschuss" der Datenbank zu einem bestimmten Zeitpunkt erstellt wird.

Mit MariaBackup habe ich auch ein physisches Backup erstellt, was eine andere Herangehensweise erforderte:

```bash
mariabackup --backup --target-dir=C:/BACKUP/physical --user=root --password=mypassword
```

Interessant war hierbei der notwendige "Prepare"-Schritt vor der Wiederherstellung, der sicherstellt, dass die Backup-Dateien in einem konsistenten Zustand sind:

```bash
mariabackup --prepare --target-dir=C:/BACKUP/physical
```

Für die Wiederherstellung wird dann der Parameter --copy-back verwendet:

```bash
mariabackup --copy-back --target-dir=C:/BACKUP/physical
```

MariaBackup unterstützt auch inkrementelle Backups, was für regelmäßige Sicherungen besonders nützlich ist, da es Zeit und Speicherplatz spart:

```bash
mariabackup --backup --target-dir=C:/BACKUP/incremental --incremental-basedir=C:/BACKUP/physical --user=root --password=mypassword
```

Zum Abschluss haben wir externe Backup-Programme wie Acronis betrachtet, die nicht nur Datenbanken, sondern ganze Systeme sichern können.

Ich habe heute viel über die Wichtigkeit regelmäßiger und durchdachter Backup-Strategien gelernt. Besonders in Unternehmensumgebungen ist es entscheidend, einen geeigneten Mix aus Voll-, differentiellen und inkrementellen Backups zu implementieren, um sowohl Datensicherheit als auch Performanz zu gewährleisten. Die praktischen Übungen haben mir geholfen, die theoretischen Konzepte besser zu verstehen und anzuwenden.


# 08.04.2025

Heute habe ich mich intensiv mit dem Thema Daten-Normalisierung und dem effizienten Import von normalisierten Daten in MySQL beschäftigt. Wir haben uns der praktischen Aufgabe "DB_Freifaecher" gewidmet, die als Vorbereitung für unsere kommende Leistungsbeurteilung dient.

Der Ausgangspunkt war eine Excel-Tabelle mit Daten zu Freifächern und SchülerInnen. Unsere Aufgabe bestand darin, diese in eine normalisierte Datenbankstruktur zu überführen. Ich habe folgende Schritte durchgeführt:

1. **Erstellung der ersten Normalform (1NF)**: Zunächst habe ich die Excel-Tabelle analysiert und alle nicht-atomaren Felder identifiziert. Besonders bei den Freifächern, wo mehrere in einer Zelle standen, musste ich die Daten aufspalten. Ich habe die Daten in Excel entsprechend aufbereitet und als mehrere CSV-Dateien exportiert.

2. **Logisches ERD erstellen**: Basierend auf der 1NF habe ich ein logisches Entity-Relationship-Diagramm mit fünf Tabellen erstellt:
   - `tbl_schueler` (mit SchülerID, Name, Vorname, Geburtsdatum)
   - `tbl_klasse` (mit KlasseID, Klassenbezeichnung)
   - `tbl_freifach` (mit FreifachID, Freifachbezeichnung)
   - `tbl_lehrer` (mit LehrerID, Name, Vorname)
   - `tbl_belegung` (als Zwischentabelle für die m:n Beziehung zwischen SchülerInnen und Freifächern)

   Bei der Definition der Kardinalitäten habe ich festgestellt, dass ein/e SchülerIn in genau einer Klasse sein muss (1:n), aber mehrere Freifächer belegen kann (m:n). Ein Freifach wird von genau einem/einer LehrerIn unterrichtet (1:n).

3. **Physisches ERD und Constraints**: Im nächsten Schritt habe ich das physische ERD erstellt und dabei besonders auf die korrekten Constraints geachtet:
   - NOT NULL (NN) für Pflichtfelder wie Namen und Fremdschlüssel
   - UNIQUE (UQ) für Felder, die eindeutig sein müssen
   - Geeignete Datentypen (VARCHAR für Namen, DATE für Geburtsdaten, etc.)
   - Korrekte Fremdschlüssel-Constraints mit den ON DELETE/UPDATE-Optionen

   Besonders interessant war die Implementierung der Zwischentabelle `tbl_belegung`, die neben den beiden Fremdschlüsseln (SchülerID und FreifachID) auch zusätzliche Attribute wie Anmeldedatum enthalten konnte.

4. **Datentransfer mit LOAD DATA LOCAL INFILE**: Der praktische Teil bestand im Import der CSV-Daten in die normalisierten Tabellen. Hier ein Beispiel für den Import der Schülerdaten:

```sql
LOAD DATA LOCAL INFILE 'C:/M164/schueler.csv'
INTO TABLE tbl_schueler
FIELDS TERMINATED BY ';'
LINES TERMINATED BY '\r\n'
IGNORE 1 ROWS
(Name, Vorname, @gebdat, @klasse)
SET 
    Geburtsdatum = STR_TO_DATE(@gebdat, '%d.%m.%Y'),
    FK_KlasseID = (SELECT KlasseID FROM tbl_klasse WHERE Klassenbezeichnung = @klasse);
```

Hierbei war es besonders wichtig, die Fremdschlüsselbeziehungen korrekt abzubilden. Durch die Verwendung von Hilfsvariablen (mit @-Präfix) und der SET-Klausel konnte ich die Werte entsprechend transformieren und die IDs korrekt zuordnen.

5. **Datenbereinigung**: Nach dem Import musste ich noch einige Inkonsistenzen bereinigen:
   - Entfernen von Duplikaten durch Identifikation mit GROUP BY und HAVING
   - Behandlung leerer Werte (NULL vs. leerer String)
   - Korrektur von Schreibweisen bei Namen und Bezeichnungen

6. **Datenvalidierung**: Mit komplexen SELECT-Abfragen und JOINS habe ich geprüft, ob die importierten Daten mit den Originaldaten übereinstimmen:

```sql
SELECT s.Name, s.Vorname, k.Klassenbezeichnung, f.Freifachbezeichnung, l.Name as Lehrer
FROM tbl_schueler s
JOIN tbl_klasse k ON s.FK_KlasseID = k.KlasseID
JOIN tbl_belegung b ON s.SchuelerID = b.FK_SchuelerID
JOIN tbl_freifach f ON b.FK_FreifachID = f.FreifachID
JOIN tbl_lehrer l ON f.FK_LehrerID = l.LehrerID
ORDER BY s.Name, s.Vorname;
```

7. **Testdatengenerierung**: Eine interessante Herausforderung war die Erzeugung von 290 zusätzlichen SchülerInnen-Datensätzen mit einem Testdatengenerator. Ich habe einen Online-Generator genutzt und die Daten dann in Excel so angepasst, dass sie zu unserer Datenbankstruktur passen (zufällige Verteilung auf Klassen und Freifächer).

8. **Komplexe Abfragen**: Zum Abschluss habe ich die geforderten SELECT-Abfragen implementiert:

```sql
-- Anzahl Teilnehmer von Inge Sommer
SELECT COUNT(*) as Teilnehmeranzahl
FROM tbl_belegung b
JOIN tbl_freifach f ON b.FK_FreifachID = f.FreifachID
JOIN tbl_lehrer l ON f.FK_LehrerID = l.LehrerID
WHERE l.Name = 'Sommer' AND l.Vorname = 'Inge';

-- Klassen mit Anzahl SchülerInnen in Freifächern
SELECT k.Klassenbezeichnung, COUNT(DISTINCT b.FK_SchuelerID) as AnzahlSchueler
FROM tbl_klasse k
JOIN tbl_schueler s ON k.KlasseID = s.FK_KlasseID
JOIN tbl_belegung b ON s.SchuelerID = b.FK_SchuelerID
GROUP BY k.Klassenbezeichnung
ORDER BY k.Klassenbezeichnung;

-- SchülerInnen in Chor oder Elektronik
SELECT s.Name, s.Vorname, f.Freifachbezeichnung
FROM tbl_schueler s
JOIN tbl_belegung b ON s.SchuelerID = b.FK_SchuelerID
JOIN tbl_freifach f ON b.FK_FreifachID = f.FreifachID
WHERE f.Freifachbezeichnung IN ('Chor', 'Elektronik')
ORDER BY f.Freifachbezeichnung, s.Name, s.Vorname;
```

Die Ergebnisse habe ich mit SELECT INTO OUTFILE in eine Datei exportiert:

```sql
SELECT s.Name, s.Vorname, f.Freifachbezeichnung
INTO OUTFILE 'C:/M164/chor_elektronik.csv'
FIELDS TERMINATED BY ';'
LINES TERMINATED BY '\r\n'
FROM tbl_schueler s
JOIN tbl_belegung b ON s.SchuelerID = b.FK_SchuelerID
JOIN tbl_freifach f ON b.FK_FreifachID = f.FreifachID
WHERE f.Freifachbezeichnung IN ('Chor', 'Elektronik');
```

Zum Abschluss habe ich ein vollständiges Backup der Datenbank erstellt:

```sql
mysqldump -u root -p --databases freifaecher > C:/M164/backup_freifaecher.sql
```

Insgesamt war heute ein sehr praxisorientierter Tag, der das theoretische Wissen über Normalisierung und die technischen Aspekte des Datenimports zusammengeführt hat. Ich fühle mich jetzt gut vorbereitet für die kommende LB2, da ich alle wichtigen Schritte von der Datenanalyse bis zur Implementierung einer normalisierten Datenbank durchgeführt habe. Besonders der Umgang mit Fremdschlüsselbeziehungen beim Datenimport war eine wertvolle Erfahrung, die mir in der Praxis sicher noch oft nützlich sein wird.

