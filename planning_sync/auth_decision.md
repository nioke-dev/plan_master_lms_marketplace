# Authentication Decision (MVP)

Decision Date: 2026-04-01

This document records architectural decisions for the authentication system based on the sync report analysis.

---

## Accepted Differences

The following differences between the planning specification and the Laravel implementation are **accepted for MVP**:

- **Session-based authentication instead of JWT.** The application uses Laravel Fortify with server-rendered Livewire views. JWT adds unnecessary complexity for this architecture. Session-based auth is the standard and recommended approach for monolithic Laravel applications.

- **String `role` column instead of a separate `roles` table.** The system has a fixed, known set of roles (admin, coordinator, sme, employee, public). A simple string column is sufficient and avoids over-engineering. A separate table can be introduced later if dynamic role management is needed.

- **Fortify Actions + Livewire instead of custom Controllers.** Laravel Fortify already provides AuthController/RegisterController/PasswordResetController behavior through its Actions pattern. Creating additional controllers would duplicate logic. The Fortify pattern is the standard for the Livewire starter kit.

- **Laravel default `password_reset_tokens` instead of a custom PasswordResetToken model.** Laravel's built-in password reset system (using `created_at` + config-based expiration) handles token expiration and single-use behavior correctly. A custom model is unnecessary.

- **2FA and Email Verification exist in implementation but not in planning.** These are bonus features provided by Fortify. They add security value and will be kept. The planning spec should be updated to reflect their existence.

---

## Deferred Features

The following features are **postponed beyond MVP**:

- **JWT / Token-based API authentication.** Only needed if a mobile app or external API consumer is introduced later.

- **Separate `roles` table with Role Eloquent model.** Only needed if roles become dynamic (admin-created, permission-based). For MVP, the 5 fixed roles are sufficient.

- **Role selection at registration form.** For MVP, all new users register as `public`. Role assignment is handled by an admin. Self-role-selection introduces complexity and potential abuse.

- **Explicit login lockout / temporary ban.** Fortify's rate limiting (5 attempts/minute) provides adequate protection for MVP. A dedicated lockout system with ban duration and unlock flow is deferred.

- **Custom PasswordResetToken model with `expired_at` field.** Laravel's config-based token expiration works correctly. A custom model adds no MVP value.

---

## Immediate Fixes

The following items **must be addressed now** to ensure system consistency:

- **Implement role-based redirect after login.** Currently all users are redirected to `/dashboard`. The system should redirect users to role-appropriate dashboards (e.g., admin → admin dashboard, public → marketplace). This is a core UX requirement.

- **Update planning spec to include 2FA and Email Verification.** These features exist in the implementation but are not documented in the planning module. The planning spec must stay synchronized.

- **Record these decisions in DECISION_LOG.md.** All accepted differences and deferred features must be logged as formal architectural decisions in the history.

---

## Final Decision Summary

For MVP, the LMS Marketplace authentication system uses:

- **Laravel Fortify** for authentication logic (login, register, password reset)
- **Livewire** for authentication views
- **Session-based authentication** (not JWT)
- **String `role` column** on the users table with 5 fixed roles: `admin`, `coordinator`, `sme`, `employee`, `public`
- **RoleMiddleware** for route-level access control
- **Default role = `public`** on registration, with admin-assigned role changes
- **2FA** (TOTP + recovery codes) via Fortify
- **Email verification** via Fortify
- **Rate limiting** on login (5 attempts/minute per email+IP)

This architecture is simple, follows Laravel conventions, and is sufficient for an MVP thesis prototype.
