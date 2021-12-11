<span style="color: rgb(230, 51, 0); display: block; text-align: center; font-size:35px"> *Ampersand and Const Uses* </span> <br>
<span style="color: rgb(0,105,0); font-size: 30px;">1. *Ampersand Operator Uses*</span>  <br>

   1. <mark>Logical AND</mark>: The expression (condition1 && condition2) returns true *only* if both statements are true. <br>

**Example:**
```c++
#include <iostream>
using namespace std;
int main() {
bool result;
int x;
cin>>x;
if(x>0 && x%2==0)
result =true;
else result = false;
cout<<result;
return 0;
}
```

   2. <mark> Address Operator</mark>: The address operator assigns a pointer to its operand. The operand must be an lvalue (when you have a pointer holdign a mutable address that is dereferenced on the left side of the assignment operator), a function designator (the expression we write when calling a function; a function designator when present in an expression normally decays into a pointer to the function), or a qualified name (a name with a class-name prefix, or an object name and a dot operator, or a pointer to an object and an arrow operator) <br>
**Example 1 (lvalue)**:
``` c++
int x1;
int &x2 = x1;
```
**Example 2(function designator)**:
```c++
#include <iostream>
using namespace std;
int square( int *n ) {
   return (*n) * (*n);
}

int main() {
   int num = 5;
   cout << square( &num ) << endl;  
   ```
**Example 3(qualified name)**:
``` c++
MyClass::f();
x.f();
p->f();
```
3. <mark> Reference Declarator</mark>: If you use & in the LHS of a variable declaration, it means that you should have a reference to the declared type.
**Example:**
``` c++
int target;
int &rtarget = target; //rtarget is a reference to an integer. The reference is initialized to refer to target.
void fun(int*& p) //p is a reference to a pointer
```
4. <mark> Bitwise AND Operator</mark>: An infix operator where it converts two or more decimal numbers to binary then performs logical AND on the bits of the numbers. It returns the decimal equivalent of the resulting binary number.<br>
**Example:**
``` c++
#include <iostream>
using namespace std;
int main () {
    int x=25&15;
    cout<<x;
    return0;
}
```
*The Bitwise Operation is done as following:* <br>

number     |  16  |  8  |  4  |  2  |  1
------|-----|------|-----|-----|-----
25  |  1  |  1  |  0  |  0 |   1  
15  |  0  |  1  |  1  |  1  |  1  
25&12|  0  |  1  |  0  | 0   | 1
**So the output is 9**
5. <mark>Declaring Rvalue References</mark>: An rvalue is an expression that does not represent an object occupying some identifiable location in memory except for some temporary register while the program is running. <br>
**Example:**
``` c++
#include <iostream>
#include <string>
using namespace std;
 
int main() {
    string s1 = "Hello";
//  string&& r1 = s1;           // error: can't bind to lvalue
 
    const string& r2 = s1 + s1; // okay: lvalue reference to const extends lifetime
 
    string&& r3 = s1 + s1;      // okay: rvalue reference extends lifetime
    r3 += "Hello";                
    cout << r3 <<endl;
}
```
6. <mark> Declaring Universal References</mark>: If a variable or parameter is declared to have type T&& for some deduced type T, that variable or parameter is a universal reference. <br>
**Example 1:**
```c++
Vehicle car;
auto&& car2 = car; // type deduction occurs, this is a universal reference
Vehicle&& car3 = car; // no type deduction, this is an rvalue reference
```
**Example 2:**
```c++
template<typename T>
void f(vector<T>&& param);     // rvalue reference

template<typename T>
void f(T&& param); // universal reference

```
7. <mark>Function Overloading</mark>: You can limit the use of a member function based on whether *this is a lvalue or an rvalue. So this is only helpful within classes.<br>
**Example:**
```c++
class Tool {
public:
  void fun() &; // used when *this is a lvalue
  void fun() &&; // used when *this is a rvalue
};

Tool makeTool(); //a factory function returning an rvalue

Tool t; // t is an lvalue

t.fun(); // Tool::fun & is called

makeTool().fun(); // Tool::fun && is called
```
<span style="color: rgb(0,105,0); font-size: 30px;">2. *Const Keyword Uses*</span>  

1. <mark>Documentation and Safety</mark>: The primary purpose of const is to provide documentation and prevent programming mistakes. Const allows you to make it clear to yourself and others that something should not be changed.<br>
**Example:**
``` c++
const float pi=3.1459;
const float g=9.81;
```
2. <mark>Const Pointers</mark>: To differentiate between const int*, const int * const, and int const *, etc.. we read it backwards (from right to left). <br>
**Example:**
```c++
int a = 5, b = 10, c = 15;

const int* foo;     // pointer to constant int.
foo = &a;           // assignment to where foo points to.

*foo = 6;           // the value of a can´t get changed through the pointer.
foo = &b;           // the pointer foo can be changed.

int *const bar = &c;  /* constant pointer to int note, you actually need   to set the pointer here because you can't change it later*/

*bar = 16;            // the value of c can be changed through the pointer.    
bar = &a;             // not possible because bar is a constant pointer.    
```
3. <mark>Const Functions</mark>: A const member function can be called by any type of object. Non-const functions can be called by non-const objects only.<br>
**Example:**
```c++
#include<iostream>
using namespace std;
class Demo {
   int val;
   public:
   Demo(int x = 0) {
      val = x;
   }
   int getValue() const {
      return val;
   }
};
int main() {
   const Demo d(28);
   Demo d1(8);
   cout << "The value using object d : " << d.getValue();
   cout << "\nThe value using object d1 : " << d1.getValue();
   return 0;
   ```

   4. <mark>Const Overloading</mark>: A class can have two member functions with identical signatures except that one is const and the other is not. If you have a class like this, then the compiler will decide which function to call based on the object you call it on: if it's a const instance of the class, the const version of the function will be called; if the object isn't const, the other version will be called. <br>
   **Example:**

 ```c++
#include<iostream>
using namespace std;
  
class Test
{
   protected:
     int num1;
   public:
     Test (int i) { }
     void fun() const
     {
         cout << "fun() const called " << endl;
     }
     void fun()
     {
         cout << "fun() called " << endl;
     }
};
  
int main()
{
    Test t1 (10);
    const Test t2 (20);
    t1.fun();
    t2.fun();
    return 0;
}
```
5. <mark>Const Iterators</mark>:They're just like normal iterators, except that they cannot be used to modify the underlying data.<br>
**Example:**
```c++
#include <iostream>
#include <vector>
using namespace std;
int main(){
vector<int> vec;
vec.push_back( 3 );
vec.push_back( 4 );
vec.push_back( 8 );
 
for ( vector<int>::const_iterator itr = vec.begin(), end = vec.end(); 
      itr != end;
      ++itr )
{
        cout<< *itr <<endl;
}
}
```
6. <mark> Const Cast</mark>: const_cast can be used to pass const data to a function that doesn’t receive const.<br>
**Example:**
```c++

#include <iostream>
using namespace std;
  
int fun(int* ptr)
{
    return (*ptr + 10);
}
  
int main(void)
{
    const int val = 10;
    const int *ptr = &val;
    int *ptr1 = const_cast <int *>(ptr);
    cout << fun(ptr1);
    return 0;
}
```
