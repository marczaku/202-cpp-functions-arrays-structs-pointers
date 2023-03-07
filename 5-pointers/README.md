# 3 Pointers

Reference types store the memory addresses of objects in order to write highly efficient programs. Instead of copying information from a to b, it can often directly be written in the correct location and passed on through its address.

## Pointers

Pointers contain both pieces of information needed to interact with information:
- address
- type
  - size of the type
	- `2 bytes`
	- `4 bytes`
  - way of interpreting it
    - `integer`
	- `floating-point`
	- `character`
	- `Book`

### Definition

Pointer-Declarator `*`

```cpp
int* myPointer;
```

### Addressing Variables

Address-Of Operator `&`
- gets the address of an object
- returns a pointer-type
  - e.g. for `int`, it returns `int*`
  - e.g. for `char*`, it returns `char**`

```cpp
#include <cstdio>

int main() {
	int number{};
	printf("number: %d\n", number);
	// get the address of variable `number`
	int* numberAddress = &number;
	// print the address to the console
	printf("&number: %p\n", numberAddress);
}
```

### Address Space Layout Randomization

If you run the program multiple, times, you'll notice some inconsistencies:

```
number: 0
&number: 0x16f57334c
```

```
number: 0
&number: 0x16d37f34c
```

- the base memory address is randomized on start-up
- to avoid exploitation e.g. through `return-oriented programs`

### Dereferencing Pointers

Dereference Operator `*`
- gets the object behind an address
- returns the pointed-to type
  - e.g. for `int*`, it returns `int`
  - e.g. for `char**`, it returns `char*`
- you can then assign the object to another variable
- or you can write a new value into the object

```cpp
#include <cstdio>

int main() {
	int number{};
	printf("number: %d\n", number);
	int* numberAddress = &number;
	printf("&number: %p\n", numberAddress);
	*numberAddress = 24;
	printf("number: %d\n", number);
	printf("&number: %p\n", numberAddress);
}
```

### Accessing Pointer Members

Member-Of-Pointer Operator `->`

To access a member of a class-pointer, you need to:
- dereference the pointer
- access a member of the pointed-to object

```cpp
#include <cstdio>

struct DateTime {
	int year;
};

void modify(DateTime* timePtr){
	(*timePtr).year = 2022;
}

int main() {
	DateTime time;
	DateTime* timePtr = &time;
	modify(timePtr);
	printf("Address of time: %p\n", timePtr);
	printf("Value of time: %d\n", (*timePtr).year);
}
```

This can be done much more elegant using the arrow-operator:
- Before: `(*variablePtr).member`
- After: `variablePtr->member`

```cpp
#include <cstdio>

struct DateTime {
	int year;
};

void modify(DateTime* timePtr){
	timePtr->year = 2022;
}

int main() {
	DateTime time;
	DateTime* timePtr = &time;
	modify(timePtr);
	printf("Address of time: %p\n", timePtr);
	printf("Value of time: %d\n", timePtr->year);
}
```

### Call-by-Reference
By passing on a pointer to a function, you can prevent your Struct from being Copied/Cloned:

```cpp
void stealHalfTheBook(Book* pBook){ // pointer to main-Book
	pBook->numberOfPages /= 2;
	printf("Pages of Book after stealing:%d\n", pBook->numberOfPages); // 100
}

int main(){
	Book book;
	book.numberOfPages = 200;
	printf("Pages of main-Book before stealing:%d\n", book.numberOfPages); // 200
	stealHalfTheBook(&book); // pass on pointer to main-Book
	printf("Pages of main-Book after stealing:%d\n", book.numberOfPages); // 100
}
```

#### Without the Array-Operator:
```cpp
void stealHalfTheBook(Book* pBook){ // pointer to main-Book
	(*pBook).numberOfPages /= 2;
	printf("Pages of Book after stealing:%d\n", (*pBook).numberOfPages); // 100
}

int main(){
	Book book;
	book.numberOfPages = 200;
	printf("Pages of main-Book before stealing:%d\n", book.numberOfPages); // 200
	stealHalfTheBook(&book); // pass on pointer to main-Book
	printf("Pages of main-Book after stealing:%d\n", book.numberOfPages); // 100
}
```

#### But be careful when dereferencing!
```cpp
void stealHalfTheBook(Book* pBook){ // pointer to main-Book
	Book book = *pBook; // clone
	pBook->numberOfPages /= 2;
	printf("Pages of Book after stealing:%d\n", book.numberOfPages); // 100
}

int main(){
	Book book;
	book.numberOfPages = 200;
	printf("Pages of main-Book before stealing:%d\n", book.numberOfPages); // 200
	stealHalfTheBook(&book); // pass on pointer to main-Book
	printf("Pages of main-Book after stealing:%d\n", book.numberOfPages); // 200
}
```

## Pointers and Arrays
Pointers are similar to Arrays:
- A pointer encodes object type + location
- An array encodes object type + location + number of objects

### Explain the Output:

```cpp
#include <cstdio>

void foo(char name[10])
{
    printf("Size within foo: %zd\n", sizeof name);
}

int main() {
    char name[10] = "Marc";
    printf("Size within main: %zd\n", sizeof name);
    foo(name);
}
```

Output:
```
Size within main: 10
Size within foo: 8

```

### Array Decay
Arrays can decay into pointers:

```cpp
int keyToTheUniverse[]{3,6,9};
int* keyPtr = keyToTheUniverse; // points to 3
```

This happens whenever you want to pass on an Array to another variable or function:

```cpp
#include <cstdio>

void printNumbers(int* numbers) {
	// how to access number of elements here?
	printf("Number: %d\n",numbers[0]);
}

int main() {
	int keyToTheUniverse[]{3,6,9};
	printNumbers(keyToTheUniverse);
}
```

### Handling Decay

This is usually solved by passing on two arguments:
- A pointer to the first array element
- The array length

```cpp
#include <cstdio>
#include <array>

void printNumbers(int* numbers, size_t count) {
	// how to access number of elements here?
	for(size_t i = 0; i < count; i++){
		printf("Number: %d\n",numbers[i]);
	}
}

int main() {
	int keyToTheUniverse[]{3,6,9};
	printNumbers(keyToTheUniverse, std::size(keyToTheUniverse));
}
```

This is ubiquitous in C-Style APIs, e.g. Windows and Linux systems.

## Pointer Arithmetic

The following four lines all do the same:

```cpp
int main() {
	int secondElement = keyToTheUniverse[1];
	secondElement = *(keyToTheUniverse+1);
	secondElement = *(1+keyToTheUniverse);
	secondElement = 1[keyToTheUniverse];
}
```

### Adding a scalar to Pointers

When you add a scalar to a pointer, it offsets the pointer address by `the pointed-to-type's size * scalar value`

```cpp
int numbers[]{1,2,3};
int* first = numbers; // address of first element
int* third = first+2; // address of third element (first + 2 * 4 bytes)
```

Same for Short:

```cpp
short numbers[]{1,2,3};
short* first = numbers; // address of first element
short* third = first+2; // address of third element (first + 2 * 2 bytes)
```

Addition is commutative, which means that you can also do:

```cpp
short* second = 1+first;
```

### Using the Bracket-Operator

`pointer[5]` is just short for: `*(pointer+5)`
- first, add 5 to the pointer
  - i.e. move 5 elements down in memory
- then, dereference the pointer to access the value

```cpp
short numbers[]{1,2,3};
short third = numbers[2];
```

This is the same as:

```cpp
short numbers[]{1,2,3};
short third = *(numbers+2);
```

Which is commutative, so the same as:

```cpp
short numbers[]{1,2,3};
short third = *(2+numbers);
```

Which can then be written as:

```cpp
short numbers[]{1,2,3};
short third = 2[numbers];
```

Looks weird, but it's valid c++ code!

## Pointer Arithmetic is Dangerous

You can access arbitrary elements using the Bracket-Operator.
- this allows any access to any memory address
- unless you access an address without permission by the OS
  - then the OS will terminate your program

```cpp
#include <cstdio>
#include <array>

int main() {
	char lower[] = "abcde";
	char upper[] = "ABCDE";

	lower[2] = 'g';
	upper[2] = 'G';

	lower[8] = 'h';
	upper[8] = 'H';

	printf("lower: %s (%p)\nupper: %s (%p)\n", lower, lower, upper, upper);
}
```

Output:

```
lower: Hbgde (0x16fdff3d8)
upper: ABGDE (0x16fdff3d0)
```

## Void-Pointer
If you want to pass on an address, but the type is relevant, you can use `void*`
- it can not be dereferenced (type `void` does not exist)
- pointer arithmetic is impossible (size is unknown)

## Byte-Pointer
If you want to pass on an address of arbitrary data, like reading, writing files, network i/o, encryption, compression, you can use `byte*`
- dereferencing only gives you information of generic type `byte`
- pointer arithmetic is possible (byte by byte)

## Nullptr
`nulltr` is the value for a pointer to 'nothing'. It is usually used to indicate that something does not exist (yet) or something went wrong.

```cpp
#include <cstdio>
#include <array>

char* names[]{"Alpha", "Beta", "Gamma", "Delta"};

char* chooseName(){
	for(char* name : names){
		printf("Do you like the name '%s'? [y/n]\n", name);
		char answer[2];
		scanf("%1s", answer);
		if(answer[0] == 'y'){
			return name;
		}
	}
	return nullptr;
}

int main() {
	char* chosenName = chooseName();
	if(chosenName == nullptr){
		printf("You're never satisfied. Are you??\n");
	}
	else{
		printf("%s is a wonderful name!", chosenName);
	}
}
```

## Pointers and Conditions

Pointers have an implicit conversion to type `bool`:
- `nullptr` converts to `false`
- any other pointer converts to `true`

Therefore:

```cpp
if(chosenName == nullptr)
// is the same as:
if(!chosenName)
```

And

```cpp
if(chosenName != nullptr)
// is the same as:
if(chosenName)
```

## EXERCISE
Write a Swap Function
- It takes two arguments of type int pointer
- It swaps the values behind both pointers
- Use the following code:

```cpp
// enter swap function here

int main(){
	int four = 4;
	int five = 5;
	printf("This is four: %d, This is five: %d", four, five);
	// invoke swap function here passing pointers to five and four.
	printf("This used to be four: %d, This used to be five: %d", four, five);
	return 0;
}
```

What happens, if you pass in `nullptr` as one of the arguments into the function?