+++
title = "Passing the Baton"
date = "2016-07-11T18:00:00+07:30"
tags = ["interview", "semaphores"]
draft = false
author = "Amitabh Das"
+++

In my last post I said that I would write about another design pattern for semaphores – passing the baton. This is the continuation of my previous post. Here is the link to my previous post:  [A Semaphore Story](/posts/semaphore) 

# What we know

Let’s look at the incorrect solution to the previous problem.

```
mutex.wait()
# Case 3. Wait for table_empty signal
if customers == 5 || waiting > 0:
    waiting += 1
    mutex.signal()
    table_empty.wait()

  # Take mutex again since you need to modify variables
    mutex.wait()
    waiting -= 1    # Need to re-acquire mutex again
    customers += 1    # New customers could take it first
    mutex.signal()

# Case 1 and Case 2
else:
    customers += 1
    mutex.signal()

# oh yeah
EAT()

mutex.wait()
customers -= 1
# Once customers count is down to 0, send table_empty signal
if customers == 0:
    table_empty.signal(min(5, waiting))
mutex.signal()
```

# So, what was the problem?

The flaw in this solution is that the waiting customers (blocked processes) may not be able to take a seat at the table because the new customers (new processes) may beat them to it. This happens when the customer is waiting on `table_empty` and then again needs to wait on `mutex` to modify the variables: `waiting` and `customers`.

In the previous post, the last customer while leaving, would acquire the lock on `mutex` and would unblock the waiting customers to enter the table. And thus, re-acquiring the lock on `mutex` would no longer be needed.

# What’s passing the baton?

Passing the baton solves the same problem - re-acquiring the lock on `mutex`.

Imagine the runners in a relay race. Each runner passes the baton to the next one, then the new runner would pass the baton on to the next runner and so on till they reach the end. Similarly, we could acquire the lock in one process and release it in another process!

Of course, semaphores were not meant to be used this way but as long as the two processes make sure that they are coordinating, it will work.

# You shall pass!

In our problem, we can have the last customer to not release the lock on `mutex` and unblock the next customer waiting in line. And the next customer would modify the variables without needing to take on the lock, just like the runner passing the baton (in this case, we are passing the lock on `mutex` to the next process). And now, this new customer would check if the table still has seats remaining and if there are customers waiting. If yes, then he would wake up the next waiting customer and the process repeats till we reach the last waiting customer, who would call `mutex.signal()` finally releasing the lock on `mutex` so that the new customers can now try and acquire the lock.

The modified solution would look something like this:

```
mutex.wait()
# Case 3. Wait for table_empty signal
if customers == 5 || waiting > 0:
    waiting += 1
    mutex.signal()
    table_empty.wait()
    # We already have the lock at this point
    waiting -= 1

customers += 1

# Pass the lock on to other waiting customers if there 
# is a place at the table
if waiting > 0 && customers < 5:
    table_empty.signal()
else:
# Coming here means that there are no waiting customers
# And we can finally release mutex
    mutex.signal()

# oh yeah
EAT()

mutex.wait()
customers -= 1
# last customer passes lock if there are waiting customers
if customers == 0 && waiting > 0:
    table_empty.signal()
else: 
# Coming here means that there are no waiting customers
# And we can finally release mutex
    mutex.signal()
```

# Conclusion

Think of the _mutex_ as a pen. Each customer takes the pen to write down his name and details in the register of the restaurant while entering. And each have to sign off as they leave. Each time they take the pen, they return it to the waiter at the restaurant.

**THERE IS ONLY ONE PEN!** Whoever gets to the pen first is allowed to use it. Waiter has no idea of the people who are waiting and when he has the pen, he could give the pen to whoever is asking for the pen at the very next moment. Now the above problem can be solved in two steps.

1.  Customer comes in. If table is full, he waits for table to become empty else he takes the pen from waiter or from another customer and enters his details in the register. If there is no space at the table, he has to give the pen back to the waiter else he passes the pen on to one of the waiting customers or back to the waiter if there is no one waiting.
2.  After customer is done eating, take the pen from waiter and sign off. If he is the last one at the table, he passes the pen on to one of the waiting customers or back to the waiter if there is no one waiting.
