# Job Management  <!-- {docsify-ignore} -->
Presented here are helpful tools and methods to manage  Slurm jobs, find detailed information of a job like memory usage, CPUs, and how to use job statistics/information to troubleshoot any job failure.


## Checking jobs after submission  <!-- {docsify-ignore} -->

`squeue` is a useful command that can help check the state and workload of the overall cluster as well as more specific information. Below is a table of options that can be added to view certain information. By default `squeue` will show all currently submitted and running jobs on Pinnacles.

|Command_Option | Use | 
| -------------| -----------------------|
| `-M merced ` | Shows all currently submitted jobs on MERCED |
|  `--me ` | Shows all currently jobs submitted by user |
| `--r` or `-array ` | Shows job arrays sumitted onto cluster |


!> Flags can be used together in the same line for example: `squeue -M merced --me`

## Job State  <!-- {docsify-ignore} -->
Job states are the current state of the jobs that were submitted. Some important state codes that are useful are given below: 

| State Codes | Meaning | 
| -------- | --------------------| 
| PD | Job is Pending |
| R | Running | 
| CF | Job is resources allocated and now booting up| 
 CD | Job has been completed |
| CA | Job has been cancelled explicitly | 
| CG | Job is in the process of completting and is deallocating the resources | 
| F | Job exited with Failure, a non-zero exit code is presented | 
| OOM | Nodes are out of memory | 
| S | Job has been suspended |
| TO | Job terminated upon reaching its time limit | 


## Nodelist Reasons  <!-- {docsify-ignore} -->

NodeList(Reason) helps to find on which nodes the job is currently running on. Also, in the case of PD Job state, this field will give more information about the reason why the job is in pending state. Below is a table that shows common Nodelist(reasons) and their meanings. 

| (Reason) | Meaning | 
| ---------- | ---------------| 
| (Resources) |  Job is waiting for resources to become available |
| (TimeLimit) | Job has hit it's max time limit for execution |
| (ReqNodeNotAvail) | The requested node is not curretly available | 
| (Nodes required for job are DOWN, DRAINED or reserved for jobs in higher priority partitions) | Job is not available |
|(Priority) | One or more higher priority jobs exist for this partition or advanced reservation. | 
| (QoSJobLimit) | The job's QOS has reached its maximum job count |
| QOSResourceLimit | The job's QOS has reached some resource limit | 

##  `sacct` command  <!-- {docsify-ignore} -->
The `sacct` displays accounting data for all jobs in the cluster queue or recent history. By default, the `sacct` command diplays JobId, JobName, Partition, Account, AllocCPUS, State and ExitCode. Below are useful options that can be added to get more specific information but all options for `sacct ` can be found through executing `sacct -e` or `sacct -h`.

| Option | Meaning | 
| ---------------- | ---------------| 
| --user=<uid_or_user_list> | Displays the list of jobs currently submitted and running on the cluster of the specified user. |
| -j, --jobs=<JobID> | Displays information about the job ID inputted |
| -C, --constraints=<constraint_list> | Comma separated list to filter jobs based on what constraints/features the job requested. Multiple options will be treated as 'and' not 'or', so the job would need all constraints specified to be returned not one or the other. | 
| -h, --help | Displays all options and descriptions for `sacct` |
| -X, --allocations | Only show statistics relevant to the job allocation itself |
| -v, --verbose|  Primarily for debugging purposes, report the state of various variables during processing. |
| --name=<jobname_list> | Display jobs that have any of these name(s).|
| --state=<state_list> | Displays states depending on which state was asked to be displayed and thier associated exit code. |


Example of `sacct -e`

    Account             AdminComment        AllocCPUS           AllocNodes
    AllocTRES           AssocID             AveCPU              AveCPUFreq
    AveDiskRead         AveDiskWrite        AvePages            AveRSS
    AveVMSize           BlockID             Cluster             Comment
    Constraints         ConsumedEnergy      ConsumedEnergyRaw   Container
    CPUTime             CPUTimeRAW          DBIndex             DerivedExitCode
    Elapsed             ElapsedRaw          Eligible            End
    ExitCode            Flags               GID                 Group
    JobID               JobIDRaw            JobName             Layout
    MaxDiskRead         MaxDiskReadNode     MaxDiskReadTask     MaxDiskWrite
    MaxDiskWriteNode    MaxDiskWriteTask    MaxPages            MaxPagesNode
    MaxPagesTask        MaxRSS              MaxRSSNode          MaxRSSTask
    MaxVMSize           MaxVMSizeNode       MaxVMSizeTask       McsLabel
    MinCPU              MinCPUNode          MinCPUTask          NCPUS
    NNodes              NodeList            NTasks              Partition
    Priority            QOS                 QOSRAW              Reason
    ReqCPUFreq          ReqCPUFreqGov       ReqCPUFreqMax       ReqCPUFreqMin
    ReqCPUS             ReqMem              ReqNodes            ReqTRES
    Reservation         ReservationId       Reserved            ResvCPU
    ResvCPURAW          Start               State               Submit
    SubmitLine          Suspended           SystemComment       SystemCPU
    Timelimit           TimelimitRaw        TotalCPU            TRESUsageInAve
    TRESUsageInMax      TRESUsageInMaxNode  TRESUsageInMaxTask  TRESUsageInMin
    TRESUsageInMinNode  TRESUsageInMinTask  TRESUsageInTot      TRESUsageOutAve
    TRESUsageOutMax     TRESUsageOutMaxNode TRESUsageOutMaxTask TRESUsageOutMin
    TRESUsageOutMinNode TRESUsageOutMinTask TRESUsageOutTot     UID
    User                UserCPU             WCKey               WCKeyID
    WorkDir

Below are defintions of some important fields from the above list that are helpful when troubleshooting or debugging. 

|Field | Use | 
| -------------| -----------------------|
| JobId | Shows the ID of the job |
| JobName |Name of the Job. |
| AllocCPUS | Count of allocated CPUs. Equal to NCPUS.|
| ReqCPUS | Required number of CPUS.| 
| ReqMem | Minimum memory required for the job in MB. A c in the end denotes Memory Per CPU and a n at the end represents Memory Per Node.|
| AveRSS | Average memory use of all tasks in the job.|
| MaxRSS | Maximum memory use of any task in the job. |
| Start | Initiation time of the job in the same format as End |
| End | Termination time of the job.|
| Elapsed | Time taken by the job. |
| State | State of the job.|
| ExitCode | Exit code returned by the job. |


Here is one use of `sacct` with the follwing syntax to retrieve useful information about the specified job:
`sacct -j <jobid>`

This will provide similar information with the same fields as shown below about the specified job: 

    JobID           JobName  Partition    Account  AllocCPUS      State ExitCode
    ------------ ---------- ---------- ---------- ---------- ---------- --------
    569893         javatest       test project_u+          1 OUT_OF_ME+    0:125
    569893.batch      batch            project_u+          1 OUT_OF_ME+    0:125
    569893.exte+     extern            project_u+          1  COMPLETED      0:0

By default, `sacct -j <jobid>` will display basic information with some fields, as seen above, jobid, jobname, partition, account, allocCPUs, state, exit code. The information given by default is useful when it comes to debugging as it allows us to see why or how the submitted job did not exit successfully. For instance in the above example we see a job called "javatest" was submitted to the `test` partition and was allocated 1 cpu but finished in a state of `OUT_OF_ME+`. The `+` is only there as the error message is to long for the complete message to be displayed. The job had an exit code of `0:125`, which means that the job ran out of memory. When debugging why this particular job failed it can be inferred that the job failed because it ran out of memory, which is supported through the finished state of the job(OOM) and the exit code that represents an out of memory error killed the job or made the job exit the partition. To view the complete sample job submission that produced this sample output refer to "Out of Memory Issues" section below. 

For debugging that requires more in-depth analysis and information adding the option `--Format=<Field>` will show additional information that can be more useful for debugging bigger issues. Below is an example use and output with the use of `--Format=<Field`.

Using the syntax: `sacct -j <jobid> --format=jobid,jobname,reqcpus,reqmem,averss,maxrss,elapsed,state,exitcode`

    JobID           JobName  ReqCPUS     ReqMem     AveRSS     MaxRSS    Elapsed      State ExitCode
    ------------ ---------- -------- ---------- ---------- ---------- ---------- ---------- --------
    569893         javatest        1         1M                         00:00:01 OUT_OF_ME+    0:125
    569893.batch      batch        1                 1304K      1304K   00:00:01 OUT_OF_ME+    0:125
    569893.exte+     extern        1                  880K       880K   00:00:01  COMPLETED      0:0

>Refer to the above table of fields to read more about each field and it's use.

Using the `sacct` with addional fields there is far more analysis that can be done to see why, when, how a job failed or began to run unsuccesfully and killed. 
>Using the table of fields and uses, there are many ways the option `--Flag=` can be used in the debugging process. 



## `scontrol` Command 
`scontrol` is a helpful command that allows to  view or configure the submitted job and it's state. scontrol is used to view or modify Slurm configuration including: job, job step, node, partition, reservation, and overall system configuration. If no command is entered on the execute line, scontrol will operate in an interactive mode and prompt for input. It will continue prompting for input and executing commands until explicitly terminated.

Use `scontrol` with the follwing syntax to retrieve useful information about the specified job:

`scontrol show job <jobid>`

Below is an example of using scontrol to get insight about an example job. An example job is running and has not yet terminated. It is shown as `JobState=RUNNING Reason=NONE` & `ExitCod=0:0`. 


    UserId=guest001 GroupId=****** MCS_label=N/A
    Priority=4294341021 Nice=0 Account=project_**** QOS=normal
    JobState=RUNNING Reason=None Dependency=(null)
    Requeue=0 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
    RunTime=00:02:33 TimeLimit=00:15:00 TimeMin=N/A
    SubmitTime=2023-07-19T12:50:52 EligibleTime=2023-07-19T12:50:52
    AccrueTime=2023-07-19T12:50:52
    StartTime=2023-07-19T12:50:53 EndTime=2023-07-19T13:05:53 Deadline=N/A
    SuspendTime=None SecsPreSuspend=0 LastSchedEval=2023-07-19T12:50:53 Scheduler=Main
    Partition=test AllocNode:Sid=10.1.2.252:279163
    ReqNodeList=(null) ExcNodeList=(null)
    NodeList=hmnode003
    BatchHost=hmnode003
    NumNodes=1 NumCPUs=1 NumTasks=1 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
    TRES=cpu=1,mem=1M,node=1,billing=1
    Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
    MinCPUsNode=1 MinMemoryNode=1M MinTmpDiskNode=0
    Features=(null) DelayBoot=00:00:00
    OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
    Command=/home/******/testoom/job.bat
    WorkDir=/home/******/testoom
    StdErr=/home/******/testoom/Appout.qlog
    StdIn=/dev/null
    StdOut=/home/******/testoom/Appout.qlog
    Power=

Note: Astericks are only in the above sample output to protect user information. 

## Common Issues  <!-- {docsify-ignore} -->
Below are common issues, that can arrise when running jobs on the clusters, and associated troubleshooting methods. 

### Out of Memory Issues <!-- {docsify-ignore} -->
Jobs can fail if the memory requested for the job exceeds the actual memory needed for the job to complete successfully.
It is good practice to always check the job state and exit code with `sacct -j <JobID>`. It can be concluded that a job has had a **OUT_OF_MEMORY** error from reading the job state column and exit code. Furthermore, the output file produced by the failed job should also contain error messages that can be associated with the job running out of memory. 

Below is a job script that will result in an out of memory error. The script is running java but has only 1M allocated in memory toward the job. Thus results in a fail in this use case as it is not enought memory to create the class file, compile  and run the program.

    #!/bin/bash
    #SBATCH --nodes=1
    #SBATCH --ntasks=1
    #SBATCH --partition test    
    #SBATCH --mem=1M # This is where the issue arrises
    #SBATCH --time=0-00:15:00 # 15 minute
    #SBATCH --output=oomout.qlog    
    #SBATCH --job-name=javatest
    #SBATCH --export=ALL

    module load openjdk/17.0.5_8    
    javac oom.java
    java oom


Sample Java program that was used in the job sample script above is shown below: 

    public class oom {
        public static void main(String[] args) throws Exception {
            System.out.println("If you are seeing this it then memory size was suffient");
        }
    }
 
 Check the status of the job using `sacct -j <jobid>` the follwing is produced: 

    JobID           JobName  Partition    Account  AllocCPUS      State ExitCode
    ------------ ---------- ---------- ---------- ---------- ---------- --------
    569908         javatest       test project_u+          1 OUT_OF_ME+    0:125
    569908.batch      batch            project_u+          1 OUT_OF_ME+    0:125
    569908.exte+     extern            project_u+          1  COMPLETED      0:0

Using the `sacct` command we see that the job failed because it ran out of memory. This is inferred through the state: `OUT_OF_ME+` and the exit code of `0:125` which correlates with an Out of Memory exit status or the reason why the job session was terminated. 

  It is possible to use `scontrol show job <sampleid>` to debug the error(s) that occured in our job.  

    JobId=569908 JobName=javatest
    UserId=******** GroupId=******** MCS_label=N/A
    Priority=4294341021 Nice=0 Account=project_****** QOS=normal
    JobState=OUT_OF_MEMORY Reason=OutOfMemory Dependency=(null)
    Requeue=0 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:125
    RunTime=00:10:27 TimeLimit=00:15:00 TimeMin=N/A
    SubmitTime=2023-07-19T12:50:52 EligibleTime=2023-07-19T12:50:52
    AccrueTime=2023-07-19T12:50:52
    StartTime=2023-07-19T12:50:53 EndTime=2023-07-19T13:01:20 Deadline=N/A
    SuspendTime=None SecsPreSuspend=0 LastSchedEval=2023-07-19T12:50:53 Scheduler=Main
    Partition=test AllocNode:Sid=10.1.2.252:279163
    ReqNodeList=(null) ExcNodeList=(null)
    NodeList=hmnode003
    BatchHost=hmnode003
    NumNodes=1 NumCPUs=1 NumTasks=1 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
    TRES=cpu=1,mem=1M,node=1,billing=1
    Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
    MinCPUsNode=1 MinMemoryNode=1M MinTmpDiskNode=0
    Features=(null) DelayBoot=00:00:00
    OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
    Command=/home/******/testoom/job.bat
    WorkDir=/home/******/testoom
    StdErr=/home/******/testoom/Appout.qlog
    StdIn=/dev/null
    StdOut=/home/******/testoom/Appout.qlog
    Power=


Looking through the output of `scontrol` we can see the job state, the state the job was last recorded at before the session was terminated or ended, was `OUT_OF_MEMORY`, the node reason was listed at `OUTofMemory` and the exit code was recorded at, before the job session was terminated, `0:125`. All of these fields are useful and allow for the debugging process to conclude that the job did not succesfully run because of a memory capacity issue. 
### Time-Out Issues <!-- {docsify-ignore} -->
One common issue for jobs failing is if job does not complete in the allocated time. This leads to a **Time-Out** State and a `(TimeLimit)` nodelist reason. The best approach is to increase the time being allocated for the job to run, ensuring that the job does not exceed the partition's max walltime. If the job continues to fail with a **Time-Out** state then it is best to break the job down into smaller jobs,  make it into a job array or change the partition that the job is being placed onto to run and compute. 

Below is a script that will result in a time-out error.

    #!/bin/bash
    #SBATCH --nodes=1
    #SBATCH --ntasks=1
    #SBATCH -p test
    #SBATCH --mem=1G  #Here is the reason why the job fails
    #SBATCH --time=0-00:01:00     # 1 mins
    #SBATCH --output=regular.stdout
    #SBATCH --job-name=test
    #SBATCH --export=ALL


    echo "Starting Process"
    sleep 180
    echo "Ending Process"


This simple job script is printing out "Starting Process" and then sleeps or waits 180 seconds before again executing the following line of printing out "Ending Process".  After submitting this script using `sbatch` it gets placed on the requested partition and then it begins to run until it hits its max wall time of 1 minute. This job will because the mininimum time needed was 180 seconds as that is the time the job sleeps after running and is the time required at minimum for the job to fully execute. 

Using `sacct -j <jobID> --format=jobid,jobname,reqcpus,reqmem,elapsed,state,exitcode`. We get a result table that shows that the first part timed out and thus resulted in a failed, timedout and cancelled state. 

    JobID           JobName  ReqCPUS     ReqMem    Elapsed      State ExitCode
    ------------ ---------- -------- ---------- ---------- ---------- --------
    569330             test        1         1G   00:01:02    TIMEOUT      0:0
    569330.batch      batch        1              00:01:03  CANCELLED     0:15
    569330.exte+     extern        1              00:01:02  COMPLETED      0:0
    569330.0           echo        1              00:00:00  COMPLETED      0:0
    569330.1          sleep        1              00:01:02  CANCELLED     0:15

Using the sacct command we see that the job resulted in `TIMEOUT` state which allows us to debug that the issue was an walltime issue issue. This can further be seen as `569330.0` or `echo` on the fourth line shows that echo was completed it was able to execute in the begginging when it printed "Starting Process" but echo was never called again as the job timed-out so the second echo which prints out "Ending Process" was never reached as the job stoped one line before it.  

 It always important to note that sometimes a job failing not the result of one issue or error, but a combination of many errors and issues. Furthermore it is best to keep track of jobs before, during and after completion. 


## Useful proccess to follow to ensure sucessful completion of jobs <!-- {docsify-ignore} -->
1. Submit the Job
    Submit the above job and see how it runs. To submit the above job, run the following command.

        sbatch script.sh
        Submitted batch job <JOBID>

2. Watch Live Status of the Job.
    Use the `watch squeue -u <username>`. Do not include anything past the `@` in the username. Ex. `watch squeue -u guest001`. Empty Version of Live Status is below. 
        
        Every 2.0s: squeue -u guest001
        CLUSTER: pinnacles                                                          
             JOBID PARTITION     NAME     USER ST	TIME  NODES NODELIST(REASON)
> To exit the live status of the watch squeue command, press Ctrl + C

3. After the job exits from the queue, run the below sacct command to check the status of the job. 
`sacct -j <JobID> ` or `scontrol show job <jobid>`

Sacct Command SampleOutput:

        JobID    JobName    Elapsed      State ExitCode
        ------------ ---------- ---------- ---------- --------
        568963             test        3      1024M                         00:01:10              TIMEOUT      0:0
        568963.batch      batch        3                12.01M     12.01M   00:01:11            CANCELLED     0:15
        568963.exte+     extern        3                 0.90M      0.90M   00:01:10            COMPLETED      0:0
        568963.0           echo        3                 3.30M      3.30M   00:00:00            COMPLETED      0:0
        568963.1          sleep        3                 3.27M      3.27M   00:01:12            CANCELLED     0:15

## Other Useful Commands  <!-- {docsify-ignore} -->

|Command | Use | 
| -------------| -----------------------|
| scancel <jobid> or skill <jobid> | These commands will kill the specified job in it's current process and state. | 
| seff <job-id> |  This command can be used to find the job efficiency report for the job(s) after it has completed and exited from the queue. Some information in the report are: State, CPU & Memory Utilized, CPU & Memory Efficiency. If the command us ysed while the job is still in the R(Running) state, this might report incorrect information.
