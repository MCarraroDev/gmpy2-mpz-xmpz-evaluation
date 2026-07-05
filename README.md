<div align="center">

# Big Numbers Benchmark: Python int vs gmpy2 (mpz, xmpz)

[![Italian Version](https://img.shields.io/badge/README-Italiano-green?style=flat-square)](README.it.md)

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat-square&logo=jupyter&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-4C72B0?style=flat-square&logo=python&logoColor=white)

</div>

## Overview

This repository contains a comprehensive performance benchmark comparing Python's native arbitrary-precision integers (`int`) against the GNU Multiple Precision Arithmetic Library (GMP) via the `gmpy2` module. 

The objective is to establish exactly when and how to utilize `gmpy2` for large-scale mathematical computations, specifically evaluating the differences between:
*   **Python int**: Native, immutable.
*   **gmpy2 mpz**: C-optimized, immutable.
*   **gmpy2 xmpz**: C-optimized, mutable (allows in-place memory modification).

## Methodology

To ensure statistical reliability and eliminate operating system background noise, every data point in this benchmark is the result of 10,000 independent runs. Execution times were measured using `time.perf_counter()` and visualized using Kernel Density Estimation (KDE) to illustrate the distribution and stability of the results.

## Results

### 1. Single Operation Crossover
Tests the execution time of multiplying two numbers of increasing sizes (from 10 to 100,000 digits). 

![Test 1: Crossover Point](./results/test_1.png)

**Conclusion:** Native Python is faster for numbers under 100 digits due to C-API overhead. For numbers exceeding 1,000 digits, `gmpy2` is strictly required for optimal performance.

### 2. Accumulation and Memory Stress
Tests a standard `for` loop executing 500,000 simple additions (`+= 1`) starting from zero.

![Test 2: Memory Stress Test](./results/test_2.png)

**Conclusion:** For simple loops keeping values relatively small, Python's native integer caching outperforms `gmpy2`. Between the GMP types, the mutable `xmpz` significantly outperforms `mpz` by avoiding object reallocation.

### 3. Algorithmic Scaling (Iterative Factorial)
Tests the calculation of 20000! using an iterative multiplication loop, simulating heavy algorithmic workloads where both iteration count and variable size scale drastically.

![Test 3: Iterative Factorial](./results/test_3.png)

**Conclusion:** `gmpy2 xmpz` is the optimal data structure for sequential heavy mathematics. It leverages both advanced C-level multiplication algorithms and memory-efficient in-place mutations, vastly outperforming both `mpz` and native Python `int`.

## Requirements
**(Windows / macOS / Linux)** Run in a terminal:
```bash
pip install -r requirements.txt
```

This will install the following packages and any related dependency
*   `gmpy2`
*   `pandas`
*   `seaborn`
*   `matplotlib`

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
