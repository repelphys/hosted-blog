+++
date = '2025-01-10T12:46:46Z'
draft = false
title = 'State Management Strategy (Registration)'
weight = 20
[params]
  author = 'Jem C.'
+++

We decided on implementing a global user context using React's Context API. This both allowed for **authentication state persistence** and **data consistency** - in that we standardized the user object structure throughout the application, making it easier to work with this user data in different components.

```
useEffect(() => {
  const unsubscribe = onAuthStateChanged(auth, (firebaseUser) => {
    if (firebaseUser) {
      const userData = {
        uid: firebaseUser.uid,
        email: firebaseUser.email,
        displayName: firebaseUser.displayName,
        photoURL: firebaseUser.photoURL,
      };
      setUser(userData);
    } else {
      setUser(null);
    }
    setLoading(false);
  });

  return () => unsubscribe();
}, []);
```