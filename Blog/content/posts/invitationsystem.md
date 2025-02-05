+++
date = '2025-01-11T12:59:43Z'
draft = false
title = 'Invitation System (AuthService)'
weight = 8
[params]
  author = 'Abhinav K.'
+++

Team collaboration is managed through a two-step process.
#### 1. Invitation creation
```
const invitationData = {
  id: Math.random().toString(36).substr(2, 9),
  projectName,
  inviterUid,
  inviterName: inviter.displayName,
  inviteeUid: inviteeRecord.uid,
  role: role,
  status: 'pending',
};
```

#### 2. Invitation Response Handling
Upon a user accepting an invitation, the system will respond accordingly by:
- Updating the project members list
- Sychronising the project data across all members in the group
- Deleting the invitation

This is to prevent duplicate invitations from repeatedly appearing, maintaining data integrity.