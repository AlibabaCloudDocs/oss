# Distributed deployment

This topic describes how to deploy ossimport in a distributed method. You can deploy ossimport in the distributed method only in Linux.

## Download

Download the tool for distributed deployment: [ossimport-2.3.5.tar.gz](https://gosspublic.alicdn.com/ossimport/international/distributed/ossimport-2.3.5.tar.gz). Download the tool to a local directory and run the command `tar -zxvf ossimport-2.3.5.tar.gz -C $HOME/ossimport` to decompress the file. The file structure of the decompressed tool is as follows:

```
ossimport
├── bin
│ ├── console.jar     # The JAR package for the Console module.
│ ├── master.jar      # The JAR package for the Master module.
│ ├── tracker.jar     # The JAR package for the Tracker module.
│ └── worker.jar      # The JAR package for the Worker module.
├── conf
│ ├── job.cfg         # The Job configuration file template.
│ ├── sys.properties  # The configuration file that contains system parameters.
│ └── workers         # The list of Workers.
├── console.sh          # The command-line tool. Currently, only Linux is supported.
├── logs                # The directory that contains logs.
└── README.md           # The file that introduces or explains ossimport. We recommend that you read this file before you use ossimport.
```

Note:

-   OSS\_IMPORT\_HOME: The root directory of ossImport. By default the directory is the `$HOME/ossimport` in the decompress command. You can also run the `export OSS_IMPORT_HOME=<dir>` command or modify the system configuration file `$HOME/.bashrc` to set the directory. We recommend that you keep the default value.
-   OSS\_IMPORT\_WORK\_DIR: The working directory of ossimport. You can specify the directory by setting the value of `workingDir` in `conf/sys.properties`. We recommend that you set the value to `$HOME/ossimport/workdir`.
-   Specify absolute paths for OSS\_IMPORT\_HOME or OSS\_IMPORT\_WORK\_DIR, such as `/home/<user>/ossimport` or `/home/<user>/ossimport/workdir`.

## Parameter

The distributed deployment of ossimport is performed based on three configuration files: `conf/sys.properties`, `conf/job.cfg`, and `conf/workers`.

-   `conf/job.cfg`: The configuration file template used to configure jobs in distributed mode. Modify the values according to the actual parameters before data migration.
-   `conf/sys.properties`: The configuration file used to configure system operating parameters, such as working directory and worker.
-   `conf/workers`: : The worker list.

**Note:**

-   Confirm the parameters in `sys.properties` and `job.cfg` before submitting a job. The parameters for a job cannot be changed after the job is submitted.
-   Configure `workers` before you start the service. The configuration in this file cannot be added or deleted after the service is started.

## Running

-   Perform migration tasks

    The distributed deployment of ossimport is performed in the following steps:

    -   Deploy the service. Run the bash console.sh deploy command in Linux.

        **Note:** Ensure the configuration files such as conf/job.cfg and conf/workers are properly configured before you deploy the service.

    -   Clear jobs with the same name. If you have run a job with the same name and want to run the job again, clear the job with the same name first. If you have not run the job or you want to retry the tasks of a failed job, do not run the clear command. To clear jobs with the same name, run the `bash console.sh clean job_name` command in Linux.
    -   Submit the data migration job. You cannot submit jobs with the same name. In this case, run the `clean` command to clear the jobs. To submit a job, you must specify the configuration file of the job. You can create the configuration file of a job by modifying the `conf/job.cfg` template. Run `bash console.sh submit [job_cfg_file]` in Linux to submit a job that uses job\_cfg\_file as its configuration file. In the command, `job_cfg_file` is optional and is set to `$OSS_IMPORT_HOME/conf/job.cfg` by default. By default, the value of `$OSS_IMPORT_HOME` is the path where `console.sh` is located.
    -   Start the service. Run the `bash console.sh start` command in Linux.
    -   View job status. Run the `bash console.sh stat` command in Linux.
    -   Retry failed tasks. Tasks may fail because of network issues or other reasons. When you run the retry command, only failed tasks are retried. Run the `bash console.sh retry [job_name]` command in Linux. In the command, `job_name` is an optional parameter that specifies the jobs in which failed tasks need to retry. If this parameter is not specified, failed tasks of all jobs are retried.``
    -   Stop the service. Run the `bash console.sh stop` command in Linux.
    Note:

    -   If an error occurs because of incorrect parameters in a command, `bash console.sh` prompts you the correct command format.
    -   We recommend that you specify absolute paths for the paths in configuration files and submitted jobs.
    -   The configurations in `job.cfg`

        **Note:** cannot be modified after the file is submitted. Confirm the items in the file before you submit the file.

-   Common causes of job failures
    -   If the files in the source path are modified during upload, an error that contains `SIZE_NOT_MATCH` are recorded in `log/audit.log`. In this case, only the files before modification are uploaded to OSS.
    -   If the source file is deleted during upload, you may fail to download the file.
    -   If the name of the file to upload does not conform to the naming conventions of OSS \(for example, start with a forward slash or be empty\), the upload fails.
    -   Fail to download the source file from OSS.
    -   The program exits unexpectedly and the job state is Abort. If this happens, contact after-sales technical support.
-   Job status and logs

    After a job is submitted, the master splits the job into tasks, the workers run the tasks and the tracker collects the task status. After a job is completed, the structure of the workdir directory is as follows:

    ```
    workdir
    ├── bin
    │ ├── console.jar     # The JAR package for the Console module.
    │ ├── master.jar      # The JAR package for the Master module.
    │ ├── tracker.jar     # The JAR package for the Tracker module.
    │ └── worker.jar      # The JAR package for the Worker module.
    ├── conf
    │ ├── job.cfg         # The Job configuration file template.
    │ ├── sys.properties  # The configuration file that contains system parameters.
    │ └── workers         # The list of Workers.
    ├── logs
    │   ├── import.log      # Migration logs.
    │   ├── master.log      # Master logs.
    │   ├── tracker.log     # Tracker logs
    │ └── workers         # Worker logs.
    ├── master
    │   ├── jobqueue                 # Jobs that are not split.
    │   └── jobs                     # Job status.
    │       └── xxtooss              # Job names.
    │           ├── checkpoints      # Checkpoints generated when Master splits jobs into tasks. 
    │           │   └── 0
    │           │       └── ED09636A6EA24A292460866AFDD7A89A.cpt
    │           ├── dispatched       # Tasks dispatched to workers but are not complete.
    │           │   └── 192.168.1.6
    │           ├── failed_tasks     # Failed tasks.
    │           │   └── A41506C07BF1DF2A3EDB4CE31756B93F_1499348973217@192.168.1.6
    │           │       ├── audit.log     # The running logs of tasks. You can view the logs to identify error causes.
    │           │       ├── DONE          # The mark file of successful tasks. If the task fails, the content is empty.
    │           │       ├── error.list    # Error list of tasks. You can view the errors in the file.
    │           │       ├── STATUS        # The mark file that indicates task status. The content of this file is Failed or Completed, indicating that the task failed or succeeded.
    │           │       └── TASK          # Description of the tasks.
    │           ├── pending_tasks    # Tasks that are not dispatched.
    │           └── succeed_tasks    # Tasks that run successfully.
    │               └── A41506C07BF1DF2A3EDB4CE31756B93F_1499668462358@192.168.1.6
    │                   ├── audit.log    # The running logs of tasks. You can view the logs to identify error causes.
    │                   ├── DONE         # The mark file of successful tasks.
    │                   ├── error.list   # Error list of tasks. If the tasks are successful ,the error list is empty.
    │                   ├── STATUS       # The mark file that indicates task status. The content of this file is Failed or Completed, indicating that the subtask failed or succeeded.
    │                   └── TASK         # Description of the tasks.
    └── worker  # Status of the task being run by the worker. After running, tasks are managed by the master.
        └── jobs
            ├── local_test2
            │   └── tasks
            └── local_test_4
                └── tasks
    ```

    **Note:**

    -   To view the running information about jobs, view `logs/import.log`.
    -   To know the causes of failed tasks, view `master/jobs/${JobName}/failed_tasks/${TaskName}/audit.log`.
    -   To view errors occurred during the task, view `master/jobs/${JobName}/failed_tasks/${TaskName}/error.list`.
    -   The preceding log files are for reference only. Do not deploy your services and application based on them.

## Common errors and troubleshooting

For more information about common errors and troubleshooting, see [FAQ](/intl.en-US/Tools/ossimport/Troubleshooting.md).

