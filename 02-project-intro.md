---
layout: page
title: R for reproducible scientific analysis
subtitle: Project management with RStudio
minutes: 15
---

> ## Learning Objectives {.objectives}
>
> * To be able to create self-contained projects in RStudio
> * To be able to use git from within RStudio
> 

### Introduction

The scientific process is naturally incremental, and many projects
start life as random notes, some code, then a manuscript, and
eventually everything is a bit mixed together.

<blockquote class="twitter-tweet"><p>Managing your projects in a reproducible fashion doesn't just make your science reproducible, it makes your life easier.</p>&mdash; Vince Buffalo (@vsbuffalo) <a href="https://twitter.com/vsbuffalo/status/323638476153167872">April 15, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Most people tend to organize their projects like this:

![](img/bad_layout.png)

There are many reasons why we should *ALWAYS* avoid this:

1. It is really hard to tell what version of your data is
the original and what is the modified;
2. It gets really messy because it mixes files with various
extensions together;
3. It probably takes you a lot of time to actually find
things, and relate the correct figures to the exact code
that has been used to generate it;

A good project layout will ultimately make your life easier.

* It makes it easier to ensure the integrity of your data
* Share your code with someone else (a lab-mate, collaborator, or supervisor)
* Allows you to easily upload your code with your manuscript submission
* Makes it easier to pick the project back up after a break

### The RStudio solution

Fortunately, RStudio can help you manage your code effectively.

One of the most powerful and useful aspects of RStudio is its project management 
functionality. We'll be using this today to create a self-contianed, reproducible
project.
 
> #### Challenge: Installing Packages {.challenge} 
> 
> The first thing we're going to do is to install a third-party package, `packrat`. 
> This is allows RStudio to create self-contained packages: any further packages 
> you download will be contained within their respective projects. This is really 
> useful, as different versions of packages can change results as new knowledge is 
> gained. This allows you to easily keep track of the version used for your analyses. 
> 
> Packages can be installed using the `install.packages` function: 
> 
> ~~~ {.r} 
> install.packages("packrat") 
> ~~~
>

> #### Challenge: Creating a self-contained project {.challenge}
>
> Now we're going to create a new project in RStudio:
> 
> 1. Click the "File" menu button, then "New Project".
> 2. Click "New Directory"
> 3. Click "Empty Project"
> 4. Type in the name of the directory to store your project, e.g. "my_project"
> 5. Make sure that the checkboxes for "Create a git repository" and "Use
>    packrat with this project" are selected >
> 6. Click the "Create Project" button
>

Now when we start R in this project directory, or open this project with RStudio,
all of our work on this project will be entirely self-contained in this directory.
By installing `packrat` and telling RStudio to use `packrat` with this project, 
any third-party packages will be installed in a separate library in the `packrat/`
subdirectory of our project. This means we don't have to worry about package
versions changing, especially when returning to a project after a long period of
time (for example when writing up your thesis!). 

> #### Tip: packrat {.callout}
>
> Any libraries you already have installed outside of your project will need
> to be reinstalled in each packrat project.
>
> Packrat will also analyse your script files and warn you if youre using 
> any libraries not installed and managed inside your project. This is useful
> if you reuse code between projects.
>
> Packrat also allows you to easily bundle up a project to share with someone
> else.
>
> RStudio has a more detailed [packrat tutorial](http://rstudio.github.io/packrat/)
>

Now lets load the packrat library:

~~~ {.r}
library("packrat")
~~~

Here we've called the function `library` and used it to load those packages
into our local namespace (our interactive R session). This means all of 
their functions are now available to us.


The main function you'll encounter in `packrat` is the `status` function:

~~~ {.r}
packrat::status()
~~~

~~~ {.output}
Up to date.
~~~

Here I've put the name of the library in front of its function, separated by
a `::`. This explicitly tells R to call the function from that library. This
can be useful to make your code clearer (`status` is fairly generic function
name and might be used by other packages), and useful when two packages have
functions with the same names (in which case order of library loading becomes
important), or you've written your own function or variable with the same 
name (you should try to avoid this).

You'll want to run this periodically (after installing libraries and writing
ew code) to make sure your project is still self-contained.

### Best practices for project organisation

Although there is no "best" way to lay out a project, there are some general
principles to adhere to that will make project management easier:

#### Treat data as read only

This is probably the most important goal of setting up a project. Data is
typically time consuming and/or expensive to collect. Working with them
interactively (e.g., in Excel) where they can be modified means you are never
sure of where the data came from, or how it has been modified since collection. 
It is therefore a good idea to treat your data as "read-only".

#### Data Cleaning

In many cases your data will be "dirty": it will need significant preprocessing
to get into a format R (or any other programming language) will find useful. This
task is sometimes called "data munging". I find it useful to store these scripts
in a separate folder, and create a second "read-only" data folder to hold the 
"cleaned" data sets.

#### Treat generated output as disposable

Anything generated by your scripts should be treated as disposable: it should 
all be able to be regenerated from your scripts.

There are lots of different was to manage this output. I find it useful to
have an output folder with different sub-directories for each separate 
analysis. This makes it easier later, as many of my analyses are exploratory
and don't end up being used in the final project, and some of the analyses
get shared between projects.

#### Separate function definition and application

The most effective way I find to work in R, is to play around in the interactive
session, then copy commands across to a script file when I'm sure they work and
do what I want. You can also save all the commands you've entered using the 
`history` command, but I don't find it useful because when I'm typing its 90%
trial and error.

When your project is new and shiny, the script file usually contains many lines
of directly executed code. As it matures, reusable chunks get pulled into their
own functions. Its a good idea to separate these into separate folders; one 
to store useful functions that you'll reuse across analyses and projects, and
one to store the analysis scripts.

> #### Tip: avoiding duplication {.callout}
>
> You may find yourself using data or analysis scripts across several projects.
> Typically you want to avoid duplication to save space and avoid having to
> make updates to code in multiple places.
>
> In this case I find it useful to make "symbolic links", which are essentially
> shortcuts to files somewhere else on a filesystem. On linux and OSX you can
> use the `ln -s` command, and on windows you can either create a shortcut or
> use the `mklink` command from the windows terminal.
>

#### Version Control

We also set up our project to integrate with git, putting it under version control.
RStudio has a nicer interface to git than shell, but is very limited in what it can
do, so you will find yourself occasionally needing to use the shell. Let's go 
through and make an initial commit of our template files.

The workspace/history pane has a tab for "Git". We can stage each file by checking the box:
you will see a Green "A" next to stage files and folders, and yellow question marks next to
files or folders git doesn't know about yet. RStudio also nicely shows you the difference
between in files between commits.

> #### Tip: versioning disposable output {.callout}
>
> Generally you do not want to version disposable output (or read-only data).
> You should modify the `.gitignore` file to tell git to ignore these files
> and directories.
>

> #### Challenge 1 {.challenge}
> 
> Use packrat to install the packages we'll be using later:
>
> * ggplot2
> * plyr
>
> Note: if you run `packrat::status` it will warn you that these libraries
> are unecessary because they're not used in any project code. 
>

> #### Challenge 2 {.challenge}
>
> Create the following directories (either through bash or RStudio)
> for the project:
>
> * `data/` for storing read-only data
> * `munge/` for storing preprocessing scripts
> * `cleaned-data/` for storing read-only preprocessed data
> * `R-funcs/` for storing any functions we write
> * `scripts/` for storing analysis scripts 
> * `output/` for storing any output we generate
>

> #### Challenge 3 {.challenge}
> 
> Modify the `.gitignore` file to contain `data/`, `cleaned-data` and
> `output/` so that disposable output isn't versioned.
>
> Add the newly created folders to version control using 
> the git interface.
>
> 