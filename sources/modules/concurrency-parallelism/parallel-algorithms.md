## Module name: Parallel algorithms

_Skeleton descriptions are typeset in italic text,_
_so please don't remove these descriptions when editing the topic._

### Overview

_Provides a short natural language abstract of the module’s contents._
_Specifies the different levels of teaching._

------------------------------------------------------------------------
Level             Objective
----------------- ------------------------------------------------------
Foundational      Knowledge about build systems

Main              --- Usage of build system to compile a executable

Advanced          --- Add external libraries as a dependencies  

------------------------------------------------------------------------

### Motivation

_Why is this important?_
_Why do we want to learn/teach this topic?_

* Allows to execute *most* of the algorithms in the C++ standard in parallel
* Parallel algorithms are one form of parallelism included in the C++ standard

### Topic introduction

_Very brief introduction to the topic._

Algorithms that range over a large data set, can be accelerated by parallel execution. 
Execution policies allow execution of the algorithms in the C++ standard on multiple cores or a single core. 

### Foundational: Knowledge about parallel execution

#### Background/Required Knowledge

A student: 
* Should know [lambdas](../functions/lambdas.md)
* Should know [algorithms](../program-design/algorithms.md)
* Should know [iterators](../program-design/iterators.md)
* Should know [containers](../program-design/containers.md)

#### Student outcomes

_A list of things "a student should be able to" after the curriculum._
_The next word should be an action word and testable in an exam._
_Max 5 items._

A student should be able to:

1. Explain the difference between parallel and sequential execution
2. Explain race conditions in parallel execution

#### Caveats

_This section mentions subtle points to understand, like anything resulting in
implementation-defined, unspecified, or undefined behavior._

1. The programmer has to make sure that the algorithm is theoretical parallel. For example, specifying parallel execution using state-full lambdas might give wrong results.
2. The programmer has to make sure that the parallel execution does not lead to race conditions. 

#### Points to cover

_This section lists important details for each point._

* The C++ 17 standard is required
* There is an optional first argument make it parallel. The default is serial execution
* Mention `std::atomic` or `std::mutex` to avoid race conditions

### Main: Knowledge about parallel algorithms

#### Background/Required Knowledge

* All of the above.

#### Student outcomes

A student should be able to:

1. Do define a function or lambda for the compute kernel 
2. Split the work in independent tasks to avoid race conditions
3. Explain the semantics of the four execution policies (`std::execution::seq`, `std::execution::par`, `std::execution::par_unseq`, and `std::execution::unseq`) 
4. Specify the appropriate execution policy for sequential or parallel execution 

#### Caveats

* The concept of parallel programming introduces bugs via race conditions
* If the implementation cannot parallelize or vectorize (e.g. due to lack of resources), all standard execution policies can fall back to sequential execution. 

#### Points to cover

* The header `<execution>` needs to be included
* If the implementation cannot parallelize or vectorize (e.g. due to lack of resources), all standard execution policies can fall back to sequential execution. 

Example using a function
```
std::vector<double> values = {1,2,3,4,5,6};
void square(double& a)
{
    a =  a * a;
}

// Parallel execution
std::for_each(std::execution::par_unseq,std::begin(values),std::end(values),square);

// Serial execution
std::for_each(std::execution::seq,std::begin(values),std::end(values),square);
```

Example using a predefined algorithm
```
std::vector<int> values(10000);

// Seed the random number generator
std::random_device rd;
std::mt19937 gen(rd());

// Define the range for the random numbers
std::uniform_int_distribution<> distrib(1, 100); // Generates numbers between 1 and 100

// Fill the vector with random numbers
std::generate(random_vector.begin(), random_vector.end(), [&]() { return distrib(gen); });

// Sort the vector in parallel
std::sort(std::exeuction::par,values.begin(),values.end())
```

### Advanced

_These are important topics that are not expected to be covered but provide
guidance where one can continue to investigate this topic in more depth._

Parallelism is depending on the hardware and runtime. Therefore, we make the following remarks:
* None of the execution policies allow for reproducibilty. This is obvious for the parallel execution policies. But even `std::ececution::seq` can execute the iterations in any order.
* NVIDIA supports to run `std::execution::par` on NVIDIA GPUs. However, that is not yet in the C++ standard and will only work with NVIDIA's HPC compiler.
* Currently, parallel algorithms are implemented using Intel's TBB library in GCC. You can set the number of used cores using `tbb::global_control(tbb::global_control::max_allowed_parallelism, nthreads);` provided by the header `#include "tbb/tbb.h"`.

