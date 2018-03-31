For deployment of parallelized [ORCA](https://orcaforum.cec.mpg.de/) on Linux-x64 computers. 

# For ORCA version 4

Tested to work under Debian 9.4 amd64 with ORCA 4.0.1.2. I tried to use the static-linked binary version but it crashes into a segfault without any meaningful error message, so we have to use the *shared-library* version here.

1. Assuming you have sudo privileges. Download and install openmpi-2.0.2 from source:

        cd /tmp
        wget https://www.open-mpi.org/software/ompi/v2.0/downloads/openmpi-2.0.2.tar.gz
        tar xvf openmpi-2.0.2.tar.gz
        cd openmpi-2.0.2
        ./configure --prefix=/usr/local/lib/openmpi-2.0.2
        make all -j4
        sudo make install

2. While it is compiling, go to https://cec.mpg.de/orcadownload/ to download ORCA version 4 archive, choosing the shared-library version. Extract it to the desired installation directory. (e.g. `/opt/orca-4.0.1.2`)

        cd /opt
        sudo tar xvf ~/Downloads/orca_4_0_1_2_linux_x86-64_shared_openmpi202.tar.xz
        sudo mv orca_4_0_1_2_linux_x86-64_shared_openmpi202 orca-4.0.1.2

3. Download the [`orcainit4`](https://raw.githubusercontent.com/lijikun/orca-linux-deployment/master/orcainit4) script from this repo. Edit it, such that the `$orca_path` variable corresponds to the right path for your orca installation.

        sed orcainit4 -i "s:/opt/orca-4.0.1.2:[your ORCA path]:g"

4. Source (in a bash, zsh, fish, etc. shell) the `orcainit4` script.

        source orcainit4

5. Test provided sample ORCA jobs. Make sure it works with openmpi.

        cd test
        orca test1-single.orca
        orca test2-2thread.orca

6. Now you are ready to go. Just remember to source `orcainit4` again each time you start a new shell.


# For ORCA version 3

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
        orca test1-single.orca
        less test1-single.orca.log
        orca test2-2thread.orca
        less test2-2thread.orca.log

7. Now you are ready to run ORCA:  

        orca JOBNAME
        less JOBNAME.log
