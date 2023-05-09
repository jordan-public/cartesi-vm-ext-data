# cartesi-vm-ext-data
# External Data Availability to the Cartesi VM

## Introduction

[Cartesi](https://cartesi.io) is an Optimistic rollup running on top of layer 1 (or 2, etc.) Ethereum Virtual Machine (EVM) blockchains. The Cartesi Virtual Machine (VM) is a RISC-V virtual machine which has the capability to mark specific locations in its state as "dirty" and occasionally save its state in a Merkle tree. This allows it to not only commit snapshots of its state, but efficiently keep an entire history of state changes. If and when a fraud dispute needs to be generated, this allows the "faulty" instruction to be pinpointed and proven fraudulent by emulating it on the EVM.

In addition the Cartesi VM is **deterministic**. It is single processor (but possibly multi-threaded) RISC-V VM. This allows multiple instances of it to produce the exact same state changes and output, given the exact same initial state and input. As such, multiple nodes can run the same execution VM and consent upon the correctness of the execution.

Even if all but one nodes produce fraudulent output, the honest node can prevail, while fraud proof(s) can be generated for the fraudulent node(s) by emulating them on the EVM. 

## Problem

Compared to the EVM, RISC-V is significantly more powerful and allows for execution of entire operating system (Cartesi Linux build) with its entire ecosystem of libraries and utilities. If RISC-V acts as a computer, EVM would compare to a programmable pocket calculator. 

However, there is a problem: the Cartesi VM can perform powerful calculations but on what data? The only way to feed it with significant amount of data is upon initialization. After that, all input has to be recorded on the underlying EVM, in order to get to Cartesi and guarantee that all nodes receive the exact same input.
## Implementation Issues

In order to guarantee determinism, the Cartesi VM cannot communicate with the "outside world". It cannot have network access, external file systems or input outside of the one recorded on the underlying EVM.

If such external data is to be accessed, it would have to be guaranteed to get to all Cartesi nodes (actually, those involved in a particular matched set, working on a common task). In addition, the eventual fraud proof would have to be able to not emulate the RISC-V on the EVM, but also produce proofs that a specific node did not get the correct data (this needs ellaboration!).
## External Data Commintment

Sometimes it is enough to have data commitment instead of availability. Using Merkle trees, large data sets can be recorded, where the root uniquely identifies the entire data set (almost; well, intractably hard to produce two distinct Merkle trees with identical Merkle root). This is in case the program running on the Cartesi node needs to check on the data at arbitrary points. In other words, to verify part of the pre-committed data.

The commitment can be recorded on the EVM as input to Cartesi, and each consequent request in the pre-committed data can be accessed or proven by passing the Merkle path through the EVM, again as input. 

This is still not enough! The Cartesi VM can do more than just verification - it can compute on big data, only of that data were available.
## External Data Availability

Let's propose a new architecture:
-  

## Cartesi Dispute Proof Issue

## Is this Feasible? How much work?