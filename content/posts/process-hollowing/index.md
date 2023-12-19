---
draft: true
---

# Process Hollowing



Here's a high-level overview of the process:

1. Create a new suspended process using the host executable: `CreateProcessA`.
2. Map the evil executable file into memory: `ReadFile`.
3. Get the current context of the suspended process (RDX will point to the PEB structure): `GetThreadContext`.
4. Obtain the base address of the main module of the suspended process: `pPEB->ImageBaseAddress`. [^1]
5. Unmap the original main module using `NtUnmapViewOfSection`.
6. Read the headers of the evil executable file (DOS and NT headers).
7. Allocate memory in the suspended process for the new executable.
8. Update the image base address of the evil executable to match the base address of the suspended process.
9. Write the headers and sections of the evil executable into the memory space of the suspended process.
10. If the base address of the suspended process differs from the original base address of the evil executable, process the relocation table to adjust the addresses accordingly.
11. Update the entry point and image base address of the suspended process to the new values from the evil executable.
12. Set the new thread context.
13. Resume the main thread of the suspended process.



## References



[^1]: https://github.com/x64dbg/x64dbg/blob/development/src/dbg/ntdll/ntdll.h
