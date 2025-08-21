---
layout: post
title: "[WIP] HVM, Interaction Combinators, and Symbolic AI"
date: 2025-08-21 01:00 +0700
#modified: 2020-03-03 16:49:47 +07:00
tags: [AI, Symbolic, Learning, Jargon]
description: Data-Driven AI is creating their magnum opus, but a new set of discoveries may soon put symbolic AI back in people's visions of future thinking machines.
image: "/assets/img/succ-combinator.jpg"
---


# Realization
Computers and computation have been a series of discoveries compared to magic in fantasy worlds with how much we have changed our lives by them, and how its true inner workings continues to elude us. If we divide how we go about computing, it would likely look like this:

- Physical Layer
- Low Level Representation
- Computation Models
- Computer Programming
- Systems & Applications


Formal computation models had an optimal computation method for decades that have gone under-utilized: interaction networks. Until recently, there have been few, if any, proper implementations that avoided tremendous overhead. A man in Brazil who calls himself Victor Taelin implemented a virtual machine that limited overhead and allowed for the optimal properties to shine, known as Higher Order Virtual Machine.  

Toying around with the tool gave me some extremely valuable insights, and an understanding of why Higher Order Co, and arguably every autonomous AI company, finds Interaction networks so powerful. 

Without knowing everything there is to know, the main innovations of note are an alternative to lambda calculus that allows for inherent memoization of all computations to never share work, and superpositions, which allows for an input to exist as two states simultaneously, achieving the work of both on call. 

Let’s look at some toy examples.

# The NAND Gate Synthesizer 
Image
This program is an automatic circuit builder. It takes a target circuit, tries every possible arrangement, tests them all, and returns the first circuit that works exactly the same as the target. 

More concretely, a visual:

IN0,IN1 and IN2 all represent an input, arriving from either another NAND gate or x and y. Any input represents either a 0 or a 1. The main idea is to have x and y static, representing a zero or a 1, and with both inserted into static positions of any generated circuit containing combinations of logic gates (in this case NAND), will provide an output of 0 or 1 with a dedicated truth table.


We want a circuit that, with minimal gates, accomplishes this configuration. Depending on the allowed depth , solving this problem can be rather prohibitive. Why? Because the search space grows explosively. After all, something like this would grow at a rate of n^2 variables per gate, with at most 2 additional gates per gate. Depth limit prohibits this from going off the deep end.

(Image)
To be clear, enumerating all of the solutions would be infinite with many, many redundant solutions. Normally, in, say, Haskell, preventing the duplication of solutions requires quite a fair amount of overhead and communication. Not to mention that much of the same work could be duplicated through each enumeration. State of the art solutions, depending on the level of complexity allowable, bypasses this through clever SAT/SMT solutions to limit redundancy and cut down on the space of potential answers efficiently. 

But with Interaction Combinators, both processes are inherent in its architecture.
Below is an implementation that provides a solution in x seconds with a maximum depth of y in Haskell. It utilizes basic memoization methods, reusing whatever sub-expressions it can without using a SAT.

Here is a SAT solution stats with the same conditions
(Image)
And here is HVM, or the I-Net solution.
(Image)
	
[The code for this program is supplied in my github, written directly into HVM.](https://github.com/MasterOfCrows/HVM_Enum)

Imagine HVM as this NAND gate system. Imagine the electricity flowing through each node, to get to the end (or not when cut off). HVM’s architecture is much like it, ultimately: when a program is developed, a graph of all of the functions and used variable is created, and when one function utilizes pieces of another, or a variable used once before, it travels through the same set of nodes, automatically memoizing the process without much of any legwork. And when dimensions of answers need to be eliminated or a graph has run its course, an ‘Erase’ node is introduced, causing the graph to never traverse the path again. 

This has the same effect as eliminating redundancy through SATs, but inherent in the computation method. This, by definition of Turing Completeness, is indeed possible in lambda calculus, but with far more overhead, as shown in the runtimes and the need for SATs, also by definition. This is why HVM beats any other tool in runtimes. The more complex the problem, the more easily HVM and future tools like it will win out.

If another example for enumeration and heuristic search is desired, I wrote up a knapsack solver in HVM as well, with impressive runtimes. It does not beat SOTA, since they tend to use various tricks to solve this problem, but an equivalently complex solution properly utilizing HVM’s superpositions and inherent memoization will theoretically surpass it as well.

The potential for computation models like HVM to aid AI planning, theorem proving, and heuristic search appears substantial. Most interesting to me is how these properties potentially make it easier to construct complex reasoning systems without the heavy engineering effort of prior. My broader question is whether the same principles can help us build symbolic AI systems that match or complement data-driven models in flexibility and speed. I personally suspect so, and had thus marked my infatuation with a particular niche of computer science.
