Usually, applications which utilize the **Sleep()** function, for instance LG Hub scripting is using the system built-in **Sleep()** function, which in fact might not be accurate at all. Whereas **Sleep(1)** is expected to be **1ms**, but is actually **15ms** when monitored by for instance **MeasureSleep**.

This can cause some very inaccurate delays when other applications are using this sleep function. So to fix it, we open up Regedit:

- Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\kernel
- create a new REG_DWORD with the name **GlobalTimerResolutionRequests** and set its value to **0x00000001(1)**

This should decrease the built-in Sleep() function to just a bit over 1~2ms instead. Which is a significant improvement in accuracy.

There are some other ways which can improve this accuracy even further, by manually finding a resolution value and set it with **SetTimerResolution**. The executable should not be run directly, instead need to pass integer arguments. Check the documentation on the **TimerResolution** repo for more information.

If the resolution is not affected even when done correctly in the steps. Make sure to check for processes that are enforcing any resolution value below 5000. Which usually are caused by RGB software drivers. Use `powercfg -energy` to check which process and kill the process(es) to see if it's solved.

Now, fine-tune the argument to somewhat above 5000 and see which gives the most stable low sleep interval. For testing purposes, simply use task manager to kill **SetTimerResolution.exe** and launch the shortcut again. Repeat until satisfied.

**Just delete the registry entry that was created if it didn't work for you.**
