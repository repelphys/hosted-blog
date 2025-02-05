+++
date = '2024-12-23T20:58:54Z'
draft = false
title = 'Error Handling (Registration)'
weight = 20
[params]
  author = 'Abhinav K.'
+++

## Error Handling + Feedback for Users
As is the standard for login/registration systems, we implemented error handling into this component.
The ```react-toastify``` library was used to share error messages with the user. Our system maps Firebase's error codes to more user-friendly messages.

```
switch (error.code) {
  case 'auth/user-not-found':
    notify('No account found with this email');
    break;
  case 'auth/wrong-password':
    notify('Incorrect password');
    break;
  // ... more cases
}
```

### What to take from this all?
- We learned the significance of handling the initial loading state in UserContext.jsx. Without such, there would be a brief flash of the unauthenticated state, even for logged-in, authenticated users.
  
```
return (
  <UserContext.Provider value={{ user, setUser, loading }}>
    {!loading && children}
  </UserContext.Provider>
);
```

- We also found that centralized error handling greatly improved the user experience of our product. 
