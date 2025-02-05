+++
date = '2025-01-11T15:28:12Z'
draft = false
weight = 8
title = 'Project - Data Synchronisation (AuthService)'
[params]
  author = 'Abhinav K.'
+++

Ensuring that project data remained consistent across all user documents was critical.
In the situation that project membership changes, additional security measures are implemented:

```
// Update all users who have this project
const usersSnapshot = await db.collection('Users').get();
const updatePromises = [];

usersSnapshot.forEach((doc) => {
  const user = doc.data();
  if (user.invitedProjects) {
    const projectIndex = user.invitedProjects.findIndex(
      (p) => p.name === invitation.projectName
    );
    if (projectIndex !== -1) {
      updatePromises.push(
        db.collection('Users').doc(doc.id).update({
          invitedProjects: updatedInvitedProjects,
        })
      );
    }
  }
});

await Promise.all(updatePromises);
```

- We start by fetching users from the aptly titled ```Users``` collection in Firebase.
- We then check if they have access to the project by searching for the project name in the ```invitedProjects``` array.
- If the user does have access, then an update operation is added to the ```updatePromises``` array - updating the user's ```invitedProjects``` array with latest data.
-  Lastly, we use ```Promise.all``` to execute all updates in parallel.