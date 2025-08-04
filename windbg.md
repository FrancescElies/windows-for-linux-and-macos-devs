# Windbg

Install it `winget install "windows driver kit" WinDbg`

## Using it

Programmatically analyze `my.dmp` file

    `C:\Program Files (x86)\Windows Kits\10\Debuggers\x64\cdb.exe` -z my.dmp -c "!analyze -v; ~*; ~*k; !pe; q"

Reading Output from `!analyze -v`, always read from the top, few lines include
the bugcheck code and probable cause, go to `STACK TEXT`, `MODULE_NAME`, and `IMAGE_NAME`, they often point to
the offending driver or component.

Use `k` to walk the stack and `!thread`, `!process`, and `!irp` commands to dig deeper based on the output.

`dt` ("Display Type") shows the structure layout of a data type, and its values
when given an address, e.g. `dt _EPROCESS <Address>`, `dt _ETHREAD <Address>`,
`dt nt!_DRIVER_OBJECT <Address>`,

lmv m mydriver   ; See symbol info for the driver
dt _DEVICE_OBJECT <address>   ; Look at device object involved
!irp <irp_address>            ; Analyze an IRP in the call stack

> [!NOTE]
> Examine internal fields (e.g., process name, PID, threads list):
> `dt nt!_EPROCESS ImageFileName`

> [!TIP]
> Use dt -r to recurse into nested structures.

## Example

### Example1
#### Setup

Set Up Symbols in WinDbg

    .sympath+ C:\Path\To\MyPlugin\Symbols
    .reload
    Optional:
    .symfix
    .reload /f

Launch or Attach to the Application
Start the app under WinDbg `windbg "C:\Path\To\hostapp.exe"` or attach to running process `File > Attach to a Process (Ctrl+P)`

Then `.load C:\Path\To\myplugin.dll`, or let the host load it, and watch with breakpoints.

#### Breakpoint Strategies
A. Set a breakpoint on DLL load

    sxe ld:myplugin

This breaks when your DLL is loaded.
B. Set a breakpoint on exported function

If your DLL has exported functions like InitPlugin, ProcessImage, etc.:

    bp myplugin!InitPlugin
    bp myplugin!ProcessImage

If you don’t know the exact exports:

x myplugin!*

#### Example Debugging Workflow

Let’s say ProcessImage() crashes with an access violation:

    Set breakpoint:

bp myplugin!ProcessImage
g

    When hit, step through the code:

t  ; step into
p  ; step over
k  ; show call stack

    If it crashes:

!analyze -v

You may see:

FAULTING_IP:
myplugin!ProcessImage+42
mov eax,dword ptr [ecx+4]

EXCEPTION_CODE: c0000005 (Access violation)

    Check registers:

r

If ecx is null or invalid, you likely dereferenced a bad pointer.

    Check the parameters passed to the function:

dv

Or inspect memory:

dd ecx

#### Analyze Your DLL's Data

    Use dt to examine structures:

dt myplugin!MyDataStruct <pointer>

    If using global/static variables:

x myplugin!*

    To trace calls back:

k

#### Common Issues Debugging DLLs in Host Apps
Problem	Diagnosis
Bad pointer dereference	!analyze -v, r, dd, k
Invalid call into DLL	Set breakpoints on entry functions
Misuse of host API	Step through calls from host to your DLL
Stack corruption	k, check frame consistency
Missing symbols	.sympath, .reload, x myplugin!*
Function not called	Use sxe ld:myplugin, or trace host's load behavior


## Summary tables
### Basic navigation

| Command               | Description                                                |
| --------------------- | ---------------------------------------------------------- |
| `!analyze -v`         | Performs a verbose crash analysis (essential for BSODs)    |
| `lm`                  | Lists loaded modules (use `lmv` or `lmf` for more detail)  |
| `!process 0 0`        | Lists all active processes                                 |
| `!thread`             | Shows the current thread and its context                   |
| `k`, `kp`, `kP`, `kv` | Stack trace with different levels of detail                |
| `~*k`                 | Get Call Stack for all Thread                              |
| `~<thread_number>k`   | Get Call Stack for a Specific Thread                       |
| `~`                   | Lists all threads in the current process                   |
| `~*`                  | List All Threads                                           |
| `~[n]s`               | Switch to thread n                                         |
| `.reload`             | Reload symbols. Use `.reload /f` to force                  |
| `.exr -1`             | Dump the last exception record                             |
| `.cxr -1`             | Dump the last context record (after `.exr`)                |
| `.frame n`            | Switch to stack frame `n`                                  |
| `!handle`             | Display handle information                                 |

### Memory and variables
| Command          | Description                                             |
| ---------------- | ------------------------------------------------------- |
| `dd`, `dq`, `dc` | Dump memory as double-words, quad-words, or characters  |
| `da`, `du`, `db` | Dump memory as ASCII, Unicode, or bytes                 |
| `dps`, `dqs`     | Dump and symbol decode memory pointers                  |
| `dt`             | Display structure of a type (e.g. `dt _EPROCESS`)       |
| `!address`       | Show virtual address space layout                       |
| `!vadump`        | Display the VAD tree (Virtual Address Descriptors)      |

### Modules and symbols
| Command                                                                | Description                                         |
| -------------------                                                    | --------------------------------------------------- |
| `x module!*symbol*`                                                    | Search for symbols                                  |
| `ln <address>`                                                         | List nearest symbol to an address                   |
| `u <address>`                                                          | Disassemble code at address                         |
| `!sym` noisy                                                           | Diagnose symbol loading issues                      |
| `.symfix`                                                              | Set the symbol path to the Microsoft symbol server  |
| ` sympath`                                                             | Show symbol path                                    |
|.sympath+  C:\Path\To\MyPlugin\Symbols                                  | Setup syymbols, don't forget `.reload`              |
|`.sympath srv*C:\Symbols*https://msdl.microsoft.com/download/symbols`   | customize symbol path                               |

### Threads and process info

| Command        | Description                                            |
| -------------- | ------------------------------------------------------ |
| `!process`     | Displays process info (use with PID or `0 0` for all)  |
| `!thread`      | Shows details of a thread (e.g. `!thread <address>`)   |
| `!teb`, `!peb` | Shows TEB or PEB for the current thread or process     |
### Crash analysis
| Command             | Description                                |
| ------------------- | ------------------------------------------ |
| `!analyze -v`       | The most used crash dump analysis command  |
| `!blueScreen`       | Analyze a blue screen error                |
| `!irp`, `!devstack` | Inspect IRPs and device stack              |
| `!drivers`          | List drivers and their base addresses      |
| `!bugcheck`         | Show bugcheck code and parameters          |

### Advanced misc
| Command                 | Description                                              |
| ----------------------- | -------------------------------------------------------- |
| `.exr`, `.cxr`          | View exception and context records                       |
| `bp`, `bu`, `ba`        | Set breakpoints (by address, unresolved symbol, access)  |
| `g`, `gh`, `gn`, `gc`   | Go (continue execution), with variations                 |
| `.logopen`, `.logclose` | Start and stop logging output to a file                  |
| `.time`                 | Show system and debugger time                            |
