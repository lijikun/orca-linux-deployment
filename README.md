For deployment of parallelized [ORCA](https://orcaforum.cec.mpg.de/) on Linux-x64 computers. 

# For ORCA version 4
1. Download and install from openmpi-2.0.2 source:

        mkdir /tmp/openmpi202
        cd /tmp/openmpi202
        wget https://www.open-mpi.org/software/ompi/v2.0/downloads/openmpi-2.0.2.tar.gz
        tar xvf openmpi-2.0.2.tar.gz
        cd openmpi-2.0.2
        ./configure --prefix=/usr/local/lib/openmpi-1.6.5
        make all -j4
        sudo make install

2. Download ORCA version 4 archive and extract to the desired installation directory. (e.g. `/opt/orca-4.0.1.2`)

3. Edit `orcainit4` script to the right path for your orca installation.

4. Source (in a bash or bash-like shell) the `orcainit4` script.

5. Test provided orca jobs.


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

u       orcapath="[Insert your ORCA path here.]"
        sed -i "s:/opt/orca-3.0.3:${orcapath}:g" orcainit

5. Using bash or a compatible shell, source the `orcainit3` script to set the proper ORCA and OpenMPI in enviromental variables:  

        source orcainit3

6. Test single-core and multi-core ORCA calculations:  

        cd test
        orca test1-single.orca
        less test1-single.orca.log
        orca test2-8thread.orca
        less test2-8thread.orca.log

7. Now you are ready to run ORCA:  

        orca JOBNAME
        less JOBNAME.log
