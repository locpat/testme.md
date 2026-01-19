# TESTME: CRUD Operations

Tests for Create, Read, Update, and Delete operations on a generic resource (Items).

## Prerequisites

- User is authenticated
- User has permission to manage items

## Setup

1. Log in with a test account that has item management permissions
2. Navigate to `/items`

## Environment

| Variable | Default | Description |
|----------|---------|-------------|
| TEST_USER_EMAIL | manager@example.com | User with CRUD permissions |
| TEST_USER_PASSWORD | manager123 | User password |

## Tests

### Create Operations

### Test: Create new item with all fields

1. Click "Add New Item" button
2. Fill the form:
   - Name: (unique name, e.g., "Test Item {timestamp}")
   - Description: "This is a test item description"
   - Category: Select "General"
   - Price: "29.99"
   - Quantity: "100"
3. Click "Save" or "Create"

**Expect:**
- Modal/form closes
- New item appears in the list
- Success notification "Item created successfully"

---

### Test: Create item validates required fields

1. Click "Add New Item" button
2. Leave Name field empty
3. Click "Save"

**Expect:**
- Form does not submit
- Error "Name is required" appears
- Focus moves to the Name field

---

### Test: Create item prevents duplicate names

1. Note an existing item's name
2. Click "Add New Item"
3. Enter the existing item's name
4. Fill other required fields
5. Click "Save"

**Expect:** Error message "An item with this name already exists"

---

### Test: Create item with minimum required fields

1. Click "Add New Item"
2. Enter only the Name (if it's the only required field)
3. Leave optional fields empty
4. Click "Save"

**Expect:**
- Item is created
- Optional fields show default or empty values

---

### Read Operations

### Test: List view shows all items

1. Navigate to `/items`

**Expect:**
- Table or list of items is displayed
- Columns show: Name, Category, Price, Actions
- Pagination controls if more than one page

---

### Test: View item details

1. In the items list, click on any item name (or "View" button)

**Expect:**
- Item detail page or modal opens
- Shows all item fields: Name, Description, Category, Price, Quantity
- Created/Updated timestamps visible

---

### Test: Search items by name

1. Enter a known item name in the search box
2. Press Enter or click Search

**Expect:**
- List filters to show matching items
- Partial matches are included
- Result count updates

---

### Test: Filter items by category

1. Select "Electronics" from category filter dropdown

**Expect:**
- Only items in Electronics category shown
- Filter shows as active
- Clear filter option available

---

### Test: Sort items by column

1. Click on "Price" column header

**Expect:** Items sorted by price ascending

2. Click on "Price" column header again

**Expect:** Items sorted by price descending (toggle)

---

### Test: Pagination works correctly

> **Note:** Requires more items than fit on one page.

1. Navigate to `/items`
2. If multiple pages, click "Next" or page 2

**Expect:**
- Second page of items loads
- Previous items are replaced
- "Previous" button becomes enabled

---

### Update Operations

### Test: Edit item and save changes

1. Find an existing item in the list
2. Click "Edit" button
3. Change the Name to "Updated Item {timestamp}"
4. Change the Price to "39.99"
5. Click "Save"

**Expect:**
- Changes are saved
- List reflects new name and price
- Success notification appears

---

### Test: Cancel edit discards changes

1. Click "Edit" on any item
2. Change several fields
3. Click "Cancel" instead of Save

**Expect:**
- Form closes
- Original values remain unchanged
- No success notification

---

### Test: Edit validates required fields

1. Click "Edit" on any item
2. Clear the Name field (make it empty)
3. Click "Save"

**Expect:**
- Validation error appears
- Changes are not saved

---

### Test: Concurrent edit warning

> **Note:** Requires two sessions or simulating concurrent access.

1. Open the same item for editing in two browser tabs
2. In Tab 1, change the name and save
3. In Tab 2, change the description and try to save

**Expect:**
- Warning about item being modified
- Option to refresh and retry or force save

---

### Delete Operations

### Test: Delete item shows confirmation

1. Find an item in the list
2. Click "Delete" button

**Expect:**
- Confirmation dialog appears
- Shows item name being deleted
- "Cancel" and "Confirm" buttons present

---

### Test: Confirm delete removes item

1. Click "Delete" on an item
2. Note the item's name
3. Click "Confirm" in the dialog

**Expect:**
- Item is removed from list
- Success notification "Item deleted"
- Item no longer accessible

---

### Test: Cancel delete keeps item

1. Click "Delete" on an item
2. Click "Cancel" in the confirmation dialog

**Expect:**
- Dialog closes
- Item remains in the list
- No changes made

---

### Test: Delete removes item permanently

1. Delete an item
2. Search for the deleted item by name

**Expect:**
- Item does not appear in search results
- Item cannot be accessed by direct URL

---

### Bulk Operations

### Test: Select multiple items

1. Check the checkbox next to 3 items
2. Observe the bulk action bar

**Expect:**
- "3 items selected" indicator appears
- Bulk action buttons appear (Delete, Export, etc.)

---

### Test: Bulk delete selected items

1. Select multiple items (3 or more)
2. Click "Delete Selected"
3. Confirm the deletion

**Expect:**
- All selected items are removed
- Success message "3 items deleted"
- List updates to reflect removal

---

### Test: Select all on page

1. Click the "Select All" checkbox in the header

**Expect:**
- All items on current page are selected
- Selection count shows total selected

---

### Error Handling

### Test: Network error during save

> **Note:** Requires network interruption simulation.

1. Open an item for editing
2. Disconnect network or block API
3. Click "Save"

**Expect:**
- Error message about connection
- Changes are preserved in form
- Option to retry

---

### Test: 404 for non-existent item

1. Navigate directly to `/items/99999` (non-existent ID)

**Expect:**
- 404 page or "Item not found" message
- Link to return to items list
