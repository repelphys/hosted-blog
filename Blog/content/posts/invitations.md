+++
date = '2025-01-07T12:22:50Z'
draft = false
title = 'Invitation Handling'
weight = 10
[params]
  author = 'Jem C.'
+++

We then implemented the logic for handling project invitations. Upon accepting or declining an invite, an API call would be triggered to update the invitation status in Firebase.

```
const handleInvitationResponse = async (invitationId, accept) => {
  try {
    const res = await axios.post('/api/respondToInvitation', {
      invitationId,
      uid: auth.currentUser.uid,
      accept,
    });
    if (res.data.Status === 'Success') {
      await fetchUserData(); // Refresh the data
      toast.success(accept ? 'Invitation accepted' : 'Invitation declined');
    }
  } catch (error) {
    toast.error(error.message);
  }
};
```

We made sure that the interface updated in real-time upon a user's response to an invitation by refetching the user data after each response. We decided on using ```axios``` for API calls due to its built-in error handling and ease of use.