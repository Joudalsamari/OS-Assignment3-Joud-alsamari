# Assignment 3 - Complete Documentation

**Student Name**: [Joud alsamari]  
**Student ID**: [445052056]  
**Date Submitted**: [4 may]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `445052056_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [3 may, 8:00pm]
**What I implemented**: 
Began by thoroughly understanding the assignment requirements and analyzing the provided code. Identified shared resources such as contextSwitchCount, completedProcessCount, totalWaitingTime, and executionLog.
**Challenges encountered**: 
Initially found it difficult to pinpoint where race conditions could occur.
**How I solved it**: 
Reviewed Chapters 3 and 5 of Operating System Concepts, focusing on identifying and managing critical sections.
**Testing approach**: 
Executed the program without synchronization to observe its behavior and identify issues.
**Time spent**: 
1 hour
---

### Entry 2 - [3 may, 9 pm]
**What I implemented**: 
I used ReentrantLock to protect shared counters
**Challenges encountered**: 
Understanding where to put lock and unlock correctly
**How I solved it**: 
Used try-finally to ensure unlocking
**Testing approach**: 
I ran it several times and verified that the counters were consistent
**Time spent**: 
1 hour
---

### Entry 3 - [3 may, 11 pm]
**What I implemented**: 
I added synchronization for executionLog
**Challenges encountered**: 
Understanding why ArrayList is not thread-safe
**How I solved it**: 
Protected it using the same lock
**Testing approach**: 
Ensured no ConcurrentModificationException occurs
**Time spent**: 
15 min
---

### Entry 4 - [3 may , 11:30pm]
**What I implemented**: 
I added Semaphore to control CPU access
**Challenges encountered**: 
Understanding how Semaphore differs from Lock
**How I solved it**: 
Used binary semaphore (1 permit)
**Testing approach**: 
Verified only one process executes at a time
**Time spent**: 
30 minuts 
---

### Entry 5 - [4 may , 1 am]
**What I implemented**: 
Final Testing and Validation
**Challenges encountered**: 
Ensuring consistent output across runs
**How I solved it**: 
Repeated execution multiple times
**Testing approach**: 
Compared outputs and verified correctness
**Time spent**: 
30 minuts
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:
The first race condition occurs in the shared variable contextSwitchCount, where multiple threads increment
the value concurrently using contextSwitchCount++. Since this operation is not atomic, threads may
overwrite each other's updates, leading to incorrect counts
The second race condition occurs in executionLog, which is implemented using ArrayList. ArrayList is not
thread-safe, so concurrent modifications can lead to inconsistent data or runtime exceptions
These race conditions can cause incorrect statistics and corrupted logs, affecting the correctness of the
simulation

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
ReentrantLock is used to provide mutual exclusion, ensuring that only one thread can access a critical section at a time. In this assignment, it was used to protect shared variables such as counters and execution logs
Semaphore, on the other hand, is used to control access to a limited number of resources. In this
assignment, a binary semaphore (1 permit) was used to simulate CPU access, ensuring that only one process
•executes at a time
Thus, locks were used for data protection, while semaphores were used for resource management

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
Deadlock is a situation where two or more threads are waiting indefinitely for resources held by each other
One prevention technique is using try-finally blocks to ensure that locks are always released, even if an
exception occurs. Another technique is avoiding nested locks and maintaining consistent locking order
In this assignment, try-finally was used to guarantee that locks and semaphores are released properly, preventing deadlocks
---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:
I used a single lock (coarse-grained locking) to protect all shared counters. This approach simplifies
implementation and reduces the risk of deadlocks
The trade-off is reduced concurrency compared to fine-grained locking, where separate locks could allow
multiple threads to update different counters simultaneously
However, given the simplicity of the assignment and the small number of shared variables, coarse-grained
locking is more suitable and easier to maintain
---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchcount, completed ProcessCount, totalWaiting Time
**Why they need protection**: 
.They are shared among multiple threads and updated concurrently
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
lock.Lock();
try {
contextSwitchCount++;
}
finally (
lock.unlock();
}
```

**Justification**: 
Ensures mutual exclusion and prevents race conditions
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog (ArrayList)
**Why it needs protection**: 
ArrayList is not thread-safe
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
lock. lock();
try {
executionlog.add(message);
} finally {
lock. unlock ();
}
```

**Justification**: 
Prevents data corruption and exceptions
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
Control CPU access
**Number of permits and why**: 
(simulate single CPU) 1
**Where implemented**: 
Inside run() method
**Code snippet**:
```java
SharedResources.cpuSemaphore.acquire();
SharedResources.cpuSemaphore.release();
```

**Effect on program behavior**: 
Ensures only one process executes at a time

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
The program produced consistent results across multiple runs

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 
Synchronization ensures correctness
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException
Checking whether concurrent modifications to the execution log cause a ConcurrentModificationException
**Testing procedure**: 
I executed the program multiple times while focusing on operations involving the shared executionLog list.
Since multiple threads attempt to write to this list, it is a potential source of concurrency issues. I verified whether any runtime exceptions occurred during execution
**Results**: 
No ConcurrentModificationException or any other runtime exception was observed during any execution of the program
**What this proves**: 
This confirms that the executionLog is properly synchronized using ReentrantLock. The lock ensures that only one thread modifies the list at a time, preventing concurrent modification issues and maintaining data consistency
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)
Verifying that the final computed values (such as total context switches, completed processes, and waiting time) are correct and logically consistent
**Expected values**: 
The number of completed processes should equal the total number of created processes
Context switches should reflect the number of scheduling operations
Total waiting time should be positive and reasonable
Average waiting time should be correctly calculated
**Actual values**: 
Based on the program output
Total Completed Processes = 16
Total Context Switches = 35
Total Waiting Time = 1221948 ms
 Average Waiting Time = 76371 ms
**Analysis**: 
The actual values match the expected behavior of a Round Robin scheduler. All processes were completed successfully, and no incorrect values were observed. The statistics are consistent with the execution flow, confirming that synchronization did not interfere with correctness but instead ensured accurate
computation
---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
Running the program with different randomly generated values for time quantum and number of processes (based on different student IDs)
**Purpose**: 
To verify that the synchronization mechanisms work correctly under different workloads and execution conditions
**Results**: 
The program behaved correctly in all scenarios. Regardless of the number of processes or time quantum, the output remained consistent and no race conditions or errors occurred
**What I learned**: 
Synchronization mechanisms such as locks and semaphores ensure stability and correctness regardless of
system load. This demonstrates their importance in real-world concurrent systems
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

Through this assignment, I learned the importance of synchronization in multi-threaded systems. I understood how race conditions occur when multiple threads access shared resources without proper coordination. I also learned how ReentrantLock provides mutual exclusion, ensuring only one thread accesses a critical section at a time. Additionally, I gained practical experience using semaphores to control access to limited resources such as the CPU. One key insight was the importance of using try-finally blocks to avoid deadlocks by ensuring locks are always released. I also realized that even simple operations like incrementing a variable can cause serious issues in concurrent environments. Overall, this assignment strengthened my understanding of concurrency control and thread safety
---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 
Banking systems, where multiple transactions update the same account balance. Without synchronization, Incorrect balances could occur
**Example 2**: 
Operating systems CPU scheduling, where multiple processes compete for CPU access. Synchronization ensures fair and correct execution
---

### How I would explain synchronization to others:

Synchronization can be explained as a way to organize access to shared resources. Imagine multiple people trying to write on the same whiteboard at the same time. Without coordination, the result will be messy and incorrect. A lock acts like giving only one person access to the whiteboard at a time. A semaphore is like limiting how many people can enter a room. In programming, synchronization ensures that threads do not interfere with each other and that shared data remains correct
---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/Joudalsamari/OS-Assignment3-Joud-alsamari.git

**Number of commits**: 9

**Commit messages**: 
1. change id 
2. add reentrantlock
3. add semaphore
4. save shared couters
5. save shared variableuseing rantlock
6. save shared var totalwaitingtime using reentlock
7. save logexecution using lock
8. semaphore to control cpu
9. apply semaphore

---

## Summary

**Total time spent on assignment**: 1 day

**Key takeaways**: 
1. Race conditions can lead to incorrect program behavior if not handled properly 

2. Locks and semaphores are essential tools for ensuring thread safety 

3. Proper synchronization improves both correctness and reliability of concurrent programs

**Most challenging aspect**: 
Understanding where race conditions occur and correctly applying synchronization without introducing
**What I'm most proud of**: 
Successfully implementing synchronization mechanisms and achieving consistent, correct results across multiple executions
---

**End of Documentation**
