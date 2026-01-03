# Student Tracker Frontend

A modern, professional frontend for the Student Tracker application built with Tailwind CSS and vanilla JavaScript.

## 🚀 Getting Started

### Prerequisites
- A web server (or use a simple HTTP server)
- Backend API running (default: `http://localhost:8000/api`)

### Setup

1. **Update API URL** (if needed):
   - Open `js/api.js`
   - Update `API_BASE_URL` to match your backend URL

2. **Start a local server**:
   ```bash
   # Using Python 3
   python -m http.server 8080
   
   # Using Node.js (http-server)
   npx http-server -p 8080
   
   # Using PHP
   php -S localhost:8080
   ```

3. **Open in browser**:
   - Navigate to `http://localhost:8080`
   - Start with `/login.html` or `/register.html`

## 📁 Project Structure

```
Frontend/
├── index.html              # Dashboard (protected)
├── login.html              # Login page
├── register.html           # Registration page
├── forgot-password.html    # Password reset request
├── js/
│   ├── api.js             # API helper functions
│   ├── auth.js            # Authentication & route protection
│   └── utils.js           # Utility functions
└── README.md
```

## 🔐 Authentication Flow

1. **Login/Register**: User authenticates and receives a token
2. **Token Storage**: Token is stored in `localStorage` as `auth_token`
3. **Route Protection**: Protected pages check for token on load
4. **API Requests**: Token is automatically included in Authorization header
5. **Logout**: Token is removed from storage

## 📝 API Functions

### `api.js` provides:
- `login(email, password)` - Login user
- `register(userData)` - Register new user
- `logout()` - Logout user
- `getCurrentUser()` - Get current user info
- `forgotPassword(email)` - Request password reset
- `resetPassword(token, email, password, password_confirmation)` - Reset password

## 🎨 Features

- ✅ Professional, modern UI with Tailwind CSS
- ✅ Responsive design (mobile-friendly)
- ✅ Form validation
- ✅ Password visibility toggle
- ✅ Loading states
- ✅ Error handling
- ✅ Route protection
- ✅ Token management
- ✅ Google OAuth support (links to backend)

## 🧪 Testing Checklist

- [ ] Login with valid credentials
- [ ] Register new account
- [ ] Token stored in localStorage after login
- [ ] Refresh page → user still logged in
- [ ] Access protected route without token → redirects to login
- [ ] Access login/register when already logged in → redirects to dashboard
- [ ] Logout works and redirects to login
- [ ] Forgot password sends email
- [ ] Error messages display correctly
- [ ] Form validation works

## 🔧 Configuration

### Update Backend URL
Edit `js/api.js`:
```javascript
const API_BASE_URL = 'http://your-backend-url/api';
```

### Update Google OAuth URL
Edit the Google login links in `login.html` and `register.html`:
```html
href="http://your-backend-url/api/auth/google"
```

## 📱 Pages

### `/login.html`
- Email/password login
- Google OAuth button
- Link to register
- Link to forgot password

### `/register.html`
- Full registration form
- Optional fields: major, university
- Password confirmation
- Google OAuth button
- Link to login

### `/forgot-password.html`
- Email input for password reset
- Instructions message
- Link back to login

### `/index.html`
- Protected dashboard
- Shows user information
- Logout button
- Quick action cards

## 🛡️ Security Notes

- Tokens stored in localStorage (consider httpOnly cookies for production)
- All API requests include Authorization header
- Route protection prevents unauthorized access
- Form validation on client-side (backend also validates)

## 🚧 Next Steps

After Phase 1 is complete, you can add:
- Reset password page (with token from email)
- Dashboard features
- Semester management
- Course management
- Notes system
- Budget tracker
- And more...

---

**Note**: Make sure your backend CORS is configured to allow requests from your frontend domain.

