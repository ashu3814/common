## 📂 Increasing Code Coverage with Cheatcodes

### **Why Use Cheatcodes?**
Cheatcodes in Foundry give you powerful assertions, the ability to alter the state of the EVM, mock data, and more. They are essential for testing and ensuring that your smart contracts behave as expected. By using cheatcodes, we can create comprehensive and robust tests to increase our code coverage.

### **Current Coverage**
- **Run Coverage:** Start by running `forge coverage` in your terminal to see the current coverage percentage. Our goal is to improve this coverage significantly from the initial 12-13%.

---

## 📂 Testing the `fund` Function

### **Fund Function Overview**
To increase code coverage, we'll test the following aspects of the `fund` function:
1. Ensure the function reverts if `msg.value` converted to USD is lower than `MINIMUM_USD`.
2. Update `s_addressToAmountFunded` mapping appropriately.
3. Update `s_funders` array with `msg.sender`.

---

## 📂 Using Cheatcodes

### **Cheatcode 1: expectRevert**
- **Usage:** `vm.expectRevert();`
- **Purpose:** This cheatcode is used to assert that the next transaction will revert. If the transaction does not revert, the test will fail.
- **Foundry Documentation:** [expectRevert](https://book.getfoundry.sh/cheatcodes/expect-revert)
- **Example in Lesson:**
  ```solidity
  function testFundFailsWithoutEnoughETH() public {
      vm.expectRevert(); // The next line should revert
      fundMe.fund();     // Sending 0 value
  }
  ```
  - **Explanation:** Here, we are testing that the `fund` function reverts when called with insufficient ETH (0 value). `vm.expectRevert()` ensures that the test passes only if the function reverts as expected.

### **Cheatcode 2: prank**
- **Usage:** `vm.prank(address);`
- **Purpose:** This cheatcode sets `msg.sender` to the specified address for the next call.
- **Foundry Documentation:** [prank](https://book.getfoundry.sh/cheatcodes/prank)
- **Example in Lesson:**
  ```solidity
  function testFundUpdatesFundDataStructure() public {
      vm.prank(alice);
      fundMe.fund{value: SEND_VALUE}();
      uint256 amountFunded = fundMe.getAddressToAmountFunded(alice);
      assertEq(amountFunded, SEND_VALUE);
  }
  ```
  - **Explanation:** In this example, `vm.prank(alice)` sets the sender of the next transaction to `alice`. This is useful for testing functions that require specific user roles or addresses.

### **Cheatcode 3: deal**
- **Usage:** `vm.deal(address, uint256);`
- **Purpose:** This cheatcode sets the ETH balance of the specified address to the given amount.
- **Foundry Documentation:** [deal](https://book.getfoundry.sh/cheatcodes/deal)
- **Example in Lesson:**
  ```solidity
  uint256 constant STARTING_BALANCE = 10 ether;

  function setUp() external {
      vm.deal(alice, STARTING_BALANCE);
      DeployFundMe deployFundMe = new DeployFundMe();
      fundMe = deployFundMe.run();
  }
  ```
  - **Explanation:** `vm.deal(alice, STARTING_BALANCE)` sets `alice`'s balance to 10 ether. This ensures that `alice` has enough ETH to interact with the `fundMe` contract, allowing us to test the `fund` function effectively.

### **Cheatcode 4: startPrank and stopPrank**
- **Usage:** `vm.startPrank(address);` and `vm.stopPrank();`
- **Purpose:** `startPrank` sets `msg.sender` for all subsequent calls until `stopPrank` is called. This is useful for simulating multiple actions by the same user in a row.
- **Foundry Documentation:** [startPrank and stopPrank](https://book.getfoundry.sh/cheatcodes/start-prank)
- **Example Usage:**
  ```solidity
  vm.startPrank(alice);
  // Multiple calls as `alice`
  vm.stopPrank();
  ```
  - **Explanation:** While not explicitly used in this lesson, these cheatcodes are handy when testing sequences of actions by the same user without having to set `prank` before each call.

---

## 📂 Refactoring for Storage Variables

### **Updating Storage Variables**
We refactored the storage variables to follow the `s_` naming convention and set them to private for better gas efficiency.

```solidity
mapping(address => uint256) private s_addressToAmountFunded;
address[] private s_funders;
```

### **Creating Getter Functions**
Since we made the storage variables private, we need to create getter functions to access them:

```solidity
function getAddressToAmountFunded(address fundingAddress) public view returns (uint256) {
    return s_addressToAmountFunded[fundingAddress];
}

function getFunder(uint256 index) public view returns (address) {
    return s_funders[index];
}
```

---

## 📂 Comprehensive Testing with Cheatcodes

### **Testing Data Structure Updates**
Adding a test to ensure that the `s_addressToAmountFunded` mapping and `s_funders` array are updated correctly:

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

By using these cheatcodes, we can efficiently and effectively test smart contracts, ensuring robust and reliable code.
