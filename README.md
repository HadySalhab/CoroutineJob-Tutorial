# CoroutineJob-Tutorial

This repository contains code for the following tutorial on Youtube:
[Kotlin Coroutine Jobs (Beginner Example)](https://www.youtube.com/watch?v=UsHTxOILP5g)

```kotlin
    private lateinit var  job: CompletableJob //allows us to invoke complete() on the job to terminate/complete the job without relying on coroutines to do so (More Control)
```

We can get a **callback** when the job is completed or cancelled using the below method:

```kotlin
  job.invokeOnCompletion {
      //handler exception will pass a message which might be null or empty
        it?.message.let {
                var msg= it
                if(msg.isNullOrEmpty()){
                    msg = "Unknown cancellation error."
                }
                println("${job} was cancelled. Reason: $msg")
            }
        }
```

```kotlin
  CoroutineScope(IO + job).launch {
//this will launch a coroutine that belongs to the IO dispatcher attached to a job
//so we can monitor its state (Complete, activte, cancelled,cancelling...)
            }
```

```kotlin
  CoroutineScope(IO).launch {
//this will launch a coroutine that belongs to the IO dispatcher)
            }
```

The difference in the above is the following:

```kotlin
val coroutineScope = CoroutineScope(IO)
coroutineScope.cancel()// will cancel all coroutines that belong to the IO dispatcher

val coroutineScope = CoroutineScope(IO+job)
job.cancel()//will cancel only that job, all coroutines attached to that job
```

We can get the state of coroutines attached to a job using the following method:

1. job.isActive //true if job is still active
1. job.isCompleted //true if job is completed
1. job.isCanceled //true if job is canceled

```kotlin
job.cancel(CancellationException("Any Message")) //this exception will be caught by  invokeOnCompletion callback
```

When a job is canceled it cannot be used again, we have to reinitialize a new job to launch more coroutines..
