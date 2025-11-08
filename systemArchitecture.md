# System Architecture – DD LOCK
# Overview
DD LOCK is a mobile-based productivity app that temporarily locks specific phone features to help users stay focused.  
It follows a client-server architecture, with a mobile frontend, backend APIs, and a cloud database.

# Architectural Components

# 1. **Frontend (Mobile App)**
- Technology:Flutter  
- **Responsibilities:**
- User authentication & profile setup  
- Focus session setup (set duration, alarm, challenge type)  
- Visual timers and progress screens  
- Alarm & notification display  
- Local storage for offline tracking  
# 2. **Backend (Server)**
- Technology: Firebase  
- **Responsibilities:**
- Handle user data and session information  
- Manage focus session states (active, completed, extended)  
- Process challenge unlock attempts  
- Store analytics (e.g., session duration, success rate) 
- **API Example:**
- `POST /api/session/start` – Start a new lock session  
- `GET /api/session/status` – Retrieve current session info  
- `POST /api/unlock/challenge` – Validate user’s unlock attempt  
# 3. **Database Layer**
- Technology: Cloud Firestore 
- **Responsibilities:**  
- Stores: user session data,
- preferences, 
- activity logs in real time.  
Each user’s data (e.g., focus time, challenge difficulty, unlock history) is saved and synced securely across devices.
- **Collections:**
- `users` → Stores profile and preferences  
- `sessions` → Tracks focus sessions, start/end times  
- `analytics` → Records productivity metrics 

# 4. Notifications & Alerts
- **Technology:** Firebase Cloud Messaging (FCM)  
- **Description:**
Sends reminders and alerts to users:
- Session start/complete notifications  
- “Emergency unlock” reminders  
- Challenge unlock prompts

# 5. Device APIs
- **Technology:** Android/iOS Native APIs  
- **Description:**  
Used to control device behaviors such as:
- Enabling/disabling notifications (DND mode)
- Temporarily restricting access to apps
- Triggering vibration or sound when the session ends

# Communication Flow
1. **User opens the mobile app** and starts a focus session.  
2. **Frontend sends request** to backend API (`/session/start`) with session details.  
3. **Backend stores session data** in the database.  
4. Firebase Functions monitor the countdown in the background.  
5. FCM sends notifications to remind or alert the user.  
6. When user attempts to unlock early:
 - Frontend triggers a **challenge question screen**.  
 - Backend validates response (`/unlock/challenge`).  
7. After session time ends, **backend sends signal** to frontend to trigger alarm and unlock mode.  

# Security & Access Control
- Authentication using Firebase Auth  
- Emergency unlock requests logged and limited to once every 30 minutes.  
- Backend validates all session-related requests to prevent unauthorized unlocks.  

# Technical Feasibility

- Uses well-supported frameworks.
- Scalable backend suitable for cloud deployment 
- Offline-first design ensures basic lock features work even without internet connection.  
- Integrate native device APIs (DND mode, notifications, alarms).  

