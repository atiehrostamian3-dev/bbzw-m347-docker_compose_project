# Architektur TicketBoard

## Komponenten
- **Frontend**: Stellt die Benutzeroberfläche dar (Nginx auf Port 3000).
- **API**: FastAPI (Backend) zur Bearbeitung von Anfragen auf Port 8000.
- **PostgreSQL**: Datenbank zur dauerhaften Datenspeicherung.
- **Adminer**: Datenbankverwaltungstool auf Port 8080.
- **Volume**: Gewährleistet die Persistenz der Datenbankdaten (postgres_data).

## Kommunikation
- Browser → Frontend
- Frontend → API
- API → Datenbank
- Adminer → Datenbank

## Architekturdiagramm
```mermaid
graph TD
    user[Browser]
    subgraph system [Docker Compose System]
        frontend[Frontend Container]
        api[API Container]
        adminer[Adminer]
        db[(PostgreSQL)]
        volume[(postgres_data Volume)]
    end
    user -->|http :3000| frontend
    user -->|http :8080| adminer
    frontend -->|http :8000| api
    api -->|DB :5432| db
    adminer -->|DB :5432| db
    db --> volume