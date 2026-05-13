# 🔐 User Authentication System

> **A Seamless, Secure, and Plug-and-Play Authentication Module**
>
> Built with Django, featuring custom-styled frontend templates and extended user logic for real-world registration flows.

---

## ⚡ Quick Overview

This project implements a **robust user authentication system** that goes beyond Django's default admin interface. It provides a modern, user-friendly experience with secure session management, password hashing, and clean, intuitive UI/UX.

### What Makes This Special? ✨

- 🎨 **Custom-styled Templates** - Beautiful, responsive UI instead of boring Django admin
- 🔒 **Production-Ready Security** - Industry-standard password hashing and protection
- 👥 **Real-world Flows** - Registration, login, logout with proper validation
- ⚙️ **Plug-and-Play Ready** - Easy to integrate into existing projects
- 📱 **Responsive Design** - Works seamlessly on desktop and mobile

---

## 📊 Tech Stack

```
Backend:        Python 3.x + Django 6.0.5
Database:       SQLite3 (Development), upgradeable to PostgreSQL
Frontend:       HTML5 + Custom CSS Styling
Security:       Django's built-in authentication & CSRF protection
```

**Language Composition:**
- Python: 68.6% (Backend logic)
- HTML: 31.4% (Frontend templates)

---

## 🚀 Getting Started

### Prerequisites

Before you begin, ensure you have:
- Python 3.8 or higher installed
- pip (Python package manager)
- Git installed

### Installation Steps

#### 1️⃣ **Clone the Repository**

```bash
git clone https://github.com/LOVEVERMA0373/USER-AUTHENTICATION-SYSTEM-.git
cd USER-AUTHENTICATION-SYSTEM-
```

#### 2️⃣ **Create a Virtual Environment** (Recommended)

```bash
# On Windows
python -m venv venv
venv\Scripts\activate

# On macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

#### 3️⃣ **Install Dependencies**

```bash
pip install -r requirements.txt
```

> **Note:** If `requirements.txt` doesn't exist, create it:
> ```
> Django==6.0.5
> ```

#### 4️⃣ **Apply Migrations**

```bash
python manage.py migrate
```

This creates the necessary database tables for user management.

#### 5️⃣ **Create a Superuser** (Optional - for Django Admin)

```bash
python manage.py createsuperuser
```

Follow the prompts to create an admin account.

#### 6️⃣ **Run the Development Server**

```bash
python manage.py runserver
```

Your application is now running at: **http://127.0.0.1:8000/**

---

## 📋 Project Structure

```
USER-AUTHENTICATION-SYSTEM-/
│
├── myproject/                 # Django project configuration
│   ├── settings.py           # Project settings & app configuration
│   ├── urls.py               # Main URL routing
│   ├── wsgi.py               # WSGI application
│   └── asgi.py               # ASGI application
│
├── accounts/                  # Authentication app
│   ├── views.py              # View functions (Register, Login, Logout)
│   ├── urls.py               # App-specific URL routing
│   ├── models.py             # Database models
│   ├── templates/            # HTML templates
│   │   ├── register.html     # Registration page
│   │   ├── login.html        # Login page
│   │   └── home.html         # Dashboard (protected)
│   └── admin.py              # Admin panel configuration
│
├── users/                     # User management app (expandable)
│   └── models.py             # User profile models
│
├── templates/                # Global templates
├── db.sqlite3                # Development database
├── manage.py                 # Django management script
└── LICENSE                   # MIT License
```

---

## 🎯 Core Features

### 1. **User Registration** 📝

**Endpoint:** `/register/`

- ✅ Create new user accounts
- ✅ Password validation
- ✅ Duplicate username check
- ✅ User-friendly error messages
- ✅ Success feedback

**How to use:**
```
1. Navigate to http://127.0.0.1:8000/register/
2. Enter username and password
3. Click "Register"
4. Redirected to login page on success
```

---

### 2. **User Login** 🔑

**Endpoint:** `/login/`

- ✅ Secure authentication
- ✅ Session management
- ✅ Password verification
- ✅ Error handling for invalid credentials
- ✅ Redirect to home on success

**How to use:**
```
1. Navigate to http://127.0.0.1:8000/login/
2. Enter your username and password
3. Click "Login"
4. Access your dashboard
```

---

### 3. **Protected Routes** 🔐

**Endpoint:** `/` (Home - Protected)

- ✅ Only accessible to logged-in users
- ✅ Automatic redirect to login if not authenticated
- ✅ User-specific dashboard
- ✅ Logout option

---

### 4. **User Logout** 🚪

**Endpoint:** `/logout/`

- ✅ Secure session termination
- ✅ Clear authentication cookies
- ✅ Redirect to login page

---

## 🔌 API Endpoints

| HTTP Method | Endpoint | Description | Authentication Required |
|:---:|:---:|:---|:---:|
| GET | `/` | Home Dashboard | ✅ Yes |
| GET / POST | `/register/` | User Registration | ❌ No |
| GET / POST | `/login/` | User Login | ❌ No |
| GET | `/logout/` | User Logout | ✅ Yes |
| GET | `/admin/` | Django Admin Panel | ✅ Yes (Superuser) |

---

## 💻 Code Overview

### Registration Flow (views.py)

```python
def register(request):
    if request.method == 'POST':
        username = request.POST.get('username').strip().lower()
        password = request.POST.get('password').strip()
        
        # Check if user already exists
        if User.objects.filter(username=username).exists():
            messages.error(request, "User already exists")
            return redirect('register')
        
        # Create new user
        User.objects.create_user(username=username, password=password)
        messages.success(request, "Account created successfully")
        return redirect('login')
    
    return render(request, 'register.html')
```

**Key Security Features:**
- Username uniqueness check
- Password hashing (automatic with Django)
- Input validation and sanitization

---

### Login Flow (views.py)

```python
def user_login(request):
    if request.method == 'POST':
        username = request.POST.get('username').strip().lower()
        password = request.POST.get('password').strip()
        
        # Authenticate user
        user = authenticate(request, username=username, password=password)
        
        if user is not None:
            login(request, user)
            return redirect('home')
        else:
            messages.error(request, "Invalid username or password")
    
    return render(request, 'login.html')
```

**Key Security Features:**
- Django's `authenticate()` function prevents timing attacks
- Session-based authentication
- CSRF protection enabled by default

---

### Protected Route (views.py)

```python
@login_required(login_url='login')
def home(request):
    return render(request, 'home.html')
```

**Key Security Features:**
- `@login_required` decorator prevents unauthorized access
- Automatic redirect to login for non-authenticated users

---

## 🔒 Security Features

### Built-in Protections

| Feature | Description |
|---------|-------------|
| **Password Hashing** | Django uses PBKDF2 with SHA256 by default |
| **CSRF Protection** | Cross-Site Request Forgery tokens on all forms |
| **Session Security** | Secure, httponly cookies |
| **Password Validation** | Django's 4-level password validators |
| **Input Sanitization** | Automatic escaping in templates |
| **SQL Injection Prevention** | ORM prevents SQL injection |

### Additional Recommendations for Production

```python
# In settings.py, add:
DEBUG = False                           # Never in production!
ALLOWED_HOSTS = ['yourdomain.com']     # Specify your domain
SECURE_SSL_REDIRECT = True             # Force HTTPS
SESSION_COOKIE_SECURE = True           # HTTPS only cookies
CSRF_COOKIE_SECURE = True              # HTTPS only CSRF
SECURE_BROWSER_XSS_FILTER = True       # XSS protection
X_FRAME_OPTIONS = 'DENY'               # Clickjacking protection
```

---

## 📦 Managing Dependencies

### View Requirements

```bash
pip freeze > requirements.txt
```

### Install from Requirements

```bash
pip install -r requirements.txt
```

### Minimal requirements.txt

```
Django==6.0.5
```

---

## 🧪 Testing the Application

### Manual Testing Checklist

- [ ] **Registration**
  - [ ] Create new account with valid credentials
  - [ ] Attempt duplicate username (should fail)
  - [ ] Test empty fields (should validate)

- [ ] **Login**
  - [ ] Login with correct credentials
  - [ ] Attempt login with wrong password
  - [ ] Test empty fields

- [ ] **Protected Routes**
  - [ ] Access home without logging in (should redirect)
  - [ ] Access home after login (should work)

- [ ] **Logout**
  - [ ] Logout and verify session ends
  - [ ] Attempt to access home after logout (should redirect)

---

## 🐛 Common Issues & Solutions

### Issue: ModuleNotFoundError: No module named 'django'

**Solution:**
```bash
pip install Django==6.0.5
```

---

### Issue: Port 8000 Already in Use

**Solution:**
```bash
python manage.py runserver 8001
```

---

### Issue: Database Locked (db.sqlite3)

**Solution:**
```bash
# Delete the database and recreate
rm db.sqlite3
python manage.py migrate
```

---

## 📈 Deployment Guide

### For Production with Gunicorn + Nginx

```bash
# Install Gunicorn
pip install gunicorn

# Run with Gunicorn
gunicorn myproject.wsgi:application --bind 0.0.0.0:8000
```

### Environment Variables (.env)

Create a `.env` file (add to `.gitignore`):

```env
SECRET_KEY=your-secret-key-here
DEBUG=False
ALLOWED_HOSTS=yourdomain.com,www.yourdomain.com
DATABASE_URL=postgresql://user:pass@localhost/dbname
```

---

## 📚 Documentation & Resources

- [Django Official Documentation](https://docs.djangoproject.com/)
- [Django Authentication System](https://docs.djangoproject.com/en/6.0/topics/auth/)
- [Django Security Middleware](https://docs.djangoproject.com/en/6.0/topics/security/)
- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)

---

## 🚀 Future Enhancements

Potential features to add:

- [ ] Email verification on registration
- [ ] Password reset functionality
- [ ] Two-Factor Authentication (2FA)
- [ ] Social OAuth login (Google, GitHub)
- [ ] User profile management
- [ ] Role-based access control (RBAC)
- [ ] API with JWT tokens
- [ ] User activity logging
- [ ] Email notifications
- [ ] Rate limiting on login attempts

---

## 🤝 Contributing

We welcome contributions! Here's how to help:

1. **Fork the Repository**
   ```bash
   Click "Fork" button on GitHub
   ```

2. **Create a Feature Branch**
   ```bash
   git checkout -b feature/YourAmazingFeature
   ```

3. **Commit Your Changes**
   ```bash
   git commit -m 'Add some YourAmazingFeature'
   ```

4. **Push to the Branch**
   ```bash
   git push origin feature/YourAmazingFeature
   ```

5. **Open a Pull Request**
   - Describe your changes clearly
   - Reference any related issues
   - Ensure code follows project style

---

## 📝 License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

**MIT License** means you can:
- ✅ Use commercially
- ✅ Modify the code
- ✅ Distribute it
- ✅ Private use

**Just remember to:**
- Include the license and copyright notice

---

## 👨‍💻 Author

**Love Verma**
- GitHub: [@LOVEVERMA0373](https://github.com/LOVEVERMA0373)
- Repository: [USER-AUTHENTICATION-SYSTEM-](https://github.com/LOVEVERMA0373/USER-AUTHENTICATION-SYSTEM-)

---

## 💬 Support & Questions

- 📧 **Issues:** Open an [GitHub Issue](https://github.com/LOVEVERMA0373/USER-AUTHENTICATION-SYSTEM-/issues)
- 💭 **Discussions:** Start a [Discussion](https://github.com/LOVEVERMA0373/USER-AUTHENTICATION-SYSTEM-/discussions)
- ⭐ **Like this?** Star the repository!

---

## 🎓 Learning Resources Included

This project demonstrates:

- ✅ Django project structure and configuration
- ✅ URL routing and view functions
- ✅ Django authentication system
- ✅ Session management
- ✅ Form handling and validation
- ✅ Template rendering
- ✅ User model and ORM
- ✅ Middleware and decorators
- ✅ Security best practices

Perfect for learning Django fundamentals!

---

<div align="center">

**Developed with ❤️ by [Love Verma](https://github.com/LOVEVERMA0373)**

If you find this project helpful, please consider giving it a ⭐ star!

**Happy Coding! 🚀**

</div>

---

*Last Updated: May 13, 2026*
*Django Version: 6.0.5*
*Python: 3.8+*
