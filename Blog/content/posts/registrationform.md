+++
date = '2025-01-03T12:44:12Z'
draft = false
title = 'Register and Login Form (Registration)'
weight = 20
[params]
  author = 'Abhinav K.'
+++

Register.jsx handles the user registration and login forms, manages the user state, as well as communicating with Firebase for authentication.
UserContext.jsx provides context to manage and save user state.

### Authentication form component
One of the first of our design decisions was to combine both registration and login into one single component that can be toggled between the two.

```
<div className='auth-toggle'>
  <button className={!isLogin ? 'active' : ''} onClick={() => setIsLogin(false)}>
    Register
  </button>
  <button className={isLogin ? 'active' : ''} onClick={() => setIsLogin(true)}>
    Login
  </button>
</div>
```

This both reduced code duplication and provides a smoother UX, by not requiring navigation between separate pages for login and registration.
