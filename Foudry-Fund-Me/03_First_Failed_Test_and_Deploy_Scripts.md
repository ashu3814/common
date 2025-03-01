# 📝 Lesson 03: First Failed Test and Writing Deploy Scripts

## 📖 Overview
In this lesson, we’ll write and run tests for the `FundMe` contract and learn how to create deploy scripts to provide flexibility for different environments.

---

## 📂 First Failed Test

### **Testing the Owner**
- Add the following function to your testing file to verify if the owner is recorded properly:
```solidity
function testOwnerIsMsgSender() public {
    assertEq(fundMe.i_owner(), msg.sender);
}
```

### **Running the Test**
- Run the test via `forge test`.

### **Output and Debugging**
- The test fails because the addresses are different. Let's add `console.log` statements to debug:
```solidity
function testOwnerIsMsgSender() public {
    console.log(fundMe.i_owner());
    console.log(msg.sender);
    assertEq(fundMe.i_owner(), msg.sender);
}
```

- Run the test with verbosity:
  ```bash
  forge test -vv
  ```

### **Understanding the Failure**
- The `FundMe` contract was deployed by the `setUp` function, part of the `FundMeTest` contract.
- To fix this, tweak the test function:
```solidity
function testOwnerIsMsgSender() public {
    assertEq(fundMe.i_owner(), address(this));
}
```

- Run the test again via `forge test`. It should pass now.

---

## 📂 Writing Deploy Scripts

### **Why Deploy Scripts?**
- Deploy scripts provide flexibility to test and deploy on different environments (e.g., Anvil, Mainnet, Arbitrum).

### **Creating the Deploy Script**
- Create a new file called `DeployFundMe.s.sol` in the `script` folder.
- State the SPDX and pragma:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;
```

### **Importing Necessary Contracts**
- Import `Script.sol` and the `FundMe` contract:
```solidity
import {Script} from "forge-std/Script.sol";
import {FundMe} from "../src/FundMe.sol";
```

### **Defining the Deploy Script**
- Define the contract and deploy the `FundMe` contract in the `run` function:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import {Script} from "forge-std/Script.sol";
import {FundMe} from "../src/FundMe.sol";

contract DeployFundMe is Script {
    function run() external {
        vm.startBroadcast();
        new FundMe();
        vm.stopBroadcast();
    }
}
```

### **Running the Deploy Script**
- Run the deploy script with:
  ```bash
  forge script DeployFundMe
  ```

---

## 🧪 Summary

In this lesson:
- We wrote and debugged tests for the `FundMe` contract.
- Learned how to identify and fix a failed test.
- Created a deploy script to provide flexibility for different deployment environments.

Writing comprehensive tests and deploy scripts are essential skills for professional developers. Mastering these skills will help you ensure your smart contracts work as intended across various environments.

---


