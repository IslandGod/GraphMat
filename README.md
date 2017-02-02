GraphMat graph analytics framework
=====================================

[![Build Status](https://travis-ci.org/narayanan2004/GraphMat.svg?branch=distributed_primitives_integration)](https://travis-ci.org/narayanan2004/GraphMat)

Note: This is a major update from GraphMat v1.0 (single node and distributed).
Please see changelog for details.

Requirements:
------------
- Intel compiler (icpc) + Intel MPI (mpiicpc + mpi libraries)

(or)

- GCC + MPICH (Other MPI libraries not tested)

- Boost serialization library (links to libboost\_serialization)

To compile with Intel compiler + Intel MPI :
--------------------------------------------
    make

To compile with gcc + MPICH:
----------------------------
    make MPICXX=mpic++ CXX=g++

To run:
-------

Set the following environment variables:

    export OMP_NUM_THREADS=[ number of cores in system ]
    export KMP_AFFINITY=scatter

Use `numactl` for NUMA (multi-socket) systems if you are running 1 MPI rank on all the sockets e.g.

    mpirun -np <NRANKS> numactl -i all bin/PageRank < graph file >
    mpirun -np <NRANKS> numactl -i all bin/BFS < graph file > < start vertex >

To compile and run tests:
-------------------------

GraphMat uses Catch, a C++ based testing framework.
  
    git submodule init
    git submodule update
    make test

To run all the tests with a single MPI rank,

    ./testbin/test 

Tests are also runnable in distributed mode with multiple ranks,

    mpirun -np <NRANKS> ./testbin/test

You can also do 
    ./testbin/test -? 
to list all the options available

Converting data to GraphMat compatible format:
----------------------------------------------

You can convert an edge list file to GraphMat compatible format using
the `graph_converter` utility. GraphMat works on a binary graph
format.

To convert from a text file with 3 white space separated columns
(`src, dst, edge_value`) to GraphMat format, do

    mpirun -np <NRANKS> bin/graph_converter --selfloops 1 --duplicatededges 1 --inputformat 1 --outputformat 0 --inputheader 0 --outputheader 1 --nvertices < nvertices > < input text file prefix> < output graphmat file prefix>

This command reads input files (inputfile0, inputfile1, inputfile2 .. inputfilen)
in text format and converts it into files
(outfile0, outfile1, outfile2 ... outfile<nranks>) in GraphMat friendly format.

You can remove selfloops and duplicatededges (when multiple edges with
same src and dst are found, only one is retained) by changing their
values in the command line from 1 to 0.

References:
-----------

If you use GraphMat in your work, please cite the following papers:

- Narayanan Sundaram, Nadathur Satish, Md Mostofa Ali Patwary, Subramanya R Dulloor, Michael J. Anderson, Satya Gautam Vadlamudi, Dipankar Das, Pradeep Dubey, 
"GraphMat: High performance graph analytics made productive", Proceedings of VLDB 2015, volume 8, pages 1214 - 1225.

- Michael J. Anderson, Narayanan Sundaram, Nadathur Satish, Md Mostofa Ali Patwary, Theodore L. Willke and Pradeep Dubey, "GraphPad: Optimized Graph Primitives for Parallel and Distributed Platforms," 2016 IEEE International Parallel and Distributed Processing Symposium (IPDPS), Chicago, IL, USA, 2016, pp. 313-322.

Paper URL: 
- www.vldb.org/pvldb/vol8/p1214-sundaram.pdf
- http://ieeexplore.ieee.org/xpls/abs_all.jsp?arnumber=7516027


More documentation coming soon. For questions, please email narayanan.sundaram@intel.com
