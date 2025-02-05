+++
date = '2025-01-18T12:28:01Z'
draft = false
title = 'Project Page'
weight = 10
[params]
  author = 'Jem C.'
+++

Project data (including files and members) was fetched from Firebase when the page loaded. This included filtering files based on the user’s role (owner, editor, or viewer).
To make sure that the interface updated in real-time when files were added/modified, we used Firebase's real-time listeners - storing file metadata (uploader, timestamps) in Firebase for easy retrieval want necessary. 

```
useEffect(() => {
  const fetchData = async () => {
    const projectDoc = await getDoc(doc(db, 'Projects', decodedProjectName));
    if (projectDoc.exists()) {
      setFiles(projectDoc.data().files || []);
    }
  };
  fetchData();
}, [decodedProjectName]);
```