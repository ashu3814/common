# 📝 Lesson 04: Forking Tests

## 📖 Overview
This lesson covers the different types of tests in Solidity smart contract development, with a focus on forking tests. Forking tests create a copy of a blockchain state at a specific point in time to run tests in a simulated environment.

### Types of Tests:
1. **Unit tests:** Focus on isolating and testing individual smart contract functions or functionalities.
2. **Integration tests:** Verify how a smart contract interacts with other contracts or external systems.
3. **Forking tests:** Create a copy of a blockchain state to run tests in a simulated environment.
4. **Staging tests:** Execute tests against a deployed smart contract on a staging environment before mainnet deployment.

---

## 📂 Testing Price Feed Version

### **Testing the Version**
- Add the following function to your test file to test if the AggregatorV3 runs the current version:
```solidity
function testPriceFeedVersionIsAccurate() public {
    uint256 version = fundMe.getVersion();
    assertEq(version, 4);
}
```

### **Running the Test**
- The test fails because the AggregatorV3 address does not exist on Anvil.
- Use the following command to run specific tests:
  ```bash
  forge test --mt testPriceFeedVersionIsAccurate
  ```

### **Forking to Fix the Issue**
- Forking is the solution to test on an anvil instance that copies the current Sepolia state where AggregatorV3 exists.

### **Create a .env File**
1. Create a `.env` file and ensure `.gitignore` contains `.env` entry.
2. Add the following entry to `.env`:
   ```plaintext
   SEPOLIA_RPC_URL=https://eth-sepolia.g.alchemy.com/v2/YOURAPIKEYWILLGOHERE
   ```

### **Run Tests with Fork URL**
- Source the .env file in your terminal:
  ```bash
  source .env
  ```
- Run the test with the fork URL:
  ```bash
  forge test --mt testPriceFeedVersionIsAccurate --fork-url $SEPOLIA_RPC_URL
  ```

### **Expected Output**
- The test should pass, confirming that the version is accurate.

---

## 📂 Coverage

### **Calculating Coverage**
- Foundry provides a way to calculate the coverage by calling:
  ```bash
  forge coverage --fork-url $SEPOLIA_RPC_URL
  ```

### **Example Output**
```plaintext
Ran 3 tests for test/FundMe.t.sol:FundMeTest
[PASS] testMinimumDollarIsFive() (gas: 5759)
[PASS] testOwnerIsMsgSender() (gas: 8069)
[PASS] testPriceFeedVersionIsAccurate() (gas: 14539)
Suite result: ok. 3 passed; 0 failed; 0 skipped; finished in 1.91s (551.69ms CPU time)

Ran 1 test suite in 2.89s (1.91s CPU time): 3 tests passed, 0 failed, 0 skipped (3 total tests)
| File                      | % Lines       | % Statements  | % Branches    | % Funcs      |
| ------------------------- | ------------- | ------------- | ------------- | ------------ |
| script/DeployFundMe.s.sol | 0.00% (0/3)   | 0.00% (0/3)   | 100.00% (0/0) | 0.00% (0/1)  |
| src/FundMe.sol            | 21.43% (3/14) | 25.00% (5/20) | 0.00% (0/6)   | 33.33% (2/6) |
| src/PriceConverter.sol    | 0.00% (0/6)   | 0.00% (0/11)  | 100.00% (0/0) | 0.00% (0/2)  |

| Total                     | 13.04% (3/23) | 14.71% (5/34) | 0.00% (0/6)   | 22.22% (2/9) |
```

- These numbers are low and need to be improved in subsequent lessons.

### **Updated Command in Latest Foundry Version**
- The `-m` flag is no longer supported. Use the following command:
  ```bash
  forge test -m testPriceFeedVersionIsAccurate -vvv --fork-url $SEPOLIA_RPC_URL
  ```

---

## 🧪 Summary

In this lesson:
- We learned about the different types of tests in Solidity development.
- Focused on forking tests and their importance.
- Implemented and ran a test for the AggregatorV3 version.
- Calculated code coverage to ensure high test coverage.

Forking tests are essential for validating contract interactions in a simulated environment. Ensuring high test coverage is key to developing robust smart contracts.

---
