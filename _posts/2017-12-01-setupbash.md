---
layout: post
title: How I set up my bash scripts
tags: [bioinformatics]
author: caryn
---

Note: some parts of this information may be specific to my hpc and my farm.
However, I've worked on two separate clusters, and what I learned on the first applied generally to the second.

Also, this doesn't go over basic bash.
Check out http://swcarpentry.github.io/shell-novice/ for some basic for how to work in the shell.

In addition, while making this, I realize that I don't often compare my bash scripts to others working in my lab or who I know.
In the future, I'd like to compare bash scripts between people I know and see what is common and perhaps what is really different.

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
echo $SLURM_JOB_ID $run_time $var1 $var1 $data1 $data2 >> run_name.info
```

Prints both the slurm information and my data and variable information into the file run_name.info.
I have found these files to be incredibly useful when going to back to understand exactly what I've done.
They are particularly useful when doing array jobs, which I'll do tomorrow.


