---
title: References & Pointers
categories: [Computer Science, C++, References & Pointers]
tags: [syntax]     # TAG names should always be lowercase
---

The purpose of this post is to make a clear distinction between references and pointers. 

``` c++
int i; 
int *pi = &i; 
// the adress of pi is set as the adress of i 

int i; 
int &ri = i; 
// the adress of ri is set as the adress of i 

//to retrieve
cout << *pi 
cout << ri; // this is why functions that take in string& s can just directly use s within it

//to modify: 
*pi = 4; 
ri = 4; ri++; //(etc.)

//but pointers can increment to the next address (to another object)
pi++

//a pointer can point to many different objects
//a reference can only point to one object during its lifetime
(e.g.)
int a = 2;
int b = 4;
int &ref = a;
// ref can never refer to b by design; it can only refer to a
```
---

An iterator is a pointer (not a reference, otherwise it wouldn't be able to traverse) 
used to traverse from one element to another (in arrays / strings / etc). 

string::iterator is a type of iterator to traverse a string by pointing at each char 
so we can use *(string::iterator "var") to return a char, and if we increment the iterator, it will point to the next char. 

---

For instance, if for a function, the first taken parameter for a function is a string::iterator& it, the parameter is a reference to the string::iterator it. 

We can refer to the iterator just as it, as references by definition can be called in this way. Then, we can *(it++) to use it's functionality as an iterator/pointer for a char of the string
and then use again one of the properties of a pointer of being able to 
point to another object unlike a reference (by incrementing, in this case). 

> A simple function that takes in reference to a pointer 

```c++
// takes in reference to a pointer like the problem 
void decompress(int* &x) {
    cout << *x; // as x can be used as a pointer (by property of reference), we can return *x or i 
}

int i = 5;
int *x = &i;
decompress(x); //x is the adress of a pointer 
```