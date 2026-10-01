# CricScore Cloud (anyone can view, signed-in scorers can edit)

## A. Firebase setup (one time, about 15 minutes, free)
1. Go to console.firebase.google.com, sign in with a Google account, click "Create a project", give it a name, turn Google Analytics off, Create.
2. Build > Authentication > Get started > Sign-in method > Email/Password > Enable > Save.
   Then the Users tab > Add user: create an email and password for each scorer.
3. Build > Firestore Database > Create database > pick a location near you (for India, asia-south1 Mumbai) > Start in production mode > Create.
   Open the Rules tab, delete everything, paste the rules below, press Publish:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}

4. Project settings (gear icon) > General > Your apps > the </> (Web) button > give a nickname > Register app.
   Copy apiKey, authDomain, projectId and appId from the code shown.
5. In GitHub open www/firebase-config.js, click the pencil, paste your four values between the quotes, commit.

## B. Build the APK
Upload this project to GitHub as before, open Actions > Build APK, wait for the green tick,
download CricScore-apk, install app-debug.apk on every phone.

## C. Who can do what
- Not signed in: view every match and scorecard live, export a PDF.
- Signed in: score, edit, delete, manage players, backup/import.
- Only one scorer per match at a time. Another scorer sees "X is scoring this match" and can press Take over.
- Old matches saved on a phone by the earlier version: sign in, open Backup & transfer, press "Upload old matches to the cloud".
- To stop strangers creating accounts: Firebase console > Authentication > Settings > User actions, and turn off sign-up if the option is shown. Keep allowSignup false in firebase-config.js.
