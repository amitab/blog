+++
title = "A Semaphore Story"
date = "2016-06-08T18:00:00+07:30"
tags = ["interview", "semaphores"]
draft = false
author = "Amitabh Das"
+++

In one of my recent interviews, I was asked this one problem based on semaphores which was quite challenging. I love interviews where I use the concepts I have learnt and need to derive the answer to a complex problem using the base concepts I know of. In this case, I had learnt about semaphores back in engineering and still remains one of my favorite OS concepts.

# The Situation

So the interviewer asks “So, what's your favorite restaurant?” I replied “I like Burger King” (Since it had come up quite recently in Bangalore). Then he asks, “Alright. There is one table in Burger King with 5 seats. A person who comes to eat here will take a seat only if there are less than 5 people already at the table. If there are 5 people already, he will assume that a family is dining and will wait till the table is completely empty and only then join the table. Assuming each customer to be a thread, write a program to simulate this behavior using semaphores.”

# Initial Approach

“Alright.”, I said.

The normal cases would be:

1.  Table is empty – Walk in, eat and leave.
2.  Table has less than 5 people – Same as above.
3.  Table has 5 people or people are already waiting – The customer waits for the table to become empty.

And the last guy needs to wake up the waiting customers.

So I write this solution:

*   **mutex** – a mutex lock to provide mutual exclusion to modifying variables
*   **table_empty** – semaphore to indicate the table is empty or not. Initialized to 1.
*   **customers** – total number of customers at the table
*   **waiting** – customers that are waiting for a seat

```
mutex.wait()
# Case 3. Wait for table_empty signal
if customers == 5 || waiting > 0:
	waiting += 1
	mutex.signal()
	table_empty.wait()

  # Take mutex again since you need to modify variables
	mutex.wait()
	waiting -= 1
	customers += 1
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

# Hit and Miss

**TADA!** I called the interviewer and asked him to inspect the solution.

Then he says, “Good start.”. My face was like :(

Continuing, he explains why I was wrong. “Suppose there were people waiting and the table had 5 customers. Now, if new customers come in, it's possible that the waiting customers will not get seats.”

Ok, so after getting the `table_empty` signal, the waiting customers will fight with the new customers over mutex. We don't know who will win here. If only there were a way to make sure that only the waiting people are served before the new guys... hold on... what if we made sure?

Of course!

# Correcting the Idea

We can get the last customer to make the changes before giving away the mutex so that the incoming customers can see the right state on arrival! So the modified solution would be:

```
mutex.wait()
# Case 3. Wait for table_empty signal
if customers == 5 || waiting > 0:
	waiting += 1
	mutex.signal()
	table_empty.wait()
	# Start eating as soon as you wake up!

 # Case 1 and Case 2
else:
	customers += 1
	mutex.signal()

EAT() # oh yeah

mutex.wait()
customers -= 1
# Once customers count is down to 0, send table_empty signal
if customers == 0 and waiting > 0:
  # Take care of waiting customers here instead
	batch = min(5, waiting)
	waiting -= batch
	customers += batch
	table_empty.signal(batch)
mutex.signal()
```

# Conclusion

Turns out, it's one of the famous semaphore problems called **The sushi problem** inspired by a problem proposed by _Kenneth Reek_. He calls the method I’ve used above as _I’ll do it for you_ approach, owing to the fact that the last customer wakes up everyone before leaving. He has also proposed another way to solve this problem called _pass the baton_ method. I will be posting about this one in the next post. For more information, you can refer this link: 

[https://www.researchgate.net/publication/221538472_Design_patterns_for_semaphores](https://www.researchgate.net/publication/221538472_Design_patterns_for_semaphores)
