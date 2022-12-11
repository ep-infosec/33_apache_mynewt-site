## Create Your First Mynewt Project

This page shows how to create a Mynewt Project using the `newt` command-line tool.

<br>

### Pre-Requisites

* Newt:
    * If you have taken the Docker route, you have already installed Newt.
    * If you have taken the native install route, you have to ensure that you have installed the Newt tool following the instructions for [Mac](../../newt/install/newt_mac.md) or [Linux](../../newt/install/newt_linux.md) as appropriate, and that the `newt` command is in your system path. 
* You must have Internet connectivity to fetch remote Mynewt components.
* You must [install the compiler tools](native_tools.md) to 
support native compiling to build the project this tutorial creates.  

<br>

### Newt New

Choose a project name. For this tutorial we will call this project `myproj`.
Enter the `newt new myproj` command. 

``` no-highlight
$ newt new myproj
Downloading project skeleton from apache/incubator-mynewt-blinky...
Installing skeleton in myproj...
Project myproj successfully created.
```

<br>

Newt populates this new project with a base skeleton of a new Apache Mynewt 
project.  It has the following structure. 

**Note**: If you do not have `tree`, install it by running `brew install tree`.

```no-highlight 
$ cd myproj
$ tree 
.
├── DISCLAIMER
├── LICENSE
├── NOTICE
├── README.md
├── apps
│   └── blinky
│       ├── pkg.yml
│       └── src
├── project.yml
└── targets
    ├── my_blinky_sim
    │   ├── pkg.yml
    │   └── target.yml
    └── unittest
        ├── pkg.yml
        └── target.yml

6 directories, 10 files
```

<br>


The Newt tool has installed the base files for a project comprising the following:

1. The file `project.yml` contains the repository list that the project uses to fetch
its packages. Your project is a collection of repositories.  In this case, the project just
comprises the core mynewt repository.  Later you will add more repositories
to include other mynewt components.
2. The file `apps/blinky/pkg.yml` contains the description of your application
and its package dependencies.
3. A `target` directory containing `my_blinky_sim`, a target descriptor used to
build a version of myproj.  Use `newt target show` to see available build 
targets.
4. A non-buildable target called `unittest`.  This is used internally by `newt` and is not a formal build target.

**NOTE:** the actual code and package files are not installed 
(except the template for `main.c`).  See the next step for installing the packages.

<br>

### Newt Install

Once you've switched into your new project's directory, the next step is to fetch
any dependencies this project has.  By default, all Newt projects rely on a
single remote repository, apache-mynewt-core.  The _newt install_ command will
fetch this repository.

```no-highlight
$ newt install
apache-mynewt-core
```

**NOTE:** _apache-mynewt-core_ may take a while to download.  To see progress,
use the _-v_ (verbose) option to install. 

<br>

Once _newt install_ has successfully finished, the contents of _apache-mynewt-core_ will have been downloaded into your local directory.  You can view them by issuing the following commands in the base directory of the new project:

```no-highlight
$ tree -L 2 repos/apache-mynewt-core/
.
<snip>
├── fs
│   ├── fs
│   └── nffs
├── hw
│   ├── bsp
│   ├── hal
│   └── mcu
├── libs
│   ├── baselibc
│   ├── bootutil
│   ├── cmsis-core
│   ├── console
│   ├── elua
│   ├── flash_test
│   ├── imgmgr
│   ├── json
│   ├── mbedtls
│   ├── newtmgr
│   ├── os
│   ├── shell
│   ├── testreport
│   ├── testutil
│   └── util
├── net
│   └── nimble
<snip>
```

As you can see, the core of the Apache Mynewt operating system has been brought 
into your local directory. 

<br>

### Test the project's packages

You have already built your first basic project. You can ask Newt to execute the unit tests in a package. For example, to test the `libs/os` package in the `apache-mynewt-core` repo, call newt as shown below.

```
$ newt test @apache-mynewt-core/libs/os
Testing package @apache-mynewt-core/libs/os
Compiling hal_bsp.c
Compiling os_bsp.c
Compiling sbrk.c
Archiving native.a
Compiling flash_map.c
<snip>
```

<br>

To test all the packages in a project, specify `all` instead of the package name.

```no-highlight
$ newt test all
...lots of compiling and testing...
...about 2 minutes later ...
Archiving bootutil.a
Linking test_bootutil
Executing test: /myproj/bin/unittest/libs/bootutil/test_bootutil
Passed tests: [net/nimble/host fs/nffs libs/os hw/hal libs/mbedtls libs/util sys/config libs/bootutil]
All tests passed
```

<br>

### Build the Project

To build and run your new application, simply issue the following command:

```
$ newt build my_blinky_sim 
Compiling base64.c
Compiling cbmem.c
Compiling datetime.c
Compiling tpq.c
Archiving util.a
Compiling main.c
Archiving blinky.a
Compiling flash_map.c
Compiling hal_flash.c
Archiving hal.a
Compiling cons_fmt.c
Compiling cons_tty.c
<snip>
Linking blinky.elf
App successfully built: /Users/sterling/dev/tmp/my_app/bin/my_blinky_sim/apps/blinky/blinky.elf
```

<br>

### Run the Project

You can run the simulated version of your project and see the simulated LED
blink.

```
$ newt run my_blinky_sim
No download script for BSP hw/bsp/native
Debugging /workspace/bin/my_blinky_sim/apps/blinky/blinky.elf
<snip>
Reading symbols from /workspace/bin/my_blinky_sim/apps/blinky/blinky.elf...done.
(gdb)
```

Type `r` at the `(gdb)` prompt to run the project. You will see an output indicating that the `hal_gpio` pin is toggling between 1 and 0 in a simulated blink. 

<br>

### Complete

Congratulations, you have created your first project!  The blinky application
is not terribly exciting when it is run in the simulator, as there is no LED to
blink.  Apache Mynewt has a lot more functionality than just running simulated
applications.  It provides all the features you'll need to cross-compile your
application, run it on real hardware and develop a full featured application.

If you're interested in learning more, a good next step is to dig in to one of
the [tutorials](../tutorials/tutorials) and get a Mynewt project running on real hardware.

Happy Hacking!
