# Command-Line Options

The CREATOR CLI accepts various command-line options to configure architecture, input files, and behavior.

## Basic Usage

```bash
creator [options]
```

## Required Options

### `--architecture`, `-a`

**Required**: Yes  
**Type**: String (path to YAML file)  
**Description**: Path to the architecture definition file

Specifies which processor architecture to simulate.

**Examples**:
```bash
# RISC-V architecture
creator --architecture architecture/riscv32.yml --assembly program.s

# MIPS architecture
creator -a architecture/mips32.yml -s program.s

# Z80 architecture
creator -a architecture/z80.yml -s program.s
```

## Input Options

### `--assembly`, `-s`

**Type**: String (path to assembly file)  
**Description**: Assembly source file to assemble and load

Assembles the specified file and loads it into memory.

**Example**:
```bash
creator -a riscv32.yml --assembly hello.s
```

### `--bin`, `-b`

**Type**: String (path to binary file)  
**Description**: Binary file to load directly into memory

Loads a pre-assembled binary file without compilation.

**Example**:
```bash
creator -a riscv32.yml --bin program.bin
```

**Note**: Binary and assembly options are mutually exclusive. If both are provided, binary takes precedence.

### `--library`, `-l`

**Type**: String (path to library file)  
**Description**: Library file to load before assembly

Loads a library of pre-assembled code that your program can reference. This is only supported when using the default CREATOR assembler.

**Example**:
```bash
creator -a riscv32.yml -l stdlib.yml -s program.s
```

## Assembler Options

### `--compiler`, `-C`

**Type**: String  
**Default**: `default`  
**Description**: Assembler backend to use

Selects which assembler to use for compilation.

**Available Options**:
- `default` - CREATOR native assembler
- `sjasmplus` - Z80 assembler with macro support
- `rasm` - Alternative Z80 assembler

**Examples**:
```bash
# Use default CREATOR assembler
creator -a riscv32.yml -s program.s

# Use sjasmplus for Z80
creator -a z80.yml -s program.s --compiler sjasmplus

# Use rasm for Z80
creator -a z80.yml -s program.s -C rasm
```

### `--isa`, `-i`

**Type**: Array of strings  
**Default**: `[]`  
**Description**: ISA extensions to load

In supportted architectures, specifies which ISA extensions to enable. If unspecified, the full ISA defined in the architecture file is used.

**Examples**:
```bash
# RISC-V with M extension (multiply/divide)
creator -a riscv32.yml --isa M -s program.s

# RISC-V with multiple extensions
creator -a riscv32.yml --isa M F D -s program.s

# RISC-V base only (no extensions)
creator -a riscv32.yml -s program.s
```

## Other Options

### `--accessible`, `-A`

**Type**: Boolean  
**Default**: `false`  
**Description**: Enable accessible mode for screen readers

Disables colors, ASCII art, and fancy formatting for compatibility with screen readers.

**Example**:
```bash
creator --accessible -a riscv32.yml -s program.s
```

**Changes in Accessible Mode**:
- No ANSI color codes
- Plain text output only
- No ASCII art
- Descriptive text for all information
- Structured table layouts

**Note**: Can also be set in configuration file. Command-line flag overrides config.

### `--config`, `-c`

**Type**: String (path to YAML file)  
**Default**: `~/.config/creator/config.yml`  
**Description**: Path to configuration file

Specifies a custom configuration file instead of the default location.

**Example**:
```bash
creator -a riscv32.yml -s program.s --config custom-config.yml
```

See [Configuration](configuration.md) for config file format.

### `--state`

**Type**: String (path to JSON file)  
**Default**: None  
**Description**: File to save execution state on exit

Saves the current simulator state to the specified file.

**Example**:
```bash
creator -a riscv32.yml -s program.s --state mystate.json
```

**Note**: Use `restore` command in interactive mode to load saved states.

### `--reference`, `-r`

**Type**: String (path to file)  
**Default**: None  
**Description**: Reference file for validation

Used for grading and validation of student exercises. Not typically used by end users. See [Grading Student Exercises](teaching-resources.md#grading-student-exercises) for details.

## Getting Help

Show all available options:
```bash
creator --help
```

## Next Steps
- Continue to the [Commands Reference](commands-reference.md) to learn about interactive commands available in the CLI.