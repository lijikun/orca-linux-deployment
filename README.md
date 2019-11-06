Scripts and installation process for the deployment of parallelized quantum chemistry program [ORCA](https://orcaforum.kofo.mpg.de/app.php/portal) on Linux-x64 computers. 

# For ORCA version 4

Tested to work under Ubuntu LTS 18.04 (amd64) with ORCA version up to 4.2.1 using OpenMPI 3.1.4. 

I tried to use the static-linked binary version but it crashed into a segfault without any meaningful error message, so we have to use the *shared-library* version here.

1. Make sure you have sudo privileges, download and install openmpi-3.1.4 from source. Change the directory names as necessory:

        cd /tmp
        wget https://download.open-mpi.org/release/open-mpi/v3.1/openmpi-3.1.4.tar.gz
        tar xvf openmpi-3.1.4.tar.gz
        cd openmpi-3.1.4
        ./configure --prefix=/opt/orca-4.2.1/openmpi-3.1.4
        make all
        sudo make install

2. While it is compiling, go to https://orcaforum.kofo.mpg.de/app.php/dlext/ (login required) to download ORCA version 4 archive, **choosing the shared-library version** compiled against OpenMPI 3.1.4. Extract it to the desired installation directory. (Here we use `/opt/orca-4.2.1`.)

        cd /opt
        sudo tar xvf ~/Downloads/orca_4_2_0_linux_x86-64_shared_openmpi314.tar.xz
        sudo mv orca_4_2_0_linux_x86-64_shared_openmpi313 orca-4.2.1

3. Now `cd` to th3 repo. Edit the initial part of the `orcainit4` script, such that the `$orca_path` and `openmpi_path` variables point to the correct paths for your ORCA and OpenMPI installations, for example:
        
        orca_ver=4.2.1
        openmpi_ver=3.1.4
        orca_path=/opt/orca-${orca_ver}
        openmpi_path=${orca_path}/openmpi-${openmpi_ver}

4. Source (in a bash, zsh, fish, etc. shell) the script.

        source orcainit4

5. Test provided sample ORCA jobs. Make sure it works with openmpi parallelism.

        cd test
        orca test1-1proc.orca test2-2procs.orca

6. Now you are ready to go. Just remember to source `orcainit4` again each time you start a new shell. Note that, like the example above, you can input multiple input files. They will be run serially.

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
