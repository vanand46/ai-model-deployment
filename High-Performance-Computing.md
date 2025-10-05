# High Performance Computing (HPC)

In some cases, model **inference** and **training** can be difficult to optimize through code alone. One common solution is to add more **GPUs** or other computational resources. This approach—aggregating compute resources to boost performance—is known as **High Performance Computing (HPC)** or **supercomputing**.

A key concept in HPC is **parallel computing**, which allows multiple processors (CPUs or GPUs) to work simultaneously on different parts of a computation. Adding multiple GPUs to training or inference tasks is an example of this in action.

## HPC and Performance Constraints

While supercomputers and parallel computing can significantly speed up training and prediction, two important principles define their limits:

- **Amdahl’s Law:** The speed-up from parallelization is limited by the portion of the program that cannot be parallelized.  
- **Gustafson’s Law:** The achievable speed-up grows with the size of the problem being solved.


## Key Terminology

- **Symmetric Multiprocessing (SMP):** Two or more identical processors share a single memory space.  
- **Distributed Computing:** Independent processing elements connected via a network.  
- **Cluster Computing:** A group of connected computers working together as a single system.  
- **Massively Parallel Processing (MPP):** Systems with hundreds or thousands of processors performing computations in parallel.  
- **Grid Computing:** Distributed computing that uses middleware to connect heterogeneous systems into a virtual supercomputer.

## HPC and Data at Scale

Working with **data at scale** is closely related to both **code optimization** and **parallel computing**.  
In this context, we often use **Apache Spark**, a cluster computing framework that enables distributed data processing and large-scale computation.

When discussing **scalability**, consider the following key questions for your model or service (where *service* refers to both the deployed model and its infrastructure):

1. **Training Scale:** Does my service train efficiently with much larger datasets?  
2. **Inference Scale:** Does my service make predictions quickly when given more data?  
3. **Load Scale:** Can my service handle additional concurrent requests?

Understanding which of these dimensions presents the primary **bottleneck** helps guide infrastructure decisions, code optimization, and model design.


## Additional Resource

- [High Performance Computing at IBM](https://www.ibm.com/topics/high-performance-computing)

[Previous Page](Performance-In-Python.md) -  [Next Page](Introduction-To-Spark.md)