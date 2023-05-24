
![coroutines](https://github.com/Vijaya9418/coroutines/assets/56352158/f8d3dd76-6310-4771-8237-9f639951a624)

# coroutines

Coroutine(background tasks) is a design pattern that we can use in android to simplify code that executes asynchronously(not simultaneous). Or we can say that it is a background thread that allow execution to be suspended and resumed generalizing subroutines for cooperative multitasking.

#benefits:-

Coroutines help to manage long-running tasks that might otherwise block the main thread and cause your app to become unresponsive.

#advantages:-

1. Coroutines decrease the overall execution time. This is so because they make the entire code non-blocking by voluntarily giving CPU to other coroutines.
2. It comes with a mutual exclusion.For instance, if process A has two coroutines, coroutine_1, and coroutine_2, then coroutine_1 will not interfere with coroutine_2 in terms of its memory, CPU, or any other resource.
If corotuine_1 has to wait for a resource or some other module’s result, then it will pause itself to give control to coroutine_2 instead of blocking it.
3. Fewer Memory leaks. A coroutine causes fewer memory leaks due to the use of structured concurrency.Structured concurrency means that any independent execution unit (say thread) is encapsulated with explicit entry and exit points.

#disadvantage:-

1. The disadvantage is that you'll need to copy out/in every time a coroutine yields.


//suspend:-

It is a function that can be paused or resumed at a later time, coroutines can be called only from a suspend function. We mark them suspend so that it can be called asynchronously and therefore runs in background thread.
It is designed to work with coroutine builders like launch or async and allows for asynchronous programming in a sequential and more readable manner.


**example with simple function**

import kotlinx.coroutines.*

fun main() {

    println("Before fetching data")

    val data = fetchData()

    println("After fetching data: $data")
}

fun fetchData(): String {

    Thread.sleep(1000) // Simulating a delay of 1 second

    return "Data fetched successfully"
}

output:-
Before fetching data

After fetching data: Data fetched successfully


When you run this code, it will execute synchronously, meaning that the execution of the main thread will be blocked for approximately 1 second during the Thread.sleep operation. As a result, the "After fetching data" line will be printed after the delay.

**example with suspend function**

import kotlinx.coroutines.*

fun main() {

    val scope = CoroutineScope(Dispatchers.Default)

    scope.launch {
        println("Before fetching data")

        val data = fetchData()

        println("After fetching data: $data")
    }

    // Allow some time for the coroutine to execute
    Thread.sleep(2000)

    // Don't forget to cleanup the scope
    scope.cancel()
}

suspend fun fetchData(): String {

    delay(1000) // Simulating a delay of 1 second

    return "Data fetched successfully"
    
}

output:- 
Before fetching data

Using suspending functions and coroutines enables non-blocking execution, where other code can continue running while waiting for long-running or asynchronous operations to complete.




//launch:-

It is a coroutine builder that construts the coroutines. 

It launches the new coroutine without blocking the current thread and returns the reference to the coroutine as a job.

import kotlinx.coroutines.*

fun main() {

    println("Before coroutine")

    val scope = CoroutineScope(Dispatchers.Default)

    scope.launch {
        println("Inside coroutine")
        delay(1000)
        println("Coroutine execution complete")
    }

    println("After coroutine launch")

    // Wait for the coroutine to complete
    Thread.sleep(2000)

    // Clean up the scope
    scope.cancel()
}

output:-
Before coroutine

After coroutine launch

Inside coroutine

Coroutine execution complete


//withcontext:-

it will also launch the coroutine but it will switch the context of coroutine with whatever we have mentioned.
they are thread independent.
for example :- If the coroutine is running in the main tread, we can use withcontext to switch the context of the coroutine to IO and then it will run in IO.

import kotlinx.coroutines.*

fun main() {

    println("Before coroutine")

    val scope = CoroutineScope(Dispatchers.Default)

    scope.launch {
        println("Inside coroutine")

        withContext(Dispatchers.IO) {
            println("Running in IO context")
            delay(1000)
        }

        println("Back to default context")
        delay(1000)

        println("Coroutine execution complete")
    }

    println("After coroutine launch")

    // Wait for the coroutine to complete
    Thread.sleep(3000)

    // Clean up the scope
    scope.cancel()
}

output:- Before coroutine

After coroutine launch

Inside coroutine

Running in IO context

Back to default context

Coroutine execution complete




//coroutine scope :- 

It is a way to group jobs togehter coroutines into a similar category so that you can take action on many of them at once.
it ensures proper cancellation and cleanup of resources associated with the coroutines.
The scope defines the context in which the coroutines will run.

import kotlinx.coroutines.*

fun main() {

    println("Before coroutine")

    val scope = CoroutineScope(Dispatchers.Default)

    scope.launch {
        println("Inside coroutine")
        delay(1000)
        println("Coroutine execution complete")
    }

    println("After coroutine launch")

    // Wait for the coroutine to complete
    Thread.sleep(2000)

    // Clean up the scope
    scope.cancel()
}

output:-
Before coroutine<br />
After coroutine launch<br />
Inside coroutine<br />
Coroutine execution complete<br />

what will happen if we will not do thread.sleep and scope.cancel() in above example:-

import kotlinx.coroutines.*

fun main() {

    println("Before coroutine")

    val scope = CoroutineScope(Dispatchers.Default)

    scope.launch {
        println("Inside coroutine")
        delay(1000)
        println("Coroutine execution complete")
    }

    println("After coroutine launch")
}

output:-
Before coroutine<br />
After coroutine launch<br />
Inside coroutine<br />

the program will exit immediately after launching the coroutine. This will result in the coroutine not having enough time to complete its execution.

There are manily twotypes of coroutine scopes:-
1. Global scope
2. Lifecycle scope


//Global scope:-

It is not bound to any particular lifecycle or context. It will continue to run even when the associated activity or fragment is destroyed.

import kotlinx.coroutines.*

fun main() {

    println("Before coroutine")
    GlobalScope.launch {
        println("Inside global scope coroutine")
        delay(1000)
        println("Coroutine execution complete")
    }

    println("After coroutine launch")

    // Rest of the program

    Thread.sleep(2000) // Wait for the coroutine to complete

    // No need to cancel the GlobalScope, as it lives for the entire application lifecycle
}

output:-
Before coroutine<br />
After coroutine launch<br />
Inside global scope coroutine<br />
Coroutine execution complete<br />

The GlobalScope does not require explicit cancellation, as it lives for the entire application lifecycle. However, it's important to note that coroutines launched within GlobalScope are not automatically cancelled when the surrounding code completes execution. It's the responsibility of the programmer to ensure proper cancellation and cleanup of resources associated with the coroutines launched in the GlobalScope.

//Lifecycle scope:-

The most commonly used scope is the lifecycle scope that is tied to a specific lifecycle or context, such as an activity, fragment, or a block of code.. It will cancel all the coroutines when the associated activity or fragment is destroyed. Which helps to prevent memory leak. It allows for proper management and cancellation of coroutines based on the lifecycle events.

import kotlinx.coroutines.*
import kotlin.coroutines.CoroutineContext

class ExampleClass : CoroutineScope {

    private val job = Job()
    override val coroutineContext: CoroutineContext
        get() = Dispatchers.Default + job

    fun startExample() {
        println("Before coroutine")

        launch {
            println("Inside lifecycle scope coroutine")
            delay(1000)
            println("Coroutine execution complete")
        }

        println("After coroutine launch")

        // Rest of the code

        // Coroutine cancellation will be handled by the lifecycle
    }

    fun stopExample() {
        job.cancel() // Cancel the coroutine job when the example stops
    }
}

fun main() {

    val example = ExampleClass()
    example.startExample()

    // Rest of the program

    Thread.sleep(2000) // Wait for the coroutine to complete

    example.stopExample()
}

output:-
Before coroutine<br />
After coroutine launch<br />
Inside lifecycle scope coroutine<br />
Coroutine execution complete<br />


The stopExample function is responsible for cancelling the coroutine job when the example stops. This ensures proper cancellation and cleanup of the coroutines associated with the lifecycle scope.

//Dispatchers:-

 Dispatchers are used to define the context or thread on which coroutines will be executed. They provide a way to control the thread or thread pool that executes the coroutines and specify the concurrency model.
 
There are three types of dispatchers:-

1. IO -
 Userd in network and local database Interaction,reading from or writing to files, making network requests.<br />
 It uses a larger pool of threads to handle I/O operations efficiently.<br /> 
 It is suitable for any work that primarily waits for I/O to complete.<br />

2. Main - 
This dispatcher is specific to Android applications and is associated with the main/UI thread.<br /> 
Used in doing stuff in main thread, manily interaction with the UI.<br />
It ensures that coroutines running on this dispatcher execute on the main thread.<br />


3. Default - 
This dispatcher is used for CPU-intensive tasks or non-blocking operations that are not tied to any specific thread.<br />
It is used for heavy completion work.<br />
It is a shared pool of threads, and the number of threads depends on the available processors<br />


import kotlinx.coroutines.*

fun main() {

    println("Before coroutine")

    // Coroutine running on the Default dispatcher
    GlobalScope.launch(Dispatchers.Default) {
        println("Inside Default dispatcher coroutine")
        // CPU-intensive work here
    }

    // Coroutine running on the IO dispatcher
    GlobalScope.launch(Dispatchers.IO) {
        println("Inside IO dispatcher coroutine")
        // I/O-bound work here
    }

    // Coroutine running on the Main dispatcher
    GlobalScope.launch(Dispatchers.Main) {
        println("Inside Main dispatcher coroutine")
        // UI-related work here
    }

    println("After coroutine launch")

    // Rest of the program

    Thread.sleep(2000) // Wait for the coroutines to complete

    // Cleanup
    // No need to cancel the GlobalScope as coroutines launched in the GlobalScope
    // are not automatically cancelled when the surrounding code completes execution.
}


// are coroutines and threads equals?

Coroutines are like thread but not threads.
A thread is a path or direction that is taken when a program is being executed.
Many coroutines can run inside a single thread.

here is a way to explain how coroutine is different from threads:-
Thread [ coroutine_1, coroutine_2, coroutine_3]
coroutine_1.delay(1000) :- it will only delay that particular coroutine_1 by 1s inside a thread, rest coroutine_2 and coroutine_3 will continue to execute.
Thread.sleep(1000):- It will delay the whole thread, which means all other coroutines inside that tread will go to sleep.


import kotlinx.coroutines.*

fun main() {

    println("Before coroutines")

    GlobalScope.launch {
        println("Coroutine 1: Start")
        delay(1000)
        println("Coroutine 1: Delay complete")
    }

    GlobalScope.launch {
        println("Coroutine 2: Start")
        println("Coroutine 2: Executing some work")
        println("Coroutine 2: Work complete")
    }

    GlobalScope.launch {
        println("Coroutine 3: Start")
        println("Coroutine 3: Executing some work")
        println("Coroutine 3: Work complete")
    }

    println("After coroutines launch")

    Thread.sleep(2000) // Delay the main thread

    println("After thread sleep")

    // Cleanup
    // No need to cancel the GlobalScope as coroutines launched in the GlobalScope
    // are not automatically cancelled when the surrounding code completes execution.
    
}

output:-

Before coroutines
After coroutines launch
Coroutine 2: Start
Coroutine 3: Start
Coroutine 2: Executing some work
Coroutine 3: Executing some work
Coroutine 2: Work complete
Coroutine 3: Work complete
Coroutine 1: Start
Coroutine 1: Delay complete
After thread sleep



//withTimeOutOrNull:-

It will wait until the job time out which we have provided and if within this time out the jobs get completed otherwise it will return null.

//completable job:-

It has more feature added to it , so we should always use completable job instead a simple job.

//job.complete:-
It will complete the job and mark as complete.

//job.complete Exceptionally:-
It will complete the job exceptionally with a given exception.

Why we write(IO+job)
Because if we want to cancel a particular job out of all the coroutines that is running inside IO, then we can simply do job.cancel, it will be independent of other coroutines.

Ex:-
val scope = CoroutineScope(IO).launch
{
//some code
}
scope.cancel

This will cancel all the coroutines running inside IO



val scope = CoroutineScope.launch(IO+Job).launch
{
//some code
}
Job.cancel
This will only cancel that particular job.


Runblocking:-

Runblocking is similar to the coroutine scope but with special properties. It runs a new coroutine and blocks the current thread interruptible until its completion, mostly it is used in test cases.

Async and await:-
It is a function to define the coroutine , indicating that the function can be paused and resumed.

Await:-
It is the keyword used within the coroutine to pause the execution go a function until a specific asynchronous operations completed.







 
