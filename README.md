# tuuam-project

TUUAM: The Ultimate Urban Art Manager. A Spring Boot-based MLOps orchestrator for graffiti analysis and LoRA training, powered by the INGRID ontology and Spring AI

# TUUAM – The Ultimate Urban Art Manager 🎨🤖

**TUUAM** ist ein professioneller MLOps-Orchestrator für die wissenschaftliche Analyse von Urban Art und das automatisierte Training von LoRA-Modellen.

## 🚀 Die Mission

Anstatt unstrukturierte Bilddaten zu verwenden, nutzt TUUAM die **INGRID-Ontologie** (Informationssystem Graffiti in Deutschland) der Universität Paderborn als taxonomisches Fundament. Das Ziel ist die Transformation von rohen Urban-Art-Aufnahmen in hochqualitative, wissenschaftlich fundierte Datensätze für generative KI.

## 🛠 Tech Stack

- **Orchestrator:** Java 21 & Spring Boot 3.x
- **AI Engine:** Spring AI (ChatClient & Prompt Templates)
- **Taxonomie:** INGRID-Datenbank Integration
- **Datenbank:** PostgreSQL mit Spring Data JPA
- **Frontend:** React (Vite) mit Tailwind CSS
- **State Management:** Zustand
- **Training Interface:** Externer Python-Microservice / Cloud-GPU API

## 🔄 Workflow Pipeline

1.  **Ingestion:** Upload von Urban Art und Mapping auf INGRID-Kategorien (Style, Surface, Tool).
2.  **AI-Augmentation:** Spring AI übersetzt trockene Metadaten mithilfe des _Persona Patterns_ in lebendige, präzise englische Captions.
3.  **Curation:** Persistenz der optimierten Bild-Text-Paare in der PostgreSQL-Datenbank.
4.  **Orchestration:** Triggering des externen GPU-Trainings direkt aus der Spring Boot Zentrale.

## 🏗 Architektur-Prinzipien

- **Spring Boot als Commander:** Java verwaltet die Geschäftslogik, Validierung und den Workflow.
- **Strict Mapping:** Jeder Datensatz folgt der INGRID-Klassifizierung für maximale Datenqualität.
- **Separation of Concerns:** Klare Trennung zwischen UI (React), Orchestrierung (Spring) und Heavy-Compute (Python/GPU).

### 📂 Project Structure

```text
tuuam-project/
├── tuuam-backend/          # Spring Boot Orchestrator (Java 21)
│   ├── src/main/java/com/tuuam/
│   │   ├── controller/     # REST API Layer
│   │   ├── service/        # Business Logic & Spring AI Integration
│   │   ├── repository/     # Database Access (Spring Data JPA)
│   │   ├── model/          # Domain Entities (INGRID Ontology)
│   │   └── dto/            # Data Transfer Objects
├── tuuam-frontend/         # React Web Interface (Vite, Tailwind, Zustand)
├── docker/                 # Persistent database storage (ignored by git)
└── docker-compose.yml      # Infrastructure (PostgreSQL & pgAdmin)
```

---

## 🚦 Quick Start (Docker Environment)

Dieses Projekt nutzt eine vollständig containerisierte MLOps-Infrastruktur (PostgreSQL, pgAdmin, Spring Boot).

### 1. Setup (Einmalig)

Da das Projekt eine Microservice-Architektur vorbereitet, nutzen wir ein geteiltes, externes Docker-Netzwerk. Dieses muss **einmalig** auf dem Host-System erstellt werden:

```bash
# Hinweis für Linux-User: Ggf. mit 'sudo' ausführen, falls der User nicht in der docker-Gruppe ist.
docker network create tuuam-net
```

### 2. Build & Start

Führe diesen Befehl im Root-Verzeichnis des Repositories aus, um das Java-Backend zu bauen und alle Services (Datenbank, UI, Backend) im Hintergrund zu starten:

```bash
# Hinweis für Linux-User: Ggf. mit 'sudo' ausführen, falls der User nicht in der docker-Gruppe ist.
docker compose -f docker-compose.yml -f tuuam-backend/compose.yaml up --build -d
```

### 3. Services im Überblick

Nach dem Start sind folgende Services erreichbar:

- **Spring Boot API:** `http://localhost:8080`
- **pgAdmin (Database UI):** `http://localhost:5050`
  - _Login:_ `admin@tuuam.local`
  - _Password:_ `admin`
- **PostgreSQL:** Port `5432`

```bash
# Backend-Logs in Echtzeit verfolgen:
docker logs -f tuuam-backend

# Alle Container stoppen:
docker compose -f docker-compose.yml -f tuuam-backend/compose.yaml down
```
