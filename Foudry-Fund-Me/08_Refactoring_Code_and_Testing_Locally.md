# 📝 Lesson 08: Refactoring Code and Testing Locally

## 📖 Overview
This lesson covers refactoring code to avoid magic numbers and deploying mock contracts for local testing. These practices ensure flexibility, maintainability, and efficient testing.

---

## 📂 Why Avoid Magic Numbers?

### **Drawbacks of Magic Numbers**
- **Reduced Readability:** It's difficult to understand what a magic number represents.
- **Increased Maintenance Difficulty:** Changes require finding and updating every instance of the magic number.
- **Debugging Challenges:** Identifying issues is harder without context for the numbers used.

### **Solution: Use Constants**
- Constants improve readability, maintainability, and reduce the risk of errors.

---

## 📂 Refactoring HelperConfig

### **Step 1: Identify Magic Numbers**
- In `HelperConfig.s.sol`, locate the magic numbers `8` (decimals) and `2000e8` (initial price) used in `MockV3Aggregator`'s constructor.

### **Step 2: Define Constants**
- Define constants at the top of the `HelperConfig` contract:
```solidity
uint8 public constant DECIMALS = 8;
int256 public constant INITIAL_PRICE = 2000e8;
```

### **Step 3: Replace Magic Numbers**
- Replace the magic numbers in `getAnvilEthConfig` with the newly created constants:
```solidity
function getOrCreateAnvilEthConfig() public returns (NetworkConfig memory) {
    if (activeNetworkConfig.priceFeed != address(0)) {
        return activeNetworkConfig;
    }
    
    vm.startBroadcast();
    mockPriceFeed = new MockV3Aggregator(DECIMALS, INITIAL_PRICE);
    vm.stopBroadcast();

    NetworkConfig memory anvilConfig = NetworkConfig({
        priceFeed: address(mockPriceFeed)
    });

    return anvilConfig;
}
```

### **Efficiency Check**
- Before deploying `mockPriceFeed`, check if it has already been deployed to avoid redundant deployments:
```solidity
function getOrCreateAnvilEthConfig() public returns (NetworkConfig memory) {
    if (activeNetworkConfig.priceFeed != address(0)) {
        return activeNetworkConfig;
    }
    
    vm.startBroadcast();
    mockPriceFeed = new MockV3Aggregator(DECIMALS, INITIAL_PRICE);
    vm.stopBroadcast();

    NetworkConfig memory anvilConfig = NetworkConfig({
        priceFeed: address(mockPriceFeed)
    });

    return anvilConfig;
}
```

### **Renaming Function**
- Rename `getAnvilEthConfig` to `getOrCreateAnvilEthConfig` to better reflect its purpose.

---

## 📂 Testing Locally with Mock Contracts

### **Deploying Mock Contracts**
- In the `test` folder, create a new folder called `mocks`.
- Inside `mocks`, create a new file called `MockV3Aggregator.sol`.
- Copy the contents of the `AggregatorV3` contract into this file.

### **Deploying Mock Contracts in HelperConfig**
- Import `MockV3Aggregator` in `HelperConfig.s.sol` and deploy it in the `getOrCreateAnvilEthConfig` function:
```solidity
import {MockV3Aggregator} from "../test/mocks/MockV3Aggregator.sol";

// In state variables section
MockV3Aggregator mockPriceFeed;

function getOrCreateAnvilEthConfig() public returns (NetworkConfig memory) {
    if (activeNetworkConfig.priceFeed != address(0)) {
        return activeNetworkConfig;
    }
    
    vm.startBroadcast();
    mockPriceFeed = new MockV3Aggregator(DECIMALS, INITIAL_PRICE);
    vm.stopBroadcast();

    NetworkConfig memory anvilConfig = NetworkConfig({
        priceFeed: address(mockPriceFeed)
    });

    return anvilConfig;
}
```

### **Testing Network Agnosticism**
- Ensure tests pass without specifying the `--fork-url` option:
  ```bash
  forge test
  ```
- Ensure tests pass with the `--fork-url` option:
  ```bash
  forge test --fork-url $SEPOLIA_RPC_URL
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
- We refactored the code to avoid magic numbers by using constants.
- Ensured efficient deployment of mock contracts by checking existing deployments.
- Created a network-agnostic testing setup using mock contracts.
- Updated the `FundMe` contract, deploy script, and tests to use the correct `priceFeed` address.

Refactoring code and testing locally with mock contracts ensures flexibility, maintainability, and efficient testing, making your smart contracts robust and reliable.

---
