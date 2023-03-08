# 4 Plain-Old-Data Classes
- C-Compatible
- Highly Efficient to Copy or Move
- Efficiently represented in Memory

## Definition

```cpp
struct Book {
	char name[256];
	int year;
	int pages;
	bool hardcover;
}
```

## Usage

```cpp
int main() {
	Book book;
	book.name = "Harry Potter Part 1";
	book.year = 2000;
	printf("Book %s (%d)", book.name, book.year);
}
```

## Size

A Size is as large as the sum of all its members:

```cpp
#include <cstdio>

struct Book {
	int numberOfPages; // 4
	char authorInitialFirstName; // 1
	char authorInitialLastName; // 1
	bool isReleased; // 1
	bool hasHardCover; // 1
};

int main() {
	printf("%zu", sizeof(Book)); // 8
}
```

## Nesting
You can nest structs without any bad impact on
- either performance
- or memory

```cpp
#include <cstdio>

struct Author {
	char initialFirstName;
	char initialLastName;
};

struct Book {
	int numberOfPages; // 4
	Author author; // 2
	bool isReleased; // 1
	bool hasHardCover; // 1
};

int main() {
	printf("%zu", sizeof(Book)); // 8
}
```

## Order
- Order of members in Memory is maintained
- CPU Register Length causes member alignment
- Rule: Order members from large to small

```cpp
#include <stdlib.h>
#include <cstdio>
#include <time.h>

struct Book {
	bool isReleased; // 1 (+3 for alignment)
	int numberOfPages; // 4
	bool hasHardCover; // 1 (+3 for alignment)
};

int main() {
	printf("%zu", sizeof(Book)); // 12
}
```

## Copy
Structs are generally, just like basic data types, copied/cloned when assigned to another variable:

```cpp
Book a;
a.numberOfPages = 200;
Book b = a; // clone
printf("Pages of Book a:%d\n", a.numberOfPages); // 200
printf("Pages of Book b:%d\n", b.numberOfPages); // 200
b.numberOfPages = 100;
printf("Pages of Book a:%d\n", a.numberOfPages); // 200
printf("Pages of Book b:%d\n", b.numberOfPages); // 100
```

## Call-By-Value
Structs are generally, just like basic data types, copied/cloned when passed on to a function:

```cpp
void stealHalfTheBook(Book book){
	book.numberOfPages /= 2;
	printf("Pages of Book after stealing:%d\n", book.numberOfPages); // 100
}

int main(){
	Book book;
	book.numberOfPages = 200;
	printf("Pages of main-Book before stealing:%d\n", book.numberOfPages); // 200
	stealHalfTheBook(book); // clone
	printf("Pages of main-Book after stealing:%d\n", book.numberOfPages); // 200
}
```

## Self-Reference
A struct cannot contain itself as a Member. Why?

```cpp
struct SnakeBody {
	SnakeBody nextBody;
};
```

## Union

- Puts all members into the same place.
- Useful in Low-Level Optimization

### Definition

```cpp
union Variant {
	bool isTrue; // 1
	int number; // 4
	float decimal; // 4
};

int main() {
	printf("%zu", sizeof(Variant)); // 4
}
```

### Usage

```cpp
Variant v;
v.number = 42;
printf("Nice: %d\n", v.number);
v.decimal = 2.71828;
printf("Nope: %d\n", v.number);
```

Output:
```
Nice: 42
Nope: 1076754509
```

## EXERCISE
- Write a Vector-Struct `Vector2D`
- Write a function named `add` that can add two vectors and returns the result.
- Test the Code.

```cpp
Output: Adding Vector(3,2) and (-1,-2)...
Output: Result: Vector(2,0)
```


## EXERCISE
- Improve the following `struct` by introducing additional `structs`:

```cpp
struct Game{
   int remainingMinutes;
   char firstPlayerName[10];
   int firstPlayerScore;
   int firstPlayerPositionX;
   int firstPlayerPositionY;
   int firstPlayerVelocityX;
   int firstPlayerVelocityY;
   char secondPlayerName[10];
   int secondPlayerScore;
   int secondPlayerPositionX;
   int secondPlayerPositionY;
   int secondPlayerVelocityX;
   int secondPlayerVelocityY;
   
   int chest1PositionX;
   int chest1PositionY;
   bool chest1Collected;
   int chest2PositionX;
   int chest2PositionY;
   bool chest2Collected;
   int chest3PositionX;
   int chest3PositionY;
   bool chest3Collected;
   int chest4PositionX;
   int chest4PositionY;
   bool chest4Collected;
   int keyPositionX;
   int keyPositionY;
   bool keyCollected;
   
   int goalPositionX;
   int goalPositionY;
   int goalWidthX;
   int goalHeightY;
};
```

## EXERCISE
- Fix the following code, so the `book` within `main()` has its `numberOfPages` changed after calling the function:
```cpp
```cpp
void stealHalfTheBook(Book book){
	book.numberOfPages /= 2;
	printf("Pages of Book after stealing:%d\n", book.numberOfPages); // 100
}

int main(){
	Book book;
	book.numberOfPages = 200;
	printf("Pages of main-Book before stealing:%d\n", book.numberOfPages); // 200
	stealHalfTheBook(book); // clone
	printf("Pages of main-Book after stealing:%d\n", book.numberOfPages); // 200
}
```

## EXERCISE
