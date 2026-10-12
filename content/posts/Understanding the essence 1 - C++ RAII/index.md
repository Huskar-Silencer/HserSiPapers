+++
date = '2026-10-13T06:42:59+08:00'
draft = false
title = 'Understanding the Essence 1 - C++ RAII'
+++

# Introduction
RAII stands for **Resource Acquisition Is Initialization**. It is not a special tool, but a programming philosophy used to improve memory management in languages without automatic garbage collection. Its core idea is to bind resource acquisition and release to the lifetime of an object: acquire the resource when the object is constructed, and release it when the object is destroyed. (The object here is usually a local stack object, whose lifetime is easy to determine.)

> My understanding of RAII: this coding style emphasizes, semantically, the ownership relationship between an object and a resource (this is even more explicit and precise in Rust). For example, if object A holds resource B, then B's lifetime is managed by its holder A, and other means should be avoided to interfere with B's lifetime. This design is very beneficial for writing C++, as it eliminates many memory issues caused by mismatched new and delete.


# Classic Uses of RAII
## Smart Pointers
Since C++11, smart pointers were introduced as an important feature, filling the gap that C++ lacked a standard reference-counting tool. When writing modern C++, you should prefer smart pointers over traditional raw pointers for dynamic memory allocation, because smart pointers automate allocation and deallocation, making code more readable and maintainable.

Under the hood, smart pointers are based on the RAII philosophy: the lifetime of a resource is managed by the smart pointer object, and users don't need to worry about it. When the number of holders drops to zero, the internal resource is automatically released.

```cpp
auto p = std::make_shared<int>(42);
auto q = p; // Reference count +1
```

## Automatic Lock Release
Automatic lock release is not a new mechanism, but a natural application of RAII in concurrent scenarios - locking on construction, unlocking on destruction, letting scope define the lifetime of the lock.

```cpp
#include <shared_mutex>

std::shared_mutex rwmtx;

{
    std::shared_lock lock(rwmtx);
}

{
    std::unique_lock lock(rwmtx);
}
```