# 📝 Lesson 05: Refactoring Code and Tests

## 📖 Overview
Refactoring code is essential for making it more modular and maintainable. This lesson focuses on refactoring the `FundMe` contract to make it flexible for deployment on different chains and updating the corresponding tests.

---

## 📂 Refactoring FundMe Contract

### **Storage Variables**
- Add a new variable to the storage section:
```solidity
AggregatorV3Interface private s_priceFeed;
```

### **Constructor Modification**
- Update the constructor to accept the `priceFeed` address:
```solidity
constructor(address priceFeed) {
    i_owner = msg.sender;
    s_priceFeed = AggregatorV3Interface(priceFeed);
}
```

### **Updating getVersion Function**
- Replace the hardcoded address with the state variable `s_priceFeed`:
```solidity
function getVersion() public view returns (uint256) {
    return s_priceFeed.version();
}
```

### **Updating PriceConverter Library**
- Modify `getPrice` function to take an input:
```solidity
function getPrice(AggregatorV3Interface priceFeed) internal view returns (uint256) {
    (, int256 answer, , , ) = priceFeed.latestRoundData();
    return uint256(answer * 10000000000);
}
```
- Update `getConversionRate` function to take a `priceFeed` input:
```solidity
function getConversionRate(uint256 ethAmount, AggregatorV3Interface priceFeed) internal view returns (uint256) {
    uint256 ethPrice = getPrice(priceFeed);
    uint256 ethAmountInUsd = (ethPrice * ethAmount) / 1000000000000000000;
    return ethAmountInUsd;
}
```

### **Passing s_priceFeed in FundMe Contract**
- Update the `fund` function in `FundMe.sol`:
```solidity
require(msg.value.getConversionRate(s_priceFeed) >= MINIMUM_USD, "You need to spend more ETH!");
```

---

## 📂 Updating Deploy Script

### **Update DeployFundMe Script**
- Modify the `run` function to accept the `priceFeed` address:
```solidity
function run() external returns (FundMe) {
    vm.startBroadcast();
    FundMe fundMe = new FundMe(0x694AA1769357215DE4FAC081bf1f309aDC325306);
    vm.stopBroadcast();
    return fundMe;
}
```

---

## 📂 Updating Tests

### **Import Deploy Script in FundMeTest**
- Import the deployment script:
```solidity
import {DeployFundMe} from "../script/DeployFundMe.s.sol";
```
- Create a new state variable for `DeployFundMe`:
```solidity
DeployFundMe deployFundMe;
```
- Update the `setUp` function:
```solidity
function setUp() external {
    deployFundMe = new DeployFundMe();
    fundMe = deployFundMe.run();
}
```

### **Fixing testOwnerIsMsgSender**
- Ensure the correct owner is tested:
```solidity
function testOwnerIsMsgSender() public {
    assertEq(fundMe.i_owner(), msg.sender);
}
```

### **Running Tests**
- Run the tests to ensure everything compiles and passes:
  ```bash
  forge test --fork-url $SEPOLIA_RPC_URL
  ```

---

## 🧪 Summary

In this lesson:
- We refactored the `FundMe` contract to make it modular and flexible for different chains.
- Updated the `PriceConverter` library.
- Modified the deploy script to accept dynamic inputs.
- Updated the tests to reflect the refactored code and deployment changes.

Refactoring code and tests improve maintainability, testing, and deployment. These practices are essential for professional developers to ensure code quality and flexibility.

---

