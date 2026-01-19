# TESTME.md Example

This directory contains example TESTME.md files for a fictional e-commerce application, demonstrating various testing patterns and use cases.

## Files

| File | Description |
|------|-------------|
| [TESTME.md](TESTME.md) | Complete example with all sections (e-commerce app) |
| [auth.testme.md](auth.testme.md) | Authentication flows (login, register, password reset) |
| [crud.testme.md](crud.testme.md) | Create, Read, Update, Delete operations |
| [api.testme.md](api.testme.md) | REST API endpoint testing |
| [form-validation.testme.md](form-validation.testme.md) | Form validation patterns |
| [async-operations.testme.md](async-operations.testme.md) | Async workflows and loading states |

## Patterns Covered

### Complete Example (`TESTME.md`)

Shows all sections working together:

- Prerequisites
- Setup
- Environment variables
- Multiple test categories
- Teardown

### Authentication (`auth.testme.md`)

- Login (success, failure, validation)
- Registration (success, duplicates, password strength)
- Password reset flow
- Session management
- Protected routes

### CRUD Operations (`crud.testme.md`)

- Create with validation
- Read (list, detail, search, filter, sort)
- Update (save, cancel, concurrent edit)
- Delete (confirmation, soft delete)
- Bulk operations
- Error handling

### API Testing (`api.testme.md`)

- Health checks
- Authentication endpoints
- RESTful resource endpoints
- Pagination and filtering
- Error responses (401, 403, 404, 429)
- Batch operations

### Form Validation (`form-validation.testme.md`)

- Required fields
- Email format
- Password strength
- Phone numbers
- Numeric inputs
- Date validation
- File uploads
- Character limits
- Cross-field validation
- Server-side validation

### Async Operations (`async-operations.testme.md`)

- Loading states
- File upload progress
- Background job processing
- Polling and real-time updates
- Timeout handling
- Batch processing
- Import/Export operations

## Using These Examples

1. **Copy and adapt**: Copy the most relevant example and modify for your needs
2. **Mix patterns**: Combine patterns from different examples
3. **Start simple**: Begin with basic tests and add complexity as needed

## Customization Tips

- Replace placeholder URLs with your actual routes
- Update environment variable names to match your config
- Adjust selectors and element references for your UI
- Add specific error messages your app uses
- Include any app-specific setup steps
