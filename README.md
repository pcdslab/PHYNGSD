# phyNGSC

It is now possible to compress and decompress large-scale Next-Generation Sequencing files taking advantage of high-performance computing techniques. To this end, we have recently introduced a scalable hybrid parallel algorithm, called phyNGSC, which allows fast compression as well as decompression of big FASTQ datasets using distributed and shared memory programming models via MPI and OpenMP. 

We present the design and implementation of a novel parallel data structure which lessens the dependency on decompression and facilitates the handling of DNA sequences in their compressed state using fine-grained decompression in a technique that is identified as *in compresso* data processing. 

Our proposed structure and methodology brings us one step closer to compressive genomics and sublinear analysis of big NGS datasets.

As a proof-of-concept, we implemented some methods developed for [DSRC v1](http://sun.aei.polsl.pl/dsrc/) to underline the compression/decompression portion of our hybrid parallel strategy, since it exhibits superior performance for sequential solutions.

### Disclaimer:

All files provided in this project are part of the article "*Scalable Data Structure to Compress Next-Generation Sequencing Files and its Application to Compressive Genomics*" to be presented at The 4th International Workshop on High Performance Computing on Bioinformatics (HPCB 2017) in conjunction with The IEEE International Conference on Bioinformatics and Biomedicine (BIBM 2017). The files should be used for testing purposes.

## Getting Started

### Prerequisites

In order to compile, build and run phyNGSC you need to install and configure the following:

* __OS:__ Any Linux distribution, preferably Ubuntu (14.04 LTS or a later) or CentOS.
* __Compiler:__ The GNU Compiler Collection (GCC) version 4.8 or later.
* __MPI__: MPICH version 3 or later. MVAPICH2, the Ohio State University derivative of MPICH, can also be use.

### Compiling and Building

To compile an build phyNGSC we provided a simple `Makefile`. Navigate to the directory containing the files and run the command:

```
make all
```

If your system configurations (compiler and MPI implementation) are correct, you should see the following output:

```
mpicxx.mpich -O3 -m64 -Wall -pedantic -fopenmp -std=c++11 -c phyNGSC.cpp -o phyNGSC.o
mpicxx.mpich -O3 -m64 -Wall -pedantic -fopenmp -std=c++11 -c bit_stream.cpp -o bit_stream.o
mpicxx.mpich -O3 -m64 -Wall -pedantic -fopenmp -std=c++11 -c huffman.cpp -o huffman.o
mpicxx.mpich -O3 -m64 -Wall -pedantic -fopenmp -std=c++11 -c tasks.cpp -o tasks.o
mpicxx.mpich -O3 -m64 -Wall -pedantic -fopenmp -std=c++11 -o phyNGSC phyNGSC.o bit_stream.o huffman.o tasks.o
mpicxx.mpich -O3 -m64 -Wall -pedantic -fopenmp -std=c++11 -c phyNGSD.cpp -o phyNGSD.o
mpicxx.mpich -O3 -m64 -Wall -pedantic -fopenmp -std=c++11 -o phyNGSD phyNGSD.o bit_stream.o huffman.o tasks.o
```

To clean up object files generated during compilation in the directory and the `phyNGSC` and `phyNGSD` programs, run:

```
make clean
```

## Running phyNGSD

To run phyNGSC for compression go to [this link](https://github.com/pcdslab/PHYNGSC) for instruction.

To run phyNGSC for compression, execute the command

```
mpiexec -np <num_of_processes> ./phyNGSD <input_file> <output_file> <num_of_threads>
```
where

* `mpiexec` – Runs an MPI program.
* `-np <num_of_processes>` – Specify the number of processes to use. `<num_of_processes>` must be at least 2.
* `./phyNGSD` – Executable phyNGSC program for decompression.
* `<input_file>` – An NGSC file with file extension `*.ngsc`. Must exist.
* `<output_file>` – A FASTQ file with file extension `*.fastq`. Created if it does not exist.
* `<num_of_threads>` – Number of OpenMP threads to use per MPI process. `<num_of_threads>` must be at least 1.

**NOTE:** Depending on the implementations of MPI currently on your system, `mpicxx` (to compile and build) and `mpiexec` (to run) can be wrappers for MPICH or OpenMPI (to mention two MPI implementations). Make sure `mpicxx` and `mpiexec` are using MPICH.

### Usage Example:
Decompress the file `input.NGSC` using 8 processes and 5 thread per process, and name the resulting FASTQ file `output.fastq`:

```
mpiexec -np 8 ./phyNGSD input.NGSC output.fastq 5
```

## Dataset Files

In this project a NGSC file named `input.ngsc` is provided for your testing purposes. It represents a FASTQ file of 100 MB, so make sure to have enough space for the decompression process.

## Authors

* **Sandino Vargas-P&eacute;rez** - Department of Computer Science at Western Michigan University, USA. [E-mail](mailto:sandinonarciso.vargasperez@wmich.edu).
* **Fahad Saeed** - Department of Electrical and Computer Engineering and Department of Computer Science, Western Michigan University, USA. [E-mail](mailto:fahad.saeed@wmich.edu).

## Acknowledgment

This material is based in part upon work supported by the National Science Foundation under Grant Numbers NSF CRII CCF-1464268 and NSF CAREER ACI-1651724. Any opinions, findings, and conclusions or recommendations expressed in this material are those of the author(s) and do not necessarily reflect the views of the National Science Foundation. We would also want to acknowledge Sebastian Deorowicz and Szymon Grabowskie for their DSRC algorithm (version 1) which underlines the compression portion of our parallel hybrid strategy.