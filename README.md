# Prim's Algorithm Parallel using ( MPI/OMP/Hybrid)

### How Prim's algorithm works
It falls under a class of algorithms called greedy algorithms that find the local optimum in the hopes of finding a global optimum.
We start from one vertex and keep adding edges with the lowest weight until we reach our goal.
The steps for implementing Prim's algorithm are as follows:
1.	Initialize the minimum spanning tree with a vertex chosen at random.
2.	Find all the edges that connect the tree to new vertices, find the minimum and add it to the tree
3.	Keep repeating step 2 until we get a minimum spanning tree
It requires O(n2) time.

### Parallelization of Prim’s algorithm using MPI

Thus, parallelization can be achieved in the following way:
 1. Partition the input set V into p subsets, such that each subset contains n/p consecutive vertices and their edges, and assign each process a different subset.
    Each process also contains part of array d for vertices in its partition.
    Let Vi be the subset assigned to process pi, and di part of array d which pi maintains.
    Partitioning of adjacency matrix is illustrated in Fig. 1. 1.
 2. Every process pi finds minimum-weight edge ei (candidate) connecting MST with a vertex in Vi. 
 3. Every process pi sends its ei edge to the root process using all-to-one reduction.
 4. From the received edges, the root process selects one with a minimum weight (called global minimum-weight edge emin), adds it to MST and broadcasts it to all other 	     processes.
 5. Processes mark vertices connected by emin as belonging to MST and update their part of array d. 6. Repeat steps 2–5 until every vertex is in MST.

### Time and Complexity of parallel

Finding a minimum-weight edge and updating of di during each iteration costs O(n/p).
Each step also adds a communication cost of all-to-one reduction and all-to-one broadcast. These operations complete in O(log p). Combined, cost of one iteration is O(n/p + log p). Since there are n iterations,
 total parallel time this algorithm runs in is:

		Tp = O(n^2/P) + O ( n log p)

### Analysis

when comparing communication time with a total computation time it can be noted that the Prim’s algorithm 
spends most of time in communication operations, and by increasing number of processes almost all the running time 
of the algorithm is spent on communication operations. A bottleneck in Prim’s algorithm is the cost of MPI_Reduce 
and MPI_Bcast communication operations. These operations require communication between all processes, and are much 
more expensive than local computation within each process, because all processes must wait until the operation is 
completed, or until the data are transmitted over the network. This prevents Prim’s algorithm from achieving substantial 
speedup of running time with increasing number of processes. Therefore, this algorithm is most efficient on the fewest number
of processes that the partitioned input graph can fit. 

### How TO Run the Project:

- Open the ubuntu terminal.
- Change the working directoy to your project directory.
For Running MPI code:
- Write the Command 
  mpicc prims.c -o prims.out
 Then
  mpiexec -np 4 ./prims.out (any command line argument) 
  
 For Running OMP code:
  gcc - fopenmp prims.c -o prims.out
 Then
 ./prims.out
 
 For Running Hybrid Code:
 gcc - fopenmp ./prims.c -o ./prims.out
 Then
 mpiexec -np 4 ./prims.out (any command line argument) 
  
  
  
  ### Details
  - prims.c is the file name
  - prims.out  is a file format used in older versions of Unix-like computer operating systems for executables, object code, and, in later systems, shared libraries
  - 4 is the number of processors 
  
## Author

#### Muhammad Awais Zahid
You can get in touch with me on my LinkedIn Profile : [![Linkedin Link](https://img.shields.io/badge/Connect-AwaisZahid-blue.svg?color=1DA1F2&logo=linkedin&longCache=true&style=for-the-badge
)](https://www.linkedin.com/in/awais-zahid-790124197)
<br><br>
For any query you can also email me : 
[![Gmail Link](https://img.shields.io/badge/Connect-zahidawais98@gmail.com-blue.svg?color=1DA1F2&logo=gmail&longCache=true&style=for-the-badge
)](mailto:zahidawais98@gmail.com)
<br><br>
You can also follow my GitHub Profile to stay updated about my latest projects: [![GitHub Follow](https://img.shields.io/badge/Connect-AwaisZahid-blue.svg?logo=Github&longCache=true&style=for-the-badg)](https://github.com/chowais181)<br>
If you liked the repo then kindly support it by giving it a star ⭐!




