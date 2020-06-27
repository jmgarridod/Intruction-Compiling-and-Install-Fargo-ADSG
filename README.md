# Intruction-Compiling-and-Install-Fargo-ADSG
#### Step by step instrucction to compilling and install Fargo-ADSG, requeriments and solution currently problems.


![image](http://fargo.in2p3.fr/local/cache-vignettes/L495xH373/meshjup-b9d9b.jpg)  

Download fargo contain many problems and headache, but don't worry is not IRAF, with this guide is possible obtaint a great results. Before to install Fargo-ADSG we need install two another "libraries" for a compiling in parallel and obtain the maximum efficiency. 

  - openmpi-4 version (4.0.0 or 4.0.4 is recomended)
  - fftw-2.1.5 (must be 2.1.5 version)
---
# OpenMPI
Before to install OpenMPI , we need minumum two compilers gcc and g++.
when you install OpenMPI, this only install "libraries"  that you have a compiler (e.g if you dont have gcc   MPI dont install mpicc).

Make shure that you have installed with:

```sh
which gcc
```
you obtaint (or another direction):
##### /usr/bin/gcc
#

or 
```sh
gcc --version
```

you obtaint (or another version no problem): 
##### gcc (GCC) 10.1.1 20200507 (Red Hat 10.1.1-1)
##### Copyright (C) 2020 Free Software Foundation, Inc.
##### This is free software; see the source for copying conditions.  There is NO
##### warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
#

the same for g++.  
If you haven't any of two, you can install with (Fedora):

```sh
sudo yum install gcc
sudo yum install gcc-c++
sudo yum install compat-gcc-34-g77
```

- Is posible that g77 is not found. try install "gfortran"
-----
Other possible repositores needed are (specially in Ubuntu):
- openmpi-bin
- openmpi-common
- openssh-client
- openssh-client
- openssh-server
- libopenmpi1.3libopenmpi-dbg
- libopenmpi-dev

It happens that when you install the compilers automatically install any of these repositories. Anyway, you can install them quickly with (for Ubuntu)

```sh
sudo apt-get install openmpi-bin openmpi-common openssh-client openssh-server libopenmpi1.3libopenmpi-dbg libopenmpi-dev
```
For Fedora don't find them in yum or dnf, just ignore them.

-----



Download openmpi-4.0.0:
| openmpi-4.0.0    | https://www.open-mpi.org/software/ompi/v4.0/ |
| --------- | -----:|  

go to the folder where you download or save openmpi-4.0.0
```sh
tar -xvf openmpi-4.0.0.tar.gz
cd openmpi-4.0.0
```
Next we configure the directory where will install openmpi. This is a "prefix" you can choose anything folder. (Choose a folder that allways you remember). I create a new folder "openmpi" in "/usr/local/openmpi". Is necesary be "su" or "sudo" user:

```sh
sudo mkdir /usr/local/openmpi
```

Now, in the folder openmpi-4.0.0 (the one you downloaded), we apply the configure.

```sh
sudo ./configure --prefix="/usr/local/openmpi"
```

go for a coffee, this can take at least 5 minutes.
Next make a "make".

```sh
sudo make
```
 and then
 
 ```sh
sudo make install
```

We install openmpi, the next step is call and/or write the parth and libries in ".bashrc". Why ? : is a permanent call (dont worry you can errase after), with this you will not have to call openmpi every time you use it.
Firts locate the file ".bashrc"
 ```sh
locate .bashrc
```
we will always find it in : "/home/name_user/.bashr" (it is a hidden file so you cannot see it directly)
we write the "path" and "libraries" of openmpi in .bashrc

 ```sh
echo export PATH="$PATH:/usr/local/openmpi/bin" >> /home/name_user/.bashrc
echo export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/openmpi/lib" >> /home/name_user/.bashrc
```
make sure to change "name_user" and the openmpi installation folder if necessary.
For make a test of installation

 ```sh
which mpirun
```

and obtaion the installation folder:

##### /usr/local/openmpi/bin/mpirun
#
if you go to the folder installation, in this case "/usr/local/openmpi/bin/", you should see many files, the most important for this instace are: mpirun, mpicc, mpic++.

---
---

# FFTW

Fargo-ADSG allows only the version 2.1.5.
| FFTW-2.1.5   | http://www.fftw.org/download.html |
| --------- | -----:|

The steps are similar to openmpi installation. We create other folder for install this library.

 ```sh
sudo mkdir /usr/locate/fftwlib
```

Next go to the directory that you donwloaded fftw-2.1.5 ande extrat
 ```sh
tar -xvf fftw-2.1.5.tar.gz
cd fftw-2.1.5
```

Again, we will assign a "prefix", but this time we must enable it with mpi.
First we must specify where mpicc is located, then we enable mpi to finish with the folder where we will install it ("prefix"). (mpicc location depends on openmpi installation folder)
```sh
sudo ./configure MPICC="/usr/local/bin/mpicc" --enable-mpi --prefix="/usr/local/fftwlib"
```
then,
```sh
sudo make install
```
This process is faster, but we must make sure that mpi was indeed enabled. If it was enabled, fftw created the libraries linked to mpi. otherwise I only create the fftw libraries. To see this we go to the installation folder.

```sh
cd /usr/local/fftwlib
ls 
```

we will find 3 folders, "include" must contain 4 files, "info" 6 files (not relevant) and "lib" 8 files.
- /include/  
        - fftw.h  
        - fftw_mpi.h  
        - rfftw.h  
        - rfftw_mpi.h  
- /info/  
        - fftw.info   
        - fftw.info-1  
        - fftw.info-2  
        - fftw.info-3  
        - fftw.info-5  
        - fftw.info-6  

- /lib/  
        - libfftw.a  
        - libfftw.la  
        - libfftw_mpi.a  
        - libfftw_mpi.la  
        - librfftw.a  
        - librfftw.la  
        - librfftw_mpi.a  
        - librfftw_mpi.la  

Files with "_mpi" ending are very important, make sure they are in the folders. If they are not found, go back to the installation step and make sure to call MPICC correctly.
If you contain all the files, you completed the installation !!!
---
---

# Compilling Fargo-ADSG
Last step is compile Fargo-ADSG. First download this version.
| Fargo-ADSG   | http://fargo.in2p3.fr/-FARGO-ADSG- |
| --------- | -----:|

Create a new folder "FargoADSG" and move the donwloaded file in to this folder. Go to "FargoADSG" folder and:
```sh
gzip -d fargoadsg.tar.gz
tar xvf fargoadsg.tar
```
this create 4 new folder "in", "idl", "out1" and "src", enter in the last.
```sh
cd src/
```

Before to compile, we need to set the commands ("prefix") that the "makefile" file will use to compile in parallel with "mpi" and "fftw". we can write this prefix in "".bashrc" so you don't have to call them back later.

```sh
echo export FFTW_PREFIX="/usr/local/fftwlib" >> /home/name_user/.bashrc
echo export MPI_PREFIX="/usr/local/openmpi" >> /home/name_user/.bashrc
```

or (if you only use this once)
```sh
export FFTW_PREFIX="/usr/local/fftwlib"
export MPI_PREFIX="/usr/local/openmpi"
```
Make sure to change "name_user" and the openmpi and fftw installation folders if necessary.
Then, we compile the code in parallel and using fftw.

```sh
make BUILD="parallelfftw"
```
or (if is necessary)

```sh
sudo make BUILD="parallelfftw"
```

If all is well, you should get a message like this and code see: successfully.txt in this repository.

##### Creating ../source.tar.bz2
##### NOTE
##### This built is PARALLEL (MPI) and uses FFTW librairies
##### If you want to change this,
##### then you need to issue:
##### gmake BUILD=parallel
##### gmake BUILD=sequentialfftw
##### gmake BUILD=sequential
#

### Congratulations !! you installed Fargo-ADSG, make sure you have the "fargo" file in the "Fargo-ADSG" folder.
 
 ---
 ---
#### Wait? ... Do you say: I have a problem in the code or compile ?

##### Dont worry, Here I leave you a compilation of the most common problems that the Fargo installation has left me

# SOLUTION PROBLEMS

...

# BUILDING ...BUILDING ...BUILDING ...BUILDING ...BUILDING



