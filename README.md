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

SBS should can only be used with a single node, but you can have as many worker threads as you need. For example, if you have a set of jobs to run, you could run the following:

    $ sbs submit -- wget http://example.com/foo.json
    1
    $ sbs submit -- wget http://example.com/bar.json
    2
    $ sbs submit -afterok 1:2 -cmd jq -s '.[0] * .[1]' foo.json bar.json
    $ sbs run -maxprocs 2
    
This isn't heavily tested code, but works for the use-case I had (having a single-file batch scheduler for when I'm not on an HPC cluster). Normally, it assumes the parameters (CPUs, Memory, etc) are present as comments in a submitted bash script (as is the norm in HPC-land). However, I also added an option to directly submit a command. stdout/stderr are all captured and stored.

The job runner also has a daemon mode, so you can keep it running in the background if you'd like to have things running on demand.

Installation is as simple as copying the sbs script someplace in your $PATH (with Python3 installed). You should also set the ENV var $SBSHOME, if you'd like to have a common place for jobs.

The usage is very similar to many HPC schedulers...

    Usage: sbs {-d sbshome} command {options}
    
    Commands:
      cancel      Cancel a job
      cleanup     Remove all completed jobs
      help        Help for a particular command
      hold        Hold a job from running until released
      release     Release a job
      run         Start running jobs
      shutdown    Stop running jobs (and cancel any currently running jobs)
      status      Get the run status of a job (run state)
      submit      Submit a new job to the queue


sbs-1.0
