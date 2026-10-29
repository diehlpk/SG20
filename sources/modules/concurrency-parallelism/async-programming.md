## # Module name: Asynchronous programming

*Skeleton descriptions are typeset in italic text,*
*so please don't remove these descriptions when editing the topic.*

### Overview

*Provides a short natural language abstract of the module’s contents.*
*Specifies the different levels of teaching.*

Asynchronous programming allows computational tasks to execute independently of the calling code, potentially enabling concurrent execution and improving responsiveness. C++ provides standard library facilities such as `std::async`, `std::future`, and `std::promise` to launch tasks, communicate results, and synchronize execution.

This module introduces the fundamental concepts of asynchronous programming and demonstrates how to launch functions and lambdas asynchronously and retrieve their results.

| Level | Objective |
|---|---|
| Foundational | Understand asynchronous programming, futures, and promises. |
| Main | Launch functions and lambdas asynchronously and retrieve their results. |
| Advanced | Understand synchronization, exception handling, and more advanced asynchronous programming techniques. |

### Motivation

*Why is this important?*
*Why do we want to learn/teach this topic?*

- Asynchronous programming allows tasks to be launched without requiring the calling code to wait immediately for their completion.
- Asynchronous programming enables concurrency and potentially parallel execution, which can improve application responsiveness and resource utilization.
- The C++ standard library provides mechanisms for asynchronous task execution, result retrieval, and synchronization.

### Topic introduction

*Very brief introduction to the topic.*

Asynchronous programming allows a function or lambda to be launched while the calling code continues executing other operations. The result of an asynchronous computation can be retrieved later when it becomes available.

C++ provides `std::async` to launch tasks and `std::future` to access their results. A future represents a value that may not yet be available. Calling `get()` retrieves the result, waiting for the computation to complete if necessary.

### Foundational: Understanding asynchronous programming

#### Background/Required Knowledge

A student:

- Should understand functions and their return values.
- Should know [lambdas](../functions/lambdas.md).

#### Student outcomes

*A list of things "a student should be able to" after the curriculum.*
*The next word should be an action word and testable in an exam.*
*Max 5 items.*

A student should be able to:

1. Explain the concept of asynchronous programming.
2. Distinguish between synchronous and asynchronous execution.
3. Explain the purpose of futures and promises.
4. Identify the roles of `std::async` and `std::future` in asynchronous programming.

#### Caveats

*This section mentions subtle points to understand, like anything resulting in implementation-defined, unspecified, or undefined behavior.*

- Asynchronous execution does not necessarily imply parallel execution.
- Retrieving a result from a future may block until the computation completes.
- The default execution policy of `std::async` does not guarantee that a task executes asynchronously.

#### Points to cover

*This section lists important details for each point.*

- Introduce synchronous and asynchronous execution.
- Explain how `std::async` launches functions or lambdas.
- Introduce `std::future` as a placeholder for a result that becomes available later.
- Explain the distinction between `std::future` and `std::promise`.
- Introduce the concept of waiting for an asynchronous computation to complete.

### Main: Launching asynchronous functions and retrieving results

#### Background/Required Knowledge

- All of the above.

#### Student outcomes

A student should be able to:

1. Define a function or lambda representing a computational task.
2. Launch a function or lambda asynchronously using `std::async`.
3. Retrieve the result of an asynchronous computation using `std::future::get()`.
4. Launch multiple independent asynchronous tasks and combine their results.
5. Explain the differences between the `std::launch::async` and `std::launch::deferred` execution policies.

#### Caveats

- `std::future::get()` blocks until the result is available and can be called only once for a given future.
- `std::async` may execute tasks immediately or defer their execution, depending on the selected launch policy.
- Using `std::launch::async` explicitly ensures asynchronous execution.
- Asynchronous tasks accessing shared mutable data require appropriate synchronization to avoid data races.
- Destruction of a future returned by `std::async` may block until the asynchronous task finishes.

#### Points to cover

- Include the `<future>` header to use `std::async` and `std::future`.
- Explain how to launch functions and lambdas using `std::async`.
- Explain how `std::launch::async` requests asynchronous execution.
- Explain that the return type of the function or lambda determines the template argument of `std::future`.
- Explain that `std::async` accepts a callable followed by its arguments, optionally preceded by an execution policy.
- Explain how to retrieve a result using `get()` and why this operation may block.
- Demonstrate how multiple asynchronous tasks can execute independently before their results are combined.

**Example using a function**

```cpp
#include <future>
#include <iostream>

double square(double a)
{
    return a * a;
}

int main()
{
    std::future<double> f =
        std::async(std::launch::async, square, 5.0);

    // Perform other work while square is computed.

    std::cout << "Result = " << f.get() << '\n';
}
```

**Example using lambdas**

```cpp
#include <future>
#include <iostream>

int main()
{
    // Compute the sum asynchronously.
    std::future<double> f1 = std::async(
        std::launch::async,
        [](double a, double b) { return a + b; },
        2.0, 3.0
    );

    // Compute the square asynchronously.
    std::future<double> f2 = std::async(
        std::launch::async,
        [](double a) { return a * a; },
        4.0
    );

    // Perform other work while tasks execute.

    // Retrieve and combine the results.
    double res = f1.get() + f2.get();

    std::cout << "Result = " << res << '\n';
}
```

### Advanced: Managing asynchronous execution

*These are important topics that are not expected to be covered but provide*
*guidance where one can continue to investigate this topic in more depth.*

#### Topics for further study

- **Execution policies:** Compare `std::launch::async` and `std::launch::deferred` and their implications for execution.
- **Futures and promises:** Use `std::promise` to communicate results between threads and `std::shared_future` to allow multiple consumers to access a result.
- **Synchronization:** Explore `std::future::wait()`, `wait_for()`, and `wait_until()`.
- **Exception handling:** Understand how exceptions thrown during asynchronous execution are propagated through `std::future::get()`.
- **Thread safety:** Investigate data races, shared state, and synchronization mechanisms.
- **Task composition:** Explore techniques for coordinating dependent asynchronous computations and alternatives such as coroutines and task-based runtimes.
