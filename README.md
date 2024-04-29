# Radix Converter

A program written in 'C' intended to perform number-base conversions.

I will use stack and queue for performing number-base conversions and hence the following sections may contain discussions about stack and queues.

### The Homework
---
```
Me: I am planning to write a 'C' code to make a decimal to binary converter FUNCTION. I am clear with the logic and how things are going to proceed, I am faced by a dilemma. 

First, instead of storing each bit, I could just output it on the screen. But this would not make the binary conversion much useful because the converted quantity is not stored anywhere. 

Second, if I try to store the converted binary, I am unable to decide over which data structure to use: - Should I go with the primary data structure such as 'int' or shall I go with strings? In case of strings, I face another problem that I have to define the string size way before any computation has taken place; what if the computation results in a value much larger than the size of the string!?
```
```
GPT: You've highlighted some important considerations in designing a decimal to binary converter. Let's break down your concerns:

Outputting Binary Digits vs. Storing the Binary Number:
Outputting binary digits directly to the screen can be useful for simple conversions or when you don't need to retain the binary representation.

However, if you want to use the binary number for further calculations or processing, storing it in memory is necessary.
Choosing a Data Structure:

Using an int data type to store the binary number is straightforward and efficient, especially if you're dealing with fixed-size integers. You can use bitwise operations to manipulate individual bits.

Using a string (character array) to store the binary number allows for more flexibility in handling variable-length binary numbers. However, as you mentioned, you need to allocate memory for the string, and it may become inefficient for very large numbers.
Given these considerations, here are some suggestions:

If you're working with fixed-size integers and the binary number won't exceed a certain number of bits (e.g., 32 or 64 bits), using an int or long long int would be a good choice. You can use bitwise operations (<<, >>, &, |, ^) to manipulate bits and convert the decimal number to binary efficiently.

If you need to handle very large binary numbers or if the size of the binary number is unknown beforehand, using a dynamic data structure like a dynamically allocated array or a dynamically allocated string (character array) could be a better option. You can allocate memory dynamically based on the size of the binary number.

If you're concerned about efficiency and memory usage, you can choose an appropriate balance between fixed-size integers and dynamically allocated memory. For example, you can use fixed-size integers for small to medium-sized numbers and switch to a dynamically allocated string for very large numbers.

Consider the requirements of your application, the range of numbers you'll be dealing with, and the trade-offs between efficiency, memory usage, and flexibility when choosing the data structure for storing the binary number.
```
---

### Why track both the first and last elements of a queue?
---
```
Me: Do i need to keep track of the last element in a queue?
```

```
GPT: It's typically necessary to keep track of both the first and last elements in a queue. While you can implement a queue with just a pointer to the first element, having a pointer to the last element provides several advantages:

Efficient Enqueue Operation: With a pointer to the last element (lastQueue), you can enqueue elements in constant time (O(1)). Without it, you would need to traverse the entire queue to find the last element, resulting in a linear time complexity (O(n)) for each enqueue operation.

Simplifies Dequeue Operation: Having both first and last pointers makes dequeuing (removing elements from the front of the queue) simpler and more efficient, especially in a singly linked list implementation. With just a pointer to the first element, removing the first element would require traversing the queue to find the new first element, resulting in linear time complexity (O(n)). With a pointer to the last element, the dequeue operation can be done in constant time (O(1)).

Implementation Flexibility: Keeping track of both the first and last elements allows for more flexibility in implementing various queue operations, especially in scenarios where you need to efficiently access both ends of the queue.

While it's technically possible to implement a queue with just a pointer to the first element, maintaining a pointer to the last element significantly improves the efficiency and simplicity of enqueue and dequeue operations, making the queue more practical for real-world use cases.
```
---

### A Mistake!
---
#### Original Code
```C
void popStackAll() {
    if(topStack == NULL) {
        printf("Stack empty! - Underflow");
    }
    else {
        struct elementStack* temp = NULL;
        while(topStack) {
            printf("%c ", topStack -> val);
            temp = topStack;
            topStack = topStack -> prev;
            free(temp);
        }
        printf("\n");
    }
    return;
}
```
> Mistake: After popping all elements from the stack, you should set topStack to NULL to signify an empty stack. Otherwise, topStack will still point to the last element that was popped, which could lead to undefined behavior if you attempt to use the stack after popping all elements.

#### Corrected Code!
```C
void popStackAll() {
    if(topStack == NULL) {
        printf("Stack empty! - Underflow\n");
    }
    else {
        struct elementStack* temp = NULL;
        while(topStack) {
            printf("%c ", topStack -> val);
            temp = topStack;
            topStack = topStack -> prev;
            free(temp);
        }
        printf("\n");
        topStack = NULL; // Reset topStack to NULL to signify an empty stack
    }
    return;
}
```
> I committed a similar mistake in dequeuing as well and corrected it accordingly.
---

### Another Mistake! (Brackets could've put me behind bars!)
---
#### Original Code
```C
((int)(frPt * toBase > 9)) ? (charOffset = 55) : (charOffset = 48);
```
> Mistake: This is a code snippet from "decToBase_afterRadixPoint(float no, int toBase)" functoin definition. I wanted to extract integer coefficient for hex digit assignment. But, for the number 0.1234, I was getting the output as 0.1F@7248, '@' has ASCII value 64. The expression "frPt * toBase" had become greater than 9 (9.4464) and thus, charOffset had set itself to 55. It should've stayed to 48 only, reason being I wanted the comparison to happen between INT of "frPt * toBase" and not convert this to INT after comparison. Brackets! Brackets! Place them carefully!

#### Corrected Code!
```C
((int)(frPt * toBase) > 9) ? (charOffset = 55) : (charOffset = 48);
```
---

### Moved The Program From 'miscellaneousPrograms' To A Separate Repo
---
Today, on 24.Apr.24 I have decided to maintain a separate directory and repository for the radixConverter program. Rather, from now on, I'd call it a project.

To find the previous commits to this repository have a look at my [miscellaneousPrograms](https://github.com/primeDevansh/miscellaneousPrograms) repository's commit history.

---

### Lets Use a Hash Table/Array To Smartly Implement **int checkBasePattern(char*, int)** Function
---
It is 29.Apr.24 I have an idea, why not use a hash array to smartly implement checkBasePattern function. Till now, I was doing it like this:

```C
int checkBasePattern(char* s, int fromBase) {
    int i = 0;
    switch(fromBase) {
        case 2:
            while(s[i]) {
                switch(s[i]) {
                    case '.':
                    case '0':
                    case '1':
                        break;
                    default: 
                        return 0;
                }
                i++;
            }
            break;

        case 8:
            while(s[i]) {
                switch(s[i]) {
                    case '.':
                    case '0':
                    case '1':
                    case '2':
                    case '3':
                    case '4':
                    case '5':
                    case '6':
                    case '7':
                        break;
                    default: 
                        return 0;
                }
                i++;
            }
            break;

        case 10:
            while(s[i]) {
                switch(s[i]) {
                    case '.':
                    case '0':
                    case '1':
                    case '2':
                    case '3':
                    case '4':
                    case '5':
                    case '6':
                    case '7':
                    case '8':
                    case '9':
                        break;
                    default: 
                        return 0;
                }
                i++;
            }
            break;

        case 16:
            while(s[i]) {
                switch(s[i]) {
                    case '.':
                    case '0':
                    case '1':
                    case '2':
                    case '3':
                    case '4':
                    case '5':
                    case '6':
                    case '7':
                    case '8':
                    case '9':
                    case 'A':
                    case 'B':
                    case 'C':
                    case 'D':
                    case 'E':
                    case 'F':
                        break;
                    default: 
                        return 0;
                }
                i++;
            }
            break;
    }
    return 1;
}
```

Let's find a better way to do it!

---

### What The Duck!
---
What the duck was I thinking when I had made such a long checkBasePattern function when it could've been implemented in such a short way below:

Like it isn't that I copied the below fragment from somewhere but why the duck had it not struck my mind before. Oh! Ducking! Foolish Deva!

```C
int checkBasePattern(char* s, int fromBase) {
    int charOffset;
    for(int i = 0; s[i]; i++) {
        if(s[i] == '.')
            continue;
        (s[i] > '9') ? (charOffset = 55) : (charOffset = 48);

        if((s[i] - charOffset) >= fromBase || (s[i] - charOffset) < 0)
            return 0;
    }
    return 1;
}
```
---