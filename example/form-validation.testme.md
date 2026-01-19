# TESTME: Form Validation

Tests for client-side and server-side form validation patterns.

## Tests

### Required Field Validation

### Test: Empty required fields show error

1. Navigate to `/contact`
2. Leave all fields empty
3. Click "Submit"

**Expect:**
- Form does not submit
- Error message appears for each required field
- First invalid field receives focus

---

### Test: Required field error clears on input

1. Navigate to `/contact`
2. Click "Submit" to trigger validation errors
3. Start typing in the Name field

**Expect:**
- Error message for Name field disappears
- Other error messages remain

---

### Email Validation

### Test: Invalid email format rejected

Test the following invalid email formats:

- `notanemail`
- `missing@domain`
- `@nodomain.com`
- `spaces in@email.com`
- `double@@at.com`

For each: enter in email field and trigger validation.

**Expect:** Error "Please enter a valid email address"

---

### Test: Valid email formats accepted

Test the following valid email formats:

- `simple@example.com`
- `user.name@example.com`
- `user+tag@example.com`
- `user@subdomain.example.com`

For each: enter in email field and trigger validation.

**Expect:** No validation error

---

### Password Validation

### Test: Password minimum length enforced

1. Navigate to `/register`
2. Enter "abc" in password field (less than 8 characters)
3. Trigger validation

**Expect:** Error "Password must be at least 8 characters"

---

### Test: Password requires uppercase

1. Enter "alllowercase123" in password field
2. Trigger validation

**Expect:** Error mentions uppercase requirement

---

### Test: Password requires number

1. Enter "NoNumbersHere" in password field
2. Trigger validation

**Expect:** Error mentions number requirement

---

### Test: Password strength indicator

1. Enter "weak" in password field

**Expect:** Strength indicator shows "Weak" (red)

2. Enter "Medium123" in password field

**Expect:** Strength indicator shows "Medium" (yellow)

3. Enter "Str0ng!P@ssword" in password field

**Expect:** Strength indicator shows "Strong" (green)

---

### Test: Confirm password must match

1. Enter "SecurePass123!" in password field
2. Enter "DifferentPass456!" in confirm password field
3. Trigger validation

**Expect:** Error "Passwords do not match"

---

### Phone Number Validation

### Test: Phone format validation

Test the following formats on a US phone field:

| Input | Expected Result |
|-------|-----------------|
| `1234567890` | Valid (formatted to (123) 456-7890) |
| `(123) 456-7890` | Valid |
| `123-456-7890` | Valid |
| `123` | Invalid - too short |
| `abcdefghij` | Invalid - not numbers |

**Expect:** Invalid formats show error, valid formats accepted

---

### Test: International phone formats

1. Enable international phone format
2. Enter "+1 555 123 4567"

**Expect:** Accepted as valid

---

### Numeric Field Validation

### Test: Number field rejects non-numeric input

1. Find a quantity or amount field
2. Try to enter "abc"

**Expect:**
- Characters are not entered, OR
- Validation error appears

---

### Test: Number field enforces range

1. Find a quantity field with max 100
2. Enter "150"
3. Trigger validation

**Expect:** Error "Value must be less than or equal to 100"

---

### Test: Decimal precision enforced

1. Find a price field (2 decimal places)
2. Enter "19.999"

**Expect:**
- Value truncated to "19.99", OR
- Error about decimal precision

---

### Date Validation

### Test: Invalid date format rejected

1. Find a date field
2. Enter "not-a-date"
3. Trigger validation

**Expect:** Error about date format

---

### Test: Date range validation

1. Find a date field with minimum date restriction
2. Enter a date before the minimum

**Expect:** Error "Date must be after [minimum date]"

---

### Test: End date must be after start date

1. Find a date range form
2. Set start date to "2024-06-15"
3. Set end date to "2024-06-10"
4. Trigger validation

**Expect:** Error "End date must be after start date"

---

### File Upload Validation

### Test: File type restriction

1. Find a file upload that accepts only images
2. Try to upload a `.txt` file

**Expect:** Error "Please upload an image file (JPG, PNG, GIF)"

---

### Test: File size limit

1. Find a file upload with 5MB limit
2. Try to upload a file larger than 5MB

**Expect:** Error "File size must be less than 5MB"

---

### Character Limits

### Test: Max length enforced

1. Find a field with 100 character max
2. Try to paste 200 characters

**Expect:**
- Input truncated to 100 characters, OR
- Error "Maximum 100 characters allowed"

---

### Test: Character counter updates

1. Find a textarea with character limit
2. Type several characters

**Expect:**
- Counter shows remaining characters
- Counter updates as you type
- Counter changes color when near limit

---

### Custom Validation

### Test: Username availability check

1. Navigate to registration form
2. Enter an existing username
3. Wait for async validation

**Expect:** Error "Username already taken"

---

### Test: Username allows new value

1. Enter a unique username (e.g., `user_{timestamp}`)
2. Wait for async validation

**Expect:**
- Green checkmark or "Available" message
- No error

---

### Cross-Field Validation

### Test: Conditional required field

1. Select "Business" as account type
2. Leave "Company Name" field empty
3. Submit form

**Expect:** Error "Company Name is required for business accounts"

---

### Test: Field becomes optional when condition changes

1. Select "Business" as account type
2. Note "Company Name" is required
3. Change to "Personal" account type

**Expect:** "Company Name" is no longer required

---

### Server-Side Validation

### Test: Server validation errors display correctly

1. Submit a form that passes client validation
2. Server returns validation errors (e.g., duplicate email)

**Expect:**
- Error message displayed inline with field
- Form does not clear
- User can correct and resubmit

---

### Test: Multiple server errors displayed

1. Submit form with multiple server-side issues

**Expect:**
- All errors displayed
- Each error associated with correct field
