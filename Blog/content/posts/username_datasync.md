+++
date = '2025-01-14T20:40:43Z'
draft = false
title = 'Usernames - Data Synchronisation (AuthService)'
weight = 20
[params]
  author = 'Abhinav K.'
+++

### 1. Username Input Validation

On the client side:

```
function isUsernameValid(username) {
  return /^[a-zA-Z0-9]+$/.test(username);
} 
```
- A regular expression is used to verify that a username only contains alphanumeric characters.
- If the username is valid, return ```true```, else return ```false```

...It's really funny how straightforward using a regular expression is to implement alphanumeric validation.

### 2. Duplicate Username Prevention
We needed to ensure usernames were unique. Seriously, imagine if two users in the same group shared the same username - what a headache.

```
async function isUsernameTaken(username) {
  const snapshot = await usersRef.where('displayName', '==', username).get();
  return !snapshot.empty;
}
```

- The function examines the ```Users``` collection in Firebase to check if any user has already requested ```displayName``` (the username)
- If this examination yields any results (```!snapshot.empty```), then that means the username is already in-use - and the function returns ```true```. Else, it returns ```false```/


