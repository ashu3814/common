# 📝 Lesson 06: Testing Locally with Mock Contracts

## 📖 Overview
In the previous lesson, we refactored our contracts to avoid being forced to use Sepolia every single time when we ran tests. Now, we'll set up local testing using mock contracts, which allows us to simulate the behavior of a real contract without interacting with a live blockchain.

---

## 📂 Creating HelperConfig

### **Step 1: Create HelperConfig File**
- Create a new file in the `script` folder called `HelperConfig.s.sol`.

### **Step 2: Define the Contract**
- Start by defining the contract and importing necessary libraries:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

import {Script} from "forge-std/Script.sol";

contract HelperConfig {
    // If we are on a local Anvil, we deploy the mocks
    // Else, grab the existing address from the live network
}
```

---

## 📂 Where the Magic Happens

### **Detecting the Environment**
- Add logic to detect if we are running on a local Anvil blockchain:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

import {Script} from "forge-std/Script.sol";
import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

contract HelperConfig is Script {
    AggregatorV3Interface internal priceFeed;

    function getPriceFeed() internal returns (AggregatorV3Interface) {
        if (block.chainid == 1337) {
            // Deploy mocks if on local Anvil
            priceFeed = deployMocks();
        } else {
            // Use existing address if on live network
            priceFeed = AggregatorV3Interface(0x694AA1769357215DE4FAC081bf1f309aDC325306);
        }
        return priceFeed;
    }

    function deployMocks() internal returns (AggregatorV3Interface) {
        // Mock deployment logic
    }
}
```

### **Deploying Mocks**
- Implement the logic to deploy mock contracts:
```solidity
import {MockV3Aggregator} from "../mocks/MockV3Aggregator.sol";

function deployMocks() internal returns (AggregatorV3Interface) {
    // Deploy a mock AggregatorV3Interface
    MockV3Aggregator mockPriceFeed = new MockV3Aggregator(8, 2000e8); // 8 decimals, initial price of 2000
    return AggregatorV3Interface(address(mockPriceFeed));
}
```

---

## 📂 Updating FundMe Contract and Tests

### **Update FundMe Contract**
- Modify the constructor to accept the `priceFeed` address from `HelperConfig`:
```solidity
constructor(address priceFeed) {
    i_owner = msg.sender;
    s_priceFeed = AggregatorV3Interface(priceFeed);
}
```

### **Update Deploy Script**
- Use `HelperConfig` to get the correct `priceFeed` address:
```solidity
import {HelperConfig} from "./HelperConfig.s.sol";

contract DeployFundMe is Script, HelperConfig {
    function run() external returns (FundMe) {
        vm.startBroadcast();
        AggregatorV3Interface priceFeed = getPriceFeed();
        FundMe fundMe = new FundMe(address(priceFeed));
        vm.stopBroadcast();
        return fundMe;
    }
}
```

### **Update Tests**
- Ensure the tests use the correct `priceFeed` address from `HelperConfig`:
```solidity
import {HelperConfig} from "../script/HelperConfig.s.sol";
import {DeployFundMe} from "../script/DeployFundMe.s.sol";

contract FundMeTest is Test, HelperConfig {
    FundMe fundMe;
    DeployFundMe deployFundMe;

    function setUp() external {
        deployFundMe = new DeployFundMe();
        fundMe = deployFundMe.run();
    }

    function testMinimumDollarIsFive() public {
        assertEq(fundMe.MINIMUM_USD(), 5e18);
    }

    function testOwnerIsMsgSender() public {
        assertEq(fundMe.i_owner(), msg.sender);
    }

    function testPriceFeedVersionIsAccurate() public {
        uint256 version = fundMe.getVersion();
        assertEq(version, 4);
    }
}
```

---

## 🧪 Summary

In this lesson:
- We learned how to create a local testing environment using mock contracts.
- Created the `HelperConfig` contract to detect the environment and deploy mocks if necessary.
- Updated the `FundMe` contract, deploy script, and tests to use the correct `priceFeed` address.

Testing locally with mock contracts ensures flexibility and efficiency, allowing us to simulate real contract behavior without interacting with a live blockchain.

---

