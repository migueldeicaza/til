Enumerations on Swift are packed, and while I knew this was the case
for structures, I did not know that this was also the case for the
stack.  I incorrectly assumed that if an enum was declared as having a
backing type of "Int", it would occupy the size of an int on the
stack.  It occupied 1 byte, which was a problem, as I was passing the
pointer to the enum to a method expecting an Int pointer.

