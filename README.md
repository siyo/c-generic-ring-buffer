Philip Thrasher's Crazy Awesome Ring Buffer Macros!
===================================================

In ringbuffer.h are some naughty macros for easily owning and manipulating
generic ring buffers. Yes, they are slightly evil in readability, but they
are really fast, and they work great. If you don't like them, don't use them.
I use them because it's the only DRY and sane way to add generic ring buffers
to your C application.

Example usage:

```c
#include <stdio.h>

// So we can use this in any method, this gives us a typedef named 'intBuffer'.
// Really, this is the only *truely* naughty portion of these macros as it
// creates a complete type definition for you. This is something people will
// have to read the header in order to grok.
ringBuffer_typedef(int, intBuffer);

// The above macro translates exactly to:
typedef struct {
  int size;
  int start;
  int end;
  int* elems;
} intBuffer;


int main() {
  // Declare vars.
  intBuffer myBuffer;

  bufferInit(myBuffer,1024,int);

  // We must have the pointer. All of the macros deal with the pointer.
  // (except for init.)
  intBuffer* myBuffer_ptr;
  myBuffer_ptr = &myBuffer;

  // Write two values.
  bufferWrite(myBuffer_ptr,37);
  bufferWrite(myBuffer_ptr,72);

  // Read a value into a local variable.
  int first;
  bufferRead(myBuffer_ptr,first);
  assert(first == 37); // true

  int second;
  bufferRead(myBuffer_ptr,second);
  assert(second == 72); // true

  return 0;
}
```

