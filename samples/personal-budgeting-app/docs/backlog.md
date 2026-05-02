# Product Backlog

## Epics

### Epic 1: Transaction Management
Enable users to create, view, edit, and delete income and expense transactions with categorization.

### Epic 2: Category System
Provide predefined categories and allow users to create and manage custom categories.

### Epic 3: Budget Tracking
Allow users to set budget limits and monitor spending against those limits with visual indicators.

### Epic 4: Reports and Analytics
Generate financial reports and visualizations showing spending patterns and trends.

### Epic 5: Offline-First Operation
Ensure full app functionality without internet connection and sync data when connectivity is restored.

### Epic 6: Data Management
Enable users to export and import their financial data for backup and portability.

### Epic 7: User Authentication
Secure user accounts and enable cloud data backup through authentication.

---

## User Stories

### Epic 1: Transaction Management

**US-1.1: Create Expense Transaction**
As a user, I want to quickly log an expense with amount, category, date, and optional notes, so that I can track my spending throughout the day.

**Acceptance Criteria:**
- User can enter expense amount (required)
- User can select category from list (required)
- Date defaults to today but can be changed
- User can add optional notes
- Transaction saves to local database immediately
- Success confirmation shown after save

**Tasks:**
- Design transaction entry form UI (Mobile UI Layer)
- Implement form validation logic (Mobile UI Layer)
- Create transaction model in WatermelonDB (Local Database)
- Implement create transaction method (Local Data Manager)
- Add transaction to sync queue (Sync Engine)
- Write unit tests for transaction creation

**Maps to:** FR-001

---

**US-1.2: Create Income Transaction**
As a user, I want to log income with amount, source, date, and optional notes, so that I can track money coming in.

**Acceptance Criteria:**
- User can enter income amount (required)
- User can select income source/category (required)
- Date defaults to today but can be changed
- User can add optional notes
- Transaction saves immediately
- Success confirmation shown

**Tasks:**
- Add income transaction type to data model (Local Database)
- Implement income entry form (Mobile UI Layer)
- Extend transaction creation logic for income (Local Data Manager)
- Add income categories to category system (Category Manager)
- Write unit tests for income transactions

**Maps to:** FR-002

---

**US-1.3: View Transaction List**
As a user, I want to see all my transactions in chronological order, so that I can review my financial activity.

**Acceptance Criteria:**
- Transactions displayed newest first
- Each transaction shows amount, category, date, and notes
- List scrolls smoothly with many transactions
- Visual distinction between income and expenses
- Pull-to-refresh updates the list

**Tasks:**
- Design transaction list UI component (Mobile UI Layer)
- Implement lazy loading for performance (Local Data Manager)
- Add pull-to-refresh functionality (Mobile UI Layer)
- Style income vs expense differently (Mobile UI Layer)
- Optimize query performance for large datasets (Local Database)

**Maps to:** FR-005

---

**US-1.4: Edit Transaction**
As a user, I want to edit a transaction I created, so that I can correct mistakes or update information.

**Acceptance Criteria:**
- User can tap transaction to open edit form
- All fields are editable
- Changes save immediately
- Updated transaction appears in list
- Edit triggers sync update

**Tasks:**
- Create edit transaction form (Mobile UI Layer)
- Implement update transaction method (Local Data Manager)
- Handle sync conflict resolution for edits (Sync Engine)
- Add edit timestamp tracking (Local Database)
- Write unit tests for transaction updates

**Maps to:** FR-003

---

**US-1.5: Delete Transaction**
As a user, I want to delete a transaction, so that I can remove incorrect or duplicate entries.

**Acceptance Criteria:**
- User can swipe or long-press to delete
- Confirmation dialog prevents accidental deletion
- Transaction removed from list immediately
- Deletion syncs to cloud
- Undo option available for 5 seconds

**Tasks:**
- Implement swipe-to-delete gesture (Mobile UI Layer)
- Create confirmation dialog (Mobile UI Layer)
- Implement soft delete with undo (Local Data Manager)
- Handle deletion in sync logic (Sync Engine)
- Write unit tests for deletion

**Maps to:** FR-004

---

**US-1.6: Filter Transactions by Date**
As a user, I want to filter transactions by date range, so that I can focus on specific time periods.

**Acceptance Criteria:**
- User can select start and end dates
- Quick filters for "This Month", "Last Month", "Last 3 Months"
- Filtered list updates immediately
- Filter state persists during session
- Clear filter option available

**Tasks:**
- Design date range picker UI (Mobile UI Layer)
- Implement date filtering logic (Local Data Manager)
- Add quick filter buttons (Mobile UI Layer)
- Optimize date range queries (Local Database)
- Write unit tests for filtering

**Maps to:** FR-006

---

**US-1.7: Filter Transactions by Category**
As a user, I want to filter transactions by category, so that I can see spending in specific areas.

**Acceptance Criteria:**
- User can select one or multiple categories
- Filtered list shows only selected categories
- Category filter works with date filter
- Filter state persists during session
- Clear filter option available

**Tasks:**
- Design category filter UI (Mobile UI Layer)
- Implement category filtering logic (Local Data Manager)
- Support multi-category selection (Mobile UI Layer)
- Combine with date filtering (Local Data Manager)
- Write unit tests for category filtering

**Maps to:** FR-007

---

**US-1.8: Search Transactions**
As a user, I want to search transactions by notes or description, so that I can quickly find specific transactions.

**Acceptance Criteria:**
- Search box at top of transaction list
- Search updates results as user types
- Searches transaction notes and category names
- Search works with other filters
- Clear search button available

**Tasks:**
- Add search input to UI (Mobile UI Layer)
- Implement full-text search (Local Database)
- Debounce search input for performance (Mobile UI Layer)
- Highlight search matches in results (Mobile UI Layer)
- Write unit tests for search functionality

**Maps to:** FR-008

---

### Epic 2: Category System

**US-2.1: Use Predefined Categories**
As a user, I want to have common expense categories available by default, so that I can start tracking immediately without setup.

**Acceptance Criteria:**
- App includes 8 predefined expense categories
- Categories have distinct colors and icons
- Categories appear in transaction entry form
- Categories cannot be deleted (only hidden)
- Categories work offline

**Tasks:**
- Define default category list (Category Manager)
- Create category data model (Local Database)
- Seed database with default categories (Local Data Manager)
- Design category selection UI (Mobile UI Layer)
- Assign colors and icons to categories (Mobile UI Layer)

**Maps to:** FR-009

---

**US-2.2: Create Custom Category**
As a user, I want to create my own expense categories, so that I can track spending specific to my lifestyle.

**Acceptance Criteria:**
- User can add new category with name
- User can choose color for category
- User can optionally add icon
- Custom category appears in category list
- Custom category available for transactions

**Tasks:**
- Design add category form (Mobile UI Layer)
- Implement color picker (Mobile UI Layer)
- Create category creation method (Category Manager)
- Validate category name uniqueness (Category Manager)
- Write unit tests for category creation

**Maps to:** FR-010

---

**US-2.3: Edit Category**
As a user, I want to edit category names and colors, so that I can refine my categorization system.

**Acceptance Criteria:**
- User can edit custom category names
- User can change category colors
- Changes apply to all transactions in that category
- Predefined categories can only have colors changed
- Changes sync to cloud

**Tasks:**
- Create edit category form (Mobile UI Layer)
- Implement category update method (Category Manager)
- Prevent editing predefined category names (Category Manager)
- Update transaction display when category changes (Mobile UI Layer)
- Write unit tests for category updates

**Maps to:** FR-011

---

**US-2.4: Delete Custom Category**
As a user, I want to delete custom categories I no longer use, so that my category list stays organized.

**Acceptance Criteria:**
- User can delete custom categories only
- Confirmation required before deletion
- Transactions in deleted category move to "Other"
- Predefined categories cannot be deleted
- Deletion syncs to cloud

**Tasks:**
- Implement category deletion (Category Manager)
- Create confirmation dialog (Mobile UI Layer)
- Reassign transactions to "Other" category (Local Data Manager)
- Prevent deletion of predefined categories (Category Manager)
- Write unit tests for category deletion

**Maps to:** FR-012

---

**US-2.5: Manage Income Categories**
As a user, I want predefined income categories, so that I can categorize different income sources.

**Acceptance Criteria:**
- App includes 4 predefined income categories
- Income categories separate from expense categories
- User can create custom income categories
- Income categories work same as expense categories
- Categories available in income transaction form

**Tasks:**
- Define default income categories (Category Manager)
- Separate income and expense category lists (Local Database)
- Implement income category management (Category Manager)
- Update UI to show appropriate categories by transaction type (Mobile UI Layer)
- Write unit tests for income categories

**Maps to:** FR-013

---

### Epic 3: Budget Tracking

**US-3.1: Set Category Budget**
As a user, I want to set monthly spending limits for each category, so that I can control spending in specific areas.

**Acceptance Criteria:**
- User can set budget amount for any category
- Budget applies to current and future months
- User can edit budget amounts
- Budget of zero means no limit
- Budget settings sync to cloud

**Tasks:**
- Design budget setting UI (Mobile UI Layer)
- Create budget data model (Local Database)
- Implement budget CRUD operations (Budget Calculator)
- Link budgets to categories (Local Data Manager)
- Write unit tests for budget management

**Maps to:** FR-014

---

**US-3.2: Set Overall Budget**
As a user, I want to set a total monthly spending limit, so that I can control my overall expenses.

**Acceptance Criteria:**
- User can set overall monthly budget
- Overall budget separate from category budgets
- User can edit overall budget
- Overall budget applies to all expense categories
- Budget settings sync to cloud

**Tasks:**
- Add overall budget field to data model (Local Database)
- Implement overall budget setting (Budget Calculator)
- Create overall budget UI (Mobile UI Layer)
- Calculate total spending across categories (Budget Calculator)
- Write unit tests for overall budget

**Maps to:** FR-015

---

**US-3.3: View Category Budget Status**
As a user, I want to see how much I've spent against each category budget, so that I can stay within limits.

**Acceptance Criteria:**
- Each category shows spent amount and budget limit
- Progress bar visualizes spending percentage
- Percentage displayed numerically
- Updates in real-time as transactions added
- Works offline with local data

**Tasks:**
- Design budget status display (Mobile UI Layer)
- Implement spending calculation per category (Budget Calculator)
- Create progress bar component (Mobile UI Layer)
- Update display when transactions change (Mobile UI Layer)
- Write unit tests for budget calculations

**Maps to:** FR-016

---

**US-3.4: View Overall Budget Status**
As a user, I want to see my total spending against my overall budget, so that I know my overall financial position.

**Acceptance Criteria:**
- Dashboard shows total spent vs overall budget
- Large progress indicator for overall budget
- Percentage and amounts displayed
- Updates in real-time
- Prominent placement in app

**Tasks:**
- Design overall budget dashboard (Mobile UI Layer)
- Calculate total spending across all categories (Budget Calculator)
- Create large progress indicator (Mobile UI Layer)
- Update dashboard when transactions change (Mobile UI Layer)
- Write unit tests for overall budget calculations

**Maps to:** FR-017

---

**US-3.5: Budget Warning Indicators**
As a user, I want visual warnings when approaching or exceeding budget limits, so that I can adjust my spending.

**Acceptance Criteria:**
- Yellow warning at 80% of budget
- Red alert at 100% of budget
- Visual indicators on category list
- Notification when crossing thresholds
- Works for both category and overall budgets

**Tasks:**
- Implement threshold detection logic (Budget Calculator)
- Design warning color scheme (Mobile UI Layer)
- Add visual indicators to UI (Mobile UI Layer)
- Create notification system (Mobile UI Layer)
- Write unit tests for threshold detection

**Maps to:** FR-018

---

### Epic 4: Reports and Analytics

**US-4.1: View Income Summary**
As a user, I want to see total income for a time period, so that I know how much money I earned.

**Acceptance Criteria:**
- Report shows total income for selected period
- Breakdown by income category
- Comparison to previous period
- Works for custom date ranges
- Updates immediately with new data

**Tasks:**
- Design income summary UI (Mobile UI Layer)
- Implement income aggregation (Report Generator)
- Add period comparison logic (Report Generator)
- Support custom date ranges (Mobile UI Layer)
- Write unit tests for income calculations

**Maps to:** FR-019

---

**US-4.2: View Expense Summary**
As a user, I want to see total expenses for a time period, so that I know how much I spent.

**Acceptance Criteria:**
- Report shows total expenses for selected period
- Breakdown by expense category
- Comparison to previous period
- Works for custom date ranges
- Updates immediately with new data

**Tasks:**
- Design expense summary UI (Mobile UI Layer)
- Implement expense aggregation (Report Generator)
- Add period comparison logic (Report Generator)
- Support custom date ranges (Mobile UI Layer)
- Write unit tests for expense calculations

**Maps to:** FR-020

---

**US-4.3: View Net Balance**
As a user, I want to see my net balance (income minus expenses), so that I know if I'm saving or overspending.

**Acceptance Criteria:**
- Report shows net balance for selected period
- Positive balance shown in green
- Negative balance shown in red
- Comparison to previous period
- Prominent display on dashboard

**Tasks:**
- Calculate net balance (Report Generator)
- Design balance display (Mobile UI Layer)
- Add color coding for positive/negative (Mobile UI Layer)
- Implement period comparison (Report Generator)
- Write unit tests for balance calculations

**Maps to:** FR-021

---

**US-4.4: View Expense Breakdown Chart**
As a user, I want to see a pie chart of expenses by category, so that I can visualize where my money goes.

**Acceptance Criteria:**
- Pie chart shows percentage per category
- Categories color-coded consistently
- Tap slice to see category details
- Legend shows category names and amounts
- Chart updates with date filter

**Tasks:**
- Implement pie chart using Victory Native (Chart Renderer)
- Calculate category percentages (Report Generator)
- Add interactivity to chart (Mobile UI Layer)
- Create chart legend (Mobile UI Layer)
- Write unit tests for chart data preparation

**Maps to:** FR-022

---

**US-4.5: View Spending Trends**
As a user, I want to see spending trends over time, so that I can identify patterns in my spending.

**Acceptance Criteria:**
- Line or bar chart shows spending over time
- Configurable time grouping (daily, weekly, monthly)
- Multiple categories can be compared
- Chart scrolls for long time periods
- Tap data point to see details

**Tasks:**
- Implement trend chart using Victory Native (Chart Renderer)
- Aggregate data by time period (Report Generator)
- Add time grouping options (Mobile UI Layer)
- Enable multi-category comparison (Report Generator)
- Write unit tests for trend calculations

**Maps to:** FR-023

---

**US-4.6: View Top Spending Categories**
As a user, I want to see my top spending categories, so that I can identify where I spend the most.

**Acceptance Criteria:**
- List shows top 5 categories by spending
- Each category shows amount and percentage
- Sorted by amount descending
- Updates with date filter
- Tap category to see transactions

**Tasks:**
- Calculate top categories (Report Generator)
- Design top categories list (Mobile UI Layer)
- Sort categories by spending (Report Generator)
- Add navigation to category transactions (Mobile UI Layer)
- Write unit tests for top category calculations

**Maps to:** FR-024

---

**US-4.7: Select Report Time Period**
As a user, I want to view reports for different time periods, so that I can analyze different timeframes.

**Acceptance Criteria:**
- Quick filters: This Month, Last Month, Last 3 Months, Last 6 Months
- Custom date range picker
- All reports update with selected period
- Period selection persists during session
- Clear indication of current period

**Tasks:**
- Design period selector UI (Mobile UI Layer)
- Implement quick filter buttons (Mobile UI Layer)
- Add custom date range picker (Mobile UI Layer)
- Update all reports with selected period (Report Generator)
- Write unit tests for period filtering

**Maps to:** FR-025

---

### Epic 5: Offline-First Operation

**US-5.1: Create Transactions Offline**
As a user, I want to create transactions when I have no internet connection, so that I can track expenses anywhere.

**Acceptance Criteria:**
- All transaction creation works offline
- No error messages about connectivity
- Transactions save to local database
- UI indicates offline status
- Transactions queue for sync

**Tasks:**
- Ensure all CRUD operations work offline (Local Data Manager)
- Implement offline status indicator (Connectivity Monitor)
- Create sync queue system (Sync Engine)
- Test all features without network (QA)
- Write unit tests for offline operations

**Maps to:** FR-026

---

**US-5.2: Store Data Locally**
As a user, I want all my data stored on my device, so that I can access it without internet.

**Acceptance Criteria:**
- All transactions stored in local database
- All categories stored locally
- All budgets stored locally
- Data persists after app restart
- Data encrypted at rest

**Tasks:**
- Configure WatermelonDB for local storage (Local Database)
- Implement data encryption (Encryption Module)
- Test data persistence (QA)
- Optimize storage for large datasets (Local Database)
- Write unit tests for data persistence

**Maps to:** FR-027

---

**US-5.3: Sync Data to Cloud**
As a user, I want my data automatically backed up to the cloud, so that I don't lose it if my phone is lost.

**Acceptance Criteria:**
- Data syncs automatically when online
- Sync happens in background
- User can manually trigger sync
- Sync status visible in UI
- Failed syncs retry automatically

**Tasks:**
- Implement sync logic with Supabase (Sync Engine)
- Create background sync scheduler (Sync Engine)
- Add manual sync trigger (Mobile UI Layer)
- Implement retry logic for failures (Sync Engine)
- Write unit tests for sync operations

**Maps to:** FR-028

---

**US-5.4: Handle Sync Conflicts**
As a user, I want the app to handle sync conflicts automatically, so that I don't lose data.

**Acceptance Criteria:**
- Last write wins for conflict resolution
- Conflicts resolved automatically
- No data loss during conflicts
- Conflict resolution logged
- User notified of major conflicts

**Tasks:**
- Implement last-write-wins strategy (Sync Engine)
- Add timestamp tracking to all records (Local Database)
- Create conflict resolution logic (Sync Engine)
- Log conflict resolutions (Sync Engine)
- Write unit tests for conflict scenarios

**Maps to:** FR-029

---

**US-5.5: Display Sync Status**
As a user, I want to see sync status, so that I know if my data is backed up.

**Acceptance Criteria:**
- Icon shows synced/syncing/offline status
- Timestamp of last successful sync
- Pending changes count when offline
- Sync progress indicator
- Error messages for sync failures

**Tasks:**
- Design sync status indicator (Mobile UI Layer)
- Implement status tracking (Connectivity Monitor)
- Show last sync timestamp (Mobile UI Layer)
- Display pending changes count (Sync Engine)
- Write unit tests for status display

**Maps to:** FR-030

---

### Epic 6: Data Management

**US-6.1: Export Data as CSV**
As a user, I want to export my transactions as CSV, so that I can use them in spreadsheet software.

**Acceptance Criteria:**
- Export button in settings
- CSV includes all transaction fields
- File saved to device downloads folder
- Success message with file location
- Works offline with local data

**Tasks:**
- Implement CSV export logic (Data Export/Import Handler)
- Format data for CSV (Data Export/Import Handler)
- Use react-native-fs for file writing (Data Export/Import Handler)
- Add export button to settings (Mobile UI Layer)
- Write unit tests for CSV export

**Maps to:** FR-031

---

**US-6.2: Export Data as JSON**
As a user, I want to export my data as JSON, so that I can back it up or migrate to another app.

**Acceptance Criteria:**
- Export button in settings
- JSON includes all data (transactions, categories, budgets)
- File saved to device downloads folder
- Success message with file location
- Works offline with local data

**Tasks:**
- Implement JSON export logic (Data Export/Import Handler)
- Serialize all data to JSON (Data Export/Import Handler)
- Use react-native-fs for file writing (Data Export/Import Handler)
- Add export button to settings (Mobile UI Layer)
- Write unit tests for JSON export

**Maps to:** FR-032

---

**US-6.3: Import Data from CSV**
As a user, I want to import transactions from CSV, so that I can migrate data from other apps.

**Acceptance Criteria:**
- Import button in settings
- File picker to select CSV
- Validation of CSV format
- Preview before import
- Error messages for invalid data

**Tasks:**
- Implement CSV parsing (Data Export/Import Handler)
- Validate CSV structure and data (Data Export/Import Handler)
- Create import preview UI (Mobile UI Layer)
- Import validated data to database (Local Data Manager)
- Write unit tests for CSV import

**Maps to:** FR-033

---

**US-6.4: Validate Imported Data**
As a user, I want invalid import data to be rejected with clear errors, so that I can fix issues.

**Acceptance Criteria:**
- Validation checks all required fields
- Validation checks data types
- Validation checks date formats
- Clear error messages for each issue
- Option to skip invalid rows

**Tasks:**
- Implement data validation rules (Data Export/Import Handler)
- Create validation error messages (Data Export/Import Handler)
- Design error display UI (Mobile UI Layer)
- Add skip invalid rows option (Data Export/Import Handler)
- Write unit tests for validation

**Maps to:** FR-034

---

### Epic 7: User Authentication

**US-7.1: Register Account**
As a user, I want to create an account, so that I can back up my data to the cloud.

**Acceptance Criteria:**
- Registration form with email and password
- Password strength requirements
- Email verification sent
- Account created in Supabase
- User logged in after registration

**Tasks:**
- Design registration form (Mobile UI Layer)
- Implement Supabase Auth registration (Authentication Service)
- Add password validation (Mobile UI Layer)
- Handle email verification (Authentication Service)
- Write unit tests for registration

**Maps to:** Security NFR

---

**US-7.2: Login to Account**
As a user, I want to log in to my account, so that I can access my cloud-backed data.

**Acceptance Criteria:**
- Login form with email and password
- Remember me option
- Forgot password link
- Error messages for invalid credentials
- Automatic login on app restart if remembered

**Tasks:**
- Design login form (Mobile UI Layer)
- Implement Supabase Auth login (Authentication Service)
- Add remember me functionality (Authentication Service)
- Implement forgot password flow (Authentication Service)
- Write unit tests for login

**Maps to:** Security NFR

---

**US-7.3: Secure Data Access**
As a user, I want my data to be secure, so that only I can access it.

**Acceptance Criteria:**
- All data encrypted in transit (HTTPS)
- All data encrypted at rest
- Authentication required for cloud access
- Session expires after inactivity
- Biometric login option (fingerprint/face)

**Tasks:**
- Configure HTTPS for all API calls (Cloud Sync Service)
- Implement local data encryption (Encryption Module)
- Add session timeout logic (Authentication Service)
- Implement biometric authentication (Mobile UI Layer)
- Write security tests

**Maps to:** Security NFR

---

**US-7.4: Auto-lock App**
As a user, I want the app to lock after inactivity, so that my financial data stays private.

**Acceptance Criteria:**
- App locks after 5 minutes of inactivity
- Lock screen requires authentication
- Biometric unlock supported
- User can configure timeout duration
- Lock applies even if app backgrounded

**Tasks:**
- Implement inactivity timer (Mobile UI Layer)
- Create lock screen (Mobile UI Layer)
- Add biometric unlock (Mobile UI Layer)
- Make timeout configurable (Mobile UI Layer)
- Write unit tests for auto-lock

**Maps to:** Security NFR