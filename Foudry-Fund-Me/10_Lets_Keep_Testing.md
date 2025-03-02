## 📂 Continuing from the Previous Lesson

In the previous lesson, we tested if the `s_addressToAmountFunded` is updated correctly when a funder contributes to the contract. Now, let's continue and test that the `funders` array is updated with `msg.sender` as expected. This ensures our contract tracks the contributors properly.

---

## 📂 Testing the `funders` Array

### **Adding the Test for Funders Array**
Add the following test to your `FundMe.t.sol` file:

```solidity
function testAddsFunderToArrayOfFunders() public {
    vm.startPrank(alice);
    fundMe.fund{value: SEND_VALUE}();
    vm.stopPrank();

    address funder = fundMe.getFunder(0);
    assertEq(funder, alice);
}
```

### **Explanation:**
- **Using `prank` Cheatcode:** 
  - `vm.startPrank(alice);` sets the sender of the next transaction to `alice`.
  - This simulates Alice funding the contract.
- **Verifying the Funder:** 
  - We use the getter function `getFunder(0)` to query the `funders` array at index `0`.
  - We then use the `assertEq` cheatcode to compare the address queried against `alice`.

### **Running the Test:**
Run the test using:
```bash
forge test --mt testAddsFunderToArrayOfFunders
```
- The test should pass, confirming the `funders` array is updated correctly.

---

## 📂 Testing the Withdraw Function

Next, we need to ensure that only the owner can withdraw funds from the contract.

### **Testing Owner Withdrawal:**
Add the following test to your `FundMe.t.sol` file:

```solidity
function testOnlyOwnerCanWithdraw() public {
    vm.prank(alice);
    fundMe.fund{value: SEND_VALUE}();

    vm.expectRevert();
    vm.prank(alice);
    fundMe.withdraw();
}
```

### **Explanation:**
- **Funding by Alice:**
  - `vm.prank(alice); fundMe.fund{value: SEND_VALUE}();` simulates Alice funding the contract.
- **Expecting Revert:** 
  - `vm.expectRevert();` ensures the next transaction should revert.
  - Alice (not the owner) tries to withdraw, which should fail.

### **Running the Test:**
Run the test using:
```bash
forge test --mt testOnlyOwnerCanWithdraw
```
- The test should pass, confirming only the owner can withdraw funds.

---

## 📂 Refactoring with Modifiers

To avoid repeating the funding code in multiple tests, let's use a modifier.

### **Adding the Modifier:**
Add the following modifier to your `FundMe.t.sol` file:

```solidity
modifier funded() {
    vm.prank(alice);
    fundMe.fund{value: SEND_VALUE}();
    assert(address(fundMe).balance > 0);
    _;
}
```

### **Explanation:**
- **Using `prank` and `assertEq`:**
  - `vm.prank(alice); fundMe.fund{value: SEND_VALUE}();` ensures Alice funds the contract.
  - `assert(address(fundMe).balance > 0);` checks if the contract balance is greater than 0, confirming the funding.

### **Refactoring the Test:**
Refactor the previous test using the modifier:

```solidity
function testOnlyOwnerCanWithdraw() public funded {
    vm.expectRevert();
    fundMe.withdraw();
}
```

---

## 📂 Testing Owner Withdrawal with Getter Functions

We need a new getter function to test owner withdrawal. Add the following to `FundMe.sol`:

```solidity
function getOwner() public view returns (address) {
    return i_owner;
}
```

### **Explanation:**
- **Getter for Owner:** 
  - This function returns the owner's address for verification purposes.

### **Testing Owner Withdrawal:**
Add the following test using the Arrange-Act-Assert methodology:

```solidity
function testWithdrawFromASingleFunder() public funded {
    // Arrange
    uint256 startingFundMeBalance = address(fundMe).balance;
    uint256 startingOwnerBalance = fundMe.getOwner().balance;

    // Act
    vm.startPrank(fundMe.getOwner());
    fundMe.withdraw();
    vm.stopPrank();

    // Assert
    uint256 endingFundMeBalance = address(fundMe).balance;
    uint256 endingOwnerBalance = fundMe.getOwner().balance;
    assertEq(endingFundMeBalance, 0);
    assertEq(startingFundMeBalance + startingOwnerBalance, endingOwnerBalance);
}
```

### **Running the Test:**
Run the test using:
```bash
forge test --mt testWithdrawFromASingleFunder
```
- The test should pass, confirming the owner can withdraw funds properly.

---

## 📂 Testing Withdrawal with Multiple Funders

Add the following test to ensure the owner can withdraw when the contract is funded by multiple users:

```solidity
function testWithdrawFromMultipleFunders() public funded {
    uint160 numberOfFunders = 10;
    uint160 startingFunderIndex = 1;
    for (uint160 i = startingFunderIndex; i < numberOfFunders + startingFunderIndex; i++) {
        hoax(address(i), SEND_VALUE);
        fundMe.fund{value: SEND_VALUE}();
    }

    uint256 startingFundMeBalance = address(fundMe).balance;
    uint256 startingOwnerBalance = fundMe.getOwner().balance;

    vm.startPrank(fundMe.getOwner());
    fundMe.withdraw();
    vm.stopPrank();

    assert(address(fundMe).balance == 0);
    assertEq(startingFundMeBalance + startingOwnerBalance, fundMe.getOwner().balance);
    assertEq((numberOfFunders + 1) * SEND_VALUE, fundMe.getOwner().balance - startingOwnerBalance);
}
```

### **Explanation:**
- **Funding by Multiple Users:**
  - Using `hoax`, which combines `deal` and `prank`, to fund the contract with multiple users.
- **Testing Withdrawal:**
  - Ensuring the owner can withdraw all funds when funded by multiple users.

### **Running the Test:**
Run the test using:
```bash
forge test --mt testWithdrawFromMultipleFunders
forge test
```
- The tests should pass, confirming the owner can withdraw funds properly even when the contract is funded by multiple users.

### **Checking Coverage:**
Run `forge coverage` to see the updated coverage percentage and ensure our tests are comprehensive.

---

## 📂 Summary

In this lesson:
- We continued testing from the previous lesson by ensuring the `funders` array is updated correctly.
- Verified that only the owner can withdraw funds using cheatcodes like `expectRevert` and `prank`.
- Refactored tests using modifiers for efficiency.
- Used the Arrange-Act-Assert methodology to structure our tests.
- Tested the withdrawal function for both single and multiple funders.
- Increased code coverage and ensured robust testing.

