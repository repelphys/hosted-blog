+++
date = '2025-01-06T12:26:34Z'
draft = false
title = 'File Editing and Saving (Frontend)'
weight = 10
[params]
  author = 'Jeremy C.'

+++

We settled on integrating [CodeMirror](https://codemirror.net/) to create a flexible code editor that can support a variety of different langauges (i.e. JavaScript, Markdown, etc.)
The editor will adjust itself based on the file type, with expected syntax highlighting and auto-completion.

```
<CodeMirror
  value={content}
  extensions={[getLanguageExtension(file.name)]}
  onChange={(value) => setContent(value)}
  readOnly={!isEditing}
/>
```

We really wanted to make sure that each version of a file was immutable and transparent. 
In order to do this - when a new version of a file is saved, it gets uploaded to **IPFS** (InterPlanetary File System) and the CID (Content Identifier) is recorded on the **Ethereum blockchain**.
