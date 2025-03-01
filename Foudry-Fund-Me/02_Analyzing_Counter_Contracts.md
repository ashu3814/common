# 📝 Lesson 02: Analyzing the Counter Contracts

## 📖 Overview
Continuing from the previous lesson, the `forge init` command populated our project with the Counter files. In this lesson, we will analyze these files and understand their purpose.

---

## 📂 Counter Contracts

### **Counter.sol**
- A simple smart contract that stores a number.
- Key functions:
  - `setNumber(uint256 newNumber)`: Sets a new number.
  - `increment()`: Increments the stored number by 1.
- **Note**: `number++` is equivalent to `number = number + 1`.

### **Counter.s.sol**
- Just a placeholder, it doesn't do anything.

### **Counter.t.sol**
- This file is crucial as it contains tests for the Counter contract.
- The test folder will become essential throughout this course.

---

## 🧪 Running Tests with Foundry

### **Command to Run Tests**:
```bash
forge test
```

### **Test Output Includes**:
- Number of tests found.
- The file in which they were found.
- Test results (pass or fail).
- Summary of test execution.

### **How `forge test` Works**:
- `forge test` has various options to configure tests, display results, and specify test environments.
- To explore options:
  ```bash
  forge test --help
  ```
- Recommended to read further in the Foundry Book.

---

## ⚙️ **Understanding `forge test` Execution**:
- Forge identifies all files in the test folder.
- Runs the `setUp` function.
- Searches for public/external functions starting with `test` and executes them.
- Runs all `assert` statements; if all are true, the test passes, otherwise, it fails.

---

## 🔍 Importance of Testing in Smart Contracts
- Emphasizes test-driven development in programming.
- Essential step for any project to ensure reliability and accuracy.

---

## 🎯 Summary

In this lesson:
- We analyzed the Counter contracts created by `forge init`.
- Learned about the importance of testing using `forge test`.
- Explored the execution flow of tests in Foundry.

Stay tuned for the next lessons where we delve deeper into smart contract development and testing!

---

```
