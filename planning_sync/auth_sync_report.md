# Authentication Sync Report

Report Date: 2026-03-31

This report compares the authentication module planning specification
(`planning/modules/module_authentication.md`) against the current Laravel implementation.

---

## Matching

The following items are implemented and aligned with the planning specification:

- **Register**: User registration is implemented via `CreateNewUser` Fortify action. Users provide name, email, and password. Input is validated (profile rules + password rules).
- **Login**: Login is handled by Fortify with email/password authentication. Login view is configured.
- **Logout**: Logout is implemented via `App\Livewire\Actions\Logout`. Session is invalidated and token regenerated.
- **Password Reset**: Password reset is implemented via `ResetUserPassword` Fortify action. Forgot password and reset password views are configured.
- **Email Uniqueness**: The `users` table enforces unique email via migration.
- **Password Hashing**: Password is cast as `hashed` in the User model (bcrypt by default).
- **Rate Limiting**: Login rate limiting is configured (5 attempts per minute per email+IP).
- **Session Handling**: Session-based authentication is in place (sessions table exists).
- **Role Column**: Role column has been added to users table (string, default: `public`).
- **Role Helpers**: User model includes `isAdmin()`, `isCoordinator()`, `isSME()`, `isEmployee()`, `isPublic()` methods.
- **Role Middleware**: `RoleMiddleware` is created and registered as `role` alias.
- **Default Role**: New users are assigned `role = 'public'` on registration.
- **Route Protection**: Dashboard requires `auth + verified`. Admin routes require `role:admin`.

---

## Missing

The following items from the planning specification are NOT yet implemented:

- **Role Entity (Separate Table)**: Planning defines a `Role` entity (id, role_name, description) as a separate table. Current implementation uses a simple string column on the users table instead.
- **PasswordResetToken Model**: Planning defines a `PasswordResetToken` Eloquent model. Laravel uses its built-in `password_reset_tokens` table without a dedicated model.
- **Role Selection at Registration**: Planning specifies users select a role during registration. Current implementation hardcodes `public` as default without a role selection form field.
- **Role-Based Redirect After Login**: Planning specifies system redirects to appropriate dashboard based on role. Current Fortify config redirects all users to `/dashboard`.
- **Login Lockout (Temporary Ban)**: Planning specifies temporary lockout after multiple failed attempts. Fortify throttles but does not implement an explicit lockout/ban mechanism.
- **Single-Use Password Reset Token**: Planning specifies reset tokens can only be used once. Laravel handles this by default (token is deleted after use), but there is no explicit enforcement documented.
- **Token Expiration Field**: Planning defines `expired_at` on the PasswordResetToken entity. Laravel uses `created_at` and a config-based expiration window instead.

---

## Differences

| Aspect | Planning Specification | Laravel Implementation |
|--------|----------------------|----------------------|
| Auth approach | Token-based (JWT) | Session-based (Fortify) |
| Role storage | Separate `roles` table with `role_id` FK | String column `role` on users table |
| Password reset token | Custom model with `expired_at` | Laravel default `password_reset_tokens` with `created_at` |
| Controllers | AuthController, RegisterController, PasswordResetController | Fortify Actions (CreateNewUser, ResetUserPassword) + Livewire |
| 2FA | Not mentioned in planning | Implemented via Fortify (TOTP + recovery codes) |
| Email verification | Not mentioned in planning | Implemented via Fortify |

---

## Recommendation

1. **Keep session-based auth for MVP.** JWT is more complex and unnecessary for a server-rendered Livewire application. Document this deviation in DECISION_LOG.md.
2. **String role column is acceptable for MVP.** If the system grows to need dynamic roles, a separate `roles` table can be introduced later.
3. **Add role selection to registration form** if the system needs users to self-assign roles (e.g., employee vs public). Otherwise, keep admin-assigned roles.
4. **Implement role-based redirect** in `FortifyServiceProvider` using `Fortify::authenticateUsing()` or a custom `LoginResponse`.
5. **Document the 2FA capability** in the planning spec since it already exists in the implementation.
6. **Document email verification** in the planning spec since it already exists in the implementation.
