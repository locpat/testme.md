# TESTME: Authentication

Tests for user authentication, registration, and password management.

## Environment

| Variable | Default | Description |
|----------|---------|-------------|
| BASE_URL | http://localhost:3000 | Application URL |
| ADMIN_EMAIL | admin@example.com | Admin account |
| ADMIN_PASSWORD | adminpass123 | Admin password |

## Tests

### Login

### Test: Successful login with email and password

1. Go to `/login`
2. Enter "user@example.com" in the email field
3. Enter "password123" in the password field
4. Click the "Sign In" button

**Expect:**
- URL changes to `/dashboard`
- Welcome message is visible
- User avatar appears in header

---

### Test: Login fails with wrong password

1. Go to `/login`
2. Enter "user@example.com" in the email field
3. Enter "wrongpassword" in the password field
4. Click the "Sign In" button

**Expect:**
- URL remains `/login`
- Error message "Invalid email or password" is displayed
- Password field is cleared

---

### Test: Login fails with non-existent email

1. Go to `/login`
2. Enter "nobody@example.com" in the email field
3. Enter "anypassword" in the password field
4. Click "Sign In"

**Expect:**
- Error message appears (same as wrong password for security)
- No indication whether email exists

---

### Test: Login form validates email format

1. Go to `/login`
2. Enter "notanemail" in the email field
3. Click outside the field or submit

**Expect:** Validation error "Please enter a valid email address"

---

### Registration

### Test: Successful user registration

1. Go to `/register`
2. Fill the form:
   - First Name: "Test"
   - Last Name: "User"
   - Email: (unique email, e.g., `test-{timestamp}@example.com`)
   - Password: "SecurePass123!"
   - Confirm Password: "SecurePass123!"
3. Check "I agree to terms" checkbox
4. Click "Create Account"

**Expect:**
- URL changes to `/verify-email` or `/dashboard`
- Success message displayed
- If email verification required, message indicates email sent

---

### Test: Registration fails with existing email

1. Go to `/register`
2. Fill form with email "admin@example.com" (existing user)
3. Complete remaining required fields
4. Click "Create Account"

**Expect:**
- Error message "Email already registered" or similar
- Form remains on registration page

---

### Test: Registration validates password strength

1. Go to `/register`
2. Fill in valid name and email
3. Enter "weak" as password
4. Click "Create Account"

**Expect:**
- Validation error about password requirements
- Should mention: minimum length, uppercase, lowercase, number, or special character as required

---

### Test: Registration requires matching passwords

1. Go to `/register`
2. Enter "SecurePass123!" in password field
3. Enter "DifferentPass456!" in confirm password field
4. Submit the form

**Expect:** Error "Passwords do not match"

---

### Password Reset

### Test: Request password reset sends email

1. Go to `/login`
2. Click "Forgot password?" link
3. Enter "user@example.com" in the email field
4. Click "Send Reset Link"

**Expect:**
- Success message appears (even if email doesn't exist, for security)
- Message indicates "If this email exists, you will receive a reset link"

---

### Test: Password reset with valid token

> **Note:** This test requires a valid reset token. Generate one via the forgot password flow first.

1. Go to `/reset-password?token={valid-token}`
2. Enter "NewSecurePass456!" in new password field
3. Enter "NewSecurePass456!" in confirm password field
4. Click "Reset Password"

**Expect:**
- Success message "Password updated successfully"
- Redirect to `/login`
- Can log in with new password

---

### Test: Password reset rejects invalid token

1. Go to `/reset-password?token=invalid-or-expired-token`

**Expect:**
- Error message "Invalid or expired reset link"
- Link to request new reset

---

### Session Management

### Test: Session persists across page reload

1. Log in with valid credentials
2. Refresh the page (F5 or browser refresh)

**Expect:**
- User remains logged in
- Dashboard or current page reloads normally

---

### Test: Session expires after timeout

> **Note:** This test requires waiting for session timeout or manually advancing time.

1. Log in with valid credentials
2. Wait for session timeout period (or simulate)
3. Try to access `/dashboard`

**Expect:** Redirect to `/login` with message "Session expired"

---

### Test: Logout invalidates session

1. Log in with valid credentials
2. Note any session cookies
3. Click "Sign Out"
4. Try to access `/dashboard` directly

**Expect:**
- Redirect to `/login`
- Cannot access protected routes
- Session cookie cleared

---

### Protected Routes

### Test: Protected routes redirect when not authenticated

The following routes should redirect to `/login` when accessed without authentication:

- `/dashboard`
- `/settings`
- `/profile`
- `/orders`

For each URL: navigate directly without being logged in.

**Expect:**
- URL redirects to `/login`
- After login, redirects back to originally requested page

---

### Test: Admin routes require admin role

1. Log in as regular user (not admin)
2. Try to navigate to `/admin`

**Expect:**
- Access denied message or redirect
- Cannot view admin dashboard

---

### Test: Admin can access admin routes

1. Log in with admin credentials (from `ADMIN_EMAIL` and `ADMIN_PASSWORD`)
2. Navigate to `/admin`

**Expect:**
- Admin dashboard loads successfully
- Admin navigation options visible
