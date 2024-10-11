# ETH Cloud Computing Architecture Project

This repository contains the code, resources, and reports for the **Cloud Computing Architecture semester project** at **ETH Zurich**, focusing on scheduling latency-sensitive and batch applications in a cloud environment. The project involves the use of **Kubernetes**, **memcached**, and the **PARSEC** benchmark suite to explore performance under different levels of hardware resource contention.

## Overview

This project explores how to schedule latency-sensitive and batch applications in a cloud cluster. The tasks are implemented within Docker containers, orchestrated using Kubernetes. The main focus is on evaluating the performance of memcached under load while running batch applications, as well as the impact of hardware resource interference.

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
