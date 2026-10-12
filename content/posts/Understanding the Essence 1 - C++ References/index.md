+++
date = '2026-10-12T10:30:48+08:00'
draft = true
title = 'Understanding the Essence 1 - C++ References'
+++

## Introduction

In traditional C language, we often use pointers as function parameters to avoid copying or wanting to modify variables themselves rather than modifying copies. But as long as pointers are used, there will always be some classic development issues, such as resource request and release issues, null pointer access issues, and the need to use **->** access when using pointers to access structures. Some literature specifically points out the two methods of transmitting values and addresses, but in fact, addressing is essentially transmitting address values. I understand that addressing can also be classified as transmitting values.

C++introduced the concept of referencing, and I have read many tutorials that explain in layman's terms that referencing is setting an alias for a variable. This kind of narration may make many beginners understand referencing this thing as a renaming mechanism? In my opinion, without looking at the implementation of the underlying compiler, from a semantic perspective, using a reference emphasizes the use of the variable itself, while using a pointer emphasizes holding a handle to the variable (from a usage perspective, a reference does seem to give an alias to the original variable). In fact, mainstream compilers rely on pointers when processing references at the underlying level, but hide and encapsulate syntactic operations such as dereference.

So, references also have the issue of whether resources are valid. For example, you cannot return a local variable by reference because the local variable becomes invalid after the function call ends. Returning its reference will result in returning an invalid variable itself or an invalid memory address

![None](01.png)

In my opinion, C++ references are essentially a syntactic sugar of pointers. References and pointers are considered exactly the same thing by the underlying compiler, just to facilitate programmers to write safer and more readable code. Advance certain errors from runtime to compile time. The issues of null pointers, wild pointers, and forgetting initialization are directly excluded by language rules in the reference system. This is a significant improvement in security compared to C in C++

C++ references force programmers to bind to an existing object, and null references are not allowed in the code. Additionally, they are used in much the same way as regular variables, serving as a restricted and safer pointer

## Underlying

```cpp
void test01() {
    int a = 10;
    int* a_p = &a;
}

void test02() {
    int a = 10;
    int& a_r = a;
}
```

```assembly
# x86-64 GCC

"test01()":
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR [rbp-12], 10
        lea     rax, [rbp-12]
        mov     QWORD PTR [rbp-8], rax
        nop
        pop     rbp
        ret
"test02()":
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR [rbp-12], 10
        lea     rax, [rbp-12]
        mov     QWORD PTR [rbp-8], rax
        nop
        pop     rbp
        ret
```

You can see that the assembly implementation corresponding to references and pointers is actually the same