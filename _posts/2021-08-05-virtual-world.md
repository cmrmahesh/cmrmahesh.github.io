---
title: Virtual World in C++
tags: c++, prep, ntm
category:
  - code
---
The need:

1. Virtual is concept heavily used in inheritence.

```cpp
class B {
   public:
      void s() {
         cout<<"In Base \n";
      }
};
class D:  public B {
   public:
      void s() {
         cout<<"In Derived \n";
      }
};
int main(void) {
    B *b = new B();
    b->s();
    
   D d; // An object of class D
   b= &d; 
   b->s(); 
   return 0;
}
```

See this

Output:

```cpp
In Base
In Base
```

Which is against our expectations, since `b` and `d` are of same type,  and b is pointing to `d` object, we expect base pointer to call derived function. This does not happen because, c++ does a static link of calls.
`virtual` is used to make this link dynamic or set at run time.

```cpp
class B {
   public:
      virtual void s() {
         cout<<"In Base \n";
      }
};
class D:  public B {
   public:
      void s() override{
         cout<<"In Derived \n";
      }
};
int main(void) {
    B *b = new B();
    b->s();
    
   D d; 
   b= &d; 
   b->s(); 
   return 0;
}
```

Output:

```cpp
In Base 
In Derived
```

As seen adding virtual here makes a big difference. 

`override` is a good way to represent that we are overriding base class function. It is optional but improves readability.

This run time binding happens via virtual pointer and vtable concepts explained at the end.

1. All virtual functions need not be overridden. If there is no overridden function it will call base class function, which is the expected behavior.

Pure virtual classes and abstract class:

Abstract class: You wont be able to create an object for an abstract class. You can only inherit from it. 
C++ allows abstract classes through pure virtual functions.

```cpp
class B {
   public:
    virtual void s() = 0;
};

class D: public B{
	public:
		void s() override{
			//do stuff
		}
}
```

 It is compulsory for all derived classes to override this pure virtual class, failing to do so will lead to a compilation error. 

## vptr and vtable

1. A virtual pointer called `vptr` and virtual table called `vtable` are created for any class that has a virtual function in it **and** any class that derives from it. 
2. `vptr` is pointer to `vtable`, maintained *per object* instance. It  is added at compile time and is invisible to the user.
3. `vtable`  static array containing function pointers to virtual functions, maintained *per class*.
4. `vtable` has only functions that are defined as virtual.

Looking at the internals.

```cpp
class BBB {
    public:
    virtual void func(){
        cout << "hey from base" << endl;
    }
    void attack(){

    }
};

class DDD: public BBB{
    public:
    void func(){
        cout << "hey from derived" << endl;
    }
};
```

 compile with `g++ -fdump-lang-class <file_name.cpp>` will generate an addtional `<>.class`  file. Searching this file will give vtable and vptrs.

```cpp
Vtable for BBB
BBB::_ZTV3BBB: 3 entries
0     (int (*)(...))0
8     (int (*)(...))(& _ZTI3BBB)
16    (int (*)(...))BBB::func

Class BBB
   size=8 align=8
   base size=8 base align=8
BBB (0x0x7fb9a40adea0) 0 nearly-empty
    vptr=((& BBB::_ZTV3BBB) + 16)
--------------------------------------------------
Vtable for DDD
DDD::_ZTV3DDD: 3 entries
0     (int (*)(...))0
8     (int (*)(...))(& _ZTI3DDD)
16    (int (*)(...))DDD::func

Class DDD
   size=8 align=8
   base size=8 base align=8
DDD (0x0x7fb9a40e7068) 0 nearly-empty
    vptr=((& DDD::_ZTV3DDD) + 16)
  BBB (0x0x7fb9a40e81e0) 0 nearly-empty
      primary-for DDD (0x0x7fb9a40e7068)
```

As shown a `vtable` is present for any base class that has a virtual function and all derived classes from this base class. 

```cpp
BBB basePtr1, basePtr2;
DDD derivedPtr1, derivedPtr2;
//each object of Base and Derived class has an instance of vptr and points to its respective class virtual tables

BBB *bptr = new DDD();
//Here vtr of base class points to Derived class virtual table
bptr->func();
//since vptr points to derived calls vtable, func of derived class is called here.
```

Further reading from [thispointer](https://thispointer.com/how-virtual-functions-works-internally-using-vtable-and-vpointer/) .

Bonus Readings:

1. Size of class with virtual functions is bigger becuase `vptr` code is added at compile time.[see [here](https://www.geeksforgeeks.org/c-virtual-functions-question-12/)]
2. Never call virtual functions during construction or destruction. [Read [here](https://www.artima.com/articles/never-call-virtual-functions-during-construction-or-destruction) and [here](https://www.opensourceforu.com/2011/08/joy-of-programming-calling-virtual-functions-from-constructors/)]
3. GfG [Quiz](https://www.geeksforgeeks.org/c-plus-plus-gq/virtual-functions-gq/).

## Virtual in Multiple Inheritence setting

<to be continued>