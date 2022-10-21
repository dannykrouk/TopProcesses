# TopProcesses

This is a tool that allows one to write a list of all processes on a system, and their resource utilization, to a timestamped file in the event that a system resource utilization is outside a target value.  The reason this tool exists is help answer the question “what is responsible?” for a resource scarcity problem.  Many tools can report that resources are scarce.  If the scarcity is episodic, it can be difficult to determine what is using the resources.  Automating this tool to execute on a regular interval (e.g. 5 minutes) can provide a trail of data that can help to identify “what”.

Using top
Open a command prompt as an administrator in the directory where top.vbs exists.   With no arguments, the script will target the computer on which it runs and it will assume you are interested in processor utilization over 75%.

For example: top.vbs

Unless your system is very busy, you are likely to see a message returned on the command line that the observed value is inside the threshold. 

More typically, the tool is called with the following command-line arguments:

Machine_name -	Local or remote
Target_type -	Allowed values: CPU_UTILIZATION, AVAILABLE_MEMORY_MB, DISK_IDLE
Threshold	- An integer value which has different meaning depending on the target_type
Drive_target -	In the case of DISK_IDLE, you may specific “C:”, “_Total”, or the letter of any other logical drive that is local to the target machine (followed by a colon).  In all other cases, you need not supply this parameter.

If you wish to gather information about a remote computer, you must have privileges to access WMI information on that computer remotely.  
Understanding Targets and Thresholds

For CPU_UTILIZATION, a threshold of 75 means that the script will report total processor utilization above 75% (e.g. 80%) as “outside” the threshold.  This is a measure of processor capacity shortage.

For AVAILABLE_MEMORY_MB, a threshold of 1000 means that the script will report available memory below 1000MB (e.g. 900MB) as outside the threshold.  This is a measure of memory shortage.

For DISK_IDLE, a threshold of 25 means that the script will report disk idle below 75% (e.g. 60%) as outside the threshold.  This is a measure of disk busy-ness.  If you do not provide a drive_target, the script will assume you mean “C:”.

Output
Comma separated values are written to the output file in the current directory.  The file will have the name <machine_name>_processes_<datetime>.csv.  The data in the file includes 

•	DateTime of the observation
•	The system resource that was being monitored
•	The system resource utilization observed
•	Process name
•	Percentage of processor used by the process
•	PrivateWorkingSet bytes used by the process
•	IO data bytes per second for the process

Private workingset is defined here: http://windows.microsoft.com/en-us/windows/what-task-manager-memory-columns-mean#1TC=windows-7 

Automating
You may use Task Manager to run this script on a schedule.  In that case, you may wish to create a .bat file with the arguments you wish use.  Also, it is recommended that you set the working directory of the Task to the script directory and that you run the Task under credentials that have privileges to execute the WMI queries and write to the working directory.

Feedback
Feedback is welcome:  Danny Krouk, dkrouk@esri.com

