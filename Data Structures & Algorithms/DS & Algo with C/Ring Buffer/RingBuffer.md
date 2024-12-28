## Ring Buffer

A **Ring Buffer** (also known as a **Circular Buffer**) is a fixed-size data structure that uses a single, fixed-size buffer as if it were connected end-to-end. It efficiently manages data in a **First-In-First-Out (FIFO)** manner, making it ideal for scenarios where data streams continuously, such as audio processing, networking, and real-time systems.

- The idea of a ring buffer is to store the deque elements in an array, with a pointer to the first element and a length field that says how many elements are in the deque. The information needed to manage the array (which is allocated using `malloc`) is stored in a `struct`.

**Key Features:**

- **Fixed Size:** Predefined capacity.
- **Circular Nature:** Wraps around to the beginning when the end is reached.
- **Efficient Memory Use:** Avoids the need to shift data after dequeue operations.
- **FIFO Behavior:** Maintains the order of data.

### Structure Definition

A ring buffer typically maintains three components:

1. **Buffer Array:** Holds the data.
2. **Head Pointer:** Points to the next position to dequeue (read).
3. **Tail Pointer:** Points to the next position to enqueue (write).

```C
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define BUFFER_SIZE 5 // Define the size of the ring buffer

typedef struct {
    int buffer[BUFFER_SIZE];
    int head; // Points to the next element to dequeue
    int tail; // Points to the next position to enqueue
    int count; // Number of elements in the buffer
} RingBuffer;
```



### C implementation # 1

#### Program Code

`practice.h`

```C
#define BUFFER_SIZE 5
typedef struct{
    int buffer[BUFFER_SIZE];
    int head;
    int tail;
    int count;
} RingBuffer;

void initRingBuffer(RingBuffer *rb);

bool enqueue(RingBuffer *rb, int data);

bool dequeue(RingBuffer *rb, int *data);

void displayBuffer(RingBuffer *rb);
```

`functions.c`

```C
void initRingBuffer(RingBuffer *rb){
    rb->head = 0;
    rb->tail = 0;
    rb->count = 0;
}

bool enqueue(RingBuffer *rb, int data){
    if(rb->count == BUFFER_SIZE){
        printf("Buffer is full. Cannot enqueue %d.\n", data);
        return false;
    }
    
    // Adds the new element at the tail position
    rb->buffer[rb->tail] = data;
    
    // Updates the tail pointer to the next position, wrapping around if necessary
    rb->tail = (rb->tail + 1) % BUFFER_SIZE;
    rb->count++;
    printf("Enqueued %d to buffer.\n", data);
    return true;
}

bool dequeue(RingBuffer *rb, int *data){
    if(rb->count == 0){
        printf("Buffer is empty. Cannot dequeue.\n");
        return false;
    }
    
    *data = rb->buffer[rb->head];
    rb->head = (rb->head + 1) % BUFFER_SIZE;
    rb->count--;
    printf("Dequeued %d from buffer.\n", *data);
    return true;
}

void displayBuffer(RingBuffer *rb){
    printf("Buffer state: ");
    // Iterates thru the buffer from the head to the tail, printing each element.
    for(int i = 0; i < rb->count; i++){
        // Uses modulo arithmetic to handle the circular nature of the buffer.
        int index = (rb->head + i) % BUFFER_SIZE;
        printf("%d ", rb->buffer[index]);
    }
    printf("\n");
}
```

`practice.c`

```C
int main()
{
    RingBuffer rb;
    initRingBuffer(&rb);

    // Enqueue elements
    enqueue(&rb, 10);
    enqueue(&rb, 20);
    enqueue(&rb, 30);
    enqueue(&rb, 40);
    enqueue(&rb, 50);
    enqueue(&rb, 60); // should indicate buffer is full

    displayBuffer(&rb); // expected output: 10 20 30 40 50

    // Dequeue elements
    int data;
    dequeue(&rb, &data);
    dequeue(&rb, &data);

    displayBuffer(&rb); // expected output: 30 40 50

    // Enqueue more elements
    enqueue(&rb, 60);
    enqueue(&rb, 70);

    displayBuffer(&rb); // expected output: 30 40 50 60 70

    // Attempt to enqueue another element
    enqueue(&rb, 80); // should indicate buffer is full
    return 0;
}
```

##### Visualization

Initial State:

```makefile
Buffer: [ _ | _ | _ | _ | _ ]
Head: 0
Tail: 0
Count: 0
```

After Enqueue 10:

```makefile
Buffer: [10 | _ | _ | _ | _ ]
Head: 0
Tail: 1
Count: 1
```

After Enqueue 20:

```makefile
Buffer: [10 | 20 | _ | _ | _ ]
Head: 0
Tail: 2
Count: 2
```

After Enqueue 30:

```makefile
Buffer: [10 | 20 | 30 | _ | _ ]
Head: 0
Tail: 3
Count: 3
```

After Enqueue 40:

```makefile
Buffer: [10 | 20 | 30 | 40 | _ ]
Head: 0
Tail: 4
Count: 4
```

After Enqueue 50:

```makefile
Buffer: [10 | 20 | 30 | 40 | 50 ]
Head: 0
Tail: 0 (wraps around)
Count: 5
```

Attempting to Enqueue 60 (Buffer Full):

```makefile
Buffer: [10 | 20 | 30 | 40 | 50 ]
Head: 0
Tail: 0
Count: 5
```

- **Result:** Cannot enqueue 60. Buffer is full.

After Dequeue 10 and 20:

```makefile
Buffer: [10 | 20 | 30 | 40 | 50 ]
Head: 2
Tail: 0
Count: 3
```

- **Elements in Buffer:** 30, 40, 50

After Enqueue 60 and 70:

```makefile
Buffer: [60 | 70 | 30 | 40 | 50 ]
Head: 2
Tail: 2
Count: 5
```

- **Buffer Wraps Around:** Tail moves to position 2 after enqueuing 60 and 70.

Attempting to Enqueue 80 (Buffer Full):

```makefile
Buffer: [60 | 70 | 30 | 40 | 50 ]
Head: 2
Tail: 2
Count: 5
```

- **Result:** Cannot enqueue 80. Buffer is full.

**Diagram Representation:**

```yaml
Step-by-Step Enqueue and Dequeue Operations:

Initial State:
Buffer: [ _ | _ | _ | _ | _ ]
Head: 0, Tail: 0, Count: 0

Enqueue 10:
Buffer: [10 | _ | _ | _ | _ ]
Head: 0, Tail: 1, Count: 1

Enqueue 20:
Buffer: [10 | 20 | _ | _ | _ ]
Head: 0, Tail: 2, Count: 2

Enqueue 30:
Buffer: [10 | 20 | 30 | _ | _ ]
Head: 0, Tail: 3, Count: 3

Enqueue 40:
Buffer: [10 | 20 | 30 | 40 | _ ]
Head: 0, Tail: 4, Count: 4

Enqueue 50:
Buffer: [10 | 20 | 30 | 40 | 50 ]
Head: 0, Tail: 0, Count: 5

Attempt Enqueue 60 (Full):
Buffer remains unchanged.
Cannot enqueue 60.

Dequeue 10:
Buffer: [10 | 20 | 30 | 40 | 50 ]
Head: 1, Tail: 0, Count: 4

Dequeue 20:
Buffer: [10 | 20 | 30 | 40 | 50 ]
Head: 2, Tail: 0, Count: 3

Enqueue 60:
Buffer: [60 | 20 | 30 | 40 | 50 ]
Head: 2, Tail: 1, Count: 4

Enqueue 70:
Buffer: [60 | 70 | 30 | 40 | 50 ]
Head: 2, Tail: 2, Count: 5

Attempt Enqueue 80 (Full):
Buffer remains unchanged.
Cannot enqueue 80.
```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Enqueued 10 to buffer.
Enqueued 20 to buffer.
Enqueued 30 to buffer.
Enqueued 40 to buffer.
Enqueued 50 to buffer.
Buffer is full. Cannot enqueue 60.
Buffer state: 10 20 30 40 50 
Dequeued 10 from buffer.
Dequeued 20 from buffer.
Buffer state: 30 40 50 
Enqueued 60 to buffer.
Enqueued 70 to buffer.
Buffer state: 30 40 50 60 70 
Buffer is full. Cannot enqueue 80.
```



### C Implementation # 2

#### Program Code

`practice.h`

```C
#define INITIAL_SIZE 8
#define DEQUE_FRONT 0
#define DEQUE_BACK 1
#define DEQUE_EMPTY -1
struct deque
{
    size_t base;
    size_t length;
    size_t size;
    int *contents;
};
typedef struct deque Deque;

Deque *dequeCreate(void);
void dequePush(Deque *d, int direction, int value);
int dequePop(Deque *d, int direction);
int dequeIsEmpty(const Deque *d);
void dequeDestroy(Deque *d);
```

`functions.c`

```C
static Deque *dequeCreateInternal(size_t size){
    Deque *d = malloc(sizeof(Deque));
    assert(d);
    
    d->base = 0;
    d->length = 0;
    d->size = size;
    
    d->contents = malloc(sizeof(int) * d->size);
    assert(d->contents);
    
    return d;
}

Deque *dequeCreate(void){
    return dequeCreateInternal(INITIAL_SIZE);
}

void dequePush(Deque *d, int direction, int value){
    // replacement deque if we grow
    Deque *d2;
    
    // old contents of d
    int *oldContents;
    
    // first make sure we have space
    // resize if deque is full
    if(d->length == d->size){
        d2 = dequeCreateInternal(d->size * 2);
        
        while(!dequeIsEmpty(d)){
            dequePush(d, DEQUE_BACK, dequePop(d, DEQUE_FRONT));
        }
        
        // do a transplant from d2 to d
        // but save old contents so we can free them
        oldContents = d->contents;
        *d = *d2; // this is equivalent to copying the components one by one
        free(oldContents);
        free(d2);
    }
    
    // depending on driection, updates base and inserts the value
    if(direction == DEQUE_FRONT){
        // d->base is unsigned, so we have to check for zero first
        if(d->base == 0){
            d->base = d->size - 1;
        }else{
            d->base--;
        }
        
        d->length++;
        d->contents[d->base] = value;
    }else{
        d->contents[(d->base + d->length++) % d->size] = value;
    }
}

int dequePop(Deque *d, int direction){
    int retval;
    
    if(dequeEmpty(d)){
        return DEQUE_EMPTY;
    }
    
    if(direction == DEQUE_FRONT){
        // base goes up by one, length goes down by one
        retval = d->contents[d->base];
        
        d->base = (d->base + 1) % d->size;
        d->length--;
        
        return retval;
    }else{
        return d->contents[(d->base + --d->length) % d->size];
    }
}

int dequeIsEmpty(const Deque *d){
    return d->length == 0;
}

void dequeDestroy(Deque *d){
    free(d->contents);
    free(d);
}
```

`practice.c`

```C
int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        fprintf(stderr, "Usage: %s <iterations>\n", argv[0]);
        return 1;
    }

    long iterations = atol(argv[1]);
    if (iterations <= 0)
    {
        fprintf(stderr, "Iterations must be a positive integer.\n");
        return 1;
    }

    Deque *d = dequeCreate();

    // push elements to the front and back
    for (long i = 0; i < iterations; i++)
    {
        dequePush(d, DEQUE_FRONT, i);
        dequePush(d, DEQUE_BACK, i + iterations);

        // Print only the first 10 iterations
        if (i < 10)
        {
            printf("Pushed Front: %ld, Back: %ld\n", i, i + iterations);
        }
    }

    // pop elements from the front and back
    for (long i = 0; i < iterations; i++)
    {
        int front = dequePop(d, DEQUE_FRONT);
        int back = dequePop(d, DEQUE_BACK);
        if (front != DEQUE_EMPTY && back != DEQUE_EMPTY)
        {
            if (i < 10)
            {
                printf("Pop from front: %d\n", front);
                printf("Pop from back: %d\n", back);
            }
        }
    }

    // check if the deque is empty
    printf("Deque is empty: %s\n", dequeIsEmpty(d) ? "Yes" : "No");

    // destroy the deque
    dequeDestroy(d);

    return 0;
}
```

`Makefile`

```makefile
# Compiler and standard
CC = clang
STD = -std=c23
# Compiler flags
CFLAGS = -Wall -Wextra -g
LIBS = -lpthread -lm -lssl -lcrypto

# Directories
LIBDIR = ./libs
OBJDIR = ./obj

# Iterations 
ITERATIONS = 10000000
VALGRIND_ITERATIONS = 100

all:practice

practice: $(OBJDIR)/practice.o $(LIBDIR)/libfunctions.a
	$(CC) $(STD) $(OBJDIR)/practice.o -L$(LIBDIR) -lfunctions -o practice $(LIBS)

$(OBJDIR)/practice.o: practice.c practice.h 
	$(CC) $(STD) $(CFLAGS) -c practice.c -o $(OBJDIR)/practice.o 

$(OBJDIR)/functions.o: functions.c practice.h
	$(CC) $(STD) $(CFLAGS) -c functions.c -o $(OBJDIR)/functions.o 

$(OBJDIR)/functions_2.o: functions_2.c practice.h 
	$(CC) $(STD) $(CFLAGS) -c functions_2.c -o $(OBJDIR)/functions_2.o

$(LIBDIR)/libfunctions.a: $(OBJDIR)/functions.o $(OBJDIR)/functions_2.o 
	ar -rcs $(LIBDIR)/libfunctions.a $(OBJDIR)/functions.o $(OBJDIR)/functions_2.o

run: practice
	./practice $(ITERATIONS)

valgrind: practice
	valgrind --leak-check=full ./practice $(VALGRIND_ITERATIONS)

clean:
	@echo "Removing everything except the source files..."
	rm -f $(OBJDIR)/*.o $(LIBDIR)/libfunctions.a practice
```



#### Program Output

- Ran 100 times to test the code

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 100
==15787== Memcheck, a memory error detector
==15787== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==15787== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==15787== Command: ./practice 100
==15787== 
Pushed Front: 0, Back: 100
Pushed Front: 1, Back: 101
Pushed Front: 2, Back: 102
Pushed Front: 3, Back: 103
Pushed Front: 4, Back: 104
Pushed Front: 5, Back: 105
Pushed Front: 6, Back: 106
Pushed Front: 7, Back: 107
Pushed Front: 8, Back: 108
Pushed Front: 9, Back: 109
Pop from front: 99
Pop from back: 199
Pop from front: 98
Pop from back: 198
Pop from front: 97
Pop from back: 197
Pop from front: 96
Pop from back: 196
Pop from front: 95
Pop from back: 195
Pop from front: 94
Pop from back: 194
Pop from front: 93
Pop from back: 193
Pop from front: 92
Pop from back: 192
Pop from front: 91
Pop from back: 191
Pop from front: 90
Pop from back: 190
Deque is empty: Yes
==15787== 
==15787== HEAP SUMMARY:
==15787==     in use at exit: 0 bytes in 0 blocks
==15787==   total heap usage: 13 allocs, 13 frees, 3,232 bytes allocated
==15787== 
==15787== All heap blocks were freed -- no leaks are possible
==15787== 
==15787== For lists of detected and suppressed errors, rerun with: -s
==15787== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

```

### Use Cases

**Ring Buffers** are particularly useful in scenarios where data streams continuously, and there’s a need for efficient, fixed-size buffering without dynamic memory allocation. Some common real-world applications include:

1. **Data Streaming:**
   - **Audio and Video Processing:** Managing real-time data streams where data is continuously produced and consumed.
   - **Networking:** Handling incoming and outgoing packets in network devices.
2. **Embedded Systems:**
   - **Microcontrollers:** Efficiently managing limited memory resources for real-time tasks.
3. **Operating Systems:**
   - **I/O Buffers:** Managing data between hardware devices and applications.
4. **Circular Queues:**
   - Implementing fixed-size queues in situations where the queue size is known and constant.