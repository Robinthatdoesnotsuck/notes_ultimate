# C++ A good modern language

## References

+ It is just that, a reference to a variable that already exists. To declare a variable reference you must use the type suffix of **&**
  + Example

  + ```C++
        int i = 20;
        int& r = i;

  + This r variable will give you the direction in memory of the i variable in an standard way(in hexadecimal)

+ Pass-by-value
  + This is general programming knowledge, this is not exclusive of C++
  + So when you call a function the parameters inside of it will be copies of the input ones, meaning if you modify anything on the input
    parameters, it will not show the changes of it
    + This is the code like you would imagine would swap to variables but it doesn't do that cause of what we explained previously

```C++
    void Swap(int a, int b) {
        int temp = a;
        a = b;
        b = temp;
    }
```

+ To change this we will pass it by reference, instead of just by value

```C++
    void Swap(int& a, int& b) {
        int temp = a;
        a = b;
        b = temp;
    }
```

+ But ] ], Why does this happens? cause as a default when you use a function in C++(and many more C-like languages) it will pass by value making a temporary copy of the variables that you use as parameters and the compiler will destroy this temp variables once it is done with executing the function
  + This leaves you with some work around
    + Since you are now passing by reference you need to use existing variables with a direction on memory, this means that your function now only works with variables instead of variables and values

+ If you are using a C-like language read the reference on how it defaults on the pass-by-x when you use functions cause depending on the language it will be pass-by-reference or by-value

## Pointers

+ THE BREAD AND BUTTER OF C++

+ What the hell is a pointer?
  + Nobody knows, bye

+ Useful know hows of pointers
  + When we declare a pointer
        ```
    int y = 100;
    int* p = &y;
        ```
    + We just store the direction on memory of that variable and well, how would anyone modify the value of a pointer variable?
          we will need to use the power of de-referencing the pointer to do this we would just use ```*p = 42;``` this is done to modify the value of the thing we are pointing to directly instead of been a silly billy to try and change an hexadecimal value on memory
  + Also unlike references pointers can point to well whatever the hell we want since these things are just memory directions

## Arrays

+ Arrays you know the drill
  + To initialize an array is simplier than you think

    + ```C++
        int a[10];
        a[0] = 50;

  + To initialize it with values you can use the {} notation

    + ```C++
        int fib[5] = {0, 1, 1, 2, 3};

  + Also if we want to access elments of an array via pointers c++ has something for us, the elements of an array are store contiguously

## Dynamic Memory allocation

+ BIG BUT **)  )** YOU HAVE TO DELETE THE VARIABLES THAT YOU DYNAMICALLY ALOCATE, CAUSE IF YOU DON'T YOU'LL CAUSE A MEMORY LEAK
+ This is the dread of using this god forsaken/given thing of c++

+ But like why do we have this?
  + In like programming and such, we have a limited space where we can store all of our shit this is the **|STACK|** and that memory space it's very limited, like ridicously limited(also depends on the compiler in usage, so there's that)
  + So we will use something called the **|HEAP|** so that we can have full control of the allocation and deallocation of variables in memory
  + All Dynamic allocations go into the heap which helps us cause well, heaps are like gigabyte in size
    + Most data in the heap persist either if the programmer destroys or repurposes that data or the program just blatantly ends

+ But like how do we do this?
  + In c++ we have various operators to allocate and deallocate memory
    + New
      + using the operator new is well the way to allocate memory
            ```
        char* dynamicChar = new char;
        char* dynArray = new char[4*4];
            ```
    + Delete
      + the operator delete is the way to deallocate memory
            ```
        delete dynamicChar;
        // For array
        delete[] dynArray;
            ```

## Classes and marxism

+ We already talked about how references lets us modify a variable with functions, but the problem comes when it lets us change a class or a parameter in a class
  + Why?
    + Cause it would create some inconsistency if we don't remember how a certain class is structured in its functionality or simple data inconsistency if we modify a data point at some point in time
  + What to do?
    + Const references
  + What do they do?
    + If you know RUST programming (check rust section ;^) ) it would be like the borrowing system but not automatic and way way worse
    + If you don't is just a way to tell the compiler to make a function to make the variable that we use as a parameter to be read only
    + This const references can be done at function level or parameter level

        ```C++
        bool Intersect(const Circle& a, Circle& b);

        float getRadius() const { return mRadius}
        ```

+ Also we can dynamically allocate classes
  + How
    + With the use of pointers silly billy
    + If you created a class like this you would need to use the -> operator to access its public methods
            ```
    Complex* c = new Complex(1.0f, 2.0f);
            c->Negate();
            ```
  + The syntax depend on how you structured your class internally
    + So remember to know what constructor to use when allocating new objects
  + This helps us in creating arrays
    + BUT )  )
      + You'll have to program a default zero parameter constructor to your class, so that the array can be initialize correctly

+ Destructors
  + It is used as the default delete for classes
  + This means if you use a class that you are gonna allocate dynamically in memory you got to have a destructor in that bad boy
    + The syntax is that, it takes no params and uses a ~ behind it

        ```C++
        Complex::~Complex()
        {
            delete complex;
        }
        ```

+ Copy constructors
  + It is a constructor provided by the compiler if not declared that lets you copy classes to another object of the same type
  + ANOTHER MASSIVE BUTT )++)
    + This causes a problem cause if we are using dynamic allocated classes we are just practically saving a direction in memory sooooo
    + If you copy something with dynamic memory you would only copy the directions in memory in another variable instead of the content
      + This is called Shallow copy
  + So the nuisance of using dynamic memory is that you have to create your own copy constructor so that this doesn't happen

## Operator Overloading

+ It is a way for classes to specify or modify the behavior of built-in operators, this is done to add custom functionality to it
+ For this you will have to use the friend thingy

    ```C++
    friend Complex operator+(const Complex& a, const Complex& b)
    {
        return Complex(a.mReal + b.mReal,
                       a.mImaginary + b.mImaginary);
    }
    ```

  + Here the + operator is overridden with what we programmed, this new + will create a new Complex object that is the sums of the real and imaginary parts of two other objects
  + You can also override boolean operators like ==, !=, etc.

    + ```C++
        friend bool operator==(const Complex&a, const Complex& b)
        {
            return (a.mReal == b.mReal) && (a.mImaginary == b.mImaginary);
        }

+ But also you can override even the ```=``` that it is called the assignment operator but you will need to implement it correctly

    ```C++
    Complex a(12);
    Complex b(14);
    a = b;

    Complex& operator=(const Complex& other) {
        delete mImaginary;
        delete mReal;
        mImaginary = new int;
        mReal = new int;
        *mReal = other.mReal
        *mImaginary = other.mImaginary

        // This is done by convention
        return *this;
    }
    ```

  + At the end we use this cause the c++ compiler already know that this is the object that it is been modified
  + Also note that the = operator does not need the friend thingy cause it is technically already a member function by default in classes

+ These are the most common stuff to override cause, C++ lets you override everything, but like it is easy to fuck up your code if you override everything that casts a shadow in front of you.

## THE RULE OF THREE

+ If a class has the necesity to implement either a copy constructor, a destructor to free memory or an assignment operator, you should always implement the three.
prefix

## Collections
