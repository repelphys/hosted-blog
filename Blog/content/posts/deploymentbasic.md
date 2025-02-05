+++
date = '2025-01-01T12:37:06Z'
draft = false
title = 'Basic Deployment'
weight = 15
[params]
  author = 'Abhinav K.'
+++

`deploy.js` contains a simple script that deploys our `FileVersioning` contract. It's best used for quick deployments.

```
const hre = require('hardhat');

async function main() {
  // Get the contract factory
  const FileVersioning = await hre.ethers.getContractFactory('FileVersioning');
  console.log('Deploying FileVersioning...');

  // Deploy the contract
  const fileVersioning = await FileVersioning.deploy();
  console.log('Contract deployed to:', await fileVersioning.getAddress());

  // Wait for 5 block confirmations
  await fileVersioning.waitForDeployment();
  console.log('Contract deployment confirmed');
}

// Execute the deployment
main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

#### What’s going on?

1. **Importing Hardhat**: First, we import the Hardhat runtime environment (`hre`). This provides access to Ethereum and Hardhat-specific functionality.
2. **Get the contract factory**: `hre.ethers.getContractFactory` is used to get an instance of the `FileVersioning` contract. To put it simply, it's basically a blueprint for deploying the contract.
3. **Contract deployment**: `deploy()` is called on the contract factory to deploy the contract to the blockchain. A contract instance is then returned.
4. **Wait**: We must then wait for this deployment to be confirmed by the blockchain (about 5 block confirmations).
5. **Log the Address**: Finally, we can then log the address of the deployed contract.

A `try-catch` block is used to ensure that any errors during deployment are caught and logged.