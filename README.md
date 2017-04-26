sbs
====
simple batch scheduler - a single user job scheduler
----

A single-user batch scheduler that stores queued job information in a 
local directory. This greatly simplifies trying to run an analysis 
pipeline on a host without a traditional scheduler (SGE, SLURM, PBS) 
installed. Storing the job queue in a directory greatly simplifies 
installation and management because it doesn't require installing a 
separate database or other libraries.

The SBS queue directory (default: ./.sbs) can be changed using the 
environment variable: `$SBSHOME`

Similar to traditional job schedulers, job scripts are submitted as 
runnable scripts with some meta-data: (mem, cpus, dependencies) added 
as comments. Options can be set as command arguments too.

Jobs are then run, respecting memory or cpu limits. The maximum memory
or processors used can be set at run time (default: max number of CPUs
on the host and no memory limits).

Example usage:

    sbs submit job_script1.sh
    sbs submit job_script2.sh
    sbs run

sbs-1.0
