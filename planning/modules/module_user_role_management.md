# User & Role Management Module

## Overview
This module manages user profiles, role definitions, and access permissions for the entire platform. Since the system uses a fixed role strategy with a straightforward string column mapping, this module currently handles the creation, listing, updating, and removal of user accounts while ensuring robust role-based constraints.

## Actors
- **Learning Administrator**: The primary actor who has full operational access to manage other user accounts and assign roles.
- **Other Users**: Can theoretically exist but do not have access to this module.

## Features
- **List Users**: Fetch and display a directory of all registered users in the system.
- **Create User**: Register a new user account and directly assign a system role.
- **Edit/Update User**: Modify an existing user's details and role assignment.
- **Delete User**: Remove user accounts from the system.

## Functional Requirements
- System must ensure that `email` remains unique across all users during creation and updates.
- System must ensure only authenticated `learning_administrator` users can access the user management interface.
- System must provide immediate visual feedback (flash messages) for successful or failed operations.

## Non-Functional Notes
- Role assignment relies on a fixed, predefined list of roles stored as strings (no separate DB tables for roles).

## Business Rules
- **Self-Delete Prevention**: A Learning Administrator cannot delete their own authenticated account to prevent lockouts.
- **Role Constriction**: Admins can only assign roles that are natively defined in the system (`learning_administrator`, `learning_coordinator`, `admin_coordinator`, `sme`, `employee`, `public`, `helpdesk_admin`).
- **Authorization Enclosure**: The entirety of user management operations is firmly restricted to the `learning_administrator` middleware layer.

## Laravel Implementation Mapping

### Models
- `User` (Eloquent Model with role checking helpers like `isLearningAdministrator()`)

### Controllers
- `UserManagementController` (Handles standard Resource actions: `index`, `create`, `store`, `edit`, `update`, `destroy`)

### Middleware
- `RoleMiddleware` (Enforces user role matches against allowed values, particularly `role:learning_administrator` for these routes).

### Routes
- `/learning-admin/users` (GET, POST)
- `/learning-admin/users/create` (GET)
- `/learning-admin/users/{user}` (PUT, DELETE)
- `/learning-admin/users/{user}/edit` (GET)

## Dependencies
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
