Download Link: https://assignmentchef.com/product/solved-coen12-project-3-sets-arrays-and-hash-tables
<br>
In this project, you will implement a set abstract data type, first for strings and then for generic pointer types. Your interface and implementation must be kept separate. Separate source files that provide main will be provided for testing your data type.

<h1>1        AnADTforStrings</h1>

<h2>1.1       Interface</h2>

The interface to your abstract data type must provide the following operations:

<ul>

 <li>SET *createSet(int maxElts); return a pointer to a new set with a maximum capacity of <em>maxElts</em></li>

 <li>void destroySet(SET *sp); deallocate memory associated with the set pointed to by <em>sp</em></li>

 <li>int numElements(SET *sp); return the number of elements in the set pointed to by <em>sp</em></li>

 <li>void addElement(SET *sp, char *elt); add <em>elt </em>to the set pointed to by <em>sp</em></li>

 <li>void removeElement(SET *sp, char *elt); remove <em>elt </em>from the set pointed to by <em>sp</em></li>

 <li>char *findElement(SET *sp, char *elt); if <em>elt </em>is present in the set pointed to by <em>sp </em>then return the matching element, otherwise return NULL</li>

 <li>char **getElements(SET *sp); allocate and return an array of elements in the set pointed to by <em>sp</em></li>

</ul>

<h2>1.2       Implementation</h2>

Implement a set using a hash table of length <em>m &gt; </em>0 and linear probing to resolve collisions. Create an auxiliary function search that contains all of the search logic as you did for the previous assignment, and use search to implement the functions in your interface. The following hash function should be used:

unsigned strhash(char *s) { unsigned hash = 0;

while (*s != ’ ’) hash = 31 * hash + *s ++;

return hash;

}

Asinthepreviousassignment, yourimplementationshouldallocatememoryandcopythestringwhenadding, and therefore also deallocate memory when removing.

1

<h1>2        AnADTforGenericPointerTypes</h1>

So far, we have only developed ADTs for strings. If we wanted to store another type of data, we would need to copy our implementation and change “char *” to the new type, which is both tedious and error-prone. Fortunately, C provides a <strong><em>genericpointertype</em></strong>: a pointer to void can be assigned to or from any other pointer type. For example, the function malloc returns a pointer to void so its result can be assigned to any pointer type. Similarly, the function free takes a pointer to void as a parameter so it can be passed any pointer type. By changing “char *” to “void *” in our implementation, we can write an ADT that works on generic pointer types, allowing us to store strings, pointers to structures, or whatever we like. The test program counts uses a generic set ADT to store structures rather than just strings in order to count the number of times each word occurs in a file.

Unfortunately, we need to do a little more than just replace “char *” with “void *” in our implementation. Our implementation needs to be told how to compare two elements as well as compute the hash value for an element. After all, strcmp and strhash only work on strings. Therefore, our createSet function must take extra parameters that are now pointers to these two functions:

SET *createSet(int maxElts, int (*compare)(), unsigned (*hash)());

These two functions must be stored internally as part of the set structure. Rather than calling strcmp and strhash in the function search, you will need to call the client-provided functions instead. Assuming that the pointer to the set is sp and the comparison function is stored as a member called compare:

(*sp-&gt;compare)(…);

Our new generic set ADT also does not know how to allocate or deallocate the elements it stores, so rather than calling strdup and free, it simply copies the pointers themselves. The client is responsible for managing memory. Such is the price we pay for using a generic ADT.

Once you have completely finished your set ADT for strings, copy it to the generic directory and modify it so it uses generic pointer types instead of strings. You can test it against the unique and counts programs in that directory. Note that since the interface is slightly different, you cannot test your implementation against the programs in the strings directory. Moving forward, all future ADTs we design will be generic ADTs.