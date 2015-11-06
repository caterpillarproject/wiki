---
title: Submitting Jobs
tags: [slurm,pbs,submit,jobs,queue,cluster,mit,bigbang,stampede]
last_updated: November 3, 2015
summary: "An introduction to SLURM scripts as well as a few examples for submitting jobs to the cluster."
---

## Example SLURM Submission Scripts

Scripts can be submitted via using the command `sbatch sub.script`. There are a number of options you can specify but the key ones are listed here.

* `#SBATCH -n 1`
This line sets the number of cores that you're requesting. Make sure that your tool can use multiple cores before requesting more than one.

* `#SBATCH -t 5`
This line specifies the running time for the job in minutes. If your job runs longer than the value you specify here, it will be cancelled. There is currently no time limit to jobs so you can leave this out for now.

* `#SBATCH -p RegNodes`
This line specifies the SLURM partition (AKA queue) under which the script will be run. The RegNodes partition is good for routine jobs that can handle being occasionally stopped and restarted. PENDING times are typically short for this queue as it has the most number of cores.

* `#SBATCH -o hostname.out`
This line specifies the file to which standard out will be appended. If a relative file name is used, it will be relative to your current working directory.

* `#SBATCH -e hostname.err`
This line specifies the file to which standard error will be appended. SLURM submission and processing errors will also appear in the file.

* `#SBATCH --mail-user=ajk@123.com`
The email address to which the --mail-type messages will be sent.

* `#SBATCH --mail-type=END`
Because jobs are processed in the "background" and can take some time to run, it is useful send an email message when the job has finished (--mail-type=END). Email can also be sent for other processing stages (START, FAIL) or at all of the times (ALL)

Here a few example scripts. Contact [mailto:brendan.f.griffen@gmail.com Brendan Griffen] if you would like help constructing something more specific. [https://rc.fas.harvard.edu/resources/running-jobs/ Harvard FAS] has a great number of examples and useful tips.

* I have a simple serial job which doesn't require much memory.

```bash
#!/bin/bash
#SBATCH -n 1
#SBATCH -o jobname.o%j
#SBATCH -e jobname.e%j
#SBATCH -p RegNodes
#SBATCH -J jobname
#SBATCH --share

./job.exe
```

* I have an openMP task which requires a large amount of memory (up to 256GB).

```bash
#!/bin/bash
#SBATCH -N 1 -n 64
#SBATCH -o jobname.o%j
#SBATCH -e jobname.e%j
#SBATCH -p AMD64
#SBATCH -J jobname

export OMP_NUM_THREADS=64
./job.exe
```

* I have a MPI job which doesn't use much memory.

```bash
#!/bin/bash
#SBATCH -N 2 -n 16
#SBATCH -o jobname.o%j
#SBATCH -e jobname.e%j
#SBATCH -p RegNodes
#SBATCH -J jobname

mpirun -np 16 ./P-Gadget3 param.txt 1>OUTPUT 2>ERROR
```

* I have a job which is requires lots of IO between MPI tasks but doesn't consume much memory.

```bash
#!/bin/bash
#SBATCH -n 16
#SBATCH -o jobname.o%j
#SBATCH -e jobname.e%j
#SBATCH -p HyperNodes
#SBATCH -J jobname

mpirun -np 16 ./job.exe
```

* I have a MPI job which requires a very large amount of memory.

```bash
#!/bin/bash
#SBATCH -n 128
#SBATCH -o jobname.o%j
#SBATCH -e jobname.e%j
#SBATCH -p RegNodes
#SBATCH -J jobname

mpirun -np 128 ./job.exe
```

## Visualizing Cluster Usage
The cluster now can be visualized interactively. Just type `> sview` in the command prompt on antares.mit.edu and you'll see the an infographic.

Alternatively, you can use Ganglia which can be found here: http://antares.mit.edu/ganglia/

## SLURM Status & Error Help 

### Status Messages
When you type `squeue -u username` you should see a shortened version of one of the following (e.g. PD for PENDING). You can also see these in the `sview`.

* `PENDING` 
Job is awaiting a slot suitable for the requested resources. Jobs with high resource demands may spend significant time PENDING.

* `RUNNING` 
Job is running.

* `COMPLETED`   
Job has finished and the command(s) have returned successfully (i.e. exit code 0).

* `CANCELLED`   
Job has been terminated by the user or administrator using scancel.

* `FAILED`  
Job finished with an exit code other than 0.

### Error Messages 

* `JOB <jobid> CANCELLED AT <time> DUE TO TIME LIMIT`   
You did not specify enough time in your batch submission script. The -t option sets time in minutes, or can also take hours:minutes:seconds form (12:30:00 for 12.5 hours)

* `Job <jobid> exceeded <mem> memory limit, being killed`   
Your job is attempting to use more memory than you've requested for it. Either increase the amount of memory requested by `--mem` or `--mem-per-cpu` or, if possible, reduce the amount your application is trying to use. For example, many Java programs set heap space using the `-Xmx` JVM option. This could potentially be reduced.

* `slurm_receive_msg: Socket timed out on send/recv operation`  
This message indicates a failure of the SLURM controller. Though there are many possible explanations, it is generally due to an overwhelming number of jobs being submitted, or, occasionally, finishing simultaneously. If you want to figure out if SLURM is working use the `sdiag` command. `sdiag` should respond quickly in these situations and give you an idea as to what the scheduler is up to.

* `JOB <jobid> CANCELLED AT <time> DUE TO NODE FAILURE`
This message may arise for a variety of reasons, but it indicates that the host on which your job was running can no longer be contacted by SLURM.

Again, see the [Harvard FAS SLURM website](https://rc.fas.harvard.edu/resources/running-jobs/) for extra information and help.