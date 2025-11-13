A **detailed breakdown of responsibilities, APIs, and data ownership** for each service in your CMS platform (using **Go + gRPC**) 👇

---

## 🧩 1. **auth-service**

**Purpose:** Handles authentication and token management.

**Responsibilities:**

- User login/signup
- Issue JWT or session tokens
- Token refresh & validation

**APIs (gRPC methods):**

- `RegisterUser(email, password)`
- `Login(email, password)`
- `ValidateToken(token)`
- `RefreshToken(refreshToken)`

**DB Tables:**

- `users_auth` (id, email, hashed_password, created_at)

---

## 👤 2. **user-service**

**Purpose:** Manage user profiles & preferences.

**Responsibilities:**

- User profile CRUD
- Avatar, bio, social links
- Roles and permissions sync (with rbac-service)

**APIs:**

- `GetUserProfile(userId)`
- `UpdateProfile(userId, profileData)`
- `ListUsers()`

**DB Tables:**

- `users` (id, name, email, avatar, bio, created_at)

---

## 🛡️ 3. **rbac-service**

**Purpose:** Role-Based Access Control (authorization layer).

**Responsibilities:**

- Manage roles (admin, editor, viewer)
- Assign permissions to users
- Check authorization for actions

**APIs:**

- `AssignRole(userId, role)`
- `CheckPermission(userId, action, resource)`
- `ListRoles()`

**DB Tables:**

- `roles` (id, name)
- `permissions` (id, action, resource)
- `role_permissions` (role_id, permission_id)
- `user_roles` (user_id, role_id)

---

## 📰 4. **content-service**

**Purpose:** Core CMS content management.

**Responsibilities:**

- Create, edit, publish articles/pages
- Versioning and scheduling
- Tagging & categorization

**APIs:**

- `CreateContent(title, body, authorId)`
- `UpdateContent(contentId, newData)`
- `GetContentById(contentId)`
- `ListContent(filter)`
- `PublishContent(contentId)`

**DB Tables:**

- `contents` (id, title, body, status, author_id, created_at, updated_at)
- `tags` (id, name)
- `content_tags` (content_id, tag_id)

---

## 💬 5. **comment-service**

**Purpose:** Handle comments for content.

**Responsibilities:**

- Create, edit, delete comments
- Nested comment threads
- Moderation flags

**APIs:**

- `AddComment(contentId, userId, text)`
- `ListComments(contentId)`
- `DeleteComment(commentId)`
- `FlagComment(commentId, reason)`

**DB Tables:**

- `comments` (id, content_id, user_id, text, parent_id, status, created_at)

---

## 📸 6. **media-service**

**Purpose:** Upload and manage images, videos, files.

**Responsibilities:**

- Upload & store media
- Generate thumbnails / metadata
- Serve URLs

**APIs:**

- `UploadMedia(file)`
- `GetMedia(mediaId)`
- `ListMedia(userId)`
- `DeleteMedia(mediaId)`

**DB Tables:**

- `media` (id, user_id, filename, url, type, size, created_at)

---

## 🔍 7. **search-service**

**Purpose:** Full-text search across content, users, and tags.

**Responsibilities:**

- Index and search content
- Real-time indexing on updates

**APIs:**

- `IndexContent(contentId)`
- `Search(query, filters)`
- `ReindexAll()`

**DB / Engine:**

- ElasticSearch / Meilisearch / Bleve (in Go)

---

## 🧾 8. **subscription-service**

**Purpose:** Manages user subscriptions and premium access.

**Responsibilities:**

- Subscription plans
- Payment gateway integration
- Check active subscription

**APIs:**

- `CreateSubscription(userId, planId)`
- `CancelSubscription(userId)`
- `CheckSubscription(userId)`
- `ListPlans()`

**DB Tables:**

- `plans` (id, name, price, duration)
- `subscriptions` (user_id, plan_id, status, expires_at)

---

## 🧑‍💼 9. **admin-service**

**Purpose:** Admin dashboard and global monitoring.

**Responsibilities:**

- View platform analytics
- Manage users, content, reports
- Trigger system-wide tasks

**APIs:**

- `GetDashboardStats()`
- `ListAllUsers()`
- `BanUser(userId)`
- `SystemHealthCheck()`

**DB Tables:**

- Aggregated from other services via gRPC

---

## 🕵️ 10. **audit-service**

**Purpose:** Tracks user actions for logging and compliance.

**Responsibilities:**

- Record all critical actions
- Retrieve audit logs
- Support analytics or security investigations

**APIs:**

- `RecordEvent(userId, action, metadata)`
- `GetLogs(filter)`
- `PurgeOldLogs(days)`

**DB Tables:**

- `audit_logs` (id, user_id, service, action, metadata, timestamp)

---

## ⚙️ 11. **util-service**

**Purpose:** Utility and shared logic for all services.

**Responsibilities:**

- Send emails, notifications
- Generate unique IDs
- Cache, metrics, rate limiting

**APIs:**

- `SendEmail(to, subject, body)`
- `GenerateID(prefix)`
- `CacheGet(key)`
- `CacheSet(key, value, ttl)`

**DB:** none (optional Redis)

---

