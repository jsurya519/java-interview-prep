# Hotel Booking System — Requirements Document

## 1. Project Overview

A backend REST API for a hotel booking platform built with Java, Spring Boot, Spring Security (JWT), and MySQL. The system supports two roles — Admin (manages hotels/rooms) and Customer (searches and books rooms) — with real booking logic: date-range availability checks, conflict prevention, and status-based booking lifecycle management.

**Goal:** Demonstrate solid backend fundamentals (layered architecture, security, validation, querying) plus one or two areas of depth (booking conflict logic, Email to customer)

**Timeline:** 3–4 weeks, structured in 4 phases.

---

## 2. Actors / Roles

| Role | Capabilities |
|---|---|
| **ADMIN** | Manage hotels, manage rooms, view all bookings, confirm/cancel any booking |
| **CUSTOMER** | Register/login, search hotels & rooms, create/cancel own bookings, view own booking history, leave a review after stay |

---

## 3. Functional Requirements by Phase

### Phase 1 — Foundation (Week 1)
Goal: working skeleton with auth and core CRUD.

- User registration & login (email + password)
- Password hashing (BCrypt)
- JWT-based authentication; role-based authorization (ADMIN vs CUSTOMER) on endpoints
- Admin CRUD: Hotels (name, city, address, description, rating)
- Admin CRUD: Rooms (linked to a hotel — room type, price per night, capacity, total count)
- Customer: read-only access to hotel & room listings
- Entity relationships set up (Hotel 1—N Rooms, User 1—N Bookings)
- Layered structure in place: Controller → Service → Repository, with DTOs (no entities exposed directly in API responses)

### Phase 2 — Core Booking Logic (Week 2)
Goal: the part of the project that's actually worth talking about in interviews.

- Customer can search room availability for a given hotel/city + check-in date + check-out date
- Booking creation: validate that the room is actually free for the requested date range (no overlap with existing CONFIRMED/PENDING bookings on the same room)
- Booking status lifecycle: `PENDING → CONFIRMED → CANCELLED` (keep it simple — auto-confirm on creation if available)
- Customer can view their own booking history
- Customer can cancel their own booking (add a simple rule: e.g., cannot cancel within 24 hours of check-in — gives you a small business rule to explain)
- Admin can view all bookings and cancel any booking
- Email customer and admin regarding the bookings and changes

### Phase 3 — Search, Validation & API Polish (Week 3)
Goal: make the API production-shaped, not just functional.

- Request validation using Bean Validation (`@Valid`) on all write endpoints
- Pagination + sorting on hotel and room listing endpoints
- Filtering: rooms by city, price range, capacity, and date availability
- Swagger / OpenAPI documentation for all endpoints
- Simple review/rating: customer can leave a 1–5 rating + comment on a hotel after a completed stay
-  Custom Exception-Handling

### Phase 4 — Containerization & Deployment (Week 4)
Goal: polish + deployability, which is cheap to add and high resume value.

- Dockerfile for the app + `docker-compose.yml` (app + MySQL)
- *(Stretch goal)* Deploy to a free tier (Render/Railway) for a live demo link
- README with setup instructions, architecture overview, and a Postman collection or `.http` file for testing endpoints

---

## 4. Non-Functional Requirements

- Clear layered architecture (Controller / Service / Repository / DTO / Entity)
- Centralized exception handling, no raw stack traces in API responses
- Passwords never stored in plain text
- Indexed DB columns on frequently queried fields (city, dates)
- Consistent API response structure (success and error)

---

## 5. Tech Stack Summary

- **Language/Framework:** Java, Spring Boot, Spring Security, Spring Data JPA
- **Database:** MySQL
- **Auth:** JWT
- **Docs:** Swagger/OpenAPI
- **Containerization:** Docker, docker-compose
- **Deployment (stretch):** Render/Railway free tier

---