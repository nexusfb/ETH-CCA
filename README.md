# ETH Cloud Computing Architecture Project

This repository contains the code, resources, and reports for the **Cloud Computing Architecture semester project** at **ETH Zurich**, focusing on scheduling latency-sensitive and batch applications in a cloud environment. The project involves the use of **Kubernetes**, **memcached**, and the **PARSEC** benchmark suite to explore performance under different levels of hardware resource contention.

## Overview

This project is divided into four parts, each designed to explore different aspects of scheduling, resource management, and performance analysis in a cloud environment using Kubernetes and various applications.

### Part 1: Memcached Performance Analysis
In Part 1, we investigate the performance of **memcached**, a latency-sensitive application, when deployed in a containerized environment. Memcached is a distributed memory caching system, and the goal of this part is to understand how it behaves under different query-per-second (QPS) loads.

**Key objectives**:
- Measure **tail latency** (95th percentile) as a function of increasing QPS.
- Introduce various types of **resource contention** (e.g., CPU, memory bandwidth, and caches) using the **iBench** microbenchmark suite.
- Analyze how hardware interference affects memcached performance.

By varying the number of cores and threads allocated to memcached, this part provides insights into optimizing latency-sensitive applications in the cloud.

### Part 2: PARSEC Benchmark Suite with Interference
In Part 2, we shift focus to batch applications by using the **PARSEC benchmark suite**. These applications are throughput-oriented and are deployed on Kubernetes. The goal is to evaluate how resource interference affects their performance and to explore their scalability with increasing parallelism.

**Key objectives**:
- Run PARSEC applications such as blackscholes, canneal, dedup, ferret, freqmine, radix, and vips.
- Introduce **hardware resource interference** (similar to Part 1) to observe how it impacts the throughput of these applications.
- Explore the scalability of these applications by varying the number of threads (1, 2, 4, and 8 threads) on an 8-core machine.

This part provides a comparison of how different workloads behave when subjected to resource interference, and how well they can scale with multiple threads.

### Part 3: Scheduling Policy Design
In Part 3, we develop and test an **optimal scheduling policy** for managing both latency-sensitive and batch applications. The goal is to minimize the total runtime (makespan) of batch jobs without violating the **Service Level Objectives (SLO)** of memcached.

**Key objectives**:
- Run memcached and batch applications concurrently, ensuring that memcached meets the **1ms latency SLO**.
- Measure and analyze the execution time of each batch job and the **SLO violation ratio** of memcached.
- Implement an optimal scheduling policy that effectively allocates CPU resources across multiple nodes in a Kubernetes cluster.

This part focuses on balancing the resource demands of latency-sensitive and batch applications, while minimizing performance degradation.

### Part 4: Dynamic Scheduling and Load Adaptation
In Part 4, we take the scheduling policy a step further by introducing a **dynamic load trace** for memcached. The load (QPS) fluctuates dynamically, and the goal is to adapt the scheduling policy in real-time to maintain memcached’s performance while completing batch jobs efficiently.

**Key objectives**:
- Implement a **dynamic controller** that adjusts CPU allocations based on real-time monitoring of CPU utilization and memcached load.
- Ensure that memcached’s **SLO violations** remain below a specified threshold, even under changing workloads.
- Run memcached and PARSEC jobs with real-time CPU adjustments and measure the total makespan and SLO violation ratio.

This part challenges the system to handle real-world dynamic conditions by adjusting resource allocations on-the-fly, ensuring both performance and efficiency in a cloud environment.

## Features

- **Memcached Performance Analysis**: Tail latency analysis of memcached with different configurations of threads and cores.
- **Batch Application Scheduling**: Implementation of batch jobs using PARSEC benchmark applications.
- **Kubernetes**: Orchestrating and scheduling jobs across cloud nodes using Kubernetes.
- **Resource Interference**: Performance evaluation of memcached and batch jobs under various resource interferences (CPU, memory bandwidth, cache).

## Table of Contents

- [Project Structure](#project-structure)
- [Dataset](#dataset)
- [Setup Instructions](#setup-instructions)
- [Running Experiments](#running-experiments)
- [Results](#results)
- [Contributors](#contributors)

## Project Structure

├── data/ # Experimental data 

├── scripts/ # Scripts for running experiments 

├── reports/ # Project reports 

├── results/ # Performance metrics and plots 

└── README.md # This file


## Dataset

The project uses workloads from the **PARSEC benchmark suite**, which includes applications such as blackscholes, canneal, dedup, ferret, freqmine, radix, and vips. Each application’s performance is evaluated under different levels of resource interference using the **iBench** suite.

## Setup Instructions

To replicate the experiments in this project:

1. **Install necessary tools**:
   - Kubernetes
   - Google Cloud SDK
   - Kops (Kubernetes Operations)

2. **Clone the repository**:
    ```bash
    git clone https://github.com/yourusername/Cloud-Computing-Architecture.git
    cd Cloud-Computing-Architecture
    ```

3. **Setup the cloud environment**:
    - Redeem cloud credits and set up your Google Cloud project.
    - Deploy a Kubernetes cluster using the `kops` tool.

4. **Run the setup scripts** to initialize your cluster and deploy memcached and PARSEC workloads.

## Running Experiments

### Memcached Performance

To run the **memcached** performance tests with varying query-per-second (QPS) loads:
1. Deploy the memcached server:
    ```bash
    kubectl create -f memcached.yaml
    ```
2. Run the **mcperf** load generator:
    ```bash
    ./mcperf -s MEMCACHED_IP -T 16 -C 4 -Q 1000 --scan 5000:125000:5000
    ```

### PARSEC Benchmark

To run **PARSEC batch jobs**:
1. Deploy PARSEC jobs using Kubernetes:
    ```bash
    kubectl create -f parsec-benchmark.yaml
    ```
2. Introduce interference using iBench (e.g., CPU interference):
    ```bash
    kubectl create -f ibench-cpu.yaml
    ```

## Results

The results for memcached latency, QPS, and PARSEC application runtimes are detailed in the project report. Key findings include:
- Memcached achieves the best performance with 2 cores and 2 threads.
- Interference (e.g., CPU contention) significantly impacts memcached performance at higher QPS.
- PARSEC applications scale well with multiple cores, but resource interference can degrade performance.

## Contributors

- **Federica Bruni** - ETH Zurich
- **Riccardo Bollati** - ETH Zurich
