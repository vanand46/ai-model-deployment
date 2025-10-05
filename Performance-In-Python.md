# Optimizing Performance in Python

Modern data scientists have access to a wide range of tools to build model-driven and algorithmic solutions. However, because many practitioners are relatively new to the field, **code optimization** is often overlooked. Optimizing performance can be critical—especially when **speed directly impacts business outcomes**.

Before diving into strategies and tools for optimization, it’s important to understand **when** optimization is worth the effort.

## When Optimization Is (and Isn’t) Worth It

### Large Model Training
Some models—such as **speech-to-text engines** or large neural networks—take days to train on multiple GPUs.  
You can profile [TensorFlow Models](https://www.tensorflow.org/tensorboard/tensorboard_profiling_keras) using [TensorBoard](https://www.tensorflow.org/tensorboard), but once models are optimized for architecture and performance, further speed improvements from code-level tweaks are minimal.  

In such cases:
- Focus on **hardware scaling** (e.g., more GPUs, distributed training).  
- Ensure **model checkpoints** are saved to avoid losses from failure.  

### Scikit-learn Models
With **scikit-learn**, optimization options are limited beyond:
- Using the `n_jobs` parameter for parallelism.  
- Trying different algorithms where applicable.  

Unless you’re implementing your own inference logic (e.g., for [Expectation-Maximization](https://en.wikipedia.org/wiki/Expectation%E2%80%93maximization_algorithm)), improving algorithmic efficiency is usually impractical.  
For **predictions**, best practices like **keeping the trained model in memory** can improve runtime performance.

---

## Non-ML Optimization Use Cases

Not all optimization problems require machine learning.  
Example: optimizing quarterly travel schedules (a [traveling salesman problem](https://en.wikipedia.org/wiki/Travelling_salesman_problem)) could be solved using:
- **Brute-force search**  
- **Minimal spanning tree** or other graph-based approaches  

Two areas where ML may not be the best fit:
1. **[Optimization](https://en.wikipedia.org/wiki/Mathematical_optimization)** problems  
2. **[Graph theory](https://en.wikipedia.org/wiki/Graph_theory)** applications  

Before building from scratch:
- Check out `scipy.optimize` for existing optimization algorithms.  
- Explore [NetworkX](https://networkx.org/documentation/stable/reference/algorithms/index.html) for graph-based problems (e.g., customer journeys, social networks).

---

## Identifying Bottlenecks

If optimized code isn’t available, you’ll need to identify performance bottlenecks using **profiling tools** such as:
- `cProfile`
- `line_profiler`
- `memory_profiler`

Once bottlenecks are found, you can apply optimization techniques to those specific areas.

To check how many CPU cores are available on your machine:
```python
import multiprocessing
print(multiprocessing.cpu_count())
````

---

## Common Techniques and Tools for Code Optimization

### 1. Use Appropriate Data Containers

* **[Sets](https://docs.python.org/2/library/sets.html)** → Faster lookups than lists
* **[Dictionaries](https://docs.python.org/3/c-api/dict.html)** → Efficient key-based access
* **[NumPy arrays](https://numpy.org/devdocs//reference/generated/numpy.array.html)** → Vectorized operations for numerical computation

### 2. [Multiprocessing](https://docs.python.org/3/library/multiprocessing.html)

* Built into Python’s standard library.
* Enables parallel execution using multiple cores.
* Suitable for CPU-bound tasks.
* API similar to the `threading` module.

### 3. [Threading](https://docs.python.org/3/library/threading.html#module-threading)

* Also part of the standard library.
* Enables concurrent execution for **I/O-bound** tasks.

### 4. [Subprocessing](https://docs.python.org/3/library/subprocess.html)

* Allows you to run external processes (e.g., Bash, R).
* Can manage input/output streams and return codes.

### 5. [MPI for Python (`mpi4py`)](https://docs.python.org/3/library/subprocess.html)

* Bindings for the **Message Passing Interface (MPI)** standard.
* Enables distributed computation across multiple processors or nodes.

### 6. [IPyParallel](https://ipyparallel.readthedocs.io/en/latest/)

* Parallel computing tools for **Jupyter** and **IPython**.
* Can work with `mpi4py`.

### 7. [Cython](https://cython.org/)

* A static compiler that translates Python code to C for performance-critical sections.
* Useful for computationally intensive loops and numeric tasks.

### 8. [CUDA(Compute Unified Device Architecture) (via PyCUDA)](https://en.wikipedia.org/wiki/CUDA)

* Nvidia’s **Compute Unified Device Architecture (CUDA)** allows GPU acceleration.
* In Python, use **PyCUDA** to offload heavy computations to CUDA-enabled GPUs.

## Additional Resources

* [SciPy Lectures: Code Optimization](https://scipy-lectures.org/advanced/advanced_python/index.html)
* [mpi4py Tutorial](https://mpi4py.readthedocs.io/en/stable/tutorial.html)
* [IPyParallel Demos](https://ipyparallel.readthedocs.io/en/latest/demos.html)
* [Cython Tutorial](https://cython.readthedocs.io/en/latest/src/tutorial/)

### Key Takeaway
 Optimize only when necessary—focus on identifying bottlenecks, leverage existing libraries, and choose the right tool (multiprocessing, Cython, or GPU acceleration) based on your task.

[Previous Page](README.md) [Next Page](High-Performance-Computing.md)