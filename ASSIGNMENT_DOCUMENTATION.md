# Assignment 3 - Complete Documentation

**Student Name**: [Muhannad Salman Al durayhim]  
**Student ID**: [445050253]  
**Date Submitted**: [3/5/2026]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [May 3, 2026, 6:00 PM]
**What I implemented**: I began by comprehending the current CPU scheduler code and locating shared resources like the execution log and counters.

**Challenges encountered**: Where race conditions could appear in the code baffled me.

**How I solved it**: I examined shared variables accessed by several threads and went over the idea of key portions from the textbook.

**Testing approach**: I examined shared variables accessed by several threads and went over the idea of key portions from the textbook.

**Time spent**: 1 hour

---

### Entry 2 - [May 3, 2026, 8:00 PM]
**What I implemented**: ReentrantLock was used to safeguard the execution log and shared counters.

**Challenges encountered**: Ensuring locks are always released to avoid deadlock.

**How I solved it**: To ensure unlock operations, try-finally blocks were used.

**Testing approach**: Several runs were tested, and no wrong counter values were found.

**Time spent**: 40 minutes 

---

### Entry 3 - [May 3, 2026, 9:30 PM]
**What I implemented**: Added Semaphore to control CPU access and limit execution to one process at a time.

**Challenges encountered**: Handling InterruptedException when using acquire().

**How I solved it**: Wrapped acquire() in try-catch and ensured release() is always called.

**Testing approach**: Observed execution order and ensured only one process runs at a time.

**Time spent**: 30 minutes 

---

### Entry 4 - [May 3, 2026, 10:00 PM]
**What I implemented**: Completed synchronization in runToCompletion() method.

**Challenges encountered**: Ensuring consistency between run() and runToCompletion().

**How I solved it**: Applied same semaphore pattern with try-finally.

**Testing approach**: Ran program and confirmed correct termination of last process.

**Time spent**: 1.5 hours

---

### Entry 5 - [May 3, 2026, 12:30 PM]
**What I implemented**: Final testing and verification of the complete synchronization system. I reviewed all implemented locks and semaphores and ensured that all shared resources are properly protected. I also finalized the output formatting and verified correctness of statistics such as waiting time and context switches.

**Challenges encountered**: Ensuring that the results remain consistent across multiple runs and confirming that no hidden race conditions still exist.

**How I solved it**: I executed the program multiple times and carefully compared the outputs. I also reviewed all critical sections again to ensure that every shared variable is properly synchronized.

**Testing approach**: Testing approach:
Ran the program more than 5 times and checked that:

Total completed processes always equals the number of processes
No runtime exceptions occur
Output remains stable and correct

**Time spent**: 1.5 hours

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:Two race conditions exist in the original code.

First, the shared counter contextSwitchCount is updated by multiple threads without synchronization. Concurrent access can cause lost updates, where increments are overwritten.

Second, the executionLog ArrayList is accessed by multiple threads. Since ArrayList is not thread-safe, concurrent modification can lead to inconsistent data or runtime exceptions.

Without synchronization, incorrect behavior such as wrong counter values or corrupted logs may occur.

[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**: ReentrantLock provides mutual exclusion, ensuring only one thread accesses a critical section at a time. Semaphore controls access using permits and can allow multiple threads depending on the number of permits.

In my code, I used ReentrantLock to protect shared variables such as counters and execution log because they require exclusive access.

I used a binary Semaphore to control CPU access, ensuring only one process executes at a time, simulating a real CPU.

[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**: Deadlock occurs when two or more threads wait indefinitely for resources held by each other.

Two prevention techniques are:

Ensuring resources are released properly.
Using consistent lock acquisition order.

In my code, I used try-finally blocks to guarantee that locks and semaphores are always released, preventing deadlocks.

[Your answer here - reference try-finally blocks, lock ordering, etc.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:
I used one lock (coarse-grained locking) to protect all three counters.

The reason is that the counters are simple variables and the overhead of multiple locks is unnecessary.

The trade-off is that coarse-grained locking reduces concurrency but simplifies implementation, while fine-grained locking improves concurrency but increases complexity.

Since the counters are independent, fine-grained locking could provide better performance, but for this assignment, simplicity and correctness were prioritized.
[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: contextSwitchCount, completedProcessCount, totalWaitingTime

**Why they need protection**: They are shared among multiple threads and updated concurrently.

**Synchronization mechanism used**: ReentrantLock

**Code snippet**:
counterLock.lock();
try {
    contextSwitchCount++;
} finally {
    counterLock.unlock();
}
```java
// Paste your implementation here
```

**Justification**: Ensures mutual exclusion and prevents race conditions.

---

### Critical Section #2: Execution Log

**What resource**: ArrayList executionLog

**Why it needs protection**: ArrayList is not thread-safe and concurrent access may cause exceptions.

**Synchronization mechanism used**: ReentrantLock

**Code snippet**:
logLock.lock();
try {
    executionLog.add(message);
} finally {
    logLock.unlock();
}
```java
// Paste your implementation here
```

**Justification**: Prevents concurrent modification issues.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: Control access to CPU

**Number of permits and why**: to allow only one process 1 (binary semaphore)

**Where implemented**: run() and runToCompletion()

**Code snippet**:
SharedResources.cpuSemaphore.acquire();
try {
    // execution
} finally {
    SharedResources.cpuSemaphore.release();
}
```java
// Paste your implementation here
```

**Effect on program behavior**: 
Ensures only one process executes at a time.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
javac SchedulerSimulationSync.java
java SchedulerSimulationSync
(repeated 5 times)
```bash
# Commands used (run the program at least 5 times)
```

**Results**: All runs produced consistent statistics such as total processes and correct waiting times.
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: Without synchronization, counters and logs may produce incorrect values.
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 
Synchronization ensures correct and consistent results.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: I compiled and executed the program multiple times to simulate concurrent access to the shared execution log. Each run involved multiple threads attempting to write to the shared ArrayList simultaneously. I carefully observed the program output during each execution to check for any runtime exceptions, specifically ConcurrentModificationException.

**Results**: No ConcurrentModificationException occurred.

**What this proves**: Execution log is properly synchronized.

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: Completed processes = total processes

**Actual values**: Matched expected values in all runs

**Analysis**: Synchronization ensures correctness.

---

### Test 4: Different Scenarios
**Scenario tested**: Different time quantum values

**Purpose**: The purpose of this test is to evaluate how the scheduler behaves under different execution scenarios, such as varying time quantum values and different numbers of processes. This helps verify that the synchronization mechanisms (locks and semaphore) work correctly regardless of scheduling conditions. It also ensures that the program maintains correct behavior, consistency, and stability under different workloads and does not introduce race conditions or synchronization errors.

**Results**: Scheduler behaved correctly in all cases

**What I learned**: Synchronization works independently of scheduling parameters.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:
Through this assignment, I learned the importance of process synchronization in a multithreaded environment. I understood how race conditions occur when multiple threads access shared resources without proper control. By implementing ReentrantLock and Semaphore, I was able to ensure mutual exclusion and prevent inconsistent updates to shared variables. I also learned the importance of using try-finally blocks to guarantee proper release of locks and avoid deadlocks. Overall, this assignment helped me connect theoretical concepts from the textbook with practical implementation in Java.

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:


Give TWO examples where synchronization is critical:

**Example 1**: Operating systems use synchronization in CPU scheduling to ensure that multiple processes do not access shared resources simultaneously, similar to how the semaphore controls CPU access in this assignment.

**Example 2**: Databases use locks to prevent multiple users from modifying the same record at the same time, ensuring data consistency and preventing corruption.

---

### How I would explain synchronization to others:
Synchronization can be explained as a way to control access when multiple people (threads) try to use the same resource at the same time. For example, if multiple students try to write on the same whiteboard at once, their writing will overlap and become unreadable. Locks and semaphores act like a rule that allows only one student at a time to write, ensuring the output remains clear and correct.

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/MrMSD333/OS-Assignment3-Muhannad-Al-durayhim

**Number of commits**: 7 commits

**Commit messages**: 
1. Change my student id 
2. Add synchronization libraries (ReentrantLock & Semaphore)
3. Add locks and semaphore to SharedResources
4. Protect shared counters using ReentrantLock
5. Protect execution log using ReentrantLock
6. Add semaphore to control CPU access
7. Add semaphore handling in runToCompletion method

---

## Summary

**Total time spent on assignment**: 5 hours 29 minutes 

**Key takeaways**: 
1. I learned how race conditions occur in multithreaded systems and how they affect shared data consistency.
2. I understood the practical use of synchronization tools such as ReentrantLock and Semaphore to ensure thread safety.
3. I gained experience in designing and testing concurrent systems while maintaining correct execution order and consistent results.

**Most challenging aspect**: The most challenging part of the assignment was correctly implementing synchronization without introducing deadlocks or performance issues. Ensuring that all shared resources were properly protected while maintaining correct program execution required careful design and multiple testing iterations.

**What I'm most proud of**: I am most proud of successfully implementing a fully synchronized CPU scheduling simulation that correctly handles multiple processes, prevents race conditions, and produces consistent results across multiple executions. The program demonstrates real operating system concepts in a practical and working implementation.

---

**End of Documentation**
