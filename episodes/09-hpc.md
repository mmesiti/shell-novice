---
title: Recommendations on using the shell on shared HPC systems 
teaching: 5
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Not cause problems to other users 

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What actions I take might interfere with what other people are doing on the system?

::::::::::::::::::::::::::::::::::::::::::::::::::


HPC systems are powerful machines composed of many parts.

The uses on a HPC system share resources. 
Part of these resources are managed by a resource manager 
like SLURM (for computing nodes),
or by a system of quotas (for disk space),
but other parts are (almost) left to the sense of the users.

## Working on the Login Node 

Problems happen from time to time when users, 
*on the login node*,
abuse the system by invoking commands that take too many resources.

While the login nodes of HPC clusters
tend to be more powerful 
than a typical laptop,
there are potentially many users working at the same time.

When working with a shell on a login node,
it is deceptively easy to run a command 
that might require too many resources 
and make it harder for other people connected to the login node 
to work normally. They might experience:
- simple commands taking much longer to execute than normal;
- even difficulties typing correctly and receiving feedback while typing

If you run a command that takes more than a couple of minutes 
or uses many cores, please require a SLURM allocation.

::::::::::::::: callout

## How can I know if my command is requiring too much resources?

- Read the documentation of the tools you are using, searching for terms like "multithreading", "multiprocessing" and "parallelism".
- Do a quick test, using `top`/`htop` to see how many cores and memory your commands are using.

Also, watch your mailbox: someone might write to you pointing out the problem and a possible cause. 

:::::::::::::::::::::::


## Working with Global Filesystems

Global filesystems (which are typically not the *only* filesystems on a HPC cluster)
has limited capacity, both in terms of bandwidth 
(amount of data that can be transferred/copied)
and in terms of *metadata operations*
(like deletion/creation/moving of files).

For this reason, typically operating on *many* tiny files on the global filesystem should be avoided.

Typically, the home directories of users are in a global filesystem.

::::::::::::::::::::::: callout

## How to learn more

Attend a course for the HPC system you intend to use,
where all the limitations and risks will be explained in concrete,
and what you can do to cope with those limits.

:::::::::::::::::::::::::::::::


:::::::::::::::::::::::::::::::::::::::: keypoints

- When working on the *login node* of a HPC system, 
  make sure your commands are not using too many computational resources
  and do not run for more than a couple of minutes.
- If you need more power, create an interactive (or batch) allocation
  with the resource manager of the HPC cluster (e.g., SLURM)
- Performing too many file operations might as well cause slowdowns for other users attempting to use the filesystem.
- Performing too many file operations might be inefficient


::::::::::::::::::::::::::::::::::::::::::::::::::
