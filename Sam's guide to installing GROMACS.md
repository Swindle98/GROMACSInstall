# Sam's guide to installing GROMACS

## Intro
*This might eventually be written*


## Prerequisites 

> Important! You need to have admin rights on your computer to install all this software and GROMACS

### Knowledge:
This guide assumes you have basic knowledge of the following :
- What is an operating system (OS)
- What is a GNU/linux / Unix like operting system
    > Note, We will be usign the term Unix Like operating system for this guide. Unix-like operating systems are the family of OSes that linux belongs to, that have many features in common to use.
- Basic BASH  commands, such as; `ls` `cd` `mkdir`

If you don't know much about these and want to learn more I reccomend [this tutorial from software carpentries](https://swcarpentry.github.io/shell-novice/)

### Software

You may have your favourite terminal, or text editor and your welcome to use them. I'm going to put the links to my favourite here. The links will take you to the page with instructions to install.

- **Code editor: VS code**

    Visual studio code (Not to be confused with visual studio, with a similar but purple logo) is a lightweight text editor by microsoft. It has some really nifty integrations with WSL. Once you have both WSL installed (read on to find out how) and VS code. You can open a file from within WSL in vs code by typing:

    `$ code <nameOf.File>`
    > Note: Don't type the `$` sign, it is standard convertion to show this at the start of a BASH command.

    You can open the current directory (folder) with:

    `$ code .`

    You Can install VScode from the other end of [this link](https://code.visualstudio.com/Download).

- **Terminal Windows: terminal** 

    Technically you don't need to download any special terminal emulator for this to work... windows has everything you need baked it. However, I prefer windows terminal(also by microsoft) because it has many great features (such as tabs) and great customisation. You can download it [here](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701)


## Installing WSL

From the start menu search for windows terminal (it might just be listed as terminal). When you find it right click and select 'run as adminsistrator'. Once opened, wait for text to apear and check that you are in a windows powershell session, it should be clear if you are or not. If not you will need to open powershell in a new tab by selecting powershell from the options that appear when you click on the arrow next to the new tab button.

In this powershell session type the following:

```
wsl --install
```

This should download and install linux with the ubunutu distrubution. If this hasn't worked make sure you are running powershell as an administrator. 

it will ask you for a name and password... make up what you want, it doesnt have to be the same as the computer.
You may need to restart your computer.

to check that you are in WSL2 run:
```
wsl -l -v
```
You should get a response something like this:
(yours may not say running), importantly we are looking for ubuntu and version 2:
```
 NAME                   STATE           VERSION
* Ubuntu                 Running         2
```

Now when you click on the dropdown menu next to the new tab button, you should have the option for a ubuntu session. If you don't restart your computer and see if it appears.


it should take some time to set up but then you should be in ubuntu. 

## Downloading pre-requisite linux software

Assuming this is a fresh install of ubuntu, you will have to install some softwarre first.

### C++ c compilers 
We are going to install the g++ open source c and c++ compiler.

type:
```
$ sudo apt update

$ sudo apt install g++
```
`sudo` is how you tell the CLI that you want to run the command as an admin (it will prompt for a password, use the one that you created earlier)
apt is a package manager, it installs softare for you.

These commands tell the `apt` that you want it to update all of its packaged sources, then search for and install g++.

It should prompt asking for y/n to install the software, type `y` then hit enter.

one its finsihed type:

```
$ g++ --version
```

If the install has worked it will tell you what verion of g++ you have.

We will do the same to install the gcc compiler:
```
$ sudo apt install gcc

$ gcc --version
```

### Installing Cmake
Next we need to install CMake, CMake is the program that builds the executable binaries from the source files we will download later.

run:
```
$ wget https://github.com/Kitware/CMake/releases/download/v3.26.4/cmake-3.26.4-linux-x86_64.sh
```
wget is a tool that downloads things from web pages, here we are downloading a scrip to install the current (at time of writing;22/06/23) verion of CMake.

run:

```
$ bash cmake-3.26.4-linux-x86_64.sh
```
`bash` is the command to exectute `.sh` files.

Press enter to read through the licence.

When prompted press `y` then the enter key to agree to the licesnce.

When prompted press `y` then the enter key to install into default location.

To check the install worked you can run:

```
$ cmake-3.26.4-linux-x86_64/bin/cmake --version
```
> What did I just do? 
Normally in a unix CLI you'd just type the name of a program to execute it. However because of how we've installed cmake, ubunutu doesn't know where to look for it. As we will probably be removing cmake after this, theres no point adding cmake to your PATH... and we can just execute it by typing in the location of the binary. 

### Download and install GROMACS

We're here its time, we're actually installing the program we set out to install. Some of these commands will take some time to run... have a cup of coffee with you.

run:
```
$ wget https://ftp.gromacs.org/gromacs/gromacs-2023.1.tar.gz

$ tar xfz gromacs-2023.1.tar.gz

$ cd gromacs-2023.1

$ mkdir build

$ cd build

$ ~/cmake-3.26.4-linux-x86_64/bin/cmake .. -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON -DCMAKE_C_COMPILER=gcc -DCMAKE_CXX_COMPILER=g++

$ make

$ make check 

$ sudo make install

source /usr/local/gromacs/bin/GMXRC
```

### [Optional] Removing CMake

```
$ cd 

$ rm -r cmake-3.26.4-linux-x86_64 && rm cmake-3.26.4-linux-x86_64.sh 
```