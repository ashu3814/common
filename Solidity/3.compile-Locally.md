
markdown
# Foundry - Forge Build vs Forge Compile

## Forge Build

### Command:
```sh
forge build
```

### Description:
`forge build` is used to compile your Solidity contracts and generate the necessary artifacts, such as the ABI (Application Binary Interface) and bytecode. It performs a full build of your project, including all dependencies and libraries.

### Key Points:
- Compiles Solidity contracts.
- Generates ABI and bytecode.
- Includes all dependencies and libraries.

## Forge Compile

### Command:
```sh
forge compile
```

### Description:
`forge compile` is also used to compile your Solidity contracts, but it focuses on compiling only the changed files. This command is more efficient for development purposes, as it reduces the compilation time by skipping unchanged files.

### Key Points:
- Compiles only changed Solidity contracts.
- Faster compilation time.
- Useful for development and iterative testing.

## Major Differences

1. **Scope of Compilation**:
   - `forge build`: Compiles all Solidity contracts, including dependencies and libraries.
   - `forge compile`: Compiles only the changed Solidity contracts.

2. **Performance**:
   - `forge build`: Takes longer to compile due to the full project scope.
   - `forge compile`: Faster compilation time as it skips unchanged files.

3. **Use Case**:
   - `forge build`: Ideal for full builds, such as preparing for deployment or generating complete artifacts.
   - `forge compile`: Ideal for development and iterative testing, where only changed files need to be compiled.

## Conclusion

Both `forge build` and `forge compile` are essential commands in Foundry for compiling Solidity contracts. `forge build` is suitable for full builds and generating complete artifacts, while `forge compile` is more efficient for development and iterative testing by compiling only the changed files.

```

