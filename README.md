# ICS 45C Homework 7

Templates and exceptions with an Array and a Matrix

## Getting the assignment

1. Accept the assignment by clicking the link in Canvas
2. Once you accept the invite, you will reach a page that says "You're ready to go"
3. Click the URL from that page that looks similar to this: `https://github.com/klefstad-teaching/ics-45c-hw7-<YourGitHubUsername>`. It may take a bit of time for the repo to be ready.
4. Click the green `<> Code` Button on the top right, and then click the middle tab `SSH`
5. Copy that link
6. Go to your hub and go into the ICS45C folder, and open up the terminal and type in the following command:
```bash
git clone git@github.com:klefstad-teaching/ics-45c-hw7-<YourGitHubUserName>.git HW7
```
7. Go to VSCode and open up the `HW7` Folder
   
## Directory Structure

```bash
├── CMakeLists.txt
├── CMakePresets.json
├── gtest
│   ├── array_gtests.cpp
│   ├── int_array_gtests.cpp
│   ├── gtestmain.cpp
│   └── matrix_gtests.cpp
└── src
    ├── alloc.cpp
    ├── alloc.hpp
    ├── array.hpp
    ├── int_array.hpp
    ├── matrix.hpp
    └── standard_main.cpp
```

## Overview and Objectives

Previous Homeworks 5 and 6 have implemented linked lists, first for type `char`, then for type `Shape pointer`. But what if the same linked list functionality is needed for any other type? Because C++ requires strong typing for the purpose of efficiency, these type-specific implementations of linked lists cannot be used for any other type.

Instead, to generalize functionality across different types, **templates** must be used. Templates define generic abstractions that can be ***instantiated*** with specific types appropriate for the problem at hand, where the template parameter would be the type of the data. So a `List` of a parameterized type `T` could be instantiated with `T` as `char` for Homework 5, then instantiated with `T` as `Shape` pointer for Homework 6. All linked list functionality, such as finding the length, reversing, or traversing a list, could thus be done for any type.

For Homework 7, the ultimate goal is to write a fully tested **template** for both an `Array` of type `T` and for a `Matrix` of type `T` with the template functions needed to fill each one with a value returned by a ***lambda*** function, and then to instantiate both `Array` and `Matrix` template classes with `ints`, `doubles`, and `chars`. You first complete the provided class `Array` of type `int`,  then use that to define an `Array` template class (and also convert a function to a template). Then use the `Array` template to develop a 2D class `Matrix`, which will be a template as well. Of course, all functionality must be well tested along the development process.

We work in these stages of development to illustrate, and help you learn, the process of breaking down an ultimate goal, a bigger problem, into smaller subproblems that can be thoroughly tested, then their functionality extended. Our ultimate goal is a template class for a 2D `Matrix`. So, we start with the simplest subset of that goal, the 1D `Array` of type `int`. We fully test it, get it fully working. Now, a matrix is an array of arrays; but we cannot make an `Array` of `Arrays` unless our `Array` is a template. So we extend class `Array` to be a template, and test it on different element types to ensure our template is fully working. Finally, we extend the `Array` to handle 2 dimensions for our `Matrix`.

Homework 7 also requires ***exceptions*** to be thrown and caught, whenever any index is out of bounds, for the operator []() for both classes `Array` and `Matrix`. You will also see that functions can be marked as ***noexcept*** to explicitly forbid throwing exceptions out of them, which is [strongly recommended](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#c66-make-move-operations-noexcept) for move operations and swaps.

Lastly, ***lambdas*** are defined and used, which are anonymous local functions that can be passed in as parameters to functions that will apply them to each element or apply them to initialize elements of the matrix. They are able to capture local variables either by reference or by copy (value), similar to how parameters may be passed by reference or by copy (value).

Steps of development are given to convert from type-specialized classes to templated classes that work, in a series of small steps to reliably keep everything working, without generating an overwhelming number of error messages from the compiler.

### Relation to Course Objectives

Homework 7 provides practice implementing template classes, exceptions, and lambdas, with additional practice in memory management for arrays.

## Organization of Classes and Files

> Be sure to use **exactly** the names given in these instructions and this repo for **files**, **functions**, and **classes**, because the autograder will be expecting exactly those names.

## 1 Class Array of int

In the file `int_array.hpp`, complete the `Array` of type `int` provided in the screenshot below. Use the tests in `int_array_gtests` to make sure everything is working correctly.

Note that previously, we separated the declaration and definition of functions / methods into a `.hpp` file and a `.cpp` file. Here, you will instead implement all functions and methods related to the templated Array  in the header (see the various provided examples in the screenshot). Many compilers need templates to be implemented this way, so you will have the opportunity to learn some special rules about writing definitions in headers:

1. Non-template functions which are defined outside of a class (like **`operator<<`** and **`operator>>`** below) must be marked with the `inline` keyword, or else you will get linker errors. Functions inside of a class as well as templated functions are always considered `inline`.
2. Templated functions and classes *must* be defined in headers.
3. You should not do **`using namespace`** inside of a header file, because every file which includes this header is forced to use that namespace as well. So instead, use fully qualified names like `std::stringstream` or `std::ostream`.

Modify the `Array` class **`operator[]()`** to throw an exception of type `std::string` if any index is out of bounds, containing the following error message:

> `Exception operator[](10) Out Of Range` (but replace `10` with the value of the index that caused the exception)

**Be sure to follow the [Steps of Incremental Development](#steps-of-incremental-development). Some relevant details may be provided only in the Steps.**

int_array.hpp screenshot

![int_array.hpp screenshot](/assets/1-1.png)

## 2 Template class Array of T

In a single file named `array.hpp`, convert the working class `Array` of type `int` implemented in `int_array.hpp` into a new class, where the element type is a template parameter. Namely, instead of holding exclusively `ints`, class `Array` will become a templated class able to hold a variety of objects, based on the template parameter. Use `array_gtests.cpp` to test your work.

Define a template method `fill_with_fn()` which takes a lambda and uses that lambda to initialize every entry of the array.

**Be sure to follow the [Steps of Incremental Development](#steps-of-incremental-development). Some relevant details may be provided only in the Steps.**

array.hpp screenshot

![array.hpp screenshot](/assets/2-1.png)

## 3 Template class Matrix of T

Use the **template class `Array`** (i.e., ***after*** `Array` has been converted to be a template class) to implement a template class `Matrix` of `T`, that behaves like a two-dimensional array, in a single file `matrix.hpp`. Use `matrix_gtests.cp` to test your work.

Define a template method `fill_with_fn()` that will fill any `Matrix` with values returned by a parameter lambda function.

Notice that `Matrix` does not define any copy/move operations or a destructor. It follows the [rule of zero](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#c20-if-you-can-avoid-defining-default-operations-do): If all data members of your class (`Array` and `int` in this case) already implement copy/move/destruct operations, then you don't need to do so for your class. Proper ones will be defined by the compiler by default.

**Be sure to follow the [Steps of Incremental Development](#steps-of-incremental-development). Some relevant details may be provided only in the Steps.**

screenshot of matrix.hpp

![screenshot of matrix.hpp](/assets/3-1.png)

## 4 standard_main.cpp

Once you have a working implementation of `Matrix<T>` and `Array<T>`, you should be able to run the provided `standard_main` program shown below. It gives you some more examples of how templates can be used by implementing a simple `Matrix * vector` multiplication and a helper `ask_for` that simplifies input.

![screenshop of main.cpp](/assets/4-1.png)

## 5 Testing

There are three provided files for tests:

 - `int_array_gtests.cpp`,
 - `array_gtests.cpp`, and 
 - `matrix_gtests.cpp`.

Start by running only `int_array_gtests`, which tests your implementation of `int_array.hpp`. The later tests will not compile until you have implemented more of `int_array.hpp`, which is why they are commented out.

The `Subscript` test will initially fail, because you are not yet throwing any exception on an out-of-bounds access. Note that you can check whether an operation throws an exception in GTest via `EXPECT_THROW_ANY(...)` (it is also possible to check whether a specific exception was thrown).

`array_gtests.cpp` and `matrix_gtests.cpp` are more barebones, and you will have to write some tests yourself to check your templated `array.hpp` and `matrix.hpp`. As usual, the autograder will have its own tests. **Make sure to test your `Array` and `Matrix` with more than just `int` as the element type!**

**Be sure to follow the [Steps of Incremental Development](#steps-of-incremental-development). Some relevant details may be provided only in the Steps.**

## Steps of Incremental Development

![code comic](/assets/5-1.png)

Small steps of incremental development and testing help to isolate and limit the number of possible issues that arise! **Taking too large a leap (with Big Bang development) leads to an overwhelming maze of issues that may be extremely difficult to diagnose and debug!**

>❗Note:  If asking for help with your program on Ed Discussion, give the Step you are working on in the Subject line of the post.

### Complete class Array of int

1. Start by working only on `int_array.hpp` and `int_array_gtests.cpp`. Parts of `Array` are already implemented, and many tests are provided.

2. Start by implementing `fill`, which should set all elements of the array to a given value. Uncomment the `Fill` test to check that it works.

3. Add the exception on out-of-bounds accesses to the array. This step will make the `OutOfBounds` test pass, which uses `EXPECT_ANY_THROW` to check for this.

4. Implement the copy and move constructors of `Array` and uncomment the relevant tests. At this point, you may also want to implement the destructor, to make sure you do not cause double-deletes.

5. Implement copy and move assignment and uncomment the relevant tests. Remember that you can use `swap` to implement these operations more easily and correctly.
   
### Convert class Array of int to be a template of any type T

6. Convert class `Array` of `int` to be a template of any type `T`. This means copy/transfering your code from `int_array.hpp` to `array.hpp` while carefully checking where you need to replace `int` with `T`.

7. Do not try to do everything at once! Copy/transfer a few functions at a time, and test that your code still compiles and works in `array_gtests.cpp`. For example, just the basic constructors and length is enough to write a small test. Make sure your code works with not just `int`s; write tests for `Array<double>` or even `Array<std::string>`.

8. Implement the templated method `fill_with_fn` to set every element of the array at once with a given lambda. Entry `i` of the array should be set to `fn(i)`. Write a test for this!

### Write Matrix to be a 2D matrix of any type T

9. Develop class `Matrix` in `matrix.hpp` to be a 2D matrix of `T`, implementing `Matrix` as an `Array` of `Array<T>`. This time, we will start directly with a templated class.

10. You can start with just the constructor and `num_rows / num_cols` to write a simple test analogous to the length test of the array. This functionality ensures that everything compiles at least.

11. Next implement **`operator[]`**. Note that **`operator[]`** should still throw an exception on out-of-bounds access, but since `Array` already does this, you do not need to repeat this in `Matrix`!

12. Implement `fill` to set every entry of the matrix to some given value.

13. Implement `fill_with_fn`: the entry in row `i` and column `j` should be set to `fn(i, j)`. Make sure to test this! It is easy to mix up `j` and `i` or accidentally go out of bounds.

14. Implement the inserter, **`operator<<`**, which should print each row of the matrix in a separate line, and the extractor, **`operator>>`**, which reads in a matrix (assuming every row is in its own line and every entry is separated by a space).

### Run standard_main to see your code working

15. You should now have fully working templated implementations of `Matrix` and` Array`. You can try them out with the provided `standard_main` that uses your classes to compute a `matrix * vector` product for both `int`s and `double`s.

16. Make sure `standard_main` and all of your GTests run without errors and without memory leaks. Use the sanitizers and/or valgrind to check for memory errors.

### Finish

17. Once you're confident that your code works, submit it to the autograder.
    

## Build Instructions

```bash
# Produce the `build` folder with the presets provided for the homework:
cmake --preset default

# Build all targets at once:
cmake --build build

# Build only standard_main.cpp:
cmake --build build --target standard_main

# Build int_array gtests:
cmake --build build --target int_array_gtests

# Build array gtests:
cmake --build build --target array_gtests

# Build matrix gtests:
cmake --build build --target matrix_gtests
```

To run the above targets after compiling them:

```bash
./build/standard_main       # Runs the 'main' function from src/standard_main.cpp
./build/int_array_gtests    # Runs the 'int_array' gtests
./build/array_gtests        # Runs the 'array' gtests
./build/matrix_gtests       # Runs the 'matrix' gtests
```

NOTE : If you want to also use valgrind to check your code, use these instructions:

```bash
# Produce the `build_valgrind` folder with the presets provided for the homework:
cmake --preset valgrind

# Build all targets at once:
cmake --build build_valgrind

# Build only standard_main.cpp:
cmake --build build_valgrind --target standard_main

# Build array gtests:
cmake --build build_valgrind --target array_gtests

# Build matrix gtests:
cmake --build build_valgrind --target matrix_gtests
```

Then run the above targets with:

```bash
./build_valgrind/standard_main        # Runs the 'main' function from src/standard_main.cpp
./build_valgrind/array_gtests         # Runs the 'array' gtests
./build_valgrind/matrix_gtests       # Runs the 'matrix' gtests
```

Once you have run the code above and it either produces the output you expected or passes
all provided tests, congratulations! You are now ready to [submit](#submission) your homework!

## Submission

As with previous submissions, submit via `GitHub` by the following steps:

1. `git add .`
2. `git commit -a -m "Commit Message Here"`
3. `git push --set-upstream origin main`

to push your changes to your private GitHub repository, and then submit `hw7` to `Gradescope`.

> ⚠️ Be sure your #includes and using match exactly what is specified, for the autograder to run your submission with our own test programs.

### Grading criteria

Points are allotted for
- Preventing memory leaks. Major points are deducted for mismatched `new` and `delete` and other memory errors.
- Passing tests of functional correctness without crashing.
- Printing `Array` and `Matrix` in the proper format.
- Correctness of constructors (including copy constructor) and destructors.

*We may adjust grades manually when warranted. For example, a submission that attempts to defraud the autograder points by not implementing the requirements may be given a 0.*


## Credit

All code moved from [ICS45c](https://github.com/RayKlefstad/ICS45c/tree/hw7)
