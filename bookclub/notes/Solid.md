# Clean Architecture

## Chapter 1: What is Design and Architecture

```
The goal of software architecture is to minimize the human resources required to build and maintain the required system.
```

```
Slow and steady wins the race.
The race is not to the swift, nor the battle to the strong.
The more haste, the less speed.
```

Developers who accept this lie exhibits the hare's overconfidence in their ability to switch modes from making messes to cleaning up messes sometime in the future, but they also make a simple error of fact. The fact is that making messes is always slower than staying clean, no matter which time scale you are using.

## Chapter 2: Why we should value architecture more than function?

If you give me a program that works perfectly but is impossible to change, then it won't work when the requirements change, and I won't be able to make it work. Therefore the program will become useless. There are systems that are pratically impossible to change, because the cost of change exceeds the benefit of change.

Just remember: If architecture comes last, then the system will become ever more costly to develop, and eventually change will become practically impossible for part or all of the system.

## Part II: Programming Paradigm

Structured Programming: imposes discipline on direct transfer of control.
Object-Oriented Programming: imposes discipline on indirect transfer of control.
Functional programming: imposes discipline upon assignment.

We use polymorphism as the mechanism to cross architectural boundaries; we use functional programming to impose discipline on the location of and access to data; and we use structured programming as the algorithmic foundation of our modules.

Dijkstra once said, "Testing shows the presence, not the absence, of bugs." We show correctness by failing to prove incorrectness, despite our best efforts. We can use tests to try to prove those small provable functions incorrect. If such tests fail to prove incorrectness, then we deem the functions to be correct enough for our purposes.

What is OO? OO is the ability, through the use of polymorphism, to gain absolute control over every source code dependency in the system. It allows the architect to create a plugin architecture, in which modules that contain high-level policies are independent of modules that contain low-level details. The low-level details are relegated to plugin modules that can be deployed and developed independently from the modules that contain high-level policies.

Why is immutability important as an architectural consideration? The answer is simple: All race conditions, deadlock conditions, and concurrent update probelms are due to mutable variables. You cannot have a race condition or a concurrent update problem if no variable is ever updated. You cannot have deadlocks without mutable locks.

Event sourcing is a strategy wherein we store the transactions, but not the state. When state is required, we simply apply all the transactions from the beginning of time.

Software is composed of sequence, selection, iteration, and indirection. Nothing more. Nothing less.

## Part III: Design Principles

## References:

1. Solid Principles by Uncle Bob Martin, https://www.youtube.com/watch?v=t86v3N4OshQ
2. Principles of OOD, http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod
3.
