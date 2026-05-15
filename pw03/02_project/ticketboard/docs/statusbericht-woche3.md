# Statusbericht – Woche 3 (DL11)

Name: <Rostamian> <Atieh>
Klasse: <S-INA24aL>

---

## Umgesetzte Arbeiten
Hardcoded Secrets entfernt: Ich habe alle Passwörter und Verbindungsdaten aus der compose.yml und der app/main.py gelöscht.   Umgebungsvariablen eingeführt: Eine .env-Datei wurde erstellt, um die Konfiguration (DB-User, Passwort, Host) sicher zu speichern.   Docker Compose Refactoring: Die compose.yml nutzt jetzt ${VAR}-Substitution, um die Werte automatisch aus der .env zu ziehen.   Python-Code angepasst: Die Datenbank-URL in FastAPI wird jetzt über os.getenv() geladen, statt als String im Code zu stehen.   Git-Sicherheit: Eine .gitignore-Datei wurde hinzugefügt, damit die sensiblen .env-Daten nicht versehentlich im Repo landen.   

---

## Aktueller Stand
Das System ist komplett lauffähig und kann mit einem einfachen docker compose up --build gestartet werden.   
Die Verbindung zur Datenbank steht: Der Endpunkt /db-check liefert erfolgreich {"db": "connected"} zurück.


---

## Offene Probleme / Herausforderungen
Auth-Error bei der DB: Am Anfang gab es Probleme mit der Passwort-Authentifizierung. Ich musste erst den alten Docker-Volume mit docker compose down -v löschen, damit die neuen Anmeldedaten aus der .env übernommen werden. Danach hat alles super funktioniert.

---

## Nächste Schritte
Beantwortung der theoretischen Fragen in der Datei questions.md.   
Finaler Push der Änderungen ins GitHub-Repository und Abgabe des Links über das Formular.

---

## Selbsteinschätzung (optional)
Ich fühle mich jetzt viel sicherer im Umgang mit Umgebungsvariablen. Besonders der Befehl docker compose config war sehr hilfreich, um zu prüfen, ob alle Variablen richtig aufgelöst werden.
