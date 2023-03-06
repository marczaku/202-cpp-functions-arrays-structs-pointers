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

## Order
- Order of members is maintained
- CPU Register Length causes member alignment
- Rule: Order members from small to large

## Union

- Puts all members into the same place.
- Useful in Low-Level Optimization

### Definition

```cpp
union Variant {
	bool isTrue;
	int number;
	double decimal;
}
```

### Usage

```cpp
Variant v;
v.number = 42;
printf("Nice: %d", v.number);
v.decimal = 2.71828;
printf("Nope: %d", v.number);
```
