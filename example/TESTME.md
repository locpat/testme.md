# TESTME: Complete Example

This example demonstrates all sections of a TESTME.md file with a realistic e-commerce application test suite.

## Prerequisites

- Docker is installed and running
- Node.js 18+ is available
- Port 3000 is available
- Port 5432 is available (for PostgreSQL)

## Setup

1. Copy `env.example` to `.env` if it doesn't exist
2. Run `docker compose up -d` to start the database
3. Run `npm install` to install dependencies
4. Run `npm run db:migrate` to set up the database schema
5. Run `npm run db:seed` to populate test data
6. Run `npm run dev` to start the application
7. Wait for `http://localhost:3000/health` to return 200

## Environment

| Variable | Default | Description |
|----------|---------|-------------|
| BASE_URL | http://localhost:3000 | Application URL |
| ADMIN_EMAIL | admin@store.example | Admin account email |
| ADMIN_PASSWORD | admin123secure | Admin account password |
| TEST_USER_EMAIL | buyer@example.com | Standard test user |
| TEST_USER_PASSWORD | buyer123 | Test user password |

## Tests

### Authentication

### Test: Login with valid credentials

1. Navigate to `/login`
2. Enter email from `TEST_USER_EMAIL` env var
3. Enter password from `TEST_USER_PASSWORD` env var
4. Click "Sign In" button

**Expect:**
- URL changes to `/dashboard`
- User's name appears in the header
- No error messages visible

---

### Test: Login with invalid credentials shows error

1. Navigate to `/login`
2. Enter email "nonexistent@example.com"
3. Enter password "wrongpassword"
4. Click "Sign In" button

**Expect:**
- Remain on `/login` page
- Error message "Invalid email or password" is visible

---

### Test: Logout clears session

1. Log in with valid credentials (as above)
2. Click the user menu in the header
3. Click "Sign Out"

**Expect:**
- URL changes to `/`
- Login link appears in header
- User's name no longer visible

---

### Shopping Cart

### Test: Add item to cart

1. Navigate to `/products`
2. Click on any product to view details
3. Note the product name and price
4. Click "Add to Cart" button

**Expect:**
- Cart icon shows "1" item
- Success toast "Added to cart" appears

---

### Test: Update cart quantity

1. Add an item to cart (as above)
2. Navigate to `/cart`
3. Find the item in the cart
4. Change quantity to 3

**Expect:**
- Quantity field shows 3
- Subtotal updates to price × 3
- Cart total updates accordingly

---

### Test: Remove item from cart

1. Add an item to cart
2. Navigate to `/cart`
3. Click "Remove" button on the item

**Expect:**
- Item no longer in cart
- If cart is empty, "Your cart is empty" message appears

---

### Checkout Flow

### Test: Complete checkout with valid payment

1. Add at least one item to cart
2. Navigate to `/cart`
3. Click "Proceed to Checkout"
4. Fill shipping address:
   - Name: "Test User"
   - Street: "123 Test Street"
   - City: "Test City"
   - Zip: "12345"
5. Click "Continue to Payment"
6. Enter test card details:
   - Card Number: "4242424242424242"
   - Expiry: "12/25"
   - CVC: "123"
7. Click "Place Order"

**Expect:**
- URL changes to `/order/confirmation`
- Order confirmation number is displayed
- Email confirmation message shown

---

### Test: Checkout validates required fields

1. Add an item to cart
2. Navigate to checkout
3. Leave all shipping fields empty
4. Click "Continue to Payment"

**Expect:**
- Form does not advance
- Error messages appear for required fields
- "Name is required" error visible
- "Address is required" error visible

---

### Admin Functions

### Test: Admin can view all orders

1. Log in with admin credentials (from `ADMIN_EMAIL` and `ADMIN_PASSWORD`)
2. Navigate to `/admin/orders`

**Expect:**
- Orders table is visible
- Table shows Order ID, Customer, Total, Status columns
- At least one order is listed

---

### Test: Admin can update order status

1. Log in as admin
2. Navigate to `/admin/orders`
3. Click on any order
4. Change status from "Pending" to "Shipped"
5. Click "Save"

**Expect:**
- Success message "Order updated" appears
- Order status shows "Shipped"

---

### Search and Filtering

### Test: Product search returns relevant results

1. Navigate to `/products`
2. Enter "shoes" in the search box
3. Press Enter or click search icon

**Expect:**
- Results contain products with "shoes" in name or description
- Result count is displayed
- If no results, "No products found" message appears

---

### Test: Filter products by category

1. Navigate to `/products`
2. Click on "Electronics" category filter

**Expect:**
- Only electronics products are shown
- Category filter shows as active
- Product count updates

---

### Test: Sort products by price

1. Navigate to `/products`
2. Select "Price: Low to High" from sort dropdown

**Expect:**
- Products reorder by price ascending
- First product has lowest price
- Last product has highest price

## Teardown

1. Run `npm run db:clean` to remove test data
2. Run `docker compose down` to stop containers
3. Remove `.env` if it was created during setup
