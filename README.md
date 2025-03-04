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
