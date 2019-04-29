## Prerequisite

* [Concurrent programming in java](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html)


Twitter future

## Interesting `macOS` bug

The following object `ConcurrentTasks` provide a way to spawn mulitple threads concurrently.

```scala
import com.twitter.util.{FutureTask, FuturePool, Future}
object ConcurrentTasks {

  val fp = FuturePool.unboundedPool

  def debug(message: String): Unit = {
    val now = java.time.format.DateTimeFormatter.ISO_INSTANT
      .format(java.time.Instant.now)
      .substring(11, 23) // keep only time component
    val thread = Thread.currentThread.getName()
    println(s"$now [$thread] $message")
  }
  
  def futureTask(label: Int) = FutureTask { 
      debug(s"Start task $label"); 
      Thread.sleep(2500); 
      debug("end task $label") 
    }
    
  def run(c: Int) = (0 until c)
  .map(futureTask)
  .map(t => fp(t.run))
}
```

If you run:

```scala
ConcurrentTasks.run(3500)
```
on `macOS`, the 3500 threads can be spawn and method `futureTask` can be executed correctly in all threads. However, when the 3500 threads are terminated, `macOS` will crush (confirmed on macOS v10.14.3). 
