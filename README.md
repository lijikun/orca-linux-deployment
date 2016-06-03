For deployment of parallelized ORCA on Linux-x64 computer. Tested to work on Debian 8 and CentOS 7, with ORCA 3.0.3.

1. ORCA 3.0.3 needs OpenMPI 1.6.5 for proper multithreaded computing. Download openmpi-1.6.5 source code:  

        wget http://www.open-mpi.org/software/ompi/v1.6/downloads/openmpi-1.6.5.tar.gz

2. Configure and install OpenMPI 1.6.5:  

        tar xvf openmpi-1.6.5.tar.gz
        cd openmpi-1.6.5
        ./configure --prefix=/usr/local/lib/openmpi-1.6.5
        sudo make all install
Make sure files are installed to /usr/local/lib/openmpi-1.6.5/

3. Download ORCA files from this website: https://orcaforum.cec.mpg.de/ .  
Extract the files to a directory (e.g. /opt/orca-3.0.3).

4. Edit the orca_path line in the orcainit script such that it points to the path of the ORCA installation.

        sed -i 's/\/opt\/orca-3.0.3/PATH-TO-YOUR-ORCA-INSTALLATION/g' orcainit

5. Using bash or a compatible shell, source the orcainit script to set the proper ORCA and OpenMPI in enviromental variables:  

        source orcainit

6. Test single-core and multi-core ORCA calculations:  

        cd test
        orca test1-single.orca
        tail test1-single.orca.log
        orca test2-8thread.orca
        tail test2-8thread.orca.log

7. Now you are ready to run ORCA:  

        orca JOBNAME
        tail JOBNAME.log
