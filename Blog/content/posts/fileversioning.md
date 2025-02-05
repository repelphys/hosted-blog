+++
date = '2024-12-28T20:43:13Z'
draft = false
title = 'File Versioning'
weight = 5
[params]
  author = 'Abhinav K.'
+++

Imagine you're in a group setting - working on a collaborative project, where multiple others are editing files and pushing changes. You would naturally want to know:
- who made changes
- when such changes were made
- what those changes were.
You would also want it to be so that no single entity controls the entirety of the data.

CollabChain uses smart contracts to allow users to:
- Add new versions of a file
- Retrieve the total number of versions for a file
- Fetch the entire version history of a file + metadata.

## Tech Stack
- **Solidity**: the programming language required for writing smart contracts on Ethereum
- **IPFS**: used to store the actual file content. Each file version is represented by its IPFS CID, which is a unique hash that points to the file.
- **Ethereum**: the blockchain that contains our smart contract.

## Solving the Problem

### Data Structures
Two key data structures were defined:
```
struct Version {
    string ipfsCid;
    address editor;
    string description;
    uint256 timestamp;
}

struct FileHistory {
    string[] versionCids;
    mapping(string => Version) versionDetails;
}
```
- **Version**: holds the metadata about a specific version of a file.
	- **ipfsCid**: the IPFS CID
	- **editor**: the Ethereum address of the person who uploaded this specific version.
	- **description**: describes what changed in this version
	- **timestamp**: when this version was added
- **FileHistory**: tracks all versions of file
	- **versionCids**: an array of IPFS CIDs, this represents the history of file
	- **versionDetails**: maps IPFS CIDs to corresponding Version struct. Makes searching for metadata easier.

### File Versions
We then defined a mapping to store file histories for projects and files

```
// projectName => fileName => FileHistory
mapping(string => mapping(string => FileHistory)) private fileVersions;
```

- The first key is the `projectName`.
- The second key is the `fileName`.
- The value is a `FileHistory` struct which holds all versions of that file.

With this design, we are able to organise files by project, and made retrieving version history for files much easier.

### Adding Versions
The ```addVersion``` function was especially crucial to getting this all to work.

```
function addVersion(
    string memory projectName,
    string memory fileName,
    string memory newCid,
    string memory description
) public {
    FileHistory storage history = fileVersions[projectName][fileName];
    
    Version memory newVersion = Version({
        ipfsCid: newCid,
        editor: msg.sender,
        description: description,
        timestamp: block.timestamp
    });
    
    history.versionDetails[newCid] = newVersion;
    history.versionCids.push(newCid);
    
    emit NewVersionAdded(
        projectName,
        fileName,
        newCid,
        msg.sender,
        history.versionCids.length
    );
}
```

1. We retrieved `FileHistory` for the a particular project and file.
2. We would then create a new `Version` struct with necessary information, such as provided IPFS CID, sender address, description, and current timestamp.
3. This version is stored in the `versionDetails` mapping and the CID is then added to the `versionCids` array.
4. Lastly, we emit a `NewVersionAdded` event to log this action on the blockchain.

### Version Retrieval
Two functions are used to fetch version data:

1. `getVersionCount`
```
function getVersionCount(string memory projectName, string memory fileName) 
    public 
    view 
    returns (uint256) 
{
    return fileVersions[projectName][fileName].versionCids.length;
}
```

This function is used to return the number of versions for any given file.

2. `getVersionHistory`
```
function getVersionHistory(string memory projectName, string memory fileName) 
    public 
    view 
    returns (
        string[] memory cids,
        address[] memory editors,
        string[] memory descriptions,
        uint256[] memory timestamps
    ) 
{
    FileHistory storage history = fileVersions[projectName][fileName];
    uint256 count = history.versionCids.length;
    
    cids = new string[](count);
    editors = new address[](count);
    descriptions = new string[](count);
    timestamps = new uint256[](count);
    
    for (uint256 i = 0; i < count; i++) {
        string memory cid = history.versionCids[i];
        Version memory version = history.versionDetails[cid];
        
        cids[i] = version.ipfsCid;
        editors[i] = version.editor;
        descriptions[i] = version.description;
        timestamps[i] = version.timestamp;
    }
    
    return (cids, editors, descriptions, timestamps);
}
```

This function will the return the complete version history for a file, including:
- the IPFS CIDs for all versions
- the addresses of editors
- the descriptions for each version
- the individual timestamps for each version's addition.

### Design Decisions
- Storing large files directly on the blockchain is very expensive. 
  **By using IPFS, only the CID is stored on it** - which keeps costs far lower, while also still ensuring data integrity.
- With nested mapping (`projectName => fileName => FileHistory`), we are able to ensure that files the file organisation and file retrieval processes are much more straightforward and intuitive.