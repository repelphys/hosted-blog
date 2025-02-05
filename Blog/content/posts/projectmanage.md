+++
date = '2025-01-12T19:57:06Z'
draft = false
weight = 8
title = 'Project Management (AuthService)'
[params]
  author = 'Abhinav K.'
+++

[RBAC (Role-based access control) is the approach of restricting system access to varying degrees of authorized users](https://en.wikipedia.org/wiki/Role-based_access_control) - implemented here with three levels:
- Owner
- Editor
- Viewer

```
const projectData = {
  name: projectName,
  createdAt: new Date().toISOString(),
  members: {
    owner: [username],
    viewer: [],
    editor: [],
  },
};
```

We do this in order to uphold the standard of file-collaboration applications, where certain members will require access to more permissions than others.