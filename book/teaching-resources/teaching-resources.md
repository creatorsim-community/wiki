# Teaching Resources

## Overview
CREATOR is a powerful tool for teaching computer architecture and assembly programming. This document provides resources and guidance for educators looking to integrate CREATOR into their curriculum.

## State Snapshots
CREATOR Supports saving and restoring complete simulator state via snapshots. This feature is particularly useful for educators who want to create predefined states for students to explore or debug.

## Grading Student Exercises
CREATOR CLI allows you to validate a program against an expected final state. This includes specifying the values of memory, registers (including floating point registers, within an error threshold), and the display buffer, and whether to error on calling convention errors (sentinel).

For this, you first need to define a YAML validator file. This file must contain the expected final state of the program, as well as any additional options you want to set for the validation.

### Validator File
The validator file is a YAML file that can contain the following fields:
- `maxCycles`: The maximum number of cycles to run the program for validation. If the program exceeds this number of cycles, it will be considered invalid.
- `floatThreshold`: The maximum allowed error for floating point register comparisons.
- `sentinel`: A boolean indicating whether to enable sentinel checking (calling convention errors).
- `state`: An object defining the expected final state of the program, which can contain:
  - `registers`: A mapping of integer register names to their expected values.
  - `floatRegisters`: A mapping of floating point register names to their expected values.
  - `memory`: A mapping of memory addresses (as strings) to their expected byte values.
  - `display`: The expected contents of the display buffer as a string.

The validator will return, following UNIX conventions, `0` if its valid and `1` if it's not, writting the error message to `stderr`.


### Example
Quick example of a validator file:
```yaml
# yaml-language-server: $schema=https://creatorsim.github.io/schema/validator-file.json
maxCycles: 100
floatThreshold: 10e-10
sentinel: true
state:
  floatRegisters:
    f0: 0x40866666
  registers:
    sp: 0x0FFFFFFC
    a1: 0x0000005A
  memory:
    "0x200000": 0x45
  display: "-144"
```

### Using the Validator
To use the validator file with CREATOR CLI, use the `--validate` option followed by the path to the validator YAML file. For example:
```bash
creator -a RV32IMFD.yml -s assembly.s --validate config.yml
```

## Creating Custom Architectures
CREATOR allows educators to create custom architectures tailored to their curriculum. This can be done by defining new architecture YAML files that specify instruction sets, registers, memory layout, and other architecture-specific details. See [Creating Custom Architectures](../development/custom-architectures.md) for more information.