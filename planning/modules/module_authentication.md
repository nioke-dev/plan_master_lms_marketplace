# Authentication Module

## Overview

The Authentication Module handles user identity verification, session management, and access control across the LMS Marketplace platform. It serves as the foundational security layer that all other modules depend on.

---

## Actors

- **Public User** — Registers and logs in to access the public course marketplace.
- **Employee** — Logs in to access internal training programs and learning activities.
- **Admin** — Logs in to manage system configuration, users, and platform operations.

---

## Features

- Register (new account creation)
- Login (credential-based authentication)
- Logout (session termination)
- Password Reset (self-service recovery)

---

## Functional Requirements

- User can register a new account with required profile information.
- User can login using valid credentials (email and password).
- System validates credentials against stored records.
- System creates a session or token upon successful authentication.
- User can logout, which invalidates the active session/token.
- User can request a password reset via registered email.
- System enforces role-based redirection after login based on user type.

---

## Non-Functional Notes

- **Security**: Passwords must be hashed before storage (e.g., bcrypt).
- **Session Handling**: Token-based session management (e.g., JWT) with expiration policy.
- **Access Control**: All protected routes must verify authentication status before granting access.
- **Rate Limiting**: Login attempts should be rate-limited to prevent brute-force attacks.

---

## Authentication Flow

### 1. User Registration Flow
- User opens the registration page.
- User fills in the registration form (name, email, password, role selection).
- System validates input (required fields, email format, password strength).
- System stores user data with hashed password.
- System redirects user to the login page.

### 2. Login Flow
- User enters credentials (email and password).
- System validates credentials against stored records.
- System creates a session/token upon successful validation.
- System redirects user to the appropriate dashboard based on role.

### 3. Logout Flow
- User clicks the logout button.
- System destroys the active session/token.
- User is redirected to the login page.

### 4. Password Reset Flow
- User requests a password reset by providing their registered email.
- System sends a password reset link to the user's email.
- User opens the reset link and sets a new password.
- System validates and updates the password.

---

## Data Entities

### 1. User
- id
- name
- email
- password
- role_id
- created_at

---

### 2. Role
- id
- role_name
- description

---

### 3. Password Reset Token
- id
- user_id
- token
- expired_at

---

## Dependencies

depends_on: []
used_by:
- User & Role Management
- Training Management
- Course Marketplace
- Learning Delivery
- Assignment Management
- Certification
- Helpdesk & Ticketing
