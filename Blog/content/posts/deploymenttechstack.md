+++
date = '2025-01-01T15:18:55Z'
draft = false
title = 'Deployment Tech Stack'
weight = 15
[params]
  author = 'Abhinav K.'
+++

The process of deploying smart contracts can be rather frustrating, especially with having to manage complex deployment workflows or deal with multiple environments.
With the help of Hardhat, this can all be made easier by providing a solid framework of testing, compiling and deploying contracts.

We implemented various tools to make the deployment work as intended. Such included:
- [Hardhat](https://hardhat.org/) - dev environment for Ethereum that helps with testing smart contracts
- [Ethers.js](https://docs.ethers.org/v5/) -  library for interacting with the Ethereum blockchain
- Hardhat Ignition - module-based deployment system for Hardhat, used for complex deployment scenarios.

### Design Decisions
- Hardhat Ignition allows for deployments to be defined as modules, which made it easier to reuse deployments.
- Value Handling: The `value` option allows us to send Ether with the deployment - very beneficial for contracts that required an upfront initial balance.