# 📝 Lesson 02: Testing Smart Contracts

## 📖 Overview
Testing is a crucial step in your smart contract development journey, as the lack of tests can be a roadblock during deployment or a smart contract audit. Effective testing sets apart the best developers.

---

## 📂 Creating the Test File

### **FundMeTest.t.sol**
- Inside the `test` folder, create a file called `FundMeTest.t.sol` (`.t` is a naming convention of Foundry).

### **Initial Setup**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract FundMeTest {

}
```

### **Importing Foundry's Test Library**
```solidity
import {Test} from "forge-std/Test.sol";

contract FundMeTest is Test {
}
```

### **Deploying the FundMe Contract**
- Do this inside the `setUp` function, which executes first whenever tests run.
- Example initial setup:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import {Test} from "forge-std/Test.sol";

contract FundMeTest is Test {

    function setUp() external { }

    function testDemo() public { }

}
```

### **Running the Test**
- Run `forge test` in your terminal to execute the tests.

---

## 🛠️ Understanding Test Execution

### **Updated Contract Example**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import {Test} from "forge-std/Test.sol";

contract FundMeTest is Test {

    uint256 favNumber = 0;
    bool greatCourse = false;

    function setUp() external { 
        favNumber = 1337;
        greatCourse = true;
    }

    function testDemo() public { 
        assertEq(favNumber, 1337);
        assertEq(greatCourse, true);
    }

}
```

### **Test Execution Flow**
- Declare state variables.
- Call `setUp()`.
- Run all test functions.

### **Using console.log for Debugging**
- Update import statement:
```solidity
import {Test, console} from "forge-std/Test.sol";
```
- Example with `console.log`:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import {Test, console} from "forge-std/Test.sol";

contract FundMeTest is Test {

    uint256 favNumber = 0;
    bool greatCourse = false;

    function setUp() external { 
        favNumber = 1337;
        greatCourse = true;
        console.log("This will get printed first!");
    }

    function testDemo() public { 
        assertEq(favNumber, 1337);
        assertEq(greatCourse, true);
        console.log("This will get printed second!");
        console.log("Updraft is changing lives!");
        console.log("You can print multiple things, for example this is a uint256, followed by a bool:", favNumber, greatCourse);
    }

}
```

### **Verbosity Levels in forge test**
- Adjust verbosity to control the output:
  ```bash
  forge test -vv
  ```

---

## 🎯 Deploying and Testing FundMe Contract

### **Steps to Deploy FundMe Contract**
1. **Import and Declare FundMe Contract**:
    ```solidity
    import {FundMe} from "../src/FundMe.sol";

    contract FundMeTest is Test {
        FundMe fundMe;
    }
    ```

2. **Deploy in setUp Function**:
    ```solidity
    function setUp() external { 
        fundMe = new FundMe();
    }
    ```

### **Testing MINIMUM_USD**
1. **Delete testDemo**
2. **Create testMinimumDollarIsFive**:
    ```solidity
    function testMinimumDollarIsFive() public {
        assertEq(fundMe.MINIMUM_USD(), 5e18);
    }
    ```
3. **Run the Test**:
    ```bash
    forge test
    ```

---

## 🧪 Summary

In this lesson:
- We learned the importance of testing in smart contract development.
- Created a test file for the FundMe contract.
- Implemented basic and advanced test setups.
- Used `console.log` for debugging.
- Deployed and tested the FundMe contract’s `MINIMUM_USD` value.

Effective testing is key to becoming a successful developer or auditor. Get used to writing comprehensive tests, as they are fundamental to ensuring your smart contracts work as intended.

---
pro developer. 🚀😊
