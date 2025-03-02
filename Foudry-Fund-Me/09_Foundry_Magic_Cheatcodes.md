# 📝 Lesson 09: Foundry Magic - Cheatcodes

## 📖 Overview
In the previous lessons, we have successfully set up our `FundMe` contract, deployed it using different configurations, and tested it using mock contracts. Now that our deployment script and tests are blockchain agnostic, we can focus on increasing our code coverage. In this lesson, we’ll employ Foundry’s cheatcodes to test the `fund` function from `FundMe.sol` and ensure robust testing.

---

## 📂 Increasing Code Coverage

### **Why Code Coverage Matters**
Having high code coverage ensures that our code is thoroughly tested, reducing the risk of bugs and vulnerabilities. We aim to bring our total coverage percentage as close to 100% as possible.

### **Current Coverage**
- Run `forge coverage` in your terminal to see the current coverage.
- Our goal is to improve the coverage percentage significantly from the initial 12-13%.

---

## 📂 Testing the `fund` Function

### **Fund Function Overview**
To increase code coverage, we'll test the following aspects of the `fund` function:
1. Ensure the function reverts if `msg.value` converted to USD is lower than `MINIMUM_USD`.
2. Update `s_addressToAmountFunded` mapping appropriately.
3. Update `s_funders` array with `msg.sender`.

---

## 📂 Using Cheatcodes

### **Testing Revert on Insufficient ETH**
Using the `expectRevert` cheatcode to test for reversion when funding with insufficient ETH:

```solidity
function testFundFailsWithoutEnoughETH() public {
    vm.expectRevert(); // The next line should revert
    fundMe.fund();     // Sending 0 value
}
```

### **Refactoring for Storage Variables**
Updating storage variables to follow the `s_` naming convention and setting them to private:

```solidity
mapping(address => uint256) private s_addressToAmountFunded;
address[] private s_funders;
```

### **Creating Getter Functions**
Updating `FundMe.sol` to include getter functions for the storage variables:

```solidity
function getAddressToAmountFunded(address fundingAddress) public view returns (uint256) {
    return s_addressToAmountFunded[fundingAddress];
}

function getFunder(uint256 index) public view returns (address) {
    return s_funders[index];
}
```

### **Testing Data Structure Updates**
Adding a test for data structure updates using the `prank` cheatcode:

```solidity
function testFundUpdatesFundDataStructure() public {
    vm.prank(alice);
    fundMe.fund{value: SEND_VALUE}();
    uint256 amountFunded = fundMe.getAddressToAmountFunded(alice);
    assertEq(amountFunded, SEND_VALUE);
}
```

### **Setting Up Constants and Addresses**
Defining a constant for the send value and declaring the `alice` address:

```solidity
address alice = makeAddr("alice");
uint256 constant SEND_VALUE = 0.1 ether;
```

### **Ensuring Sufficient Balance**
Using the `deal` cheatcode to ensure `alice` has sufficient balance for testing:

```solidity
uint256 constant STARTING_BALANCE = 10 ether;

function setUp() external {
    vm.deal(alice, STARTING_BALANCE);
    DeployFundMe deployFundMe = new DeployFundMe();
    fundMe = deployFundMe.run();
}
```

### **Running the Test**
Running the specific test to check the updates:

```bash
forge test --mt testFundUpdatesFundDataStructure -vvv
```

### **Debugging Test Failures**
Using verbose mode to get more information on test failures and debugging them:

```bash
forge test --mt testFundUpdatesFundDataStructure -vvv
```

---

## 📂 Summary

In this lesson:
- We linked our testing efforts with the previous lessons.
- Used Foundry’s cheatcodes to test the `fund` function and ensure it reverts on insufficient ETH.
- Tested updates to data structures using cheatcodes like `expectRevert`, `prank`, and `deal`.
- Increased code coverage significantly by writing comprehensive tests.

By using Foundry’s cheatcodes, we can efficiently and effectively test smart contracts, ensuring robust and reliable code.

---
