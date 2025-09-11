# Firebase Setup Guide

## Fix "Missing or insufficient permissions" Error

The error you're experiencing is due to Firebase Firestore security rules that prevent unauthorized access. Here's how to fix it:

### Option 1: Update Firestore Security Rules (Recommended)

1. **Go to Firebase Console**:
   - Visit: https://console.firebase.google.com/
   - Select your project: `vsla-35d27`

2. **Navigate to Firestore Database**:
   - Click on "Firestore Database" in the left sidebar
   - Click on the "Rules" tab

3. **Replace the existing rules** with the content from `firestore.rules` file:
   ```javascript
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       // Users can read and write their own user document
       match /users/{userId} {
         allow read, write: if request.auth != null && request.auth.uid == userId;
       }
       
       // Groups: users can create groups and read/write groups they own
       match /groups/{groupId} {
         allow read, write: if request.auth != null && 
           (resource.data.treasurerId == request.auth.uid || 
            request.auth.uid == resource.data.treasurerId);
         allow create: if request.auth != null && 
           request.resource.data.treasurerId == request.auth.uid;
         
         // Members subcollection: only group owner can read/write
         match /members/{memberId} {
           allow read, write: if request.auth != null && 
             get(/databases/$(database)/documents/groups/$(groupId)).data.treasurerId == request.auth.uid;
         }
       }
     }
   }
   ```

4. **Publish the rules**:
   - Click "Publish" button

5. **Authorized domains (required for web apps)**:
   - Go to Authentication → Settings → Authorized domains
   - Ensure your local domains are listed, e.g. `localhost`, `127.0.0.1`, and any custom domain you use
   - This prevents errors like `auth/invalid-credential` when signing in from an unauthorized origin

### Option 2: Temporary Development Rules (For Testing Only)

If you want to test quickly, you can temporarily use these rules (NOT for production):

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

⚠️ **Warning**: These rules allow any authenticated user to read/write any document. Only use for testing!

### Option 3: Enable Authentication Methods

Make sure these authentication methods are enabled in Firebase Console:

1. **Go to Authentication**:
   - Click "Authentication" in the left sidebar
   - Click "Sign-in method" tab

2. **Enable Email/Password**:
   - Click on "Email/Password"
   - Enable "Email/Password" provider
   - Click "Save"

3. **Verify web configuration**:
   - Go to Project settings → General → Your apps (Web app)
   - Confirm your config matches the app (especially `authDomain` and `storageBucket`)
   - For this project the storage bucket should be `vsla-35d27.appspot.com` (not `firebasestorage.app`)

### Verify Setup

After updating the rules:

1. **Test the application**:
   - Try signing up with a new account
   - Try creating a group
   - Try adding members

2. **Check Firebase Console**:
   - Go to Firestore Database
   - You should see collections: `users`, `groups`, and `groups/{groupId}/members`

### Troubleshooting

If you still get permission errors:

1. **Check Authentication**:
   - Make sure users are properly authenticated
   - Check browser console for authentication errors
   - If you see `auth/invalid-credential`, confirm:
     - The email/password are correct
     - The domain is in Authentication → Settings → Authorized domains
     - You are not mixing sign-in methods (e.g., trying password login for a Google-created account)

2. **Check Rules Syntax**:
   - Make sure the rules are properly formatted
   - Use the Firebase Rules Simulator to test

3. **Clear Browser Cache**:
   - Sometimes cached rules cause issues
   - Try in an incognito/private window

4. **Confirm Firestore & Storage configuration**:
   - Project settings → General: verify `projectId` and `storageBucket`
   - If you changed the bucket name format, update the app config to `vsla-35d27.appspot.com`

### Security Notes

The recommended rules ensure:
- Users can only access their own data
- Group owners can only manage their own groups
- Members can only be added by group owners
- All operations require authentication

This provides a secure foundation for your VSLA management system.
