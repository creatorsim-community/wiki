# CREATOR - didaCtic and geneRic assEmbly progrAmming simulaTOR

Welcome to the CREATOR documentation! CREATOR is a comprehensive, open-source assembly programming simulator designed for education and development across multiple computer architectures.

### 👨‍🎓 **Students, Start Here!**
New to CREATOR? Get up and running quickly:
1. **[Try it now in your browser](https://rajayonin.github.io/creatorV/)** - No installation needed!
2. Learn about the **[Web Interface](web/user-interface.md)** to navigate the simulator

### 👨‍🏫 **Teachers, Start Here!**
Setting up CREATOR for your course:
1. **[Installation Guide](cli/installation.md)** - Get the CLI version up and running
2. **[Teaching Resources](teaching-resources/teaching-resources.md)** - Useful materials for educators
3. **[Custom Architectures](development/custom-architectures.md)** - Create your own instruction sets

### Developers, Start Here!
Interested in contributing or extending CREATOR?
1. **[Development Guide](development/dev.md)** - Learn how to set up the development

---

## What is CREATOR?

CREATOR is a didactic and generic assembly programming simulator that provides a user-friendly environment for writing, assembling, and simulating assembly code. It supports various architectures, including RISC-V, MIPS, and Z80, and allows users to define custom architectures. It is available as both a web-based application and a command-line interface (CLI) tool.

## Key Features
- **Device System**: Console I/O, OS drivers, and custom device support
- **Interrupt Handling**: Software, timer, and external interrupt simulation
- **Stack Tracking**: Automatic call stack tracking and visualization
- **Memory Hints**: Tag memory locations with type information for easier debugging
- **Privileged Instructions**: Kernel/user mode separation with privilege levels