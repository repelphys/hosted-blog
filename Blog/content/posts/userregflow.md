+++
date = '2025-01-13T12:52:51Z'
draft = false
title = 'User Registration Flow (AuthService)'
weight = 20
[params]
  author = 'Abhinav K.'
+++

- When a user is done registrating, a client will send their email and password to the `/auth/register` folder on Firebase.
- Firebase will then use this information to create an authentication record.

```
app.post('/auth/register', async (req, res) => {
  const { email, pass } = req.body;
  
  try {
    // Create auth record
    const userRecord = await auth.createUser({
      email,
      password: pass,
    });

    // Initialize Firestore document
    await db.collection('Users').doc(userRecord.uid).set({
      displayName: '',
      email: userRecord.email,
      photoURL: '',
      myProjects: [],
      invitedProjects: [],
      invitations: [],
      createdAt: admin.firestore.FieldValue.serverTimestamp(),
    });

    res.status(200).json({
      Status: 'User Created',
      User: userRecord
    });
  } catch (error) {
    console.error('Error creating user:', error.message);
    res.status(200).json({ Status: 'Error', message: error.message });
  }
});

```

- Firestore then initialises user document data structure.

```
{
  displayName: '',
  email: userRecord.email,
  photoURL: '',
  myProjects: [],      // Projects they own
  invitedProjects: [], // Projects they're part of
  invitations: [],     // Pending invites
  createdAt: timestamp
}
```