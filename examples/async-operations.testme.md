# TESTME: Async Operations

Tests for long-running processes, background jobs, and async workflows.

## Environment

| Variable | Default | Description |
|----------|---------|-------------|
| DEFAULT_TIMEOUT | 30000 | Default wait timeout in ms |
| LONG_TIMEOUT | 300000 | Extended timeout for slow operations |

## Tests

### Loading States

### Test: Loading indicator appears during fetch

1. Navigate to `/dashboard` (or any page that loads data)
2. Observe the initial state

**Expect:**
- Loading spinner or skeleton appears immediately
- Spinner disappears when data loads
- Content replaces loading state

---

### Test: Loading state on button click

1. Click a button that triggers an async action
2. Observe the button state

**Expect:**
- Button shows loading state (spinner or disabled)
- Button is not clickable during loading
- Button returns to normal after completion

---

### File Upload Progress

### Test: Upload progress indicator

1. Navigate to file upload page
2. Select a large file (>10MB recommended)
3. Start the upload

**Expect:**
- Progress bar appears
- Percentage increases over time
- Reaches 100% on completion
- Success message displayed

---

### Test: Upload can be cancelled

1. Start uploading a large file
2. Click "Cancel" while upload is in progress

**Expect:**
- Upload stops
- Progress bar disappears or resets
- No error about failed upload
- Can start new upload

---

### Background Job Processing

### Test: Job status updates in real-time

> **Note:** This test may take 1-2 minutes.

1. Trigger a background job (e.g., report generation)
2. Observe the status display

**Expect:**
- Initial status shows "Queued" or "Pending"
- Status changes to "Processing"
- Final status shows "Complete" or "Done"
- Real-time updates without page refresh

---

### Test: Job progress shows percentage

1. Trigger a multi-step job
2. Watch the progress indicator

**Expect:**
- Progress starts at 0%
- Progress increases as steps complete
- Progress reaches 100% at completion

---

### Test: Failed job shows error

1. Trigger a job that will fail (if test mode available)
2. Wait for processing

**Expect:**
- Status changes to "Failed"
- Error message is displayed
- Retry option is available

---

### Polling and Real-Time Updates

### Test: Data refreshes automatically

1. Navigate to a page with live data
2. Update data from another source (API, admin, etc.)
3. Wait without manually refreshing

**Expect:**
- Updated data appears automatically
- No page refresh required
- Update happens within reasonable time (e.g., 30 seconds)

---

### Test: Manual refresh button works

1. Navigate to data listing page
2. Click "Refresh" button

**Expect:**
- Loading indicator appears briefly
- Data is refreshed
- Timestamp or indicator shows last updated time

---

### Timeouts and Error Handling

### Test: Timeout shows user-friendly message

> **Note:** May require network throttling or server delay.

1. Trigger a request that will timeout
2. Wait for timeout duration

**Expect:**
- Loading state eventually ends
- Error message about timeout appears
- Retry option is available
- Suggestion to try again later

---

### Test: Retry after timeout works

1. Trigger a timeout (as above)
2. Click "Retry" button

**Expect:**
- Request is made again
- If successful, data loads normally

---

### Test: Network error handling

> **Note:** Requires network disconnection simulation.

1. Disconnect network
2. Trigger a data fetch

**Expect:**
- Error message about connection
- No infinite loading state
- Retry option when connection restored

---

### Batch Processing

### Test: Batch operation progress

1. Select multiple items (10+)
2. Trigger batch operation (delete, export, etc.)
3. Watch progress

**Expect:**
- Progress indicator shows items processed
- Display like "Processing 5 of 10..."
- Final count matches selection

---

### Test: Batch operation partial failure

1. Select items including some that will fail
2. Trigger batch operation
3. Wait for completion

**Expect:**
- Summary shows successes and failures
- Individual failures listed
- Successfully processed items updated
- Option to retry failed items

---

### Import Operations

### Test: Import file with progress

1. Navigate to import page
2. Upload a file with many records (100+)
3. Start import

**Expect:**
- Progress indicator shows records processed
- Import completes successfully
- Summary shows imported count

---

### Test: Import validates before processing

1. Upload a file with invalid format or data
2. Start import

**Expect:**
- Validation runs first
- Errors listed before processing starts
- Option to cancel or proceed with valid records

---

### Export Operations

### Test: Export generates downloadable file

1. Select data to export
2. Choose export format (CSV, PDF, etc.)
3. Click "Export"

**Expect:**
- Loading/generating indicator appears
- File downloads when ready
- File contains expected data

---

### Test: Large export shows progress

> **Note:** Requires large dataset.

1. Export a large dataset (1000+ records)
2. Watch for progress indication

**Expect:**
- Progress shows during generation
- User informed of estimated time for large exports
- Download starts on completion

---

### Notifications

### Test: Real-time notification appears

1. Set up a scenario that triggers a notification
2. Wait for the notification event

**Expect:**
- Notification appears without page refresh
- Notification badge updates
- Sound or visual alert (if enabled)

---

### Test: Notification links to relevant page

1. Receive a notification
2. Click on it

**Expect:**
- Navigates to relevant page/item
- Context is preserved (right item selected)
