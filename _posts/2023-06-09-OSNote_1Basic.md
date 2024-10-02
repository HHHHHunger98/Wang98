---
layout: post
usemathjax: false
title: OSNote_1Basic
date: 2023-06-09
tags: OS learning
---

> Note For OS Learning - Part 1 Basic

# Hardware Architecture

## How does a CPU execute a program

> Do you know how the statement `a = 1 + 2` be executed?

In this section we will find out the anwser.

<!--more-->

### Outline
1. Turning Machine
2. von Neumann Architecture
3. 线路位宽和CPU位宽
4. The Process Of Programm Execution
5. How `a = 1 + 2` Works In Real-World

### Turning Machine
A typical turning machine consists of three parts:
1. a very long tape
2. read/writer
3. store unit, control unit, compute unit

The basic working process of `1 + 2`is:
- the read/writer will read 1 and 2 into the store unit of the machine
- when comes to `+`, the status of the machine will change, the control unit notices that `+` is not a number but a operator, so it control the compute unit to process the add operation
- at the end the compute unit returns the result to control unit and then control unit returns it to the read/writer, as output the writer will print the result `3` on the tape

### von Neumann Architecture

> A typical von Neumann computer consists of 5 parts:
1. Arithmetic Logic Unit(Accumulator)
2. Control Unit
3. Memory
4. Input Device
5. Output Device

![vonNeumannArch]({{site.baseurl}}/assets/img/Von_Neumann_architecture.svg)


In order to let all those units work and communicate with each other, they need a bus to transmit data, 总线是必要的，冯诺依曼结构以中央处理器（控制单元）为核心，所有其他设备都需要和他打交道，因此离不开总线

![冯诺依曼结构]({{site.baseurl}}/assets/img/冯诺依曼模型.jpg)
