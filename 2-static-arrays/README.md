# 2 Static Arrays

```c++
int numbers[100];
```

Size of the Array must be constant!
- unless you compile with `gcc`/`g++`

```c++
int size = 5;
int numbers[size]; // ERROR!
```

```c++
const int size = 5;
int numbers[size]; // OK!
```

## Uninitialized Arrays
Have random values! Do never forget to initialize:

```c++
int numbers[5];
for(int number : numbers){
	printf("%d, ", number);
}
```

Output:
```
-1521484208, 72, -538963304, 32758, -538959480, 
```

## Initialization

```c++
int array[] = {1, 2, 3, 4};
for(int number : numbers){
	printf("%d, ", number);
}
```

Output:
```
1, 2, 3, 4, 
```

## Index Operator

```c++
scanf_s("%d", &(array[0]);
cin >> array[1];
```

```c++
printf("Second element: %d", array[2]); // 2
cout << array[3];
```

## for

```c++
for(int i = 0; i < 10; ++i) {
	printf("%d\n", i);
}
```

Use `size_t` when you're not sure about the size of an Array (it could be larger than 1.2bln elements)
```c++
for(size_t i = 0; i < std::size(array); i++) {
	printf("%d\n", array[i]);
}
```

## Range-Based For
Like `foreach`:

```c++
for(int number : array) {
	printf("%d\n", number);
}
```

## Number of Elements in Array

The hard, manual way:

```c++
size_t count = sizeof(array) / sizeof(int);
```

Better:

```c++
#include <array>
size_t count = std::size(array);
```

## EXERCISE
Ask the user for 9 numbers. Store them in an array. Return the Average of all numbers.

## BONUS EXERCISE
Return the Median of those numbers.

## EXERCISE
Left-Rotate an Array.
- All elements should move one index to the left (to a lower index)
- The first element should move to the last Index

```
Before: 12 14 16 18 20
After: 14 16 18 20 12
```
