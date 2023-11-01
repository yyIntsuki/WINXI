Usually, applications which utilize the **Sleep()** function, for instance LG Hub scripting is using the system built-in **Sleep()** function, which in fact might not be accurate at all. Whereas **Sleep(1)** is expected to be **1ms**, but is actually **15ms** when monitored by for instance **MeasureSleep**.

This can cause some very inaccurate delays when other applications are using this sleep function. So to fix it, we open up Regedit:

- Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\kernel
- create a new REG_DWORD with the name **GlobalTimerResolutionRequests** and set its value to **0x00000001(1)**

This should decrease the built-in Sleep() function to just a bit over 1~2ms instead. Which is a significant improvement in accuracy. There are some other ways which can improve this accuracy even further, but I will not cover it here as it didn't seem to work for me, as well as that level of accuracy is not really worth the hassle.

Check MeasureSleep again after rebooting your PC to see if it's in effect.
**Just delete the registry entry that was created if it didn't work for you.**
