# 15-418-618-Exercise-1
15-418/618 Exercise 1

**Download Link:https://programming.engineering/product/15-418-618-exercise-1/**

Description
5/5 – (2 votes)
Overview

This exercise is designed to help you better understand the lecture material and be prepared for the style of questions you will get on the exams. The questions are designed to have simple answers. Any explanation you provide can be brief—at most 3 sentences. You should work on this on your own, since that’s how things will be when you take an exam.

You will submit an electronic version of this assignment to Gradescope as a PDF file. For those of you familiar with the LATEX text formatter, you can download the template and configuration files at:

http://www.cs.cmu.edu/˜418/exercises/config-ex1.tex

http://www.cs.cmu.edu/˜418/exercises/ex1.tex

Instructions for how to use this template are included as comments in the file. Otherwise, you can use this PDF document as your starting point. You can either: 1) electronically modify the PDF, or 2) print it out, write your answers by hand, and scan it. In any case, we expect your solution to follow the formatting of this document.


Problems

Consider the following code where each line within the function represents a single instruction.

typedef struct {

float x;

float y;

} point;

inline void innerProduct(point *a, point *b, float *result) {

float x1 = a->x; // Uses a load instruction float x2 = b->x;

float product1 = x1*x2;

float y1 = a->y;

float y2 = b->y;

float product2 = y1*y2;

float inner = product1 + product2;

*result = inner; // Uses a store instruction

}

void computeInnerProduct(point A[], point B[], float result[], int N) {

for (int i = 0; i < N; i++) innerProduct(&A[i], &B[i], &result[i]);

}

In the following questions, you can assume the following:

N is very large (> 106).

The machines described have modern CPUs, providing out-of-order execution, speculative execution, branch prediction, etc.

– There are ample resources for fetching, decoding, and committing instructions. The only per-

formance limitations are due to the number, capabilities, and latencies of the execution units.

– The branch prediction is perfect.

There are no cache misses.

The overhead of updating the loop index i is negligible.

The load/store units perform any necessary address arithmetic.

The overhead due to procedure calls, as well as starting and ending loops, is negligible.


Problem 1: Instruction-Level Parallelism

Suppose you have a machine M1 with two load/store units that can each load or store a single value on each clock cycle, and one arithmetic unit that can perform one arithmetic operation (e.g., multiplication or addition) on each clock cycle.

Assume that the load/store and arithmetic units have latencies of one cycle. How many clock cycles would be required to execute computeInnerProduct as a function of N? Explain what limits the performance.

Now assume that the load/store and arithmetic unit have latencies of 10 clock cycles, but they are fully pipelined, able to initiate new operations every clock cycle. How many clock cycles would be required to execute computeInnerProduct as a function of N? Explain how this relates to your answer to part A.


Problem 2: SIMD with ISPC

Consider running the following ISPC code.

export void computeInnerProductISPC(uniform point[] A, uniform point[] B,

uniform float[] result,

uniform int N)

{

foreach(i = 0 … N)

{

result[i] = A[i].x * B[i].x + A[i].y * B[i].y;

}

}

Suppose machine M2 has one 8-wide SIMD load/store unit, and one 8-wide SIMD arithmetic unit. Both have latencies of one clock cycle.

How many clock cycles would be required to execute computeInnerProductISPC as a function of N? Explain what limits the performance.

If we were to run the code shown in computeInnerProductISPC on a five-core machine M3, where each core has the same SIMD capabilities as M2, what would be the best speedup it could achieve over the single-core performance of part A? Explain.


Problem 3: SIMD, Multicore, and Multi-Threaded Parallelism with ISPC

Consider the five-core machine M3 described in Problem 2B. Suppose you could write multi-threaded code where there is no overhead associated with the threads. Each thread would run the function computeInnerProductISPC to compute some subset of the N elements. What is the maximum speedup you could achieve relative to the single-threaded code running on machine M2?

Now suppose we have a machine M4, identical to M3, except that each core supports three-way simul-taneous multithreading. What is the maximum speedup your multithreaded code could achieve relative to what it achieved running on machine M3.

Let N = 106. Running on machine M3, if we were to write a Pthreads program that spawns 250 threads, each computing 4000 inner products using computeInnerProductISPC, would this program get an overall performance improvement over one that uses a single thread to compute all N inner prod-ucts? (Use your own intution about the cost of spawning new threads in Pthreads when answering this question.) Explain.


Let N = 106. Running on machine M3, if we were to write an ISPC program that launches 250 tasks, each computing 4000 inner products using computeInnerProductISPC, would this program get an overall performance improvement over one that uses a single task to compute all N inner products? (Consider what you know about the performance characteristics of ISPC tasks.) Explain.

6


