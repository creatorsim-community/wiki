# CREATOR API (CAPI)

CAPI allows instruction definitions to interact with custom CREATOR functions.

> [!TIP]
> `CAPI` is a global variable, so you can access it from your browser's developer console if you're using the web application.


## Memory
Interaction with CREATOR's memory.


### `CAPI.MEM.write`
```ts
CAPI.MEM.write(
  address: bigint,
  bytes: number,
  value: bigint,
  reg_name?: string,
  hint?: string,
  noSegFault: boolean = true,
): void { }
```

Writes a specific `value` to the specified `address` and number of `bytes`. You can provide extra information about which register used to hold the value (`reg_name`), or any hint about what that value might be (`hint`). To enable checking if the address is in a writable segment, set `noSegFault` to false.

E.g.:
```js
CAPI.MEM.write(registers.t0, 1, registers.t0, "t0");
```


### `CAPI.MEM.read`
```ts
CAPI.MEM.read(
  address: bigint,
  bytes: number,
  reg_name: string,
  noSegFault: boolean = true,
): bigint { }
```

Reads a specific number of `bytes` in the specified `address` and returns them. You can provide extra information about which register will hold the value (`reg_name`). To enable checking if the address is in a readable segment, set `noSegFault` to false.

E.g.:
```js
registers.t0 = CAPI.MEM.read(registers.t1, 1, "t0");
```


<!--
### `CAPI.MEM.alloc`
> [!WARNING]
> This function is not currently implemented.

```ts
CAPI.MEM.alloc(bytes: number): number { }
```

Allocates the specified `size` number of bytes in the heap.
-->



### `CAPI.MEM.addHint`

```ts
CAPI.MEM.addHint(address: bigint, hint: string, sizeInBits?: number): boolean { }
```

Adds a `hint` (description of what the address holds) for the specified memory `address`. If a hint already exists at the specified address, it replaces it. You can optionally specify the size of the stored type in bits (`sizeInBits`). Returns `true` if the hint was successfully added, else `false`.

E.g.:
```js
CAPI.MEM.addHint(registers.f0, "float64", 64);
```


## System calls
CREATOR's system calls.


### `CAPI.SYSCALL.exit`

```ts
CAPI.SYSCALL.exit(): void { }
```

Terminates the execution of the program.

E.g.:
```js
CAPI.SYSCALL.exit();
```



### `CAPI.SYSCALL.print`

```ts
CAPI.SYSCALL.print(
  value: number | bigint,
  type: "int32" | "float" | "double" | "char" | "string",
): void { }
```

Prints the specified `value` of the specified `type` to the console.

Supported types are:
- `"int32"`: signed 32-bit integer
- `"float"` / `"double"`: [JS's Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)
- `"char"`: UTF-16 character value
- `"string"`: address of a null-terminated array of 1-byte chars

E.g.:
```js
CAPI.SYSCALL.print(registers.a0, "char");
```


### `CAPI.SYSCALL.read`

```ts
CAPI.SYSCALL.read(
  dest_reg_info: string,
  type: "int32" | "float" | "double" | "char" | "string",
  aux_info?: string,
): void { }
```

Reads the specified value `type` from the console and stores it in the specified register by name (`dest_reg_info`).

Supported types are:
- `"int32"`: signed 32-bit integer
- `"float"` / `"double"`: [JS's Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)
- `"char"`: UTF-16 character value
- `"string"`: `aux_info` is the name of the register that holds the address of a null-terminated array of 1-byte chars

E.g.:
```js
CAPI.SYSCALL.read("a0", "char");
CAPI.SYSCALL.read("a0", "string", "a1");
```



### `CAPI.SYSCALL.get_clk_cycles`

```ts
CAPI.SYSCALL.get_clk_cycles(): number { }
```

Returns the number of clock cycles that have passed since the program started.

E.g.:
```js
CAPI.SYSCALL.get_clk_cycles();
```


<!--
### `CAPI.SYSCALL.sbrk`

```ts
CAPI.SYSCALL.sbrk(value1: string, value2: string): void { }
```


E.g.:
```js
CAPI.SYSCALL.sbrk("a0", "v0");
```
-->



## Validation

### `CAPI.SYSCALL.raise`

```ts
CAPI.SYSCALL.raise(msg: string): never { }
```

Raises an error with a specific `msg`.

E.g.:
```js
CAPI.SYSCALL.raise("Help!");
```


### `CAPI.SYSCALL.isOverflow`

```ts
CAPI.SYSCALL.isOverflow(op1: bigint, op2: bigint, res_u: bigint): boolean { }
```

Checks if the result `res_u` of operating two operands `op1` and `op2` caused an overflow.

E.g.:
```js
CAPI.SYSCALL.isOverflow(registers.t0, registers.t1, registers.t0 + registers.t1);
```


## Stack
These functions are used for the stack tracker and sentinel modules, and are a way to tell CREATOR when a new function frame begins and ends. They should be included in the instructions that jump to, o return from, a routine, as is the case of RISC-V's `jal` and `jr` instructions.


### `CAPI.STACK.beginFrame`

```ts
CAPI.STACK.beginFrame(addr?: bigint): void { }
```

Marks the beginning of a function frame at `address`. If not specified, it takes the current (real) value of the program counter.

E.g.:
```js
registers.pc = ...
CAPI.SYSCALL.beginFrame();
```


### `CAPI.STACK.endFrame`

```ts
CAPI.STACK.endFrame(): void { }
```

Ends the current stack frame.

E.g.:
```js
registers.pc = ...
CAPI.SYSCALL.endFrame();
```



## Floating point


## Registers


### `CAPI.REG.read`

```ts
CAPI.REG.read(name: string): bigint { }
```

Returns the value stored in register `name`.

E.g.:
```js
const foo = CAPI.REG.read("t0");
```


### `CAPI.REG.write`

```ts
CAPI.REG.write(value: bigint, name: string): void { }
```

Stores `value` in register `name`.

E.g.:
```js
CAPI.REG.write(0x69n, "t0");
```



## Architecture
`CAPI.ARCH` exposes the interface of the currently loaded [architecture plugin](custom-architectures.md#plugins).

The supported plugins are [`riscv`](#risc-v), [`mips`](#mips) and [`z80`](#z80).

### RISC-V

#### `toJSNumberD`


```ts
CAPI.ARCH.toJSNumberD(bigIntValue: bigint): [number, string] { }
```

<!--
Used when the D extension is enabled
These are the possible cases:
1. The value is a float64 NaN
    1.1 The value is a NaN-boxed float32 (upper 32
        bits are all 1's, lower 32 bits contain
        float32 representation).
    1.2 The value is the canonical NaN (0x7ff8000000000000n)
2. The value is a valid float64 representation
-->

E.g.:
```js
let value, type;
[value, type] = CAPI.ARCH.toJSNumberD(registers.t0);
```


<!-- TODO: finish -->


### MIPS



### Z80



## Interrupts
Functions to manage interrupts and privilege modes.

For more information, see [Interrupt Support](custom-architectures.md#interrupt-support).


### `CAPI.INTERRUPTS.setUserMode`

```ts
CAPI.INTERRUPTS.setUserMode(): void { }
```

Sets the privilege level to User.

E.g.:
```js
CAPI.INTERRUPTS.setUserMode();
```


### `CAPI.INTERRUPTS.setKernelMode`

```ts
CAPI.INTERRUPTS.setKernelMode(): void { }
```

Sets the privilege level to Kernel.

E.g.:
```js
CAPI.INTERRUPTS.setKernelMode();
```


### `CAPI.INTERRUPTS.create`

```ts
CAPI.INTERRUPTS.create(type: InterruptType): void { }
```

Creates an interrupt of the specified `type`.

E.g.:
```js
CAPI.INTERRUPTS.create(InterruptType.Software);
```


### `CAPI.INTERRUPTS.enable`

```ts
CAPI.INTERRUPTS.enable(type: InterruptType): void { }
```

Enables an interrupt `type`.

E.g.:
```js
CAPI.INTERRUPTS.enable(InterruptType.Software);
```


### `CAPI.INTERRUPTS.globalEnable`

```ts
CAPI.INTERRUPTS.globalEnable(): void { }
```

Globally enables interrupts.

E.g.:
```js
CAPI.INTERRUPTS.globalEnable();
```


### `CAPI.INTERRUPTS.disable`

```ts
CAPI.INTERRUPTS.disable(type: InterruptType): void { }
```

Disables an interrupt `type`.

E.g.:
```js
CAPI.INTERRUPTS.disable(InterruptType.Software);
```


### `CAPI.INTERRUPTS.globalDisable`

```ts
CAPI.INTERRUPTS.globalDisable(): void { }
```

Globally disables interrupts.

E.g.:
```js
CAPI.INTERRUPTS.globalDisable();
```


### `CAPI.INTERRUPTS.isEnabled`

```ts
CAPI.INTERRUPTS.isEnabled(type: InterruptType): boolean { }
```

Checks if an interrupt `type` is enabled.

E.g.:
```js
CAPI.INTERRUPTS.isEnabled(InterruptType.Software);
```


### `CAPI.INTERRUPTS.isGlobalEnabled`

```ts
CAPI.INTERRUPTS.isGlobalEnabled(): boolean { }
```

Checks if interrupts are globally enabled.

E.g.:
```js
CAPI.INTERRUPTS.isGlobalEnabled();
```


### `CAPI.INTERRUPTS.clear`

```ts
CAPI.INTERRUPTS.clear(type: InterruptType): void { }
```

Clears interrupts of the specified `type`.

E.g.:
```js
CAPI.INTERRUPTS.clear(InterruptType.Software);
```


### `CAPI.INTERRUPTS.globalClear`

```ts
CAPI.INTERRUPTS.globalClear(): void { }
```

Clears all interrupts.

E.g.:
```js
CAPI.INTERRUPTS.globalClear();
```


### `CAPI.INTERRUPTS.setCustomHandler`

```ts
CAPI.INTERRUPTS.setCustomHandler(): void { }
```

Sets the interrupt handler to the custom handler.

E.g.:
```js
CAPI.INTERRUPTS.setCustomHandler();
```


### `CAPI.INTERRUPTS.setCREATORHandler`

```ts
CAPI.INTERRUPTS.setCREATORHandler(): void { }
```

Sets the interrupt handler to the default CREATOR handler.

E.g.:
```js
CAPI.INTERRUPTS.setCREATORHandler();
```


### `CAPI.INTERRUPTS.setHighlight`

```ts
CAPI.INTERRUPTS.setHighlight(): void { }
```

Highlights the current instruction as "interrupted" in the UI.

E.g.:
```js
CAPI.INTERRUPTS.setHighlight();
```


### `CAPI.INTERRUPTS.clearHighlight`

```ts
CAPI.INTERRUPTS.setHighlight(): void { }
```

Removes the "interrupted" highlight in the UI. Typically used in instructions that return from the interrupt handler, such as RISC-V's `mret`.

E.g.:
```js
CAPI.INTERRUPTS.clearHighlight();
```




