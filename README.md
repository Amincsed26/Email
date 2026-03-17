# Email Management System

A robust, full-stack web application designed to simulate a high-performance mail server. This project focuses on implementing complex data structures, design patterns, and a seamless RESTful communication layer between a Java backend and a Vue.js frontend.

---

## 🏗️ Architecture & Design Patterns
The system is built with scalability and clean code principles in mind:
* **Singleton Pattern:** Used for managing global configurations and user sessions.
* **Factory Pattern:** Implemented for creating different types of mail folders (Inbox, Sent, Trash).
* **Command Pattern:** Utilized to handle undo/redo operations for mail actions.
* **Proxy Pattern:** For lazy loading of large email attachments.

---

## 📂 Repository Breakdown

### 🖥️ Backend (Spring Boot)
The server handles business logic, data persistence, and security.
* `controller/`: Defines the REST endpoints for Mail, User, and Folder operations.
* `service/`: Contains the core logic (e.g., sorting algorithms, searching criteria).
* `model/`: Java POJOs representing `User`, `Email`, and `Attachment` entities.
* `criteria/`: Implementation of the **Criteria Pattern** for advanced filtering.

### 🎨 Frontend (Vue.js)
A reactive interface built for speed and user experience.
* `components/`: Reusable UI elements like `MailRow.vue`, `Sidebar.vue`, and `ComposeModal.vue`.
* `store/`: Vuex modules for centralized state management (User auth, current folder).
* `router/`: Handles navigation between the login suite and the dashboard.
* `assets/`: Global styles and static resources.

---

## 📊 Core Functionalities
| Feature | Description |
| :--- | :--- |
| **Dynamic Sorting** | Sort emails by Date, Priority, Sender, or Subject using a custom Sort engine. |
| **Folder Management** | Create, rename, and delete custom folders with drag-and-drop capability. |
| **Search Engine** | Multi-attribute search (e.g., searching "Invoice" within "Sent" folder). |
| **Bulk Actions** | Select multiple emails for mass deletion or moving to folders. |
| **Priority Queue** | System-level handling of urgent emails based on user-defined flags. |

---

## 🛠️ Tech Stack
* **Language:** Java 17+, JavaScript (ES6+)
* **Frameworks:** Spring Boot 3.x, Vue.js 3.x
* **Build Tools:** Maven, npm
* **API Style:** RESTful JSON
