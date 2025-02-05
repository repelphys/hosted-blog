+++
date = 2024-12-24T04:14:54-08:00
draft = false
title = 'Dashboard'
weight = 10
[params]
  author = 'Jeremy C.'
+++



The dashboard is where users can view their projects, invitations, and invited projects. The approach to designing it was:

- Fetching the user’s data from Firebase. 
	This includes their projects, invitations, and role-based access. We used Firebase’s `getDoc` to retrieve the data and filtered it based on the user’s role (owner, editor, or viewer) to ensure that users would only see the projects that they had access to.

```
const fetchUserData = async () => {
  if (auth.currentUser) {
    const docRef = doc(db, 'Users', auth.currentUser.uid);
    const docSnap = await getDoc(docRef);
    if (docSnap.exists()) {
      const userData = docSnap.data();
      setProjects(userData.myProjects || []);
      setInvitedProjects(userData.invitedProjects || []);
    }
  }
};
```

- The `fetchUserData` function retrieves the user’s projects and invitations from Firebase. It filters projects where the user is the owner and those where they’ve been invited.
- Users can create new projects, which are stored in Firebase and linked to their account.
- Project owners can invite new members by entering their email and assigning a role (viewer or editor).

`react-toastify` is the React library that we decided on using to provide notifications, alerting users for situations such as project creation, invitation responses, and errors.
