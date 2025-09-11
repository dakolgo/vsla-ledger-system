# Akolgo Digital Ledger System

A modern, secure, and user-friendly platform designed to help VSLA treasurers in the Bongo District manage their group's finances with ease.

## Features

- **User Authentication**: Secure signup and login with Firebase Authentication
- **Group Management**: Create and manage VSLA groups
- **Member Management**: Add and track group members
- **Real-time Updates**: Live updates using Firebase Firestore
- **Responsive Design**: Works on desktop and mobile devices

## Firebase Integration

This application uses Firebase for:
- **Authentication**: User signup, login, and password reset
- **Database**: Firestore for storing groups and members data
- **Real-time Sync**: Live updates across all connected devices

## Setup Instructions

1. **Firebase Project**: The application is already configured with Firebase project `vsla-35d27`
2. **Authentication**: Email/password authentication is enabled
3. **Firestore**: Database is configured for groups and members collections

## How to Run

1. Start a local server:
   ```bash
   python3 -m http.server 8000
   ```

2. Open your browser and navigate to:
   ```
   http://localhost:8000/index%201.html
   ```

## Usage

1. **Sign Up**: Create a new account with your name, email/phone, and password
2. **Log In**: Use your credentials to access the dashboard
3. **Create Groups**: Add new VSLA groups from the dashboard
4. **Manage Members**: Click on a group to add and view members
5. **Real-time Updates**: All changes are automatically saved and synced

## Firebase Configuration

The application uses the following Firebase services:
- Authentication (Email/Password)
- Firestore Database
- Real-time listeners for live updates

## Error Handling

The application includes comprehensive error handling for:
- Network connectivity issues
- Authentication errors
- Database operation failures
- User input validation

## Security

- All data is stored securely in Firebase
- User authentication is handled by Firebase Auth
- Real-time security rules protect user data
- Input validation prevents malicious data entry
