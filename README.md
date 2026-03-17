# 📧 Email Management System

> A robust, full-stack web application designed to simulate a high-performance mail server.

This project focuses on implementing complex data structures, design patterns, and a seamless RESTful communication layer between a **Java Spring Boot** backend and a **Vue.js** frontend.

---

## 🛠️ Tech Stack

| Layer     | Technology                        |
|-----------|-----------------------------------|
| Backend   | Java 17+, Spring Boot 3.x, Maven  |
| Frontend  | Vue.js 3.x, Vuex, Vue Router, npm |
| API Style | RESTful JSON                      |

---

## 📁 Project Structure

```
Email/
├── Backend/
│   ├── controller/       # REST endpoints for Mail, User, and Folder operations
│   ├── service/          # Core business logic (sorting, searching, filtering)
│   ├── model/            # Java POJOs: User, Email, Attachment
│   └── criteria/         # Criteria Pattern for advanced email filtering
├── Frontend/
│   ├── components/       # Reusable UI: MailRow.vue, Sidebar.vue, ComposeModal.vue
│   ├── store/            # Vuex modules (auth, current folder state)
│   ├── router/           # Navigation between login and dashboard
│   └── assets/           # Global styles and static resources
└── README.md
```

---

## 🏗️ Architecture & Design Patterns

| Pattern           | Usage                                                              |
|-------------------|--------------------------------------------------------------------|
| **Singleton**     | Managing global configurations and user sessions                   |
| **Factory**       | Creating different types of mail folders (Inbox, Sent, Trash)      |
| **Command**       | Handling undo/redo operations for mail actions                     |
| **Proxy**         | Lazy loading of large email attachments                            |
| **Criteria**      | Advanced multi-attribute email filtering                           |

---

## ✨ Core Features

| Feature              | Description                                                                 |
|----------------------|-----------------------------------------------------------------------------|
| **Dynamic Sorting**  | Sort emails by Date, Priority, Sender, or Subject using a custom sort engine |
| **Folder Management**| Create, rename, and delete custom folders with drag-and-drop capability      |
| **Search Engine**    | Multi-attribute search (e.g., search "Invoice" within the "Sent" folder)    |
| **Bulk Actions**     | Select multiple emails for mass deletion or moving to folders               |
| **Priority Queue**   | System-level handling of urgent emails based on user-defined flags          |
| **Undo / Redo**      | Reverse or replay mail actions via the Command pattern                      |
| **Lazy Attachments** | Large attachments are loaded on demand via the Proxy pattern                |

---

## ⚙️ Setup

### Prerequisites

- **Java 17+**
- **Maven**
- **Node.js & npm**

Verify:

```bash
java --version
mvn --version
node --version
npm --version
```

---

### 1. Backend Setup

```bash
cd Backend
mvn clean install
mvn spring-boot:run
```

The backend API will start at `http://localhost:8080`.

---

### 2. Frontend Setup

```bash
cd Frontend
npm install
npm run dev
```

The frontend dev server will start at `http://localhost:5173`.

---

## 📡 API Documentation

All endpoints are prefixed with `/api`. Base URL: `http://localhost:8080`.

---

### 🔐 Authentication

| Method | Endpoint             | Description           | Auth Required |
|--------|----------------------|-----------------------|---------------|
| POST   | `/api/auth/register` | Register a new user   | No            |
| POST   | `/api/auth/login`    | Login and get a token | No            |
| POST   | `/api/auth/logout`   | Logout current user   | Yes           |

**Register — Request Body:**
```json
{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "SecurePass@123"
}
```

**Login — Request Body:**
```json
{
  "email": "john@example.com",
  "password": "SecurePass@123"
}
```

**Login — Response:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "userId": 1,
  "username": "john_doe"
}
```

---

### 📬 Emails

| Method | Endpoint                        | Description                          | Auth Required |
|--------|---------------------------------|--------------------------------------|---------------|
| GET    | `/api/emails`                   | Get all emails for current user      | Yes           |
| GET    | `/api/emails/{id}`              | Get a specific email by ID           | Yes           |
| GET    | `/api/emails/folder/{folderId}` | Get all emails in a specific folder  | Yes           |
| POST   | `/api/emails`                   | Compose and send a new email         | Yes           |
| PATCH  | `/api/emails/{id}/read`         | Mark an email as read                | Yes           |
| PATCH  | `/api/emails/{id}/unread`       | Mark an email as unread              | Yes           |
| PATCH  | `/api/emails/{id}/move`         | Move email to a different folder     | Yes           |
| PATCH  | `/api/emails/{id}/priority`     | Set priority flag on an email        | Yes           |
| DELETE | `/api/emails/{id}`              | Delete (trash) an email              | Yes           |
| DELETE | `/api/emails/{id}/permanent`    | Permanently delete an email          | Yes           |
| POST   | `/api/emails/bulk/delete`       | Bulk delete multiple emails          | Yes           |
| POST   | `/api/emails/bulk/move`         | Bulk move multiple emails to folder  | Yes           |

**Send Email — Request Body:**
```json
{
  "to": ["recipient@example.com"],
  "cc": ["cc@example.com"],
  "bcc": [],
  "subject": "Project Update",
  "body": "Hi, here is the latest update...",
  "priority": "NORMAL",
  "attachments": []
}
```

**Email Object:**
```json
{
  "id": 42,
  "from": "john@example.com",
  "to": ["recipient@example.com"],
  "subject": "Project Update",
  "body": "Hi, here is the latest update...",
  "isRead": false,
  "priority": "NORMAL",
  "folderId": 1,
  "sentAt": "2025-06-01T10:00:00",
  "attachments": []
}
```

> `priority` can be: `LOW`, `NORMAL`, `HIGH`, `URGENT`

---

### 🔍 Search & Sort

| Method | Endpoint           | Description                          | Auth Required |
|--------|--------------------|--------------------------------------|---------------|
| GET    | `/api/emails/search` | Search emails by multiple attributes | Yes           |
| GET    | `/api/emails/sort`   | Get emails sorted by a given field   | Yes           |

**Search — Query Parameters:**

| Param      | Type   | Description                                  |
|------------|--------|----------------------------------------------|
| `query`    | String | Keyword to search in subject, body, or sender |
| `folderId` | Long   | (Optional) Limit search to a specific folder  |
| `from`     | String | (Optional) Filter by sender address           |
| `subject`  | String | (Optional) Filter by subject keyword          |
| `hasAttachment` | Boolean | (Optional) Filter emails with attachments |

**Example:**
```
GET /api/emails/search?query=Invoice&folderId=2
```

**Sort — Query Parameters:**

| Param     | Type   | Options                                  |
|-----------|--------|------------------------------------------|
| `sortBy`  | String | `date`, `sender`, `subject`, `priority`  |
| `order`   | String | `asc`, `desc`                            |
| `folderId`| Long   | (Optional) Limit to a specific folder    |

**Example:**
```
GET /api/emails/sort?sortBy=date&order=desc&folderId=1
```

---

### 📁 Folders

| Method | Endpoint              | Description                  | Auth Required |
|--------|-----------------------|------------------------------|---------------|
| GET    | `/api/folders`        | Get all folders for the user | Yes           |
| GET    | `/api/folders/{id}`   | Get a specific folder        | Yes           |
| POST   | `/api/folders`        | Create a new custom folder   | Yes           |
| PUT    | `/api/folders/{id}`   | Rename a folder              | Yes           |
| DELETE | `/api/folders/{id}`   | Delete a folder              | Yes           |

**Folder Object:**
```json
{
  "id": 5,
  "name": "Work",
  "type": "CUSTOM",
  "emailCount": 12,
  "unreadCount": 3
}
```

> System folders: `INBOX`, `SENT`, `TRASH`, `DRAFTS`, `SPAM`  
> User-created folders have type: `CUSTOM`

---

### 📎 Attachments

| Method | Endpoint                              | Description                      | Auth Required |
|--------|---------------------------------------|----------------------------------|---------------|
| GET    | `/api/attachments/{id}`               | Download/fetch an attachment     | Yes           |
| POST   | `/api/emails/{emailId}/attachments`   | Upload an attachment to an email | Yes           |
| DELETE | `/api/attachments/{id}`               | Remove an attachment             | Yes           |

**Attachment Object:**
```json
{
  "id": 7,
  "emailId": 42,
  "filename": "report_q3.pdf",
  "mimeType": "application/pdf",
  "sizeBytes": 204800
}
```

---

### 👤 Users

| Method | Endpoint            | Description               | Auth Required |
|--------|---------------------|---------------------------|---------------|
| GET    | `/api/users/{id}`   | Get user profile          | Yes           |
| PUT    | `/api/users/{id}`   | Update user profile       | Yes           |
| PATCH  | `/api/users/{id}/password` | Change password    | Yes           |
| DELETE | `/api/users/{id}`   | Delete user account       | Yes           |

**User Object:**
```json
{
  "id": 1,
  "username": "john_doe",
  "email": "john@example.com",
  "createdAt": "2024-01-15T08:30:00"
}
```

---

### ↩️ Undo / Redo

| Method | Endpoint        | Description                            | Auth Required |
|--------|-----------------|----------------------------------------|---------------|
| POST   | `/api/actions/undo` | Undo the last mail action          | Yes           |
| POST   | `/api/actions/redo` | Redo the last undone mail action   | Yes           |

> Supported undoable actions: send, delete, move, mark as read/unread, set priority.

---

### ❗ Error Responses

All errors follow this structure:

```json
{
  "status": 404,
  "error": "Not Found",
  "message": "Email with ID 99 not found",
  "timestamp": "2025-06-01T10:00:00"
}
```

| Status Code | Meaning               |
|-------------|-----------------------|
| 200         | OK                    |
| 201         | Created               |
| 400         | Bad Request           |
| 401         | Unauthorized          |
| 403         | Forbidden             |
| 404         | Not Found             |
| 500         | Internal Server Error |
