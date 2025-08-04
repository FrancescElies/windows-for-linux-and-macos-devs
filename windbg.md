# Windbg

Install it `winget install "windows driver kit" WinDbg`

Programmatically analyze `my.dmp` file

    `C:\Program Files (x86)\Windows Kits\10\Debuggers\x64\cdb.exe` -z my.dmp -c "!analyze -v; ~*; ~*k; !pe; q"

## Using it

Reading Output from `!analyze -v`, always read from the top, few lines include
the bugcheck code and probable cause, go to `STACK TEXT`, `MODULE_NAME`, and `IMAGE_NAME`, they often point to
the offending driver or component.

Use `k` to walk the stack and `!thread`, `!process`, and `!irp` commands to dig deeper based on the output.

`dt` ("Display Type") shows the structure layout of a data type, and its values
when given an address, e.g. `dt _EPROCESS <Address>`, `dt _ETHREAD <Address>`,
`dt nt!_DRIVER_OBJECT <Address>`,

> [!NOTE]
> Examine internal fields (e.g., process name, PID, threads list):
> `dt nt!_EPROCESS ImageFileName`

> [!TIP]
> Use dt -r to recurse into nested structures.


### Basic navigation

| Command               | Description                                                |
| --------------------- | ---------------------------------------------------------- |
| `!analyze -v`         | Performs a verbose crash analysis (essential for BSODs).   |
| `lm`                  | Lists loaded modules (use `lmv` or `lmf` for more detail). |
| `!process 0 0`        | Lists all active processes.                                |
| `!thread`             | Shows the current thread and its context.                  |
| `k`, `kp`, `kP`, `kv` | Stack trace with different levels of detail.               |
| `~*k`                 | Get Call Stack for all Thread                              |
| `~<thread_number>k`   | Get Call Stack for a Specific Thread                       |
| `~`                   | Lists all threads in the current process.                  |
| `~*`                  | List All Threads                                           |
| `~[n]s`               | Switch to thread n.                                        |
| `.reload`             | Reload symbols. Use `.reload /f` to force.                 |
| `.exr -1`             | Dump the last exception record.                            |
| `.cxr -1`             | Dump the last context record (after `.exr`).               |
| `.frame n`            | Switch to stack frame `n`.                                 |
| `!handle`             | Display handle information.                                |

### Memory and variables
| Command          | Description                                             |
| ---------------- | ------------------------------------------------------- |
| `dd`, `dq`, `dc` | Dump memory as double-words, quad-words, or characters. |
| `da`, `du`, `db` | Dump memory as ASCII, Unicode, or bytes.                |
| `dps`, `dqs`     | Dump and symbol decode memory pointers.                 |
| `dt`             | Display structure of a type (e.g. `dt _EPROCESS`).      |
| `!address`       | Show virtual address space layout.                      |
| `!vadump`        | Display the VAD tree (Virtual Address Descriptors).     |

### Modules and symbols
| Command             | Description                                         |
| ------------------- | --------------------------------------------------- |
| `x module!*symbol*` | Search for symbols.                                 |
| `ln <address>`      | List nearest symbol to an address.                  |
| `u <address>`       | Disassemble code at address.                        |
| `!sym noisy`        | Enable symbol load diagnostics.                     |
| `.symfix`           | Set the symbol path to the Microsoft symbol server. |
| `.sympath`          | Show or set symbol path.                            |
|`.sympath srv*C:\Symbols*https://msdl.microsoft.com/download/symbols`| Where to search for pdb files                |

### Threads and process info

| Command        | Description                                            |
| -------------- | ------------------------------------------------------ |
| `!process`     | Displays process info (use with PID or `0 0` for all). |
| `!thread`      | Shows details of a thread (e.g. `!thread <address>`).  |
| `!teb`, `!peb` | Shows TEB or PEB for the current thread or process.    |
### Crash analysis
| Command             | Description                                |
| ------------------- | ------------------------------------------ |
| `!analyze -v`       | The most used crash dump analysis command. |
| `!blueScreen`       | Analyze a blue screen error.               |
| `!irp`, `!devstack` | Inspect IRPs and device stack.             |
| `!drivers`          | List drivers and their base addresses.     |
| `!bugcheck`         | Show bugcheck code and parameters.         |

### Advanced misc
| Command                 | Description                                              |
| ----------------------- | -------------------------------------------------------- |
| `.exr`, `.cxr`          | View exception and context records.                      |
| `bp`, `bu`, `ba`        | Set breakpoints (by address, unresolved symbol, access). |
| `g`, `gh`, `gn`, `gc`   | Go (continue execution), with variations.                |
| `.logopen`, `.logclose` | Start and stop logging output to a file.                 |
| `.time`                 | Show system and debugger time.                           |
