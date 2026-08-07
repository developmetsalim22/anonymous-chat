# Hello World - Anonymous Random Chat

A real-time anonymous chat application built with Firebase v10 Modular SDK, similar to Omegle or OmeTV. Users can chat with strangers anonymously with features like typing indicators, seen status, voice messages, and emoji support.

## Features

- **Anonymous Matching**: Users are matched randomly using Firebase transactions to prevent duplicate matches
- **Real-time Chat**: Instant messaging with Firestore real-time listeners
- **Typing Indicators**: See when your partner is typing
- **Seen Status**: Know when your messages have been read
- **Voice Messages**: Record and send voice messages
- **Emoji Support**: Quick emoji picker for expressive messaging
- **Online Counter**: Real-time count of online users
- **Auto Reconnect**: Automatically reconnects after internet loss
- **Privacy Focused**: Messages and matches are deleted when users disconnect
- **Single User Handling**: Displays "Waiting for another user..." when only one user is online
- **Comprehensive Logging**: Console logs for debugging and monitoring

## Tech Stack

- **Frontend**: HTML5, Bootstrap 5, Vanilla JavaScript
- **Backend**: Firebase Firestore (Real-time Database)
- **Firebase SDK**: v10 Modular SDK
- **Deployment**: Netlify (static hosting)

## Firebase Collections

### users
- `id`: Unique user ID
- `displayName`: User's display name
- `status`: online, searching, connected, offline
- `createdAt`: Timestamp when user was created
- `lastSeen`: Last activity timestamp
- `currentMatch`: Current match ID (if connected)

### queue
- `userId`: User ID
- `displayName`: User's display name
- `timestamp`: When user joined queue
- `status`: searching

### matches
- `id`: Unique match ID
- `users`: Array of two user IDs
- `createdAt`: Match creation timestamp
- `status`: active

### messages
- `matchId`: Associated match ID
- `senderId`: Sender's user ID
- `text`: Message text (for text messages)
- `audioUrl`: Base64 audio data (for voice messages)
- `type`: text or voice
- `timestamp`: Message timestamp
- `seen`: Boolean indicating if message was read

### presence
- `userId`: User ID (document ID)
- `online`: Boolean indicating online status
- `lastSeen`: Last activity timestamp
- `typing`: Boolean indicating if user is typing

## Setup Instructions

### 1. Firebase Project Setup

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project or select an existing one
3. Enable Firestore Database:
   - Go to Build > Firestore Database
   - Click "Create Database"
   - Select a location (choose closest to your users)
   - Start in test mode (we'll update rules later)
4. Enable Authentication (optional, for production):
   - Go to Build > Authentication
   - Enable Anonymous authentication
   - Or use your preferred auth method

### 2. Update Firebase Configuration

Open `index.html` and update the Firebase configuration with your project's credentials:

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT.firebasestorage.app",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID",
    measurementId: "YOUR_MEASUREMENT_ID"
};
```

You can find these values in your Firebase Console under Project Settings > General > Your apps.

### 3. Deploy Firestore Security Rules

1. Go to Firebase Console > Firestore Database > Rules tab
2. Copy the contents of `firestore.rules`
3. Paste the rules and click "Publish"

**Important**: The current rules allow anonymous access. For production, you should:
- Enable Firebase Authentication
- Update rules to use `request.auth.uid` instead of allowing all authenticated users
- Consider adding rate limiting and additional security measures

### 4. Deploy to Netlify with Auto-Deploy from GitHub (Recommended)

This is the best option for automatic updates - every time you push to GitHub, Netlify will automatically redeploy.

#### Step 1: Initialize Git Repository

Open terminal/command prompt in your chat folder:

```bash
cd c:\Users\judi\Desktop\chat
git init
git add .
git commit -m "Initial commit"
```

#### Step 2: Create GitHub Repository

1. Go to [GitHub](https://github.com) and sign in
2. Click the "+" icon > "New repository"
3. Repository name: `anonymous-chat` (or any name you want)
4. Make it **Public** or **Private** (your choice)
5. Click "Create repository"

#### Step 3: Push to GitHub

Copy the commands from GitHub (under "…or push an existing repository from the command line"):

```bash
git remote add origin https://github.com/YOUR_USERNAME/anonymous-chat.git
git branch -M main
git push -u origin main
```

Replace `YOUR_USERNAME` with your actual GitHub username.

#### Step 4: Connect to Netlify for Auto-Deploy

1. Go to [Netlify](https://app.netlify.com) and sign in/login
2. Click "Add new site" > "Import an existing project"
3. Click "GitHub" to connect your GitHub account
4. Authorize Netlify to access your repositories
5. Select your `anonymous-chat` repository
6. Configure build settings:
   - **Build command**: (leave empty - it's a static site)
   - **Publish directory**: `.` (root directory)
   - Click "Show advanced" > "Add environment variable" if needed
7. Click "Deploy site"

#### Step 5: Enable Automatic Deploys

1. After the first deploy, go to **Site settings** > **Build & deploy**
2. Scroll to "Continuous Deployment"
3. Make sure "Auto deploy" is **ON**
4. You can also enable "Deploy previews" for pull requests

#### Step 6: How to Update Your Site

Now whenever you make changes:

```bash
# Make your changes to index.html
git add .
git commit -m "Your change description"
git push
```

Netlify will **automatically** detect the push and redeploy your site within seconds!

---

#### Alternative: Manual Deploy (Netlify Drop)

If you don't want auto-deploy:

1. Go to [Netlify Drop](https://app.netlify.com/drop)
2. Drag and drop the `chat` folder
3. Your site will be live instantly (but you'll need to manually re-drop for updates)

## Testing the Application

### Testing with Multiple Users

To test the matching system:

1. Open the application in two different browsers (or use incognito/private mode)
2. Create different profiles in each browser
3. Click "Start Chatting" in both browsers
4. They should automatically connect to each other
5. Test sending messages, typing indicators, and voice messages

### Console Logs

The application includes comprehensive console logging. Open the browser console (F12) to see:
- User joined queue
- Searching status
- Partner found
- Match created
- Opening chat
- Next clicked
- Queue removed
- Match deleted
- And much more

## Troubleshooting

### Users not connecting

1. Check browser console for errors
2. Verify Firebase configuration is correct
3. Ensure Firestore rules are published
4. Check if both users are online (check online counter)
5. Verify Firebase project has Firestore enabled

### Messages not sending

1. Check if users are still matched
2. Verify network connection
3. Check browser console for Firebase errors
4. Ensure Firestore rules allow message creation

### Voice messages not working

1. Ensure microphone permissions are granted
2. Check browser console for media device errors
3. Verify browser supports MediaRecorder API
4. Some browsers may require HTTPS for microphone access

### Online counter not updating

1. Verify presence collection is being updated
2. Check Firestore rules for presence collection
3. Ensure presence system is running (check console logs)

## Browser Compatibility

- Chrome/Edge: Full support
- Firefox: Full support
- Safari: Full support (may require HTTPS for microphone)
- Mobile browsers: Full support

## Security Considerations

### Current Implementation

- Uses Firebase transactions to prevent duplicate matches
- Messages and matches are deleted on disconnect for privacy
- Firestore rules restrict access to own documents
- Presence system tracks online status

### For Production

1. **Enable Firebase Authentication**:
   - Add authentication to verify user identity
   - Update Firestore rules to use `request.auth.uid`

2. **Add Rate Limiting**:
   - Implement rate limiting on message sending
   - Add cooldown between matches

3. **Content Moderation**:
   - Add profanity filters
   - Implement report system
   - Add automatic blocking for abusive behavior

4. **Data Retention**:
   - Consider implementing message retention policies
   - Add option for users to report inappropriate content

5. **HTTPS**:
   - Always use HTTPS in production
   - Required for microphone access in most browsers

## Performance Optimization

- Firestore real-time listeners are efficient and only sync changes
- Voice messages are stored as base64 (consider using Firebase Storage for larger files)
- Automatic cleanup removes inactive users every minute
- Presence updates every 10 seconds to reduce database load

## License

This project is open source and available for educational purposes.

## Support

For issues or questions:
1. Check the browser console for error messages
2. Review Firebase Console for database issues
3. Verify Firestore rules are correctly configured
4. Ensure Firebase project is properly set up

## Credits

Developed by DV
- TikTok: [@developeerr](https://www.tiktok.com/@developeerr)
