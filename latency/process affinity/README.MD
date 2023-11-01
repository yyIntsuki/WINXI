Install LatMon if you haven't, and launch it. Begin monitoring for a while. Usually the case with high DPC latency comes from nvlddmkm.sys.

nvlddmkm.sys is a driver from NVIDIA displaye driver, which itself actually is faulty, and the main problem for it having high DPC latency, which can go above 1000µs and cause audio stutters.

If this is indeed the culprit as shown in LatMon, then try this method:

- download Microsoft Interrupt Affinity Tool and launch **intPolicy_x64.exe** in **administrative rights**
- in the list of all drivers, find your NVIDIA display driver (usually named "NVIDIA GeForce [**model**]")
- click on the driver, ignore the "Registry value..." error and press OK for the warning
- check on LatMon, **CPUs** tab which CPU has low **Interrupt cycletime**
- now back to affinity tool, press **Set Mask** and set it to the CPU(s) that you want the driver to run on instead. Note that you can spread this mask across multiple CPUs. In my case I picked CPU 4 and 5
- press **Done** and let the application restart the driver for you

**This fix is not permanent, and could potentially be reverted by the system on reboot or driver update.** Now, re-monitor the latency with LatMon. Usually the affinity is in effect directly. If you wish to revert the changes, just click on **Delete Mask** from second step and press **Done** again.
