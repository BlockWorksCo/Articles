---
title: "Complexity"
date: 2019-01-27T19:59:54Z
showDate: false
draft: false
tags: ["blog","story"]
---

# Inherent complexity 
(a.k.a. Essential complexity)

Given a task to solve, the inherent complexity of the problem is a statement about how complex the problem is, *not*
about how complex *your solution* to the problem is.

This is the complexity of the problem domain. You cannot do anything to change this.

# Accidental complexity

The accidental complexity of a task on the other hand is a statement about how much complexity your solution has added on top of the
inherent complexity.
A better phrase maybe "implementation complexity".

We, as software developers directly influence (and cause) the complexity, so it is up to us to attempt to minimise it.

--------------

Good, well-engineered software should aim to minimise the accidental complexity.

How do we do this?

The first step is to identify what is the essential complexity of the problem at hand.

For example, consider the problem of summing a list of numbers.
At a minimum, we need a way of accumulating a value. We also need a way of reading each value in the list and way of identifying 
when we have completed.

Lets take this in reverse, from simple to complex.


~~~~~~~~~~~~~~~~
int sum1( int list[10] )
{
    int total   = 0;
    for(int i=0; i<10; i++)
    {
        total   = total + list[i];
    }

    return total;
}
~~~~~~~~~~~~~~~~
This requires nothing but the base language.
There are no libraries, no additional data structures, no macros and most importantly, no additional knowledge of concepts 
necessary to understand the function.
The functions operation is also clear without the use of comments.
It could also be translated straight into any other language with no issues.

The cognitive load of this function is very low.

Now, lets step it up a little:
~~~~~~~~~~~~~~~~~
#include <vector>

int sum2( vector<int> list )
{
    int total   = 0;
    for(int i=0; i<list.size(); i++)
    {
        total   = total + list[i];
    }

    return total;
}
~~~~~~~~~~~~~~~~~
Now, we have added the concept of a vector, data structres and generics/templates.

Lets make it a bit more like real life code...
~~~~~~~~~~~~
class Summable
{
public:
    virtual int sumOf() = 0;
};

class MyList : public Summable
{
public:
    MyList()
    {
        list.push_back(1);
        list.push_back(2);
        list.push_back(3);
        list.push_back(4);
    }
    
    int sumOf()
    {
        int total   = 0;
        for(int value: list)
        {   
            total   += value;
        }
        
        return total;
    }

private:
    std::vector<int>     list;
};
~~~~~~~~~~~~

Ok, what concepts do we need to know about here?

* Classes.
* Objects.
* Templates.
* Iterators.
* Interfaces.
* Data hiding.

We could go on and on, now this is admittedly a toy example and there are genuine reasons for adding each feature. 
Nevertheless, you have to consider that the essential complexity of the operation you're performing is *much* less than
the end result. 

This is the accidental complexity. 

At each stage, we add seemingly harmless concepts and assumed-knowledge, all in the best of intentions. The reality though
is that we've added significant amounts of complexity. Admit it, you couldn't glance at the last example and grok it could you?
Whereas the first you simply absorbed without thought.
The first example can be read in your sleep. The last example on the other hand requires significant thought, even for this trivial example.



------------------------

# Architectural complexity

Above, we described the complexity of a particular implementation.
The architecture of the software (and system) as a whole also need to be taken into account.

--------------------

In summary, reduce the number of moving parts and document everything.

* Remove unnecessary abstractions (YAGNI, etc).
* Reduce unnecessary dependencies
* Reduce required knowledge.
* Reduce the number of features of a language we use.
* Dont attempt to be overly flexible.
* Remove legacy code and features.
* Consistency.

----------------

# Further reading.

* [No Silver Bullet](http://faculty.salisbury.edu/~xswang/Research/Papers/SERelated/no-silver-bullet.pdf)


