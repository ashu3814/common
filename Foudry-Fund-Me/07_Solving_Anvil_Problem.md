# 📝 Lesson 07: Solving the Anvil Problem

## 📖 Overview
When deploying to the Anvil local blockchain, the necessary contracts (like the Sepolia `priceFeed`) do not exist. To solve this, we will deploy mock contracts that simulate the behavior of real contracts during testing.

---

## 📂 Deploying Mock Contracts

### **Why Deploy Mocks?**
- **Flexibility:** Avoids hardcoding addresses and allows for local testing.
- **Simulation:** Mocks simulate the behavior of real contracts, enabling efficient testing.

---

## 📂 Creating HelperConfig

### **Step 1: Update HelperConfig File**
- Ensure you import `Script.sol` and inherit from `Script`:
```solidity
import {Script} from "forge-std/Script.sol";

contract HelperConfig is Script {

}
```

### **Step 2: Creating MockV3Aggregator**
- In the `test` folder, create a new folder called `mocks`.
- Inside `mocks`, create a new file called `MockV3Aggregator.sol`.
- Copy the contents of the `AggregatorV3` contract into this file.

### **Step 3: Deploying Mock Contracts**
- Import `MockV3Aggregator` in `HelperConfig.s.sol` and deploy it in the `getAnvilEthConfig` function:
```solidity
import {MockV3Aggregator} from "../test/mocks/MockV3Aggregator.sol";

// In state variables section
MockV3Aggregator mockPriceFeed;

function getAnvilEthConfig() public returns (NetworkConfig memory) {
    vm.startBroadcast();
    mockPriceFeed = new MockV3Aggregator(8, 2000e8);
    vm.stopBroadcast();

    NetworkConfig memory anvilConfig = NetworkConfig({
        priceFeed: address(mockPriceFeed)
    });

    return anvilConfig;
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
- We learned how to solve the Anvil problem by deploying mock contracts.
- Created the `HelperConfig` contract to deploy mocks and provide the correct `priceFeed` address.
- Updated the `FundMe` contract, deploy script, and tests to use the correct `priceFeed` address.

Deploying mock contracts allows us to simulate real contract behavior and run tests locally without depending on external networks.

---
