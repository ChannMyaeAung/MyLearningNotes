#### Union

- In C, a union is a user-defined data type that allows you to store different types of data in the same memory location.

- It is similar to a `struct` but with a key difference.

- In a union, all members share the same memory location. You can define a union with many members, but only one member can contain a value at any given time.

- In a `struct`, each member has its own memory space.

- A union lets you reuse memory space. The size of a union is always equal to the size of its largest data member.

  ```C
  quantity(might be a float or a short);
  
  // If a float takes 4 bytes, and a short takes 2, then this space will be 4 bytes long.
  ```

  

- Every time we create an instance of `struct`, the computer will lay out the fields in memory, one after the other.

- A union is different. A union will use the space for just one of the fields in its definition.

  - For instance, if we have a union called `quantity`, with fields called `count`, `weight`, and `volume`, and the computer will give the union enough space for its largest field, and then leave it up to you which value you will store in there. Whether you set the `count`, `weight` or `volume` field, the data will go into the same space in memory.

    ```C
    typedef union{
        short count;
        float weight;
        float volume;
    } quantity;
    
    // Each of these fields will be stored in the same space.
    ```

    

  - When we assign a value to a member of the union, it stores the value in its memory location. If you then assign  a value to a different member, it overwrites the previous value because both members share the same memory location.

- Unions are commonly used in situations where you need to conserve memory or when you want to represent a single value in different ways.




```C
union Data{
    int i;
    float f;
    char str[20];
}

int main(){
    union Data data;
    
    data.i = 10;
    printf("data.i : %d\n", data.i);
    
    data.f = 220.5;
    printf("data.f :%.2f\n", data.f);
    
    strcpy(data.str, "C Programming");
    printf("data.str :%s\n", data.str);
    
    return 0;
}
```



In this example, a union `Data` is defined with three members: i, f and `str`. 

- The size of the union will be the size of `str` because `str` is the largest member.
- When we assign a value to `data.i`, it is stored in the memory location of `data`.
- Then when we assign a value to `data.f`, it overwrites the value of `data.i`, because i and f share the same memory location. The same happens when we assign a value to `data.str`.
- So, when we print the values, only `data.str` will output the expected value ("C Programming") because it was the last one to be assigned. 
- The values of `data.i` and `data.f` will be lost.



#### How do we use a union?

1. C89 style for the first field.

   - To give the union a value for its first field, just wrap the value in braces:

     ```C
     quantity q = {4};
     
     // This means the quantity is a count of 4.
     ```

2. Designated initializers set other values.

   - A designated initializer sets a union field value by name, like this:

     ```C
     quantity q = {.weight=1.5};
     
     // This will set the union for a floating point weight value.
     ```

3. Set the value with dot notation

   - The third way of setting a union value is by creating the variable on one line and setting a field value on another line:

   - ```C
     quantity q;
     q.volume = 3.7;
     ```



**Remember: whichever way we set the union's value, there will only ever be one piece of data stored. The union just gives you a way of creating a variable that supports several different data types.**



Q: Why is a union always set to the size of the largest field?

A: The computer needs to make sure that a union is always the same size. The only way it can do that is by making sure it is large enough to contain any of the fields.



Q: Why does the C89 notation only set the first field? Why not set it to the first float if I pass it a float value?

A: To avoid ambiguity, if you had, let's say, a float and a double field, should the computer store `{2.1}` as a float or a double? By always storing the value in the first field, you know exactly how the data will be initialized.



- Designated initializers allow you to set `struct` and `union` fields by name and are part of the C99 C standard. 

- Objective C supports designated initializers but C++ does not.

- Designated initializers can be used to set the initial values of fields in `structs` as well.

- They can be very useful if you have a `struct` that contains a large number of fields and you initially just want to set a few of them.

  ```C
  typedef struct{
      const char *color;
      int gears;
      int height;
  } bike;
  bike b = {.height=17, .gears=21};
  ```

  

#### unions are often used with `struct`s

```C
typedef union{
    short count;
    float weight;
    float volume;
} quantity;

typedef struct{
    const char *name;
    const char *country;
    quantity amount;
} fruit_order;
```



We can access the values in the `struct/union` combination using the dot or `->` notation:

```C
fruit_order apples = {"apples", "England", .amount.weight=4.2};
// It's .amount because that's the name of the struct quantity variable.
// Here we are using a double designated identifier .amount for the struct and .weight for the .amount.

printf("This order contains %2.2f lbs of %s\n", apples.amount.weight, apples.name);

//This order contains 4.20 lbs of apples.
```



```C
margarita m = {2.0, 1.0, {0.5}};
// This one compiles perfectly. 

margarita m;
m = {2.0, 1.0, {0.5}};
// This one doesn't compile becase the compiler will only know that {2.0, 1.0, {0.5}} represents a struct if it's used on the same line that a struct is declared. When it's on a separate line, the compiler thinks it's an array.
```



- We can store lots of possible values in a union, but we have no way of knowing what type it was once it's stored.

- The compiler won't be able to keep track of the fields that are set and read in a union, so there's nothing to stop us setting one field and reading another. 

- Sometimes it can be a BIG PROBLEM.

  ```C
  #include <stdio.h>
  typedef union{
      float weight;
      int count;
  } cupcake;
  
  int main(){
      cupcake order = {2};
      printf("Cupcakes quantity: %i\n", order.count);
      return 0;
  }
  ```

  - In this code, let's say the code has set the weight not the count in the line `cupcake order ={2}` by mistake.

  - But on the next line, he's reading the count.

  - Let's see what happens when he runs the code:

  - ```C
    Cupcakes quantity: 1073731824
    ```

  - That's a lot of cupcakes.

  - We need some way of keeping track of the values we've stored in a union.

  - One trick is to create an `enum`.



#### An enum variable stores a symbol

Sometimes you don't want to store a number or a piece of text. Instead, you want to store something from a list of symbols.

`enum` short for enumeration in C is a user-defined data type that consists of integral constants. To define an `enum`, 

- We use `enum` keyword followed by `enum_name` (optional), and then in curly braces`{}`, we define a list of identifiers known as enumerators as values. 
- By default, the first enumerator has the value 0, and the value of each successive enumerator is increased by one.

enums let us create a list of symbols like this:

```C
enum colors{RED, GREEN, PUCE};

//Values are separated by commas.
```

- Any variable that is defined with a type of `enum colors` can then only be set to one of the keywords in the list. 

- So we might define an `enum colors` variables like this:

  ```C
  enum colors favorite = PUCE;
  ```

- The computer will just assign numbers to each of the symbols in our list, and the `enum` will just store a number. We don't need to worry about what the numbers are: our C code can just refer to the symbols. That'll make our code easier to read and it will prevent storing values like `RED` or `PUSE`:

  - ```C
    enum colors favorite = PUSE;
    
    // The computer will spot that this is not a legal value, so it won't compile.
    ```

  - Let's say we have days of the week.

    ```C
    enum Day {Sunday, Monday, Tuesday, Wednesday, Friday, Saturday};
    
    ```

  - In this `enum`, Sunday is 0, Monday is 1, Tuesday is 2 and so on.

- We can also assign specific values to the enumerators:

  - ```C
    enum Day {Sunday = 1, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday};
    ```

  - in this `enum`, Sunday is 1, Monday is 2, Tuesday is 3 and so on.

#### Bitfields

- Sometimes you want control at the bit level.

- Let's say we have a `struct` that contain a lot of yes/no values,

  ```C
  typedef struct{
      short low_level_vcf;
      short filter_coupler;
      short reverb;
      short sequential;
      ... // Each of these fileds will contain 1 for true or 0 for false.
  } syth;
  ```

  - Each field will use many bits.
  - A `short` field will take up a lot more space than the single bit that you need for `true/false` values. 
  - It's wasteful. It would be much better if we could create a `struct` that could hold a sequence of single bits for the values which is why `bitfields` are created.

- When we're dealing with binary value, it would be great if we had some way of specifying the 1s and 0s in a literal like:

  ```C
  int x = 01010100;
  ```

- Unfortunately, C doesn't support `binary literals`, but it does support `hexadecimal literals`.

- Everytime C sees a number beginning with 0x, it treats the number as base `6:

  ```C
  int x = 0x54;
  ```

- We can convert hex to binary one digit at a time.

  ```C
  0x54
  5 - 0101
  4 - 0100
  ```

- Each hexadecimal digit matches a binary digit of length 4.



#### Bitfields store a custom number of bits

A bitfield lets you specify how many bits an individual field will store.

```C
typedef struct{
    unsigned int low_pass_vcf:1;
    unsigned int filter_coupler:1;
    unsigned int reverb:1;
    unsigned int sequential:1;
    ...
        // Each field will only use 1 bit of storage.
}synth;
```

- By using bitfields, we can make sure each field takes up only one bit. 
- If we have  a sequence of bitfields, the computer can squash them together to save space.
- If we have eight single-bit bitfields, the computer can store them in a single byte.
- Bitfields can save space if they are collected together in a `struct`.
- If the compiler finds a single bitfield on its own, it might still have to pad it out to the size of a word. That's why bitfields are usually grouped together.



```C
typedef struct{
    unsigned int first_visit:1;
    unsigned int come_again:1;
    unsigned int fingers_lost:4;
    unsigned int shark_attack:1;
    unsigned int days_a_week:3;
} survey;
```



- 1 bit can store 2 values: true/false.
- 4 bits are needed to store up to 10.
- 3 bits can store numbers up to 7.

With 4 bits, you can represent 2*4 = 16 different values. Since we are starting counting from 0, the range of numbers you can represent is from 0 to 2^4 - 1 = 15. So, with 4 bits, you can represent values from 0 to 15.



Similarly with 3 bits, you can represent 2^3 = 8 different values.  Again, starting counting from 0, the range of numbers you can represent is from 0 to 2^3 - 1 = 7. So, with 3 bits, you can represent values from 0 to 7.



The formula 2^n represents the number of unique combinations you can have with n bits.



Q: Why doesn't C support binary literals?

A: Because they take up a lot of space, and it's usually more  efficient to write hex values.



Q: Why do I need 4 bits to store a value up to 10?

A: Four bits can store values from 0 to binary 1111 which is 15. But 3 bits can only store values up to binary 111, which is 7.



Q: So what if I try to put the value 9 into a 3-bit field?

A: The computer will store a value of 1 in it, because 9 is 1001 in binary so the computer transfer 001.



Q: Are bitfields really just used to save space?

A: No, they are important if you need to read low-level binary information such as if you're reading or writing some sort of custom binary file.



#### Bullet Points - Chapter 5

- A union allows you to store different data types in the same memory location.

- A designated initializer sets a field value by name.

- Designated initializers are part of the C99 Standard. They are not supported in C++.

- If you declare a union with a value in {braces}, it will be stored with the type of the first field.

  ```C
  #include <stdio.h>
  union SampleUnion{
      int integer;
      float floating_point;
      char character;
  };
  
  int main(){
      // Initializing with an integer value.
   	union SampleUnion myUnion = {10};
      
      //Accessing fields
      printf("Integer: %d\n", myUnion.integer);
      printf("Floating Point: %f\n", myUnion.floating_point);
      printf("Character: %c\n", myUnion.character);
      return 0;
      
  }
  ```

  - When we initialize `myUnion` with a value in braces `{10}`, it is stored with the type of the first field which is `integer`, and the other fields(`floating_point` and `character`) are left uninitialized.

    Code Execution:

    ```bash
    chan@CMA:~/C_Programming/practice
    $ make practice
    cc     practice.c   -o practice
    
    chan@CMA:~/C_Programming/practice
    $ ./practice
    Integer: 10
    Floating Point: 0.000000
    Character: 
    
    ```

- The compiler will let you store one field in a union and read a completely different field. But be careful. This can cause bugs.

- `enum`s store symbols.

- Bitfields allow you to store a field with a custom number of bits.

- Bitfields should be declared as `unsigned int` due to the following reasons:

  1. **Logical Operations**: Bit manipulation operations like shifting, masking, and logical operations (AND, OR, XOR) work more predictably with unsigned integers. Using signed integers might lead to unexpected results due to sign extension during operations.
  2. **Defined Behavior**: The C standard specifies the behavior of unsigned integer operations more precisely than signed integer operations. This makes the behavior of bit manipulation operations more predictable when using `unsigned int` for bitfields.
  3. **Avoiding Sign Extension**: When using signed integers for bitfields, there's a risk of sign extension if the most significant bit (MSB) is set. This can lead to unintended behavior when manipulating the bitfield. Using unsigned integers avoids this issue entirely.
  4. **Compiler Optimization**: Compilers often generate more efficient code when dealing with unsigned integers, especially in the context of bit manipulation operations.
  5. **Consistency**: Using `unsigned int` for bitfields ensures consistency in the codebase. Developers expect bitfields to behave like unsigned integers, so using signed integers might introduce confusion and make the code harder to understand.

- A `struct` combines data types together. You can initialize `struct`s with {array, like, notation}.

- You can read `struct` fields with dot notation. `->` notation lets you easily update fields using a `struct` pointer.

- Designated initializers let you set `struct` and union fields by name.

  ```C
  typedef struct{
      int x;
      int y;
      int z;
  } Point;
  
  int main(){
      struct Point p = {.y = 10, .z=20};
      // p = {0, 10, 20}
      
      return 0;
  }
  ```

  - We can also use designated initializers with arrays:

    ```C
    int arr[5] = {[20] = 10, [4] = 20};
    ```

- `typedef` lets you create an alias for a data type.

- `union` can hold different data types in one location.

- `enums` let you create a set of symbols.

- Bitfields give you control over the exact bits stored in a `struct`.

---



