# Creating Custom Architectures

CREATOR supports defining custom architectures through YAML configuration files. This allows adding new instruction sets or modifying existing ones.

## Creating an Architecture File
An architecture file is a YAML file that describes the architecture's properties, including its instruction set, registers, memory layout, and other relevant details. Instructions are defined with their binary encoding, assembly syntax, and semantics. The actual definition for an instruction is simple javascript code to manipulate the simulator state.

### Example
Let's create a simple architecture with a few instructions. The first step is to create a YAML file, e.g., `SimpleArch.yml`:

We will define a simple architecture with two instructions: `NOP` and `ADD`.
The `NOP` instruction does nothing, while the `ADD` instruction adds the values of two registers and stores the result in a destination register.

```yaml
# yaml-language-server: $schema=./schema.json
version: 2.0.0
config:  
  name: Simple8Bit
  word_size: 8
  byte_size: 8
  description: "A simple custom 8-bit architecture"
  endianness: little_endian
  memory_alignment: true
  passing_convention: true
  sensitive_register_name: false
  main_function: main
  comment_prefix: ";"
  start_address: 0x0
  pc_offset: 0

templates:
  - name: standard
    nwords: 1
    clk_cycles: 1
    fields:
      - name: opcode
        type: co
        startbit: 7
        stopbit: 0
        order: 0

components:
  - name: Control registers
    type: ctrl_registers
    double_precision: false
    elements:
      - name:
          - PC
        nbits: 8
        encoding: 0
        value: 0
        default_value: 0
        properties:
          - read
          - write
          - program_counter
  - name: Integer registers
    type: int_registers
    double_precision: false
    elements:
      - name:
          - A
        encoding: 0
        nbits: 8
        value: 0
        default_value: 0
        properties:
          - read
          - write
      - name:
          - B
        encoding: 1
        nbits: 8
        value: 0
        default_value: 0
        properties:
          - read
          - write
      - name:
          - SP
        encoding: 2
        nbits: 8
        value: 0
        default_value: 0
        properties:
          - read
          - write
          - stack_pointer

memory_layout:
  text:
    start: 0x0000
    end: 0x03FF
  data:
    start: 0x0400
    end: 0x7FFF
  stack:
    start: 0x8000
    end: 0xFFFF

instructions:
  base:
    - name: nop
      template: standard
      fields:
        - field: opcode
          value: "0x00"
      definition: ""

    - name: add
      template: standard
      fields:
        - field: opcode
          value: "0x80"
      definition: |
        const oldValueA = registers.A;
        registers.A = (oldValueA + registers.B) & 0xFFn;
        registers.F = CAPI.ARCH.calculateFlags_ADD(oldValueA, registers.B);


directives:
  - name: .data
    action: data_segment
    size: null
  - name: .text
    action: code_segment
    size: null
  - name: .bss
    action: global_symbol
    size: null
  - name: .zero
    action: space
    size: 1
  - name: .space
    action: space
    size: 1
  - name: .align
    action: align
    size: null
  - name: .balign
    action: balign
    size: null
  - name: .globl
    action: global_symbol
    size: null
  - name: .string
    action: ascii_null_end
    size: null
  - name: .asciz
    action: ascii_null_end
    size: null
  - name: .ascii
    action: ascii_not_null_end
    size: null
  - name: .byte
    action: byte
    size: 1
  - name: .half
    action: half_word
    size: 2
  - name: .word
    action: word
    size: 4
  - name: .dword
    action: double_word
    size: 8
  - name: .float
    action: float
    size: 4
  - name: .double
    action: double
    size: 8
