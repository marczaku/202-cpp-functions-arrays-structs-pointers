# 1 Functions

Logical units inside the source code. Goal: Reusable Code.
- saves time
- reduces complexity, error-proneness

## IPO Principle
- Input (Parameters)
- Processing (Function Body)
- Output (Return Value)

## Definition

```c++
void printHello() {
  printf("Hello!\n");
}
```

## Invocation

```c++
int main() {
  printHello();
}
```

## Parameters

```c++
void printSum(int a, int b){
  printf("%d", a+b);
}
```

## Invocation with Parameters

```c++
printSum(3, 5); // 8
```

## Return Type

```c++
int addNumbers(int a, int b) {
	return a + b; // need to return a value matching the return type!
}
```

## Return (void)

You can use `return` in `void`-type functions as well
- this will end the function execution instantly
- and execute none of the remaining lines of code

```c++
void getLoan(int age){
  if(age < 18){
    return;
  }
  printf("Got a Loan!\n");
}
```

## Using Return Value

```c++
int result = addNumbers(3, 5); // 8
printf("%d\n", result);
```

```c++
printf("%d\n", addNumbers(4, 6)); // 10
```

## Fun Fact: Void Parameter

In the early days, you had to explicitly express if your function had no parameters:
- by using `void` as parameter list

You can still do that today!
- but you shouldn't

```c++
void sayHello(void){
  printf("Hello!\n");
}
```

## Call Stack

Remember, how the Call Stack works!

```c++
void bar(int number){
  printf("bar %d\n", number);
}

void foo(){
  printf("before bar\n");
  bar(1);
  printf("after bar\n");
}

int main(){
  printf("before foo\n");
  foo();
  printf("after foo\n");
  bar(2);
}
```

## Order Matters
This won't compile because `bar` is not defined when `foo` is defined:
```c++
void foo(){
  bar();
}
void bar(){}
```

### Inefficient Solution: Reorder
You can reorder the code to ensure that the needed functions always comes first
```c++
void bar(){}
void foo(){
  bar();
}
```

This also wouldn't work for this example:

```c++
void bar(){
  foo();
}
void foo(){
  bar();
}
```

### Better Solution: Forward-Declaration
You can declare functions before defining them. This makes sure that the order won't matter
```c++
// declare the functions before defining them:
void bar();
void foo();

// now, define them. The order doesn't matter anymore.
void bar(){
  foo();
}
void foo(){
  bar();
}
```

## Overloading
Overloading describes providing multiple functions with the same name (but different arguments):

```c++
int max(int a, int b){printf("calling int\n"); return b > a ? b : a};
float max(float a, float b){printf("calling float\n"); return b > a ? b : a};

int main() {
  printf("max(3, 4): %d\n", max(3, 4));
  printf("max(3.2f, 4.2f): %f\n", max(3.2f, 4.2f));
}
```

Output:
```
calling int
max(3, 4): 4
calling float
max(3.2f, 4.2f): 4.200000
```

### Same Arguments not possible!
Else, the compiler can't guarantee to know, which one to call.
```c++
#include <limits>
#include <iostream>

int max_num(bool positive) {
	return positive ? std::numeric_limits<int>::max() : std::numeric_limits<int>::min();
}
float max_num(bool positive) { 
	return positive ? std::numeric_limits<float>::max() : std::numeric_limits<float>::min();
}

int main() {
	std::cout << max_num(true); // which one is it supposed to call??
}
```

Compile Error:
```
Error (active)	E0311	cannot overload functions distinguished by return type alone.
```

## EXERCISE
Implement the Fibonacci Formula
- recursively
- iteratively