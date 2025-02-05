+++
date = '2025-01-02T12:39:02Z'
draft = false
title = 'Advanced Deployment'
weight = 15
[params]
  author = 'Abhinav K.'
+++

`Lock.js` utilises **Hardhat Ignition** to deploy a `Lock` contract. This is much more flexible than `deploy.js`

```
const { buildModule } = require("@nomicfoundation/hardhat-ignition/modules");

const JAN_1ST_2030 = 1893456000;
const ONE_GWEI = 1_000_000_000n;

module.exports = buildModule("LockModule", (m) => {
  const unlockTime = m.getParameter("unlockTime", JAN_1ST_2030);
  const lockedAmount = m.getParameter("lockedAmount", ONE_GWEI);

  const lock = m.contract("Lock", [unlockTime], {
    value: lockedAmount,
  });

  return { lock };
});
```

1. **Hardhat Ignition**: `buildModule` is imported from Hardhat Ignition. This allows us to define a deployment module.
2. **Define constants**: Two constants are defined:
    - `JAN_1ST_2030`: A Unix timestamp for January 1st, 2030.
    - `ONE_GWEI`: This represents a single Gwei (a small unit of Ether).
3. **Build module**: `buildModule` defines a deployment module called `LockModule` - inside of which contains:
    - parameter retrieval (`unlockTime` and `lockedAmount`) with default values.
    - `Lock` contract is deployed, which will send `unlockTime` as a constructor argument and forwards `lockedAmount` of Ether with the deployment.
4. **Return the Contract**: Lastly, the deployed `Lock` contract instance is returned.