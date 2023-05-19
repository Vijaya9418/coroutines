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

//launch:-

It is a coroutine builder that construts the coroutines. 
It launches the new coroutine without blocking the current thread and returns the reference to the coroutine as a job.

//withcontext:-

it will also launch the coroutine but it will switch the context of coroutine with whatever we have mentioned.
they are thread independent.
for example :- If the coroutine is running in the main tread, we can use withcontext to switch the context of the coroutine to IO and then it will run in IO.

//coroutine scope :- 

It is a way to group jobs togehter coroutines into a similar category so that you can take action on many of them at once.
There are three types of coroutine scope:-

1. IO - Userd in network and local database Interaction.
2. Main - Used in doing stuff in main thread, manily interaction with the UI.
3. Default - It is used for heavy completion work.

//Lifecycle scope:-

The most commonly used scope is the lifecycle scope. It will cancel all the coroutines when the associated activity or fragment is destroyed. Which helps to prevent memory leak.

//Global scope:-

It is not bound to any particular lifecycle or context. It will continue to run even when the associated activity or fragment is destroyed.


// are coroutines and threads equals?

Coroutines are like thread but not threads.
A thread is a path or direction that is taken when a program is being executed.
Many coroutines can run inside a single thread.

here is a way to explain how coroutine is different from threads:-
Thread [ coroutine_1, coroutine_2, coroutine_3]
coroutine_1.delay(1000) :- it will only delay that particular coroutine_1 by 1s inside a thread, rest coroutine_2 and coroutine_3 will continue to execute.
Thread.sleep(1000):- It will delay the whole thread, which means all other coroutines inside that tread will go to sleep.

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







 
