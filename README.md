# AXL Gallery

A modern, feature-rich photo gallery application built with cutting-edge web technologies. AXL Gallery provides a seamless experience for browsing, organizing, and sharing your favorite photos with real-time synchronization and cloud storage capabilities.

## 📋 Table of Contents

- [Project Description](#project-description)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Setup Instructions](#setup-instructions)
- [Firebase Configuration](#firebase-configuration)
- [Usage Guide](#usage-guide)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

## 📖 Project Description

AXL Gallery is a responsive web application designed to provide users with an intuitive platform to view, manage, and share photo collections. The application leverages Firebase for backend services, including authentication, real-time database, and cloud storage, ensuring secure and scalable operations.

Whether you're a photographer looking to showcase your portfolio or simply want to organize your personal photo library, AXL Gallery offers a sleek interface and powerful features to meet your needs.

### Key Objectives

- Provide a user-friendly interface for photo management
- Enable real-time synchronization across devices
- Ensure secure authentication and data storage
- Deliver responsive design for all device sizes
- Support collaborative photo sharing

## ✨ Features

### Core Features

- **User Authentication**
  - Secure sign-up and login with email/password
  - Firebase Authentication integration
  - Password reset functionality
  - User profile management

- **Photo Management**
  - Upload photos to cloud storage
  - Organize photos into albums/collections
  - Search and filter capabilities
  - Photo metadata display (date, size, dimensions)
  - Bulk operations support

- **Real-time Synchronization**
  - Instant updates across multiple devices
  - Real-time album changes
  - Cloud-based storage with Firebase
  - Offline-first architecture support

- **Responsive Design**
  - Mobile-optimized interface
  - Tablet and desktop support
  - Touch-friendly controls
  - Progressive Web App (PWA) capabilities

- **Photo Viewing**
  - High-quality image display
  - Image zoom and pan controls
  - Slideshow functionality
  - Full-screen viewing mode
  - Keyboard navigation support

- **Social Features**
  - Share albums with other users
  - Comment on photos
  - Like/favorite functionality
  - User permissions management

- **Performance Optimization**
  - Image compression and optimization
  - Lazy loading for galleries
  - Caching strategies
  - Fast loading times

## 🛠️ Tech Stack

### Frontend
- **Framework**: React.js / Vue.js / Angular (specify your choice)
- **Styling**: Tailwind CSS / Material-UI / Custom CSS
- **State Management**: Redux / Vuex / Context API (specify your choice)
- **Build Tool**: Webpack / Vite

### Backend
- **Firebase Services**
  - Firestore Database
  - Firebase Authentication
  - Firebase Cloud Storage
  - Firebase Hosting (optional)

### Development Tools
- **Version Control**: Git & GitHub
- **Package Manager**: npm / yarn
- **Testing**: Jest / Vitest
- **Linting**: ESLint / Prettier

## 🚀 Setup Instructions

### Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v14.0.0 or higher)
- **npm** (v6.0.0 or higher) or **yarn** (v1.22.0 or higher)
- **Git** (v2.0 or higher)
- A modern web browser (Chrome, Firefox, Safari, or Edge)

### Installation Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/Samidgn46/axl-gallery.git
   cd axl-gallery
   ```

2. **Install Dependencies**
   ```bash
   npm install
   # or
   yarn install
   ```

3. **Create Environment Configuration**
   
   Create a `.env.local` file in the project root directory:
   ```bash
   touch .env.local
   ```

4. **Add Firebase Configuration**
   
   Add your Firebase credentials to the `.env.local` file (see [Firebase Configuration](#firebase-configuration) section below).

5. **Start Development Server**
   ```bash
   npm run dev
   # or
   yarn dev
   ```

   The application will be available at `http://localhost:3000` (or your configured port).

6. **Build for Production**
   ```bash
   npm run build
   # or
   yarn build
   ```

7. **Deploy**
   
   See [Firebase Deployment](#firebase-deployment) instructions below.

### Project Installation Video

For a visual guide, watch our setup tutorial (if available):
```
[Video link or placeholder]
```

## ⚙️ Firebase Configuration

### Step 1: Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Create a project"**
3. Enter your project name (e.g., "AXL-Gallery")
4. Accept the terms and click **"Create project"**
5. Wait for the project to initialize

### Step 2: Register Your Web App

1. In the Firebase Console, click the **Web** icon (</>) to create a web app
2. Enter your app name (e.g., "axl-gallery-web")
3. Click **"Register app"**
4. Copy the Firebase configuration object

### Step 3: Configure Environment Variables

Add the Firebase credentials to your `.env.local` file:

```env
REACT_APP_FIREBASE_API_KEY=your_api_key
REACT_APP_FIREBASE_AUTH_DOMAIN=your_auth_domain
REACT_APP_FIREBASE_PROJECT_ID=your_project_id
REACT_APP_FIREBASE_STORAGE_BUCKET=your_storage_bucket
REACT_APP_FIREBASE_MESSAGING_SENDER_ID=your_messaging_sender_id
REACT_APP_FIREBASE_APP_ID=your_app_id
REACT_APP_FIREBASE_MEASUREMENT_ID=your_measurement_id
```

Example:
```env
REACT_APP_FIREBASE_API_KEY=AIzaSyDf1234567890abcdefghijklmnop
REACT_APP_FIREBASE_AUTH_DOMAIN=axl-gallery.firebaseapp.com
REACT_APP_FIREBASE_PROJECT_ID=axl-gallery-12345
REACT_APP_FIREBASE_STORAGE_BUCKET=axl-gallery-12345.appspot.com
REACT_APP_FIREBASE_MESSAGING_SENDER_ID=123456789012
REACT_APP_FIREBASE_APP_ID=1:123456789012:web:1234567890abcdef
REACT_APP_FIREBASE_MEASUREMENT_ID=G-1234567890
```

### Step 4: Enable Authentication

1. In Firebase Console, go to **Authentication** → **Sign-in method**
2. Enable **Email/Password** authentication
3. (Optional) Enable other providers:
   - Google
   - Facebook
   - GitHub

### Step 5: Set Up Firestore Database

1. Go to **Firestore Database** in Firebase Console
2. Click **"Create database"**
3. Choose your preferred location
4. Select **"Start in test mode"** (for development) or **"Start in production mode"** (for production)
5. Create the following collections:
   - `users` - User profile information
   - `albums` - Photo album metadata
   - `photos` - Photo information and metadata

### Step 6: Configure Cloud Storage

1. Go to **Storage** in Firebase Console
2. Click **"Get Started"**
3. Select a location for your storage bucket
4. Review security rules (see [Security Rules](#security-rules) below)
5. Click **"Done"**

### Step 7: Set Security Rules

#### Firestore Security Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users collection - users can read/write their own data
    match /users/{userId} {
      allow read, write: if request.auth.uid == userId;
    }

    // Albums collection - users can read/write their own albums
    match /albums/{document=**} {
      allow read, write: if request.auth.uid == resource.data.ownerId || request.auth.uid in resource.data.sharedWith;
    }

    // Photos collection - users can read/write their own photos
    match /photos/{document=**} {
      allow read, write: if request.auth.uid == resource.data.userId;
    }

    // Public albums can be read by anyone
    match /albums/{albumId} {
      allow read: if resource.data.public == true;
    }
  }
}
```

#### Cloud Storage Security Rules

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /users/{userId}/{allPaths=**} {
      allow read, write: if request.auth.uid == userId;
    }

    match /public/{allPaths=**} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

### Step 8: Initialize Firebase in Your Application

Create a `firebase.js` or `firebase.ts` file:

```javascript
import { initializeApp } from 'firebase/app';
import { getAuth } from 'firebase/auth';
import { getFirestore } from 'firebase/firestore';
import { getStorage } from 'firebase/storage';

const firebaseConfig = {
  apiKey: process.env.REACT_APP_FIREBASE_API_KEY,
  authDomain: process.env.REACT_APP_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.REACT_APP_FIREBASE_PROJECT_ID,
  storageBucket: process.env.REACT_APP_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.REACT_APP_FIREBASE_MESSAGING_SENDER_ID,
  appId: process.env.REACT_APP_FIREBASE_APP_ID,
  measurementId: process.env.REACT_APP_FIREBASE_MEASUREMENT_ID,
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const storage = getStorage(app);

export { auth, db, storage, app };
```

### Firebase Deployment

Deploy to Firebase Hosting:

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login to Firebase
firebase login

# Initialize Firebase in your project
firebase init

# Build and deploy
npm run build
firebase deploy
```

## 📚 Usage Guide

### Getting Started

1. **Create an Account**
   - Click **"Sign Up"** on the landing page
   - Enter your email and create a strong password
   - Verify your email address
   - Complete your profile setup

2. **Upload Your First Photo**
   - Click the **"+"** button or **"Upload"** button
   - Select one or more photos from your device
   - Add optional metadata (title, description, tags)
   - Click **"Upload"** to save to cloud storage

3. **Organize into Albums**
   - Click **"Create Album"**
   - Name your album and add a description
   - Drag photos into the album or select from existing uploads
   - Save your album

### Main Features Usage

#### Browsing Photos

- **Grid View**: Default view showing all photos in a grid layout
- **List View**: Switch to list view from the view options menu
- **Zoom**: Use the mouse wheel or pinch gesture to zoom in/out
- **Full Screen**: Click the full-screen icon for immersive viewing

#### Searching and Filtering

```
Search by:
- Photo title or description
- Date range using the calendar picker
- Tags (custom labels you assign)
- Album name
- File size
```

Example: Search for "vacation 2025" to find all vacation photos from 2025

#### Creating and Managing Albums

```
1. Click "Albums" in the main menu
2. Click "Create New Album"
3. Enter album details:
   - Title
   - Description
   - Privacy settings (Private/Shared/Public)
4. Add photos by:
   - Uploading new photos
   - Selecting from existing uploads
5. Save the album
```

#### Sharing Albums

1. Open the album you want to share
2. Click the **"Share"** button
3. Enter email addresses of people to share with
4. Select permission level:
   - **View Only**: Can view but not modify
   - **Edit**: Can view and edit (add/remove photos)
5. Click **"Send Invitations"**

#### Photo Comments and Reactions

```
On any photo:
1. Click the comment icon
2. Type your comment
3. Click "Post" to share
4. Click the heart icon to like a photo
5. View comments from other users
```

#### Downloading Photos

```
1. Click on the photo you want to download
2. Click the download icon
3. Choose quality:
   - Original: Full resolution
   - High: Optimized for quality
   - Medium: Balanced quality and size
   - Low: Smallest file size
4. The photo will download to your device
```

#### Settings and Profile Management

Navigate to **Settings** (⚙️ icon) to:
- Update profile information
- Change password
- Configure privacy settings
- Manage connected devices
- Enable two-factor authentication
- Review activity logs
- Manage storage usage
- Delete account

### Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Arrow Left` | Previous photo |
| `Arrow Right` | Next photo |
| `Esc` | Exit full-screen |
| `F` | Toggle full-screen |
| `+` | Zoom in |
| `-` | Zoom out |
| `0` | Reset zoom |
| `S` | Toggle slideshow |
| `Delete` | Delete selected photo (with confirmation) |
| `Enter` | Open selected item |

### Best Practices

#### Photo Organization

- Create albums by category (Vacations, Family, Events, etc.)
- Use consistent naming conventions
- Tag photos with relevant keywords
- Regularly review and delete duplicates
- Back up important photos

#### Storage Management

- Monitor your storage quota in Settings
- Archive old albums you rarely access
- Delete low-quality photos
- Compress photos before uploading large batches

#### Security

- Use a strong, unique password
- Enable two-factor authentication
- Don't share your login credentials
- Review shared albums periodically
- Use HTTPS when accessing the site

#### Sharing Tips

- Share with specific people for sensitive albums
- Use public links for sharing with many people
- Set appropriate permission levels
- Monitor who has access to your photos
- Revoke access when no longer needed

## 📁 Project Structure

```
axl-gallery/
├── public/                 # Static assets
│   ├── index.html         # Main HTML file
│   └── favicon.ico        # Website icon
├── src/                   # Source code
│   ├── components/        # Reusable React components
│   │   ├── Gallery/      # Gallery display components
│   │   ├── Upload/       # Upload functionality
│   │   ├── Auth/         # Authentication components
│   │   └── ...
│   ├── pages/            # Page components
│   │   ├── Home.js
│   │   ├── Albums.js
│   │   ├── Profile.js
│   │   └── ...
│   ├── services/         # Firebase and API services
│   │   ├── authService.js
│   │   ├── storageService.js
│   │   ├── firestoreService.js
│   │   └── ...
│   ├── hooks/            # Custom React hooks
│   ├── context/          # React Context for state management
│   ├── styles/           # Global styles and themes
│   ├── utils/            # Utility functions
│   ├── firebase.js       # Firebase configuration
│   ├── App.js            # Root component
│   └── index.js          # Entry point
├── .env.local            # Environment variables (create this)
├── .env.example          # Example environment variables
├── .gitignore            # Git ignore rules
├── package.json          # Dependencies and scripts
├── README.md             # This file
└── [config files]        # ESLint, Prettier, etc.
```

## 🤝 Contributing

We welcome contributions from the community! Please follow these guidelines:

1. **Fork the Repository**
   ```bash
   git clone https://github.com/Samidgn46/axl-gallery.git
   cd axl-gallery
   ```

2. **Create a Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Commit Your Changes**
   ```bash
   git commit -m "Add: Your feature description"
   ```

4. **Push to Your Fork**
   ```bash
   git push origin feature/your-feature-name
   ```

5. **Open a Pull Request**
   - Describe your changes clearly
   - Reference any related issues
   - Ensure all tests pass

### Code Standards

- Follow the project's coding style
- Write meaningful commit messages
- Add comments for complex logic
- Test your changes locally
- Update documentation if needed

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

For issues, questions, or suggestions:

- **GitHub Issues**: [Open an issue](https://github.com/Samidgn46/axl-gallery/issues)
- **Email**: [Your contact email]
- **Documentation**: [Link to detailed docs]

## 🙏 Acknowledgments

- Firebase team for excellent backend services
- Open-source community for amazing tools and libraries
- All contributors who have helped improve this project

---

**Last Updated**: January 3, 2026

**Status**: Active Development 🚀

For more information and updates, visit [axl-gallery repository](https://github.com/Samidgn46/axl-gallery)
