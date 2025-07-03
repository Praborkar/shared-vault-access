# 🔐 Shared Vault Access – Secure File Sharing Backend

A secure and role-based file sharing system built using **FastAPI** and **MongoDB Atlas**, designed to allow controlled file upload and secure sharing via encrypted download links and verified user access.

---

## 👤 Author

**Prashansa Agarwal**

---

## 📌 Specifications

### ⚙️ Tech Stack

| Layer         | Technology               |
|---------------|--------------------------|
| **Backend**   | FastAPI (Python 3.10+)    |
| **Database**  | MongoDB Atlas (NoSQL)     |
| **Auth**      | JWT (OAuth2 password flow)|
| **File Storage** | Local filesystem (`/uploads`) |
| **Email Service** | SMTP-based (configurable) |
| **Server**    | Uvicorn (ASGI)            |
| **Testing**   | Pytest                    |
| **API Testing** | Postman + Swagger UI     |
| **Containerization** | Docker               |

---

## 👥 User Roles

### 1. **OpsUser**
- Login with credentials
- Upload files (only `.pptx`, `.docx`, `.xlsx`)
- No access to download or list files

### 2. **ClientUser**
- Sign up (returns an encrypted verification URL)
- Verify email via token
- Login to receive JWT
- List all uploaded files
- Request secure download link for a file
- Access download only via authorized encrypted link

---

## 🔐 Authentication & Security
- JWT-based authentication for both user types
- Role-based access guards using FastAPI dependencies
- Passwords hashed with `bcrypt`
- Encrypted download links (UUID-based with HMAC or Fernet)
- Links are client-specific and access-controlled
- Unauthorized attempts result in `403 Forbidden`

---

## 📁 File Handling

| Action | Description |
|--------|-------------|
| Upload | Allowed only for OpsUser. Validates file types. |
| Storage | Files saved under `uploads/` locally. |
| Download | Secure encrypted token required. Access granted to linked ClientUser only. |

---

## ✉️ Email Verification

- On signup, ClientUser receives a verification email.
- Verification is done by visiting:  
  `/client/verify/{token}`
- Accounts remain inactive until verified.

---

## 🔧 API Endpoints

| Method | Endpoint | Role | Description |
|--------|----------|------|-------------|
| POST   | `/ops/login`         | OpsUser   | Login |
| POST   | `/ops/upload`        | OpsUser   | Upload `.pptx`, `.docx`, `.xlsx` files |
| POST   | `/client/signup`     | ClientUser | Signup + send verification token |
| GET    | `/client/verify/{token}` | ClientUser | Verify email via token |
| POST   | `/client/login`      | ClientUser | Login |
| GET    | `/client/files`      | ClientUser | List all uploaded files |
| GET    | `/client/download/{file_id}` | ClientUser | Generate secure download link |
| GET    | `/client/download/file/{secure_token}` | ClientUser | Download file securely |

---

## 📬 Environment Variables

Copy `.env.example` to `.env` and update:

```env
MONGO_URI=mongodb+srv://<user>:<pass>@cluster.mongodb.net/securefiles
SECRET_KEY=your_jwt_secret_key
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_email_password
