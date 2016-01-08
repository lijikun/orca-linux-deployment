A method for deployment of parallelized ORCA on Linux-x64 computer. Tested to work on Debian 8 and CentOS 7, with ORCA 3.0.3.


0. ORCA 3.0.3 needs OpenMPI 1.6.5 for proper multithreaded computing.

1. Download openmpi-1.6.5 source code:
    % wget http://www.open-mpi.org/software/ompi/v1.6/downloads/openmpi-1.6.5.tar.gz

2. Configure and install:
    % tar xvf openmpi-1.6.5.tar.gz
    % cd openmpi-1.6.5
    % ./configure --prefix=/usr/local/lib/openmpi-1.6.5
    % sudo  make all install
Make sure files are installed to /usr/local/lib/openmpi-1.6.5/

3. Edit the orca_path line in the orcainit script so it points to the proper path of the orca installation.

Using bash or a compatible shell, source the orcainit script to set the proper orca and openmpi in enviromental variables:
    % source orcainit

3.5 Test single-core and multi-core orca calculations:
	% cd test
	% orca test1-single.orca
	% orca test2-8thread.orca
		
4. Now you are ready to run ORCA:
    % orca JOBNAME
