# Standalone deployment

This topic describes how to deploy ossimport in a standalone method. You can deploy ossimport in the standalone method in Linux or Windows systems.

JDK 1.7 and 1.8 have been installed.

## Quick start

1.  Download [ossimport-2.3.5.zip](https://gosspublic.alicdn.com/ossimport/international/standalone/ossimport-2.3.5.zip) and decompress the file.

    The structure of the decompressed file is as follows:

    ```
    ossimport
    ├── bin
    │ └── ossimport2.jar  # The JAR package that contains the Master, Worker, TaskTracker, and Console modules.
    ├── conf
    │ ├── local_job.cfg   # The Job configuration file.
    │ └── sys.properties  # The configuration file that contains system parameters.
    ├── console.bat       # The command-line tool in Windows used to run tasks step by step.
    ├── console.sh        # The command-line tool in Linux used to run tasks step by step.
    ├── import.bat        # The script that automatically imports files based on the conf/local_job.cfg configuration file in Windows. The configuration file contains parameters that specify operations involved in data migration, such as start, migration, verification, and retry.
    ├── import.sh         # The script that automatically imports files based on the conf/local_job.cfg configuration file in Linux. The configuration file contains parameters that specify operations involved in data migration, such as start, migration, verification, and retry.
    ├── logs              # The directory that contains logs.
    └── README.md         # The file that introduces or explains ossimport. We recommend that you read this file before you use ossimport.
    ```

2.  Edit the following configuration files as needed: conf/sys.properties and conf/local\_job.cfg.

    Do not modify the following items:

    -   workingDir, workerUserName, workerPassword, and privateKeyFile in conf/sys.properties.
    -   The name and path of `conf/local_job.cfg` and `jobName` in the configuration file.
    For more information about how to edit configuration files, see [Configuration file examples](/intl.en-US/Tools/ossimport/Architectures and configurations.md).

    **Note:** Confirm the parameters in sys.properties and local\_job.cfg before submitting a job. The parameters for a job cannot be changed after the job is submitted.

3.  Start migration jobs.

    -   Double-click the import.bat file in Windows.
    -   Run bash import.sh in Linux.

## Running method

In ossimport deployed in the standalone method, data migration jobs are implemented in the following methods:

-   One-click import: This method encapsulates all steps into a script. You can implement a data migration job by following the prompts of the script. If you are a beginner, we recommend that you use this method to implement data migration jobs, as described in the [Quick start](#step_lqm_2qw_ymi) section.
    1.  Start one-click import.

        -   Double-click the import.bat file in Windows.
        -   Run bash import.sh in Linux.
        **Note:** If the last job that you have run is not complete, ossimport prompts you whether to continue the job from the recorded checkpoint or restart a synchronization job. If you want to start a new data migration job or have modified the source and destination buckets, restart a synchronization job.

    2.  In Windows, a new Command Prompt is launched to implement the job and display the logs. In the Command Prompt window that runs ossimport, the status of the job is displayed every 10 seconds. Do not close either of the Command Prompt window during the job. In Linux, the job is implemented in the background.
    3.  If a task failed during the job, you are prompted whether to retry. Input y to retry, or input n to skip and exit.
    4.  If a data migration job fails, you can view master/jobs/local\_test/failed\_tasks/<tasktaskid\>/audit.log to identify the cause of the failure.
-   Step-by-step import: This method performs the following steps to implement a data migration job.
    1.  Clear jobs with the same name.

        If you have run a job with the same name and want to run the job again, clear the job with the same name first. If you have not run the job or you want to retry the tasks of a failed job, do not run the clear command.

        -   In Windows, run console.bat clean in the Command Prompt.
        -   In Linux, run the bash console.sh clean command.
    2.  Submit the data migration job.

        You cannot submit jobs with the same name. In this case, clear the job with the same name first. The configuration file of the job to submit is conf/local\_job.cfg. The default name of the job to submit is `local_test`. Run the following commands to submit the job.

        -   In Windows, run console.bat submit in the Command Prompt.
        -   In Linux, run bash console.sh submit.
    3.  Start the service.
        -   In Windows, run console.bat start in the Command Prompt.
        -   In Linux, run bash console.sh start.
    4.  View job status.
        -   In Windows, run console.bat stat in the Command Prompt.
        -   In Linux, run bash console.sh stat.
    5.  Retry failed tasks.

        Tasks may fail because of network issues or other reasons. When you run the retry command, only failed tasks are retried.

        -   In Windows, run console.bat retry in the Command Prompt.
        -   In Linux, run bash console.sh retry.
    6.  Stop the service.
        -   In Windows, close the %JAVA\_HOME%/bin/java.exe window.
        -   In Linux, run bash console.sh stop.

## Job status and logs

After a job is submitted, the master splits the job into tasks, the workers run the tasks and the tracker collects the task status. After a job is completed, the structure of the ossimport directory is as follows:

```
ossimport
├── bin
│   └── ossimport2.jar    # The JAR package of the ossimport deployed in the standalone method.
├── conf
│   ├── local_job.cfg     # The Job configuration file for the standalone method.
│   └── sys.properties    # The configuration file that contains system parameters.
├── console.sh            # The command-line tool.
├── import.sh             # The script for one-click import.
├── logs
│   ├── import.log        # Migration logs.
│   ├── job_stat.log      # Job status logs.
│   ├── ossimport2.log    # Running logs of ossimport deployed in the standalone method.
│   └── submit.log        # Logs of submitted jobs.
├── master
│   ├── jobqueue                 # Jobs that are not split.
│   └── jobs                     # Job status.
│       └── local_test           # Job name.
│           ├── checkpoints      # Checkpoints generated when Master splits jobs into tasks.
│           │   └── 0
│           │       └── 034DC9DD2860B0CFE884242BC6FF92E7.cpt
│           ├── dispatched       # Tasks dispatched to workers but are not complete.
│           │   └── localhost
│           ├── failed_tasks     # Failed tasks.
│           ├── pending_tasks    # Tasks that are not dispatched.
│           └── succeed_tasks    # Tasks that run successfully.
│               └── A41506C07BF1DF2A3EDB4CE31756B93F_1499744514501@localhost
│                   ├── audit.log    # The running logs of tasks. You can view the logs to identify error causes.
│                   ├── DONE         # The mark file of successful tasks.
│                   ├── error.list   # Error list of tasks. You can view the errors in the file.
│                   ├── STATUS       # The mark file that indicates task status. The content of this file is Failed or Completed, indicating that the subtask failed or succeeded.
│                   └── TASK         # Description of the tasks.
└── worker      # Status of the task being run by the worker. After running, tasks are managed by the master.
    └── jobs
        └── local_test
            └── tasks
```

**Note:**

-   To view the running information about jobs, view logs/ossimport2.log or logs/import.log.
-   To know the causes of failed tasks, view master/jobs/$\{JobName\}/failed\_tasks/$\{TaskName\}/audit.log.
-   To view errors occurred during the task, view master/jobs/$\{JobName\}/failed\_tasks/$\{TaskName\}/error.list.
-   The preceding log files are for reference only. Do not deploy your services and application based on them.

## Common problems about migration failures

-   If the files in the source path are modified during upload, an error that contains `SIZE_NOT_MATCH` are recorded in log/audit.log. In this case, only the files before modification are uploaded to OSS.
-   If the source file is deleted during upload, the migration job may fail.
-   If the name of the file to upload does not conform to the naming conventions of OSS \(for example, start with a forward slash or be empty\), the upload fails.
-   If you failed to download the source object from OSS because of network conditions or insufficient permissions, view logs/ossimport2.log or logs/import.log to identify the cause of the failure.
-   The program exits unexpectedly and the job status is Abort. If this happens, contact [after-sales technical support](https://selfservice.console.aliyun.com/ticket/createIndex).

