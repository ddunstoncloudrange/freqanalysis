# ReadMe

When using this notebook template, be careful with the size of the dataset and the threshold because it can cauuse the browser to hang if there are lot of results. That is due to the rendering of the output into HTML. Using the JupyterLab application - https://jupyter.org/ - may offer better performance and all libaries are contained within a bundled application.

Modifying the code can allow the results to be saved into a file format such as CSV for analyzing within a spreadsheet.

The results are on a dataset of 2000 Windows computers - Desktop and Server.

The results:
```
Count	 Log Proportion	 Value
50	  -3.499038	      sbdinst.exe
54	  -3.465614	      paexec.exe
96	  -3.215737	      Powershell.exe
100	  -3.198008	      schtasks.exe
106	  -3.172702	      mshta.exe
```

The results show an LOLBin (`mshta.exe`) being executed across 106 systems. That is unusual because it is usually invoked when reading help files for Windows applications - which is not that abnormal. `paexec.exe` is an an open source alterternative to `psexec.exe`. Both could be legit programs if used within the organization. However, it must be verified with the administrators if the program is legit, where it is run from, how it is used and if it was used on a given day and time. The reason is because it has the potential to obtain SYSTEM level access if it is executed by an administration of the system or a domain admin. Further this view aggregates all exes running across the organization.

The powershell and schtasks may not be that unusual, though it is good to see the arguments and run an analyis on the arguments and determine if there are differences among hosts. This is why it is important to run frquency analysis against hosts that are configured similarly. On a varied dataset, it creates a lot of results that have to be analyzed. Running on similar systems reduces the number of false positives and the dataset that must be analyzed.

The executables that are executed the least often are the starting points of determining why they are not executed often.  That is when you can look at the arguments to the process, its path, and the user running the process.  Further, the parent process is important to know what caused it to execute.

The second execution shows the same executables, but the details for further analysis. That clearly shows some anomalies with paexec.exe running a command on a remote host to obtain a shell and encoded Powershell commands. An organization may use encoded commands, but it must be decoded and analyzed.

The same for the scheduled tasks run .vbs script. Again, it may be legit. However, some stand out:

`C:\Windows\System32\schtasks.exe /create /tn "a63vb9ifko" /tr "C:\Windows\System32\WScript.exe C:\Users\aripark58\Dropbox\{f4e478b8-640a-4bad-ac39-1f5e71e386c4}\dc986bba0fe84f64974657b143c48be3.vbs" //B /sc minute /mo 25, C:\Users\aripark58\AppData\Local\Temp` for example. High-entropy task name, execution path, and the use of the temp directory.

CONTEXT! CONTEXT! CONTEXT!
