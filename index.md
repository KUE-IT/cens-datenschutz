# Datenschutzerklärung — Cens

**App:** Cens
**Verantwortliche Stelle:** Kantonsschule Uetikon am See (KUE), Bergstrasse 113, 8707 Uetikon am See
**Entwickler:** Nikola Jovanov, kue.it@kuezh.ch
**Stand:** 19. Juni 2026

Cens ist eine Inventar- und Ausleihverwaltungs-App für Schulen. Aktuell wird sie an der Kantonsschule Uetikon am See (KUE) eingesetzt und über Apple School Manager als Custom App auf KUE-iPads verteilt; eine Ausweitung auf weitere Schweizer Schulen ist perspektivisch vorgesehen. Diese Datenschutzerklärung beschreibt den Einsatz an der KUE und erläutert transparent, welche Daten Cens verarbeitet, wo sie gespeichert werden und wer Zugriff darauf hat.

---

## 1. Zusammenfassung in einem Satz

Cens speichert Inventar- und Ausleihdaten ausschliesslich auf den KUE-iPads sowie im privaten iCloud-Konto der Schule (CloudKit). Es gibt **keine Werbung**, **keine Analyse-Tracker**, **keine Drittanbieter-SDKs**, **keinen Verkauf von Daten** und **keine Übermittlung an Dritte** ausser den unten aufgeführten technisch notwendigen Diensten.

---

## 2. Welche Daten Cens verarbeitet

### 2.1 Geräte- und Inventardaten
- Inventarnummer, QR-Code, Bezeichnung, Hersteller, Modell, Seriennummer
- Anschaffungsdatum, Preis, Händler/Lieferant
- Standort (Raum), Zustand, Status, Garantieinformationen
- Fotos der Geräte (vom Personal aufgenommen)
- Vollständiger Änderungsverlauf pro Gerät (wer hat wann was geändert)

### 2.2 Ausleihdaten zu Personen
Beim Erfassen einer Ausleihe können folgende Daten zu der ausleihenden Person erfasst werden:
- **Pflichtfeld:** Name
- **Optional:** E-Mail-Adresse, Anrede, Personentyp (Schüler/in, Lehrperson, Mitarbeitende, extern)
- **Optional bei der Geräteausgabe:** Fotos der Übergabe (Zustand des Geräts), digitale Unterschrift
- **Optional bei der Rückgabe:** Fotos des Zustands bei der Rückgabe

Die Erfassung des Personentyps erfolgt automatisch aus der E-Mail-Domain (`@stud.kuezh.ch` → Schüler/in, `@kuezh.ch` → Lehrperson). Eine manuelle Korrektur ist jederzeit möglich.

### 2.3 Benutzerkonten innerhalb der App
Personen, die die App bedienen (z. B. Lehrpersonen, Hausdienst, Administration), werden in der App als Benutzerkonten geführt. Erfasst werden:
- Name, E-Mail (optional), Rolle (Admin, Adjunkt, Hauswart, Hausmeister, Lehrperson, …)
- Sicherheitscode (nur für Admin-Konten): wird **nicht** im Klartext gespeichert, sondern als kryptographischer Hash (SHA-256 mit PBKDF2, 600 000 Iterationen, individuellem Salt) im Apple-Keychain abgelegt.

### 2.4 Aktivitätsprotokoll
Die App führt ein internes Protokoll über durchgeführte Aktionen (z. B. „Ausleihe erfasst", „Gerät bearbeitet", „Inventur gestartet"). Pro Eintrag wird gespeichert: Zeitpunkt, Benutzer-Name, Aktionstyp, Kategorie, Kurzbeschreibung. Dieses Protokoll dient ausschliesslich der Nachvollziehbarkeit innerhalb der KUE und verlässt die App nicht.

### 2.5 App-Einstellungen
Konfigurationsdaten wie Schul-Standort (für die Schuljahrsberechnung), bevorzugte Fachschaft (pro iPad), Mailserver-Einstellungen (Host, Port, Benutzername). **Das Mailserver-Passwort wird ausschliesslich im Apple-Keychain gespeichert und nie ins Aktivitätsprotokoll oder in die Cloud synchronisiert.**

### 2.6 Schlüssel-, Badge- und PIN-Zuordnung
Cens verwaltet die Ausgabe von mechanischen Schlüsseln, Zutritts-Badges und Tür-PIN-Codes an Personen oder Spezial-Halter (z. B. Tresor, Schlüsselkasten). Erfasst werden:
- **Pflichtfeld:** Name des Halters (bei interner Ausgabe) bzw. Bezeichnung des Spezial-Halters
- **Optional:** E-Mail-Adresse (ausschliesslich für Rückgabe-Erinnerungen), Asset-Typ (Schlüssel/Badge/PIN), Schließgruppe, Ausgabe-/Rückgabedatum, Status, Lagerort
- **Optional bei der Übergabe:** Foto, digitale Unterschrift, Bestätigung der Haftungs-/Verlustbelehrung
- **PIN-/Badge-Codes** werden **niemals im Klartext** gespeichert, sondern reversibel verschlüsselt (AES-GCM, Schlüssel im Apple-Keychain). Sie werden nicht in Backups, CSV-Exporte oder E-Mails übernommen und sind nur für berechtigte Rollen einsehbar; jede Klartext-Anzeige wird im Aktivitätsprotokoll vermerkt.

**Zweckbindung:** Diese Daten dienen ausschliesslich der Zutrittssicherheit und dem Inventar-Nachweis. Sie werden **nicht** zur Anwesenheits-, Verhaltens- oder Leistungskontrolle verwendet.

**Zugriff:** Das Schlüsselinventar ist nur für die Rollen Hauswart, Hausmeister und Admin sichtbar und bearbeitbar.

---

## 3. Wo die Daten gespeichert werden

### 3.1 Lokal auf dem iPad
Sämtliche Inventar-, Ausleih- und Benutzerdaten werden zunächst lokal auf dem iPad in einer SwiftData-Datenbank gespeichert. Fotos und Unterschriften werden im geschützten App-Sandbox-Bereich abgelegt.

### 3.2 Synchronisation via Apple iCloud (CloudKit)
Die App synchronisiert ihre Daten über Apples **CloudKit-Dienst** in einem **privaten CloudKit-Container** (`iCloud.ch.kue.Sora`), der mit dem KUE-eigenen Apple-Konto verknüpft ist. Das bedeutet:
- Daten werden auf Apples Servern (in der Regel in der EU) verschlüsselt gespeichert.
- Zugriff haben ausschliesslich Geräte, die mit dem KUE-Apple-Konto über Apple School Manager verbunden sind.
- Apple selbst kann auf die Inhalte technisch nicht zugreifen (Verschlüsselung im Ruhezustand und während der Übertragung).
- **Falls die iCloud-Verbindung nicht verfügbar ist, läuft die App lokal weiter.** Daten werden bei der nächsten Verbindung nachsynchronisiert.

### 3.3 Backups
Die App erstellt automatisch ein tägliches Backup (ZIP-Datei) im Dokumentenordner des iPads und behält die letzten fünf Backups. Diese Backups verlassen das iPad nur, wenn Sie sie aktiv exportieren (z. B. per AirDrop oder Mail).

### 3.4 Cache
Druck-Etiketten und temporäre Bilder werden im Caches-Ordner abgelegt und beim Start der App regelmässig gelöscht.

---

## 4. Datenübertragung an Dritte

Cens überträgt Daten ausschliesslich an die folgenden Dienste, alle für den Betrieb der App technisch erforderlich:

### 4.1 Apple iCloud / CloudKit
Wie unter Punkt 3.2 beschrieben. Apple agiert als Auftragsverarbeiter. Apples Datenschutzbestimmungen: https://www.apple.com/legal/privacy/

### 4.2 Open-Meteo (Wetterdienst)
Das Dashboard kann lokales Wetter anzeigen. Dazu sendet die App die ungefähren Koordinaten Ihres Standorts (gerundet auf ca. 10 Meter Genauigkeit) an **open-meteo.com**. Es wird **keine Benutzer-ID**, **keine Geräte-ID** und **kein API-Schlüssel** mitgeschickt. Der Aufruf ist anonym. Datenschutz von Open-Meteo: https://open-meteo.com/en/terms

**Sie können das Wetter-Feature jederzeit deaktivieren**, indem Sie der App in den iPad-Einstellungen den Standortzugriff entziehen.

### 4.3 E-Mail-Versand (optional, durch die KUE konfigurierbar)
Falls in den Einstellungen ein Mailserver hinterlegt ist, versendet die App Rückgabe-Erinnerungen und Mahnungen an die in den Ausleih- bzw. Schlüsseldaten erfassten E-Mail-Adressen. Die E-Mails enthalten: Geräte- bzw. Schlüsselbezeichnung, Inventarnummer bzw. Schließgruppe, Rückgabedatum, Name der betroffenen Person. **Tür-/PIN-Codes werden nie per E-Mail versendet.** Der Versand erfolgt **direkt vom iPad an den von der KUE festgelegten SMTP-Server** über eine verschlüsselte Verbindung (STARTTLS). Es wird kein externer E-Mail-Dienstleister eingebunden.

### 4.4 Keine weiteren Dienste
Cens nutzt **keine** Analytics-, Crash-Reporting-, Werbe- oder Tracking-SDKs. Es gibt **keine** Verbindungen zu Facebook, Google, Microsoft, Firebase, Segment oder ähnlichen Diensten.

---

## 5. iOS-Berechtigungen

Die App fragt nur die folgenden System-Berechtigungen an:

| Berechtigung | Zweck | Kann verweigert werden? |
|---|---|---|
| **Kamera** | QR- und Barcode-Scan von Inventargeräten | Ja, dann ist die manuelle Eingabe erforderlich |
| **Standort (bei Nutzung)** | Anzeige des lokalen Wetters im Dashboard | Ja, dann zeigt das Dashboard kein Wetter |
| **Mitteilungen** | Erinnerungen an überfällige Ausleihen | Ja, dann erscheinen die Hinweise nur in der App |

Die App fragt **nicht** nach: Mikrofon, Kontakte, Kalender, Gesundheitsdaten, Fotos-Mediathek (der Foto-Auswahldialog von Apple läuft ohne Berechtigung).

---

## 6. Wer hat Zugriff auf Ihre Daten

- **Innerhalb der KUE:** Lehrpersonen und Mitarbeitende, die in der App ein Konto haben, sehen Inventar- und Ausleihdaten gemäss ihrer Rolle. Administrative Aktionen (Datenbank zurücksetzen, Schulname ändern usw.) sind nur Administratoren mit Sicherheitscode möglich.
- **Apple:** Hat technischen Zugang zur verschlüsselten Speicherung, kann jedoch die Inhalte nicht einsehen.
- **Open-Meteo:** Erhält anonyme Koordinaten, falls Wetter aktiviert ist.
- **SMTP-Empfänger:** Erhalten die E-Mail-Inhalte gemäss Punkt 4.3.
- **Niemand sonst.** Daten werden nicht verkauft, nicht zu Werbezwecken genutzt und nicht an unbeteiligte Dritte weitergegeben.

---

## 7. Aufbewahrungsdauer

- **Inventardaten** werden so lange gespeichert, wie das Gerät zum Bestand der KUE gehört.
- **Ausleihdaten** bleiben aus organisatorischen und Nachvollziehbarkeitsgründen bestehen, mindestens für die Dauer der Ausleihe und das laufende Schuljahr. Ältere Datensätze können vom Administrator manuell archiviert oder gelöscht werden.
- **Schlüssel-, Badge- und PIN-Daten** werden gespeichert, solange das Asset zum Bestand der KUE gehört. Die **Personenzuordnung** (Name, E-Mail, Unterschrift, Foto) wird nach Rücknahme bzw. Austritt der Person und Ablauf einer festzulegenden Frist (in Abstimmung mit dem kantonalen Datenschutzbeauftragten) **anonymisiert**; die Vorgangshistorie (Datum, Asset, Vorgangstyp) bleibt zu Nachweiszwecken **ohne Personenbezug** erhalten. PIN-/Badge-Codes werden bei Deaktivierung bzw. Austritt rotiert oder entfernt.
- **Aktivitätsprotokolle** werden ohne festgelegte Frist aufbewahrt; sie sind Teil der Datenbank.
- **Backups** werden rollierend gespeichert (jeweils die letzten 5 Tagesbackups).

---

## 8. Ihre Rechte

Nach dem revidierten Schweizer Datenschutzgesetz (revDSG), dem Informations- und Datenschutzgesetz des Kantons Zürich (IDG ZH — die KUE ist als kantonale Schule ein öffentliches Organ) und, sofern anwendbar, der EU-DSGVO haben Sie das Recht:
- Auskunft über die zu Ihrer Person gespeicherten Daten zu erhalten
- Berichtigung unrichtiger Daten zu verlangen
- Löschung Ihrer Daten zu beantragen, sofern keine gesetzlichen Aufbewahrungspflichten dem entgegenstehen
- Der Verarbeitung in begründeten Fällen zu widersprechen
- Eine Datenübertragung in einem üblichen Format zu erhalten

Wenden Sie sich für die Ausübung dieser Rechte an die KUE-Verwaltung oder direkt an den Entwickler (siehe Kontakt unten).

---

## 9. Sicherheitsmassnahmen

- Daten auf dem iPad sind durch die Apple-Geräte­verschlüsselung geschützt (FileProtection bis zur ersten Benutzer-Authentifizierung).
- iCloud-Übertragungen sind TLS-verschlüsselt.
- Mailserver-Verbindungen erfolgen via STARTTLS.
- Administrative Funktionen sind durch einen separaten Sicherheitscode geschützt; nach 5 falschen Eingaben wird der Code-Dialog automatisch geschlossen.
- Passwörter (Mailserver, Admin-Code) sind ausschliesslich im Apple-Keychain hinterlegt und werden nicht in die Cloud oder ins Protokoll geschrieben.
- PIN- und Badge-Codes werden reversibel verschlüsselt (AES-GCM) gespeichert; der Verschlüsselungsschlüssel liegt im Apple-Keychain und wird nie in Backups, Exporte oder die Cloud-Klartextdaten geschrieben. Jede Klartext-Anzeige eines Codes wird im Aktivitätsprotokoll vermerkt.

---

## 10. Änderungen dieser Erklärung

Wir behalten uns vor, diese Datenschutzerklärung anzupassen, wenn sich die App oder rechtliche Anforderungen ändern. Die jeweils aktuelle Fassung finden Sie unter `https://kue-it.github.io/cens-datenschutz/` sowie in der App unter **Einstellungen → Datenschutz**.

---

## 11. Kontakt

Bei Fragen zum Datenschutz oder zur Ausübung Ihrer Rechte:

**Kantonsschule Uetikon am See**
Bergstrasse 113, 8707 Uetikon am See

**Technischer Kontakt:**
Nikola Jovanov
kue.it@kuezh.ch
