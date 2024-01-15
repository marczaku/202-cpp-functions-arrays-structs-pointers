# TLDR

## Functions
- Mostly like C#
- But the order matters!
- You can forward declare them to ensure that the order doesn't matter:

```c++
void foo();
void bar();

void foo(){
    bar();
}
void bar(){
    foo();
}
```

## Static Arrays

Do not exist the same way in C#

```c++
// define and initialize array
int numbers[10] {1,2,3,4,5};
// get the size of an array
size_t size = std::size(numbers);
// iterate over an array
for(size_t i = 0; i < size; i++){
   // assign value to array
   cin >> numbers[i];
}
for(size_t i = 0; i < size; i++){
   // access array value
   cout << numbers[i];
}
// iterate using
for(int number : numbers) {
	printf("%d\n", number);
}
```

## C-Style Strings
- `char[]` (Character Arrays)
- end on occurence of character `'\0' (0)`

## PODs
- Plain-Old-Data Objects
- Can be defined using the `struct` keyword
- Work almost the same as C#'s `struct`s


### Definition

#### C#

```csharp
public struct Book {
    public string name;
	public int year;
	public int pages;
	public bool hardcover;
}
```

#### C / C++

```c++
struct Book {
	char name[256];
	int year;
	int pages;
	bool hardcover;
};
```

### Usage

#### C#

```csharp
Book book = new Book();
book.name = "Harry Potter";
Console.WriteLine($"{book.name} ({book.year})");
```

#### C/C++

```c++
Book book();
strcpy_s(book.name, "Harry Potter");
Console.WriteLine($"{book.name} ({book.year})");
```

## Pointers
- Makes `struct` behave like C# `class`
- Used to pass on a variable without creating copies
- `&book` operator is used to get the address of a variable
- `Book*` is used to store the address of a `Book` variable
- `book->title` is used to access members of address variables
- Static Arrays are basically pointers

## References
- Safer to use than Pointers
- Can not contain `nullptr` value
- `Book&` is used to store a Book reference
- Use whenever you can

## Auto
- Same as C#'s `var`
- Edge cases:
  - `auto& player = getPlayer()`

## Dynamic Memory Management
- Is used by C# automatically whenever you have reference types

### Creating an Object
```c++
Player* player = new Player();
```

### Deleting an Object
```c++
delete player;
```