# SYSTEM MASTER INDEX

This document is the main architectural map of the LMS Marketplace system.

All AI agents must read this document before making any modification to the planning repository.

---

# System Overview

The system being developed is a **Marketplace Learning Management System (LMS Marketplace)** designed for enterprise training management and public course commercialization.

The platform supports the full lifecycle of training programs, including:

- training needs submission
- training program planning
- course production
- learning delivery
- evaluation and certification
- commercialization of training content for public users

The system is developed as a thesis project prototype (MVP) intended to digitize and streamline the training management processes.

---

# Main Actors

The system involves several main actors:

- Learning Coordinator
- Admin Coordinator
- SME (Subject Matter Expert)
- Learning Administrator
- Internal Participants (Employees)
- Public Participants
- Helpdesk Administrator

---

# Core Modules

## Core Business Modules

### 1. Authentication Module
Handles user authentication, authorization, and session management across all actor types.
Responsibility: Manage login, registration, password recovery, and token-based access control.

### 2. User & Role Management Module
Manages user profiles, role definitions, and role-based access permissions for the entire platform.
Responsibility: Define and enforce actor roles (Learning Coordinator, SME, Admin, Participants, etc.) and their access scopes.

### 3. Training Management Module
Supports the end-to-end lifecycle of enterprise training programs from needs submission to scheduling.
Responsibility: Manage training needs requests, program planning, scheduling, and training program records.

### 4. Course Marketplace Module
Provides a public-facing storefront for browsing, previewing, and purchasing training courses.
Responsibility: Manage course catalog listings, search, filtering, and course detail pages for public and internal users.

### 5. Learning Delivery Module
Handles the delivery of learning content to participants, supporting blended learning formats.
Responsibility: Manage course content presentation, learning sessions, progress tracking, and attendance recording.

### 6. Assignment Management Module
Manages the creation, distribution, submission, and grading of assignments within courses.
Responsibility: Handle assignment lifecycle including creation by SME, submission by participants, and grading workflows.

## Supporting Modules

### 7. Certification Module
Issues and manages certificates upon successful course or program completion.
Responsibility: Generate certificates, validate completion criteria, and maintain certification records.

### 8. Survey & Evaluation Module
Collects feedback and evaluation data from participants and stakeholders after training delivery.
Responsibility: Manage survey creation, distribution, response collection, and evaluation reporting.

### 9. Helpdesk & Ticketing Module
Provides a support channel for users to submit and track issues or inquiries.
Responsibility: Manage ticket creation, assignment, status tracking, and resolution workflows.

## External / Integration Modules

### 10. Payment Module
Processes transactions for public course purchases on the marketplace.
Responsibility: Handle payment processing, transaction records, invoice generation, and payment status tracking.

---

# Module Dependencies

### Authentication Module
depends_on: []
used_by:
- User & Role Management
- Training Management
- Course Marketplace
- Learning Delivery
- Assignment Management
- Certification
- Helpdesk & Ticketing

---

### User & Role Management Module
depends_on:
- Authentication
used_by:
- Training Management
- Course Marketplace
- Learning Delivery
- Assignment Management
- Certification
- Survey & Evaluation
- Helpdesk & Ticketing

---

### Training Management Module
depends_on:
- Authentication
- User & Role Management
used_by:
- Learning Delivery
- Certification
- Survey & Evaluation

---

### Course Marketplace Module
depends_on:
- Authentication
- User & Role Management
used_by:
- Payment
- Learning Delivery

---

### Learning Delivery Module
depends_on:
- Authentication
- User & Role Management
- Training Management
- Course Marketplace
used_by:
- Assignment Management
- Certification
- Survey & Evaluation

---

### Assignment Management Module
depends_on:
- Authentication
- User & Role Management
- Learning Delivery
used_by:
- Certification

---

### Certification Module
depends_on:
- User & Role Management
- Learning Delivery
- Assignment Management
used_by: []

---

### Survey & Evaluation Module
depends_on:
- Learning Delivery
- User & Role Management
used_by: []

---

### Helpdesk & Ticketing Module
depends_on:
- Authentication
- User & Role Management
used_by: []

---

### Payment Module
depends_on:
- Course Marketplace
used_by: []

---

# Notes

This file acts as the main navigation entry for the planning repository.
All modules, features, and subsystems must reference this document.