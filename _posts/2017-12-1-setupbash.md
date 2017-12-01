---
layout: post
title: How I set up my bash scripts
tags: [bioinformatics]
author: caryn
---

Note: some parts of this information may be specific to my hpc and my farm.
However, I've worked on two separate clusters, and what I learned on the first applied generally to the second.

~~~bash
#!/bin/bash -l

#SBATCH -D /path/to/project/directory
#SBATCH -J jobname
#SBATCH -o /path/to/project/directory/logs/jobname_%j.out
#SBATCH -e /path/to/project/directory/logs/jobname_%j.out
#SBATCH --time=1:00:00
#SBATCH --mem=2000

set -u
set -e

####################
## ENV

# here I load my modules

###################
## Data

# here I identify paths to data and local variables

####################
## MAIN

#I will often time the main processes of my script. I use this infromation to better inform the time constraints for the future.

start=`date +%s`

# Here I do the stuff I want to do
# usually with lots of echos

end=`date +%s`
((run_time=$end - $start))
echo the XX step of $SLURM_JOB_ID took $run_time s
~~~

this last line will print a statement that looks like: "the XX steo of 1234559 took 1243 s

The basic philosophy is to set up the script clearly so that it's easy to read in the future.
I usually generate all of the local variables in the data section to prevent the main section from getting cluttered.

I also like to include a line of code at the end of the script that will put the script information into an info file in the Project directory.
This looks somethign like:

```
echo $SLURM_JOB_ID $run_tie $var1 $var1 $data1 $data2 >> run_name.info
```

If you're unfamiliar with bash, the $ infront of something indicates that it is a variable.
As bash is beyond the lesson, there are lots of cool resources to reference.

