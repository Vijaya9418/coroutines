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

// are coroutines are threads equals?

Coroutines are like thread but not threads.
A thread is a path or direction that is taken when a program is being executed.
Many coroutines can run inside a single thread.

here is a way to explain how coroutine is different from threads:-
Thread [ coroutine_1, coroutine_2, coroutine_3]
coroutine_1.delay(1000) :- it will only delay that particular coroutine_1 by 1s inside a thread, rest coroutine_2 and coroutine_3 will continue to execute.
Thread.sleep(1000):- It will delay the whole thread, which means all other coroutines inside that tread will go to sleep.



 
