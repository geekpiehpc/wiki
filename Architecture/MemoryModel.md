# Memory model

### Memory Coherence

**Memory coherence:** a memory system is coherent if any read of a data item returns the most recently written value of that data item. 

#### Coherent memory system: 

1. For a memory address wrote by a processor P, the next reads of P should get the written value. 
2. For a memory address wrote by a processor P1, after enough time, another processor P2 can get the value written by P1.
3. The write operations for one memory address are serialized, so if there are 2 writes to a memory address by any processor, any processor cannot get two results in different order. 

The coherence model does not define when a wrote by P1 can be read by P2. The memory consistency model is responsible for it. 

### Memory Consistency

**Memory consistency:** A memory consistency model for a shared address space specifies constraints on the order in which memory operations must appear to be performed (i.e. to become visible to the processors) with respect to one another.(when a written value will be returned/seen by a read).

The memory consistency model defines the order of operation pairs in **different** addresses.  

#### Sequential consistency model

1. In each processor, the read operation should always get the value of the last write operation in program order. 

```pseudocode
# Processor 1
Flag1 = 1
if (Flag2 == 0)
	do sth

# Processor 2
Flag2 = 1
if (Flag1 == 0)
	do sth
```

For P1, SC can guarantee that if the value of Flag2 is 0, the write of Flag1 happens before the write and read of P2. So there is at most one processor is in the `do sth` section (Neither processor failed to get in the critical section is also possible). 

2. There is only one order visible to all processors. For two write operations `W1, W2` (can be done by different processors), each processors should get the same sequence. 

```
# Processor 1
A = 1

# Processor 2
if (A == 1)
    B = 1

# Processor 3
if (B == 1)
    get(A)
```

If P3 gets the `B == 1`, the value of A must be 1. Since the write sequence seen by P2 is `A = 1 -> B = 1`. 

Sequential consistency can produce non-deterministic results. This is  because the sequence of sequential operations between processors can be different during different runs of the program. All memory operations need to happen in the program order.

#### Relaxed memory consistency models

Suppose  `A->B` means for one processor, the operation `A` is done before operation `B`. 

If `W->R` is violated, the order is **Total Store Ordering**. It is used by x86-64 architecture. 

If `W->W` is violated, the order is Partial Store Ordering. 

...

More memory models can be seen from

https://en.wikipedia.org/wiki/Consistency_model

#### Synchronized-with and happens-before

The **synchronized-with** relationship is something that you can get only between **suitably tagged** (the default is `memory_order_seq_cst`, which is a suitable tag) operations on **atomic types** (data structures like mutex contains these atomic types). If A writes on x and B reads on x, there is a synchronizes-with relationship between A and B. 

The **happens-before** relationship specifies which operations see the effects of which other operations. For a single thread, the happens-before relationship can be easily determined by the program order. For multi-threading, if operation A on one thread **inter-thread happens-before** operation B on another thread, then A happens-before B. The inter-thread happens-before relies on the synchronizes-with relationship. If operation A in one  thread synchronizes-with  operation B in another thread, then A inter-thread happens-before B. This relationship is transitive. 

These rules means if you make changes in one thread, you need only one synchronizes-with relationship for the data to be visible to subsequent operations on other threads.

### C++ Memory Order

C++ has 6 memory ordering options on atomic types. 

```
momory_order_relaxed,
memory_order_consume,
memory_order_acquire,
memory_order_release,
memory_order_acq_rel,
memory_order_seq_cst.
```

They represents 3 memory models:

```
Sequential Consistency (memory_order_seq_cst)
Relaxed (memory_order_relaxed)
Acquire-Release (memory_order_consume, memory_order_acquire, memory_order_release, memory_order_acq_rel)
```

For x86-64 architecture, the acquire-release ordering do not require additional instructions. Sequential consistent ordering  has small additional cost on store operations. But it will also influence the instruction reordering of compiler, so they all have potential cost except `memory_order_relaxed`.

In non-sequentially consistent memory orderings, **threads don’t have to agree on the order of events on atomic variables**. **In the absence of other ordering constraints, the only
requirement is that all threads agree on the modification order of each individual variable.**

#### std::memory_order_seq_cst

> If all operations on instances of atomic types are sequentially consistent, the behavior of a multithreaded program is as if all these operations were performed in some particular sequence by a single thread. This is by far the easiest memory ordering to understand, which is why it’s the default: all threads must see the same order of operations. ...  It also means that operations can’t be reordered; if your code has one operation before another in one thread, that ordering must be seen by all other threads.
>
> A sequentially consistent store **synchronizes-with** a sequentially consistent load of the same variable that reads the value stored. 
>
> -- <cite> C++ concurrency in action 2nd edition, P124  </cite>

#### std::memory_order_relaxed

> Operations on atomic types performed with relaxed ordering don’t participate in
> synchronizes-with relationships. Operations on the same variable within a single
> thread still obey happens-before relationships, but there’s almost no requirement on
> ordering relative to other threads. 
>
> --  <cite> C++ concurrency in action 2nd edition, P127  </cite>

```c++
# Processor 1
x.store(true, std::memory_order_relaxed);
y.store(true, std::memory_order_relaxed);

# Processor 2
while (!y.load(std::memory_order_relaxed));
if (x.load(std::memory_order_relaxed)) ++z;
```

Here z can be 0. Since there are no ordering guarantees relating to the visibility to different threads. 

The relaxed ordering gives a well-defined behavior in multi-threading compared to `volatile` keyword or only uses a normal variable. Since the sematic is **atomic**, the `fetch_add` method is also atomic, which means you can use it as a counter. In x86, the `fetch_add` method with `std::memory_order_relaxed` is implemented as `lock xadd`, which is same as using  `std::memory_order_seq_cst` (but the former one can be reordered by the compiler, and in other architectures, the implementation may be different). 

Problem may encounter in using relaxed ordering: OOTA

http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1217r2.html

#### std::memory_order_acquire && std::memory_order_release

std::memory_order_release only used in `store()`, std::memory_order_acquire only used in `load()`. The operation pair can form a **synchronizes-with** relationship. 

- Any writes or reads before `store()` should not be moved after it.
- Any writes or reads after `load()` should not be moved before it.

Typical usage: 

```c++
# Processor 1
data = 100;                                       // A
ready.store(true, std::memory_order_release);     // B

# Processor 2
while (!ready.load(std::memory_order_acquire))    // C
    ;
assert(data == 100); // never failed              // D
```

TODO: std::memory_order_consume 

#### Double-checking locking

```c++
if (!x_init.load(memory_order_acquire)) {
    lock_guard<mutex> _(x_init_mutex);
    if (!x_init.load(memory_order_relaxed)) { // <- Already hold the lock!
        initialize x;
        x_init.store(true, memory_order_release);
    }
}
```

#### Initial load for compare-exchange

```c++
unsigned long expected = x.load(memory_order_relaxed); 
// <- result does not affect correctness since the CAS will check again. 
while (!x.compare_exchange_weak(expected, f(expected))) {}
```



References: 

- [1] [C++ concurrency in action, 2nd edition](https://www.manning.com/books/c-plus-plus-concurrency-in-action-second-edition)
- [2] [理解 C++ 的 Memory Order](http://senlinzhan.github.io/2017/12/04/cpp-memory-order/)
- [3] [x86-TSO: A Rigorous and Usable Programmer’s Model for
  x86 Multiprocessors](https://www.cl.cam.ac.uk/~pes20/weakmemory/cacm.pdf)

- [4] [Linux kernel memory barriers document](https://www.kernel.org/doc/Documentation/memory-barriers.txt)

- [5] [A Relaxed Guide to memory_order_relaxed - Paul E. McKenney & Hans Boehm - CppCon 2020](https://www.youtube.com/watch?v=cWkUqK71DZ0)