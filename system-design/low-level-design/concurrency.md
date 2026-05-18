# Concurrency

3 categories of concurrency problems
- Correctness
- Coordination
- Scarcity

### Correctness issue
This means shared state gets corrupted because two threads access it same time
- This is the most common bug in concurrency.
- Lets say two same threads are booking a seat and both book the ticket. Then this is called correctness issue.
- First code pattern is CHECK-THEN-ACT pattern, basically we check 
first and then do some operation.
- Second code pattern is READ-MODIFY-WRITE, basically we first read, modify the value and then write modified
value (Example, read and increment counter where counter is global variable: seen in bank data, metrics aggregation)
- 

FIX: CHECK-THEN-ACT should be atomic that is one operation. Solution is LOCK.
Solution in java
- Use synchronized block
  - Everything inside this block is atomic

FIX: For READ-MODIFY-WRITE. 
Use ATOMIC variable. This is hardware level atomic operation means hardware guarantees that

the entire operation will happen in 1 cpu cycle hence no other thread will be seeing it.
Solution in java
```java
private AtomicInteger count = new AtomicInteger(10);

public void increment() {
    count.incrementAndGet();
}
```

When to use Lock vs Atomic variable
FIX is same - make whole operation as atomic
- Atomic variable : When we are doing simple operation like updating the flags. 
- Lock: When we need to update two things and need to remain consistent with each other


### Coordination Issue
This happens when we are handing task from producer to consumer.

Basically consider this usecase

Producer 1                                    Consumer 1
Producer 2         -------- Queue --------    Consumer 2
Producer 3                                    Consumer 3

Basically how do we handover task from producer to consumer in this

####### Bad Approach
Producer puts task in queue but how does consumer read from it.

- First thought would be to keep on polling

```java
// BAD: busy waiting burns cpu cycle
while(true) {
    if(!queue.isEmpty()) {
        Task task = queue.poll();
        process(task);
    }
    // spins forever even qhen queue is empty
}
```

- Second thought would be to add sleep in above code, we will save some cycles
But this brings problem of increased latency while processing task

```java
// BAD: busy waiting burns cpu cycle
while(true) {
    if(!queue.isEmpty()) {
        Task task = queue.poll();
        process(task);
    } else {
        Thread.sleep(10); //10ms latency on every task
            }
    // spins forever even qhen queue is empty
}
```

Solution: Use Bounded Blocking queue - blocks insertion when queue is full (back pressure)

```java
BlockingQueue<Task> queue = new LinkedBlockingQueue<>(100);

//producer - API Handler
public void handleSignup(User user) {
    saveUser(user);
    queue.put(new EmailTask(user));
    return success();
}

//consumer - worker thread
while(true) {
    Task task = queue.take(); // blocks if queue is empty, zero CPU. Whenever producer adds task, this immediately
                             // wakes up, hence latency is also saved.
    process(task);
        }
```

### Scarcity
This is when we want to limit access to finite resource. 
Example: External API which has rate limiting

SOLUTION: semaphore

The solution below is where we can access resource concurrently at some maximum rate.
Hence, we are effectively doing counting

```java
Semaphore permit = new Semaphore(5); //Max 5 concurrent

public void download(String url) {
    permits.acquire(); // grab a permit (blocks if none available
    try {
        doDownload(url);
    } finally {
        permits.release(); //put it back, always runs even if exception thrown. ALWAYS return in finally
    }
}
```

Now for a situation like DB connection where we have connection pool and we cant create
new connection everytime. Here we have fixed set of resources.
When managing actual objects with state, create a pool of blocking queue.

```java

//initialize pool with connection
BlockingQueue<Connection> pool = new BlockingQueue<>(10);
for(int i=0; i<10; i++) {
    pool.put(createConnection());
}

//usage
public Result query(String sql) {
    Connection connection = pool.take(); // blocks if pool is empty
    try {
        return connection.execute(sql);
    } finally {
        pool.put(connection); // same rule: ALWAYS return in finally
    }
}
```


### Interview mental model for concurrency

1. Do we have a shared state to access (Correctness)
   2. Locks
   3. Atomic Variable
4. Is work flowing from one thread to another  (Work is overflowing or underflowing)
   5. Blocking queue
6. Is theres a scarce issue
   7. Semaphore
   8. Blocking queue


## Resources
1. [Youtube Hello interview](https://www.youtube.com/watch?v=d8rmosXttTE)
2. [Read from Hello interview](https://www.hellointerview.com/learn/low-level-design/concurrency/intro) 





