# On-Campus Placement System

A full‑stack web application to streamline the on‑campus recruitment process, connecting **Students**, **Recruiters**, and **Administrators**.

> Frontend: Angular (TypeScript) • Backend: Spring Boot (Java) • Database: MySQL

---

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Setup & Installation](#setup--installation)
  - [1) Database](#1-database)
  - [2) Backend](#2-backend)
  - [3) Frontend](#3-frontend)
- [Running the App](#running-the-app)
- [Database Schema (Overview)](#database-schema-overview)
- [Stored Procedures & Triggers](#stored-procedures--triggers)
- [Role Workflows](#role-workflows)
  - [Students](#students)
  - [Recruiters](#recruiters)
  - [Administrators](#administrators)
- [Roadmap / Future Work](#roadmap--future-work)
- [Contributors](#contributors)
- [License](#license)

---

## Overview
The **On‑Campus Placement System** centralizes the entire campus recruitment lifecycle. Students build profiles and apply to jobs; recruiters post roles, review applications, and schedule interviews; administrators manage users and on‑campus events like job fairs.

## Features
- Student profiles with skills & certifications
- Job listings with application tracking
- Interview scheduling & status updates
- Role‑based access (Student, Recruiter, Admin)
- Event & sponsor management for campus fairs

## Tech Stack
**Frontend**
- Angular (TypeScript)
- CSS, Angular Material

**Backend**
- Java, Spring Boot
- Maven

**Database**
- MySQL

**Dev Environment**
- VS Code / IntelliJ IDEA
- Node.js (Angular CLI), JDK
- MySQL Workbench

## Architecture
### Backend (Spring Boot)
Layered architecture with:
- **Controllers** (`com.campus.controller`) – REST endpoints
- **Services** (`com.campus.service`) – business logic & transactions
- **Repositories** (`com.campus.repository`) – data access (incl. SP calls)
- **Models/DTOs** (`com.campus.model`) – POJOs & DTOs
- **Security** (`com.campus.security`, `com.campus.config`) – Spring Security (JWT‑style flow), role‑based authorization

### Frontend (Angular)
- **Components** for UI screens (e.g., Login, Student Dashboard, Job List)
- **Services** (API, Auth) & **Interceptors** (attach auth tokens)
- **Routing** with guards (role‑based)
- **Models** mirroring backend DTOs
- Angular Material for UI

## Project Structure
```
BackEnd/
└── On-Campus-placement-backend/          # Spring Boot application
    ├── src/main/java/com/campus/
    │   ├── config/                      # Security configuration
    │   ├── controller/                  # API endpoints
    │   ├── model/                       # POJOs & DTOs
    │   ├── repository/                  # Data access interfaces
    │   ├── security/                    # Security implementation
    │   └── service/                     # Business logic
    ├── src/main/resources/
    │   ├── application.properties       # Spring Boot config
    │   ├── schema.sql                   # DB table definitions
    │   └── procedures.sql               # DB stored procedures
    └── pom.xml                          # Maven config

FrontEnd/
└── On-Campus-placement-frontend/         # Angular application
    ├── src/
    │   ├── app/
    │   │   ├── components/              # Login, Dashboard, etc.
    │   │   ├── guards/                  # Route guards
    │   │   ├── interceptors/            # HTTP interceptors (Auth)
    │   │   ├── models/                  # Frontend models
    │   │   └── services/                # API & Auth services
    │   ├── assets/                      # Static assets
    │   ├── index.html                   # Main HTML
    │   └── main.ts                      # Entry point
    ├── angular.json                     # Angular CLI config
    └── package.json                     # NPM scripts & deps
```

## Prerequisites
- Node.js & npm (or yarn)
- Java JDK (compatible with Spring Boot)
- Apache Maven
- MySQL Server
- Git (optional)

## Setup & Installation

### 1) Database
1. Ensure **MySQL Server** is running.
2. Create a database, e.g. `campus_placement_db`.
3. In a SQL client (e.g., MySQL Workbench), run:
   - `schema.sql` to create tables, relationships, and triggers.
   - Optional: load the provided dump file for initial data.
4. Configure DB credentials in backend `src/main/resources/application.properties`:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/campus_placement_db
   spring.datasource.username=YOUR_USER
   spring.datasource.password=YOUR_PASSWORD
   ```

### 2) Backend
```bash
cd BackEnd/On-Campus-placement-backend
mvn clean install
mvn spring-boot:run
# Server starts on http://localhost:8080
```

### 3) Frontend
```bash
cd FrontEnd/On-Campus-placement-frontend
npm install
npm start        # or: ng serve
# App runs on http://localhost:4200
```

## Running the App
Start **backend** (8080) and **frontend** (4200), then open `http://localhost:4200`. Register or log in based on your role to begin.

## Database Schema (Overview)
Key entities:
- `user` (base user, role) ↔ 1:1 ↔ `student` / `recruiter` / `admin`
- `company`, `college`
- `joblisting` (many‑to‑one → company)
- `application` (joins student ↔ joblisting)
- `interview` (many‑to‑one → application, recruiter)
- Lookup: `skill`, `certification`
- Many‑to‑many joins: `studentskill`, `studentcertification` (with attributes like `certificationDate`), `eventcompany`, `eventsponsor`
- `event`, `sponsor` for fairs/workshops

## Stored Procedures & Triggers
**Stored Procedures (examples):**
- `sp_get_student_skills(p_student_id)`
- `sp_find_or_create_skill_and_link_student(...)`
- `sp_remove_student_skill(p_student_id, p_skill_id)`
- `sp_get_student_certifications(p_student_id)`
- `sp_add_student_certification(...)`
- `sp_remove_student_certification(...)`
- `sp_get_students_by_ids(p_student_ids)`
- `sp_get_application_by_job_and_student(p_job_id, p_student_id)`

**Trigger**
- `update_student_application_count` (AFTER INSERT on `application`) increments `student.totalApplicationsCount`

## Role Workflows

### Students
- Register & log in
- Build profile: add **skills** and **certifications**
- Browse **job listings**, apply, and track status
- View **interview** schedules & results

### Recruiters
- Register & log in (company‑linked)
- Create/manage **job listings**
- Review **applications** & applicant profiles
- **Schedule interviews**, record feedback & results

### Administrators
- Log in with admin role
- Manage **users**, **events**, **companies**, **sponsors**
- Optional: reports/dashboards for placement statistics

## Roadmap / Future Work
- Advanced search & filtering (skills, GPA, salary, location)
- In‑app/email notifications
- Admin analytics dashboards (placement rates, funnel analytics)
- Resume parsing to auto‑populate profiles
- Direct messaging (recruiter ↔ student)
- Event enhancements (session scheduling, feedback)
- DB optimization (indexes, SP refinements)
- API docs (Swagger/OpenAPI)
- UI/UX polish and bulk actions

## Contributors
- Dinesh Bachchani
- Saravjit Singh
- Ratan Pyla

