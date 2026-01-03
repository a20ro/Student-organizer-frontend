# Student Tracker Backend - Complete Project Report

## 📋 Overview
A Laravel backend API for a student management system that helps university students manage semesters, courses, GPA, notes, budgets, goals, and sync with Google Calendar.

---

## 🗄️ Database Tables

### 1. **users**
Stores all user accounts (students and admins)
- `id` - Unique ID
- `name` - User's name
- `email` - Email address (unique)
- `password` - Encrypted password
- `avatar` - Profile picture URL
- `major` - Student's major
- `university` - University name
- `role` - 'student' or 'admin'
- `status` - 'active' or 'suspended'
- `last_login` - Last login timestamp
- `google_id` - Google account ID (for Google login)
- `google_email` - Google email
- `google_access_token` - Encrypted Google access token
- `google_refresh_token` - Encrypted Google refresh token
- `created_at`, `updated_at`

### 2. **semesters**
Each user can have multiple semesters
- `id` - Unique ID
- `user_id` - Links to users table
- `title` - Semester name (e.g., "Fall 2025")
- `start_date` - Semester start date
- `end_date` - Semester end date
- `notes` - Optional notes
- `created_at`, `updated_at`

### 3. **courses**
Each semester has multiple courses
- `id` - Unique ID
- `semester_id` - Links to semesters table
- `name` - Course name
- `code` - Course code (e.g., "ITCS-204")
- `instructor` - Professor name
- `credit_hours` - Number of credits
- `room` - Classroom location
- `color_tag` - Color for UI
- `created_at`, `updated_at`

### 4. **assessments**
Each course has multiple assessments (quizzes, exams, assignments)
- `id` - Unique ID
- `course_id` - Links to courses table
- `title` - Assessment name
- `type` - 'quiz', 'midterm', 'final', 'assignment', 'project'
- `grade_received` - Score received
- `grade_max` - Maximum possible score
- `due_date` - Due date
- `weight_percentage` - How much it counts toward final grade
- `created_at`, `updated_at`

### 5. **notes**
Course notes with markdown support
- `id` - Unique ID
- `course_id` - Links to courses table
- `title` - Note title
- `content` - Markdown content
- `week_number` - Week number
- `attachments` - JSON array of file attachments
- `is_pinned` - Boolean (pinned notes)
- `is_favorite` - Boolean (favorite notes)
- `share_token` - Unique token for sharing
- `is_public` - Boolean (public sharing)
- `tags` - JSON array of tags
- `created_at`, `updated_at`

### 6. **note_versions**
Version history for notes (keeps last 10 versions)
- `id` - Unique ID
- `note_id` - Links to notes table
- `content` - Markdown content at this version
- `version_number` - Version number (e.g., "1.0", "1.1")
- `created_by` - User who created this version
- `created_at`, `updated_at`

### 7. **events**
Calendar events for reminders
- `id` - Unique ID
- `user_id` - Links to users table
- `title` - Event title
- `description` - Event description
- `date` - Event date
- `time` - Event time
- `location` - Event location
- `reminder_before` - Reminder time (e.g., "30 minutes")
- `created_at`, `updated_at`

### 8. **transactions**
Budget transactions (income/expenses)
- `id` - Unique ID
- `user_id` - Links to users table
- `type` - 'income' or 'expense'
- `category` - Category name (e.g., "Food", "Transport")
- `amount` - Transaction amount
- `date` - Transaction date
- `note` - Optional note
- `created_at`, `updated_at`

### 9. **recurring_transactions**
Recurring income/expenses
- `id` - Unique ID
- `user_id` - Links to users table
- `type` - 'income' or 'expense'
- `category` - Category name
- `amount` - Amount
- `note` - Optional note
- `frequency` - 'daily', 'weekly', 'monthly', 'yearly'
- `start_date` - When it starts
- `end_date` - When it ends (optional)
- `next_occurrence` - Next time it happens
- `is_active` - Boolean
- `created_at`, `updated_at`

### 10. **budgets**
Monthly budgets per category
- `id` - Unique ID
- `user_id` - Links to users table
- `category` - Category name (null = overall budget)
- `amount` - Budget amount
- `year` - Year (e.g., 2025)
- `month` - Month (1-12)
- `created_at`, `updated_at`

### 11. **goals**
High-level goals
- `id` - Unique ID
- `user_id` - Links to users table
- `title` - Goal title
- `description` - Goal description
- `target_date` - Target completion date
- `completed` - Boolean
- `created_at`, `updated_at`

### 12. **tasks**
Tasks (can belong to goals or be standalone)
- `id` - Unique ID
- `goal_id` - Links to goals table (optional)
- `user_id` - Links to users table
- `parent_task_id` - For subtasks (links to tasks table)
- `title` - Task title
- `description` - Task description
- `due_date` - Due date
- `completed` - Boolean
- `created_at`, `updated_at`

### 13. **habits**
Habit tracker
- `id` - Unique ID
- `user_id` - Links to users table
- `name` - Habit name
- `frequency_type` - 'daily', 'weekly', 'monthly'
- `target_count` - Target number
- `created_at`, `updated_at`

### 14. **habit_logs**
Daily habit tracking
- `id` - Unique ID
- `habit_id` - Links to habits table
- `date` - Date of log
- `count` - Count for that day
- `created_at`, `updated_at`

### 15. **file_attachments**
File attachments (polymorphic - can attach to notes, assessments)
- `id` - Unique ID
- `attachable_id` - ID of parent (note/assessment)
- `attachable_type` - Type ('Note' or 'Assessment')
- `user_id` - Links to users table
- `original_name` - Original filename
- `file_name` - Stored filename
- `file_path` - Storage path
- `mime_type` - File type
- `file_size` - Size in bytes
- `created_at`, `updated_at`

### 16. **user_sessions**
Track all user login sessions
- `id` - Unique ID
- `user_id` - Links to users table
- `token_id` - Sanctum token ID
- `device_name` - Device name
- `ip_address` - IP address
- `user_agent` - Browser info
- `last_activity` - Last activity time
- `is_active` - Boolean
- `created_at`, `updated_at`

### 17. **announcements** (Admin)
Admin announcements
- `id` - Unique ID
- `admin_id` - Admin who created it
- `title` - Announcement title
- `message` - Message content
- `audience` - 'all', 'students', 'single'
- `target_user_id` - If audience is 'single'
- `scheduled_at` - When to send
- `sent_at` - When it was sent
- `email_stats` - JSON with send stats
- `created_at`, `updated_at`

### 18. **system_logs** (Admin)
System activity logs
- `id` - Unique ID
- `admin_id` - Admin who created log
- `title` - Log title
- `message` - Log message
- `type` - Log type
- `level` - Log level (info, warning, error)
- `context` - JSON with additional info
- `created_at`, `updated_at`

---

## 🔧 Controllers & Functions

### **AuthController**
Handles user authentication
- `signup()` - Create new account
- `login()` - Login with email/password
- `logout()` - Logout and revoke token
- `me()` - Get current user info
- `forgotPassword()` - Request password reset
- `resetPassword()` - Reset password with token

### **GoogleAuthController**
Google OAuth integration
- `redirect()` - Redirect to Google login
- `callback()` - Handle Google login response, link/create account
- `disconnect()` - Disconnect Google account

### **GoogleCalendarController**
Google Calendar sync
- `syncEvent()` - Sync event to Google Calendar
- `syncAssessment()` - Sync assessment to Google Calendar

### **SemesterController**
Manage semesters
- `index()` - Get all user's semesters
- `show()` - Get one semester
- `store()` - Create semester
- `update()` - Update semester
- `destroy()` - Delete semester

### **CourseController**
Manage courses
- `index()` - Get courses in a semester
- `store()` - Create course
- `update()` - Update course
- `destroy()` - Delete course

### **AssessmentController**
Manage assessments
- `index()` - Get assessments in a course
- `show()` - Get one assessment
- `store()` - Create assessment
- `update()` - Update assessment
- `destroy()` - Delete assessment

### **NoteController**
Manage notes with enhancements
- `index()` - Get notes (with filters: pinned, favorite, search)
- `show()` - Get one note
- `store()` - Create note
- `update()` - Update note (creates version)
- `destroy()` - Delete note
- `togglePin()` - Pin/unpin note
- `toggleFavorite()` - Favorite/unfavorite note
- `generateShareLink()` - Create shareable link
- `revokeShareLink()` - Remove shareable link
- `versions()` - Get note version history
- `search()` - Search notes by title/content/tags
- `showPublic()` - View public note by token

### **FileAttachmentController**
File uploads
- `upload()` - Upload file (max 10MB)
- `download()` - Download file
- `destroy()` - Delete file

### **EventController**
Manage calendar events
- `index()` - Get all user's events
- `show()` - Get one event
- `store()` - Create event
- `update()` - Update event
- `destroy()` - Delete event

### **TransactionController**
Manage budget transactions
- `index()` - Get all transactions
- `store()` - Create transaction
- `update()` - Update transaction
- `destroy()` - Delete transaction
- `summary()` - Get balance summary (income, expense, net)
- `reports()` - Get monthly reports with category breakdown
- `exportCsv()` - Export transactions as CSV

### **RecurringTransactionController**
Manage recurring transactions
- `index()` - Get all recurring transactions
- `store()` - Create recurring transaction
- `update()` - Update recurring transaction
- `destroy()` - Delete recurring transaction

### **BudgetController**
Manage budgets
- `index()` - Get budgets (filter by year/month/category)
- `store()` - Create/update budget
- `update()` - Update budget
- `destroy()` - Delete budget

### **GoalController**
Manage goals
- `index()` - Get all goals
- `store()` - Create goal
- `update()` - Update goal
- `destroy()` - Delete goal
- `complete()` - Mark goal as complete

### **TaskController**
Manage tasks
- `index()` - Get tasks for a goal
- `store()` - Create task (can have subtasks)
- `update()` - Update task
- `destroy()` - Delete task
- `complete()` - Mark task as complete

### **HabitController**
Manage habits
- `index()` - Get all habits
- `store()` - Create habit
- `markToday()` - Mark habit for today
- `history()` - Get habit history
- `destroy()` - Delete habit

### **UserSessionController**
Manage sessions
- `index()` - Get all user sessions
- `destroy()` - Revoke one session
- `revokeAll()` - Revoke all other sessions

### **Admin Controllers**
- `AdminDashboardController` - Analytics overview
- `AdminUserController` - User management
- `AdminAnnouncementController` - Send announcements
- `AdminAIController` - AI monitoring (placeholder)
- `AdminSubscriptionController` - Subscription management (placeholder)
- `AdminSystemController` - System settings (placeholder)
- `AdminLogController` - View system logs

---

## 🛣️ API Routes

### **Public Routes** (No authentication)
- `POST /api/signup` - Create account
- `POST /api/login` - Login
- `POST /api/forgot-password` - Request password reset
- `POST /api/reset-password` - Reset password
- `GET /api/auth/google` - Start Google login
- `GET /api/auth/google/callback` - Google callback
- `GET /api/notes/public/{token}` - View public note

### **Protected Routes** (Require Bearer token)

#### **User Management**
- `POST /api/logout` - Logout
- `GET /api/me` - Get current user
- `GET /api/sessions` - Get all sessions
- `DELETE /api/sessions/{id}` - Revoke session
- `POST /api/sessions/revoke-all` - Revoke all sessions
- `POST /api/google/disconnect` - Disconnect Google

#### **Semesters**
- `GET /api/semesters` - List semesters
- `GET /api/semesters/{id}` - Get semester
- `POST /api/semesters` - Create semester
- `PUT /api/semesters/{id}` - Update semester
- `DELETE /api/semesters/{id}` - Delete semester

#### **Courses**
- `GET /api/semesters/{semesterId}/courses` - List courses
- `POST /api/semesters/{semesterId}/courses` - Create course
- `PUT /api/courses/{courseId}` - Update course
- `DELETE /api/courses/{courseId}` - Delete course

#### **Assessments**
- `GET /api/courses/{courseId}/assessments` - List assessments
- `POST /api/courses/{courseId}/assessments` - Create assessment
- `GET /api/assessments/{assessmentId}` - Get assessment
- `PUT /api/assessments/{assessmentId}` - Update assessment
- `DELETE /api/assessments/{assessmentId}` - Delete assessment
- `POST /api/assessments/{id}/sync-google` - Sync to Google Calendar

#### **Notes**
- `GET /api/courses/{courseId}/notes` - List notes
- `POST /api/courses/{courseId}/notes` - Create note
- `GET /api/notes/{noteId}` - Get note
- `PUT /api/notes/{noteId}` - Update note
- `DELETE /api/notes/{noteId}` - Delete note
- `POST /api/notes/{noteId}/pin` - Pin/unpin note
- `POST /api/notes/{noteId}/favorite` - Favorite/unfavorite note
- `POST /api/notes/{noteId}/share` - Generate share link
- `POST /api/notes/{noteId}/revoke-share` - Revoke share link
- `GET /api/notes/{noteId}/versions` - Get version history
- `GET /api/notes/search` - Search notes

#### **File Attachments**
- `POST /api/attachments/upload` - Upload file
- `GET /api/attachments/{id}/download` - Download file
- `DELETE /api/attachments/{id}` - Delete file

#### **Events**
- `GET /api/events` - List events
- `GET /api/events/{id}` - Get event
- `POST /api/events` - Create event
- `PUT /api/events/{id}` - Update event
- `DELETE /api/events/{id}` - Delete event
- `POST /api/events/{id}/sync-google` - Sync to Google Calendar

#### **Transactions**
- `GET /api/transactions` - List transactions
- `POST /api/transactions` - Create transaction
- `PUT /api/transactions/{id}` - Update transaction
- `DELETE /api/transactions/{id}` - Delete transaction
- `GET /api/transactions/summary` - Get balance summary
- `GET /api/transactions/reports` - Get monthly reports
- `GET /api/transactions/export-csv` - Export CSV

#### **Recurring Transactions**
- `GET /api/recurring-transactions` - List recurring
- `POST /api/recurring-transactions` - Create recurring
- `PUT /api/recurring-transactions/{id}` - Update recurring
- `DELETE /api/recurring-transactions/{id}` - Delete recurring

#### **Budgets**
- `GET /api/budgets` - List budgets
- `POST /api/budgets` - Create budget
- `PUT /api/budgets/{id}` - Update budget
- `DELETE /api/budgets/{id}` - Delete budget

#### **Goals**
- `GET /api/goals` - List goals
- `POST /api/goals` - Create goal
- `PUT /api/goals/{id}` - Update goal
- `DELETE /api/goals/{id}` - Delete goal
- `POST /api/goals/{id}/complete` - Mark complete

#### **Tasks**
- `GET /api/goals/{goalId}/tasks` - List tasks
- `POST /api/goals/{goalId}/tasks` - Create task
- `PUT /api/tasks/{id}` - Update task
- `POST /api/tasks/{id}/complete` - Mark complete
- `DELETE /api/tasks/{id}` - Delete task

#### **Habits**
- `GET /api/habits` - List habits
- `POST /api/habits` - Create habit
- `POST /api/habits/{habitId}/mark-today` - Mark for today
- `GET /api/habits/{habitId}/history` - Get history
- `DELETE /api/habits/{habitId}` - Delete habit

### **Admin Routes** (Require admin role)
- `GET /api/admin/analytics` - Dashboard analytics
- `GET /api/admin/users` - List users
- `GET /api/admin/users/{id}` - Get user
- `PUT /api/admin/users/{id}/role` - Change role
- `POST /api/admin/users/{id}/suspend` - Suspend user
- `POST /api/admin/users/{id}/activate` - Activate user
- `DELETE /api/admin/users/{id}` - Delete user
- `GET /api/admin/announcements` - List announcements
- `POST /api/admin/announcements` - Create announcement
- `POST /api/admin/announcements/{id}/send` - Send announcement
- `GET /api/admin/logs` - View logs
- And more...

---

## ✨ Key Features

### **1. Authentication**
- Email/password signup and login
- Google OAuth login
- Password reset via email
- Session tracking
- Token-based authentication (Laravel Sanctum)

### **2. Academic Management**
- Semesters management
- Courses per semester
- Assessments (quizzes, exams, assignments)
- GPA calculation ready (via assessments)

### **3. Notes System**
- Markdown support
- Version history (last 10 versions)
- Pin/favorite notes
- Full-text search
- Shareable public links
- Tags for organization
- File attachments

### **4. Calendar & Events**
- Create events with date/time/location
- Reminders
- Google Calendar sync (one-way: app → Google)

### **5. Budget Management**
- Income/expense tracking
- Categories
- Recurring transactions (daily/weekly/monthly/yearly)
- Monthly budgets per category
- Balance summary
- Monthly reports
- CSV export

### **6. Goals & Tasks**
- Goals with target dates
- Tasks under goals
- Subtasks support
- Mark complete functionality

### **7. Habit Tracker**
- Create habits
- Daily tracking
- History view
- Frequency types (daily/weekly/monthly)

### **8. File Attachments**
- Upload files (max 10MB)
- Attach to notes or assessments
- Download files
- Local storage

### **9. Admin Dashboard**
- User management
- Announcements (send via email)
- Analytics overview
- System logs
- Role-based access control

### **10. Google Integration**
- Google login
- Google Calendar sync
- Account linking

---

## 🔐 Security Features

- Password hashing (bcrypt)
- Token-based authentication
- Encrypted Google tokens
- User ownership validation (users can only access their own data)
- Admin middleware for admin routes
- File upload validation
- SQL injection protection (Eloquent ORM)
- XSS protection (Laravel built-in)

---

## 📦 Technologies Used

- **Laravel 12** - PHP framework
- **Laravel Sanctum** - API authentication
- **Laravel Socialite** - Google OAuth
- **MySQL/SQLite** - Database
- **Eloquent ORM** - Database queries

---

## 📝 Notes

- All dates use Laravel's Carbon for date handling
- File uploads stored in `storage/app/public/attachments`
- Google tokens encrypted before storing
- Version history keeps last 10 versions automatically
- Sessions tracked automatically on login
- Email sending configured with Gmail SMTP

---

## 🚀 How to Use

1. **Setup**: Run `php artisan migrate` to create tables
2. **Login**: Use `/api/login` or `/api/auth/google`
3. **Get Token**: Copy token from login response
4. **Use API**: Add `Authorization: Bearer TOKEN` header
5. **Test**: Use Postman or frontend to call endpoints

---

**End of Report**

