---
draft: true
---

# Windows Kernel-mode Debugging



Two separate computer systems are typically used for debugging because instruction execution on the processor is commonly paused during the process. The debugger runs on the *host* system, and the code that you want to debug runs on the *target* system.

For this post we will use two virtual machines PC-A as the Debugger and PC-B as the Debugee.



<img src="/home/ishtar/github/xmpf.github.io/content/posts/windows-kernel-debugging/assets/kernel-mode-debugging-setup.png" alt="img" style="zoom:75%;" />



Download and install [Windows Debugger (WinDbg)](https://aka.ms/windbg/download) in PC-A.



On the Debugee:

```
C:\> bcdedit /debug on
C:\> bcdedit /dbgsettings hostip:x.x.x.x port:50005
C:\> bcdedit /set testsigning on
```



## References

[^1]: https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/setting-up-kernel-mode-debugging-in-windbg--cdb--or-ntsd
[^2]: https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-reference