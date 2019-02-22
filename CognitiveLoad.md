---
title: "Cognitive load"
date: 2019-02-17T22:21:33Z
draft: false
---

The concept of cognitive load comes from psychology and refers to the amount of effort required to maintain items
in working memory.

Our working memory has its limits, generally we can hold between 5 and 9 things in memory at one time.

The process of software development relies heavily on the developer understanding large portions of the codebase and
doing this requires them to juggle many quite abstract concepts in their head simultaneously. 
This directly relates to cognitive load.

Relationship to software engineering
------------------------------------

Code can be written in many different styles and still give the same behaviour.
Different styles will cause differing amounts of cognitive load on the reader as they attempt to understand it.


Example
-------

Consider the following toy code:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~
void bubbleSort(int arr[], int n) 
{ 
   int i, j; 
   for (i = 0; i < n-1; i++)       
       for (j = 0; j < n-i-1; j++)  
           if (arr[j] > arr[j+1]) 
              swap(&arr[j], &arr[j+1]); 
} 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If we consider the cognitive load required to understand the above code and attempt to reduce or remove it, we can 
end up with the following:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~
// A function to implement bubble sort 
void bubbleSort(int arr[], int n) 
{ 
    for (int i = 0; i<n-1; i++)       
    {
        // Last i elements are already in place    
        for (int j = 0; j < n-i-1; j++)  
        {
            if (arr[j] > arr[j+1]) 
            {
                swap( &arr[j], &arr[j+1] ); 
            }
        }
    }
} 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

What have we done here?

- Naming.
- Grouping.
- Commenting.
- Density.


A heavy cognitive load typically creates error or some kind of interference in the task at hand

Dont make the reader rearrange code in their head, i.e. using Yoda conditions.

commented code reduces cognitive load by not having to mentally keep track of what the code is doing at any given point.

Naming is important; names mean we don't have to mentally map between what a variable is doing and what its actual name
is. Bad naming is not neutral, it actively harms this as we naturally assume particular operations from particular names.

Style is important; Be consistent and (hopefully) clear. A change in style is disruptive to the mental process of 
grokking code.

Keep a low cyclomatic complexity, break up large functions into small, well-named ones.

When trying to understand a bad codebase, rewrite the names into what you understand the function to be.

Single responsibility principle. One thing at a time.

Relation to complexity
----------------------


https://chrismm.com/blog/writing-good-code-reduce-the-cognitive-load/


Readbility, whitespace, comments.


Reduce tool usage.

Relationship to abstractions
----------------------------



"programs must be written for people to read, and only incidentally for machines to execute" (Abelson & Sussman, "Structure and Interpretation of Computer Programs", preface to the first edition)

