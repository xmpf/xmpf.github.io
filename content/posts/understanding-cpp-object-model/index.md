---
title: Understanding C++ Object Models
data: 15/06/2023
author: 0xishtar
description: Understanding C++ Object Models
draft: true
---

## Scope

### Local Object

Local objects are objects that are declared as local variables. Their scope (lifetime) is defined from the point of their declaration until the block exit. Space for these objects is reserved on the stack. The destructor is called automatically when the object goes out of scope.

### Dynamic Object

Dynamic objects are created via the `new` operator which acts [similar](https://en.cppreference.com/w/cpp/memory/new/operator_new) to `malloc` in C. The new operator takes the size of the object as
parameter, allocates memory of this size in the heap, and finally returns the address of this buffer. The returned address is then passed to the constructor as the `this` pointer.

```c
void* operator new(std::size_t sz)
{
    std::printf("new(size_t), size = %zu\n", sz);
    if (sz == 0)
        ++sz; // avoid std::malloc(0) which may return nullptr on success
 
    if (void *ptr = std::malloc(sz))
        return ptr;
 
    throw std::bad_alloc{}; // required by [new.delete.single]/3
}
```

The destructor has to be invoked explicitly via the delete operator.

```c
void operator delete[](void* ptr) noexcept
{
    std::puts("delete(void* ptr)");
    std::free(ptr);
}
```



## Exploring C++ Polymorphism

```cpp
#pragma pack(1)
#pragma align 1 (Class)
class Class {
public:
    Class() {
        this->field_a = 1337;
        this->field_d = 1337;
    }

    virtual int getField() {
        return this->field_b;
    };

protected:
    int field_a;
    int field_b;
    int field_c;
    int field_d;
};

class ClassB : public Class {
public:
    int getField() override {
        return this->field_e + 1;
    }

protected:
    int field_e;
};

int main() {
    ClassB *cls = new ClassB();
    delete cls;
}
```



```basic
Class::Class() [base object constructor]:
        push    rbp
        mov     rbp, rsp
        mov     QWORD PTR [rbp-8], rdi
        mov     edx, OFFSET FLAT:vtable for Class+16
        mov     rax, QWORD PTR [rbp-8]
        mov     QWORD PTR [rax], rdx
        mov     rax, QWORD PTR [rbp-8]
        mov     DWORD PTR [rax+8], 1337
        mov     rax, QWORD PTR [rbp-8]
        mov     DWORD PTR [rax+20], 1337
        nop
        pop     rbp
        ret
            

Class::getField():
        push    rbp
        mov     rbp, rsp
        mov     QWORD PTR [rbp-8], rdi
        mov     rax, QWORD PTR [rbp-8]
        mov     eax, DWORD PTR [rax+12]
        pop     rbp
        ret
                
                
ClassB::getField():
        push    rbp
        mov     rbp, rsp
        mov     QWORD PTR [rbp-8], rdi
        mov     rax, QWORD PTR [rbp-8]
        mov     eax, DWORD PTR [rax+24]
        add     eax, 1
        pop     rbp
        ret
                

ClassB::ClassB() [base object constructor]:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        mov     QWORD PTR [rbp-8], rdi
        mov     rax, QWORD PTR [rbp-8]
        mov     rdi, rax
        call    Class::Class() [base object constructor]
        mov     edx, OFFSET FLAT:vtable for ClassB+16
        mov     rax, QWORD PTR [rbp-8]
        mov     QWORD PTR [rax], rdx
        nop
        leave
        ret


main:
        push    rbp
        mov     rbp, rsp
        push    rbx
        sub     rsp, 24
        mov     edi, 28
        call    operator new(unsigned long)
        mov     rbx, rax
        mov     QWORD PTR [rbx], 0
        mov     DWORD PTR [rbx+8], 0
        mov     DWORD PTR [rbx+12], 0
        mov     DWORD PTR [rbx+16], 0
        mov     DWORD PTR [rbx+20], 0
        mov     DWORD PTR [rbx+24], 0
        mov     rdi, rbx
        call    ClassB::ClassB() [complete object constructor]
        mov     QWORD PTR [rbp-24], rbx
        mov     rax, QWORD PTR [rbp-24]
        test    rax, rax
        je      .L8
        mov     esi, 28
        mov     rdi, rax
        call    operator delete(void*, unsigned long)
.L8:
        mov     eax, 0
        mov     rbx, QWORD PTR [rbp-8]
        leave
        ret


vtable for ClassB:
        .quad   0
        .quad   typeinfo for ClassB
        .quad   ClassB::getField()
vtable for Class:
        .quad   0
        .quad   typeinfo for Class
        .quad   Class::getField()
typeinfo for ClassB:
        .quad   vtable for __cxxabiv1::__vmi_class_type_info+16
        .quad   typeinfo name for ClassB
        .long   0
        .long   1
        .quad   typeinfo for Class
        .quad   0
typeinfo name for ClassB:
        .string "6ClassB"
typeinfo for Class:
        .quad   vtable for __cxxabiv1::__class_type_info+16
        .quad   typeinfo name for Class
typeinfo name for Class:
        .string "5Class"
```

When a class contains virtual functions (inherits or defines), the compiler adds a virtual table (`vtable`) to the object's structure. The `vtable` is located in the first 8 bytes (`x86_64`) of the object structure in memory.

As the class definition contains a virtual method, the class `Class` representation in memory will look like this:

```
+---------------------+
|     vftable_ptr     |
+---------------------+
|       field_a        |
+---------------------+
|       field_b        |
+---------------------+
|       field_c        |
+---------------------+
|       field_e        |
+---------------------+
```

The representation for derived class `ClassB` is as follows:

```
+---------------------+
|     vftable_ptr     |
+---------------------+
|       field_a        |
+---------------------+
|       field_b        |
+---------------------+
|       field_c        |
+---------------------+
|       field_d        |
+---------------------+
|       field_e        | 
+---------------------+
```

The member variables of `ClassB` are simply appended after the base class `Class`.
Each base class that contains at least one virtual function will contain its own `vtable` pointer.

> ```c
> struct C {
>     int c1;
>     void cf();
> };
> 
> struct D : C {
>     int d1;
>     void df();
> };
> ```
>
> Within D, there is no requirement that C’s instance data precede D’s. But by laying D
> out this way, we ensure that the address of the C object within D corresponds to the
> address of the first byte of the D object. This layout is used by all known C++ implementations. 
> Source: http://www.openrce.org/articles/files/jangrayhood.pdf

Regarding member functions and `this` pointer:

> Each (non-static) member function of a class `C` receives a special hidden `this` parameter of type `C * const`, which is implicitly initialized from the object the member function is applied to.
> Source: http://www.openrce.org/articles/files/jangrayhood.pdf

The following pattern can be identified when calling member functions:

```assembly
mov     rdx, QWORD PTR [rax] ; member function
mov     rax, QWORD PTR [rbp-24] ; this ptr
mov     rdi, rax ; this ptr
call    rdx
```

The above is an example of dynamic dispatch due to the fact that the member function is declared as virtual.

### Run-Time Type  Information (RTTI)

> RTTI is available only for classes that are polymorphic, which means they have at least one virtual method. In C++, RTTI can be used to do safe typecasts, using the `dynamic_cast<>` operator.
> Source: https://en.wikipedia.org/wiki/Run-time_type_information

### Virtual Inheritance (Diamond Problem)

Source: https://en.wikipedia.org/wiki/Virtual_inheritance

```cpp
/*  
          D  
         / \  
        B   C  
         \ /  
          A 
*/

class A {};
class B : D {};
class C : D {};
class D : B, C {};
```

Since both `B` and `C` inherit from `A`, they each contain a copy of the `A` instance data. Unless something is done, each `D` will contain two instances of `A`, one from each base. The solution to this problem is Virtual Inheritance:

```cpp
class A {};
class B : virtual D {};
class C : virtual D {};
class D : B, C {};
```



## Disclaimer

I am not an expert. I am currently learning. If you spot any mistakes please reach out to me.

## References

- https://www.blackhat.com/presentations/bh-dc-07/Sabanal_Yason/Paper/bh-dc-07-Sabanal_Yason-WP.pdf
- http://www.openrce.org/articles/files/jangrayhood.pdf
- https://en.wikipedia.org/wiki/Virtual_inheritance
- https://youtu.be/85vGoUkYIeY
