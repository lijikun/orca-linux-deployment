Scripts and installation process for the deployment of parallelized quantum chemistry program [ORCA](https://orcaforum.kofo.mpg.de/app.php/portal) on Linux-x64 computers. Also works on WSL Debian or Ubuntu.

# For ORCA version 4-6

Tested to work under Ubuntu LTS 22.04, Debian 12 and WSL-Debian under Windows 11 with ORCA version up to 6.0.0 with OpenMPI 4.1.6, with modern AMD or Intel x64 processors. 

I tried to use the static-linked binary version but it crashed into a segfault without any meaningful error message, so we have to use the *shared-library* version here.

0. Take note of the version number of the ORCA package you want to use, along with the OpenMPI version against which it is compiled.

1. Make sure you have sudo privileges. Download and install openmpi-4.1.6 (or any other specific version ORCA is compiled against) from source by running the following commands sequentially (Change the directory names as necessary):

        cd /tmp
        wget https://download.open-mpi.org/release/open-mpi/v4.1/openmpi-4.1.6.tar.gz
        tar xvf openmpi-4.1.6.tar.gz
        cd openmpi-4.1.6
        ./configure --prefix=/opt/orca-6.0.0/openmpi-4.1.6
        make all
        mkdir -p /opt/orca-6.0.0
        sudo make install

2. While it is compiling, open your web browser and go to https://orcaforum.kofo.mpg.de/app.php/dlext/ (login required) to download ORCA version x.y.z, **choosing the shared-library version** linked against the above version of OpenMPI (e.g. "ORCA 6.0.0, Linux, x86-64 (AVX2), .tar.xz Archive Dynamically linked serial & parallel binaries linked against OpenMPI 4.1.6. Requires AVX2 instruction set!"). Extract it to the desired installation directory. (Here I use `/opt/orca-6.0.0`.)

        cd /opt
        sudo tar xvf ~/Downloads/orca_6_0_0*.tar.xz
        sudo mv orca_6_0_0* orca-6.0.0

3. Download the `orcainit[4|5|6]` script from this repo to your computer and put it wherever appropriate (I'd just put it in the same directory as ORCA). Edit the initial part of the script, such that the `$orca_path` and `openmpi_path` variables contain the correct paths for your ORCA and OpenMPI installations, for example:
        
        orca_ver=6.0.0
        openmpi_ver=4.1.6
        orca_path=/opt/orca-${orca_ver}
        openmpi_path=${orca_path}/openmpi-${openmpi_ver}

4. Source (in a bash, zsh, fish, etc. shell) the script.

        source /opt/orca-6.0.0/orcainit6

5. Test provided sample ORCA jobs. Make sure it works with openmpi parallelism.

        cd test
        orca test1-1proc.orca test2-2procs.orca

6. Now you are ready to go. Just remember to source `orcainit[4|5|6]` again each time you start a new shell. Note that, like the example above, you can give it multiple input files. They will be run sequentially.

        orca [your job file 1] [your job file 2] ...
        
**Bonus:** `janpa_orca` is a helper script to use [JANPA](http://janpa.sourceforge.net/) with ORCA. Put it in the same folder as your JANPA .jar files, and edit it to point to the correct paths for JANPA and ORCA. Source this script, and you can just run `janpa_orca myOrcaJobName.orca` (provided the ORCA job has already been completed) to automate the JANPA analysis.


# For ORCA version 3 (Not tested or updated anymore).

Tested to work on Debian 8 and CentOS 7, with ORCA 3.0.3.

1. ORCA 3.0.3 requires OpenMPI 1.6.5 for multithreaded computing. Download openmpi-1.6.5 source code:  

        wget http://www.open-mpi.org/software/ompi/v1.6/downloads/openmpi-1.6.5.tar.gz

2. Configure and install OpenMPI 1.6.5:  

        tar xvf openmpi-1.6.5.tar.gz
        cd openmpi-1.6.5
        ./configure --prefix=/usr/local/lib/openmpi-1.6.5
        make all 
        sudo make install
    Make sure that files are installed to /usr/local/lib/openmpi-1.6.5/

3. Download ORCA files from the website: https://orcaforum.cec.mpg.de/ .  
Extract the files to target directory (e.g. `/opt/orca-3.0.3`).
        
        mkdir /opt/orca-3.0.3
        cd /opt/orca-3.0.3
        tar xvf [orca archive file location]/orca_3_0_3_linux_x86-64.tbz

4. Edit the orca_path line in the `orcainit3` script such that it points to the path of the ORCA installation.

        orcapath="[Insert your ORCA path here.]"
        sed -i "s:/opt/orca-3.0.3:${orcapath}:g" orcainit3

5. Using bash or a compatible shell, source the `orcainit3` script to set the proper ORCA and OpenMPI in enviromental variables:  

        source orcainit3

6. Test single-core and multi-core ORCA calculations:  

        cd test
        orca test1-1proc.orca
        less test1-1proc.orca.log
        orca test2-2procs.orca
        less test2-2procs.orca.log

7. Now you are ready to run ORCA:  

        orca JOBNAME
        less JOBNAME.log
