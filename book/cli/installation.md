# Installation

## Installing CREATOR CLI
### Installation via precompiled Binary
The easiest way to get started with the CLI version is to use the precompiled binaries. They include everything you need to run CREATOR without additional dependencies.
Download the latest precompiled binary for your OS from the [releases page](https://github.com/creatorsim/creator/releases)
and place it in a directory included in your system's PATH.

You're all set! You can verify the installation by running:

```bash
creator --help
```

To access the CLI from any terminal, ensure the binary is in your PATH or create a symbolic link to it in a directory that is. See [below](#creating-an-alias-recommended) for instructions on creating an alias.


### Installation from Source
Alternatively, you can run the CREATOR CLI directly using Deno. Node.js is NOT supported due to webassembly limitations.

#### Prerequisites
- [Deno](https://deno.land/) must be installed on your system. Follow the instructions on the Deno website to install it.
- [Bun](https://bun.sh/) is the recommended package manager to install dependencies.

#### Steps
1. Clone the CREATOR repository:

    ```bash
    git clone https://github.com/creatorsim/creator.git && cd creator
    ```

2. Install dependencies using Bun:
    ```bash
    bun install
    ```

3. Run the CLI:
    ```bash
    deno run --allow-all src/cli/creator6.mts --help
    ```

### Creating an Alias (Recommended)

For easier access, create an alias or script:

#### macOS/Linux (bash/zsh)

Add to your `~/.bashrc`, `~/.zshrc`, or `~/.bash_profile`:

```bash
alias creator='deno run --allow-all /path/to/creator/src/cli/creator6.mts'
```

Then reload your shell:
```bash
source ~/.bashrc  # or ~/.zshrc
```

### Verifying Installation

Test your installation:

```bash
creator --help
```

You should see the CREATOR help message with available options.

## Getting Architecture Files

CREATOR requires architecture definition files to simulate different processors. These files are in YAML format and define the instruction set, registers, memory layout, and other architecture-specific details.

### Default Architectures

The repository includes several architecture files in the `architecture/` directory:

- `RV32IMFD.yml` - RISC-V 32-bit
- `RV64IMFD.yml` - RISC-V 64-bit
- `RV32IMFD-Interrupts.yml` - RISC-V 32-bit with interrupt support
- `MIPS32.yml` - MIPS 32-bit
- `Z80.yml` - Z80 8-bit processor (with interrupts)

## Configuration

The CLI version supports user configuration via a YAML file located at:

- **macOS/Linux**: `~/.config/creator/config.yml`
- **Windows**: `%USERPROFILE%\.config\creator\config.yml`

This file is automatically created with defaults on first run. See the [Configuration](../cli/configuration.md) section for details.

## Updating

If you installed via precompiled binaries, download the latest version from the [releases page](https://github.com/creatorsim/creator/releases).

To update the CLI version, pull the latest changes from the repository and reinstall dependencies:

```bash
git pull origin main && bun install
```

## Next Steps
- Proceed to the [Command-Line Options](command-line-options.md) to learn how to run programs with different architectures and settings.