# Fragen – Konfiguration und Umgebungsvariablen (DL11)

Name: <Rostamian> <Atieh>
Klasse: <S-INA24aL>

---

## 1. Konfiguration

Welche Werte waren ursprünglich hardcoded in `compose.yml` und `app/main.py`?

Antwort:
In der compose.yml standen die Datenbank-Zugangsdaten wie POSTGRES_USER, POSTGRES_PASSWORD (z.B. "secret") und POSTGRES_DB im Klartext. In der app/main.py war die komplette DATABASE_URL (inklusive Passwort) direkt als String fest einprogrammiert.

Warum ist es ein Problem, Passwörter direkt in `compose.yml` einzutragen?

Antwort:
Weil die compose.yml ins Git eingecheckt wird. Da der Git-Verlauf permanent ist , wäre das Passwort einmal hochgeladen für immer und für alle sichtbar, auch wenn man es später wieder aus der Datei löscht. Das ist ein riesiges Sicherheitsrisiko.

Was ist der Unterschied zwischen `.env` und `.env.example`?

Antwort:
Die .env-Datei enthält die echten, sensiblen Werte (echte Passwörter) und darf nie ins Git hochgeladen werden. Die .env.example ist dagegen nur ein Template, das nur Schlüsselnamen oder Dummy-Werte enthält. Sie wird ins Git eingecheckt, damit andere Entwickler sehen, welche Variablen sie lokal anlegen müssen.

Warum muss `.env` in `.gitignore` eingetragen sein?

Antwort:
Die .gitignore schützt vor versehentlichem Commit sensibler Dateien. Das ist unsere Schutzschicht, damit die echten Passwörter aus der .env lokal bleiben und nicht ins GitHub-Repo gepusht werden.

## 2. Variablen in Compose

Wie referenziert man eine Variable aus `.env` in `compose.yml`?

Antwort:
Man nutzt dafür das Dollarzeichen und geschweifte Klammern, also zum Beispiel ${VARIABLE_NAME}. Docker Compose liest die Werte dann beim Starten automatisch aus der .env-Datei aus.

Was passiert, wenn eine Variable in `.env` fehlt, aber in `compose.yml` verwendet wird?

Antwort:
Dann setzt Docker Compose meistens einfach einen leeren String ein (außer man hat einen Fallback definiert). Das führt dann oft zu Fehlern beim Starten, weil z.B. das Datenbank-Passwort fehlt und die Authentifizierung fehlschlägt. Man kann das gut überprüfen, wenn Variablen möglicherweise fehlen.

Was zeigt der Befehl `docker compose config`? Wann ist er nützlich?

Antwort:
Der Befehl gibt die vollständig aufgelöste Compose-Konfiguration aus. Er zeigt uns, ob alle ${VARIABLE}-Platzhalter durch echte Werte ersetzt wurden. Das ist echt praktisch und ideal zur Fehlersuche vor dem Start , besonders wenn man an der .env was geändert hat.

## 3. Dockerfile und Build

Warum wird `requirements.txt` in einem eigenen `COPY`-Schritt vor dem App-Code kopiert?

Antwort:
Das ist wegen des Docker Layer-Caches extrem wichtig für schnellere Builds im Alltag. Die requirements.txt ändert sich eher selten , der App-Code aber ständig. Wenn wir die Requirements zuerst kopieren und installieren, kann Docker diesen Layer cachen und muss nicht jedes Mal alle Packages neu herunterladen, wenn wir nur eine Zeile im Python-Code ändern.

Was bewirkt `.dockerignore`? Welche Dateien sollten darin stehen?

Antwort:
Sie definiert Dateien, die beim docker build NICHT in den Build-Kontext (ins Image) übertragen werden. Da sollten unbedingt sensible Dateien wie .env rein , aber auch Git-Metadaten (.git) und gecachte Sachen wie __pycache__ oder .venv.

## 4. Systemtest

Funktioniert `/db-check` nach Ihrer Konfigurationsanpassung?

Antwort:
Ja, nach ein paar anfänglichen Problemen läuft es super. Zuerst hatte ich einen Authentifizierungsfehler, aber nachdem ich das alte Volume mit docker compose down -v gelöscht und neu gebuildet habe, hat alles geklappt!

Was zeigt der Endpunkt `/db-check` an, wenn die Verbindung funktioniert?

Antwort:
Er liefert im Browser das JSON {"db": "connected"} zurück.

## 5. Reflexion

Was war der wichtigste Schritt in dieser Woche?

Antwort:
Für mich war es das "Refactoring" der Passwörter – also zu sehen, wie die .env, compose.yml und os.getenv() in Python zusammenarbeiten. Es ist wichtig zu verstehen, wie man Secrets professionell und sicher managt.

Was ist noch unklar oder möchten Sie besser verstehen?

Antwort:
Aktuell ist das Prinzip gut verständlich. Mich würde für die Zukunft interessieren, wie man in großen Projekten in der Produktion Passwörter verwaltet, wenn man keine lokalen Dateien nutzen möchte.