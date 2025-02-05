+++
date = '2025-01-06T12:29:27Z'
draft = false
title = 'File Upload (Frontend)'
weight = 10
[params]
  author = 'Jeremy C.'
+++

We made it so that only users with the `editor` or `owner` role can upload files - stored on IPFS (Interplanetary File System).

```
const handleFileUpload = async (event) => {
  const file = event.target.files[0];
  const cid = await uploadToPinata(file);
  const newFile = {
    name: file.name,
    cid: cid,
    uploadedBy: auth.currentUser.displayName,
    uploadedAt: new Date().toISOString(),
  };

  const projectRef = doc(db, 'Projects', decodedProjectName);
  await updateDoc(projectRef, { files: [...files, newFile] });
};
```

To handle large file uploads that could potentially lead to browser slowdown and crashes, we used chunked uploads and progress indicators.