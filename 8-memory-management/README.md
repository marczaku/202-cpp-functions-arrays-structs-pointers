# 8 Memory Management

## The Problem:

```cpp
char* askForName() { // name enters scope
  printf("Pick a name.");
  char name[100];
  scanf_s("%s", name, 100);
  return name; // we return a pointer to memory of this scope
} // name leaves scope

int main(){
  char* names[3];
  for(int i = 0; i < 3; ++i){
    names[i] = askForName();
  }

  for(int i = 0; i < 3; ++i){
    printf("Name #%d: %s", i, names[i]);
  }
}
```

- Input: `Marc, Sarah, Alex`
- Output: `undefined`
  - might be `Name #0: AlexName #1: AlexName #2: Alex`
  - or `Name #0: ╠╠╠╠╠╠╠╠╠╠╠╠╠╠╠╠h°/⌠*Name #1: ╠╠╠╠╠╠╠╠╠╠╠╠╠╠╠╠h°/⌠*Name #2: ╠╠╠╠╠╠╠╠╠╠╠╠╠╠╠╠h°/⌠*`

Problem: We're returning the address to Memory that is allocated for the scope of the function only.
- it is then freed when the function exits
- but we still have a pointer to that address
- that the system believes can be used for other variables now

To solve this, we need to understand

## Object's Lifetime

1. The object’s storage duration begins, and storage is allocated.
2. The object’s constructor is called.
3. The object’s lifetime begins.
4. You can use the object in your program.
5. The object’s lifetime ends.
6. The object’s destructor is called.
7. The object’s storage duration ends, and storage is de-allocated.

## Automatic Storage Duration

```cpp
void addNumbers(int a, int b) {
	int result = a+b;
	return result;
}
```

What Objects:
- objects defined within a scope
- "local variables"

Storage Duration:
- automatically allocated at the beginning of the enclosing scope
- automatically de-allocated at the end of the enclosing scope

In above example, `a`, `b` and `result` are automatically allocated when the function is invoked and automatically de-allocated just before the function returns

## Static Storage Duration

```cpp
#include <cstdio>

int globalCounter{0};
static int staticGlobalCounter{0};

void runCounters() {
	static int staticLocalCounter{0};
	globalCounter++;
	staticGlobalCounter++;
	staticLocalCounter++;
	printf("globalCounter: %d, staticLocalCounter: %d, staticGlobalCounter: %d\n", globalCounter, staticLocalCounter, staticGlobalCounter);
}

int main() {
	runCounters();
	runCounters();
	runCounters();
}

```

What Objects:
- declared at namespace scope (including global namespace)
- declared with `static` or `extern` keywords

Storage Duration:
- automatically allocated on program start
- automatically de-allocated on program exit

In above example, `globalCounter`, `staticGlobalCounter` and `staticLocalCounter` all exist with only one instance in memory and allocated as long as the program runs.

### Difference between the statics
- al have a static storage duration
- `static` global has an internally visible linker-symbol
- global has an externally visible linker-symbol
- `static` local has a visibility limited by its scope

## Thread Storage Duration

```cpp
#include <cstdio>

thread_local int globalCounter{0};
static thread_local int staticGlobalCounter{0};

void runCounters() {
	static thread_local int staticLocalCounter{0};
	globalCounter++;
	staticGlobalCounter++;
	staticLocalCounter++;
	printf("globalCounter: %d, staticLocalCounter: %d, staticGlobalCounter: %d\n", globalCounter, staticLocalCounter, staticGlobalCounter);
}

int main() {
	runCounters();
	runCounters();
	runCounters();
}
```

What Objects:
- declared with `thread_local` keyword

Storage Duration:
- automatically allocated on thread start
- automatically de-allocated on thread exit

In above example, `globalCounter` and `staticGlobalCounter` are allocated separately on each thread and de-allocated for each thread when it ends.

## Dynamic Storage Duration

```cpp
#include <cstdio>

struct Battery {
	int power;
  static int totalPower;
};
thread_local int Battery::totalPower{200};

int main() {
	Battery* battery{nullptr};
	for(int i = 0; i < 3; i++) {
		{
			printf("Do you want to create a battery? [y/n]\n");
			fflush(stdin);
			char answer = getchar();
			if(answer == 'y') {
				battery = new Battery();
        battery.power = 10;
        Battery::totalPower += 10;
			}
		}
		{
			printf("Do you want to delete the battery? [y/n]\n");
			fflush(stdin);
			char answer = getchar();
			if(answer == 'y') {
				delete battery;
        Battery::totalPower -= 10;
				battery = nullptr;
			}
		}
	}
	printf("In the end, we have a power of: %d\n",Battery::totalPower);
}
```

What Objects:
- explicitly allocated using the `new` keyword

Storage Duration:
- allocated on `new` (old: `malloc`)
- de-allocated on `delete` or `delete[]` (old: `free`)

In above example, `battery` is allocated and de-allocated explicitly only, if the user approves it. 

### new

When the `new` expression executes, the C++ runtime 
- allocates memory to store a `Battery`-object
- invokes the class's constructor
- returns its pointer `Battery*`

### delete
When the `delete` expression executes, the C++ runtime
- invokes the class's destructor
- de-allocates the object's memory
- returns `void`

Note: The Memory does not get cleaned up
- Bug: Use after Free
- You might use an object that's already been deleted
- It doesn't become obvious anytime soon, because the values look good
- But when a new object gets allocated in the same memory address
- Things go down-under. Connection between `delete` and the Bug appearing is unclear

### Dynamic Arrays
Allow to store arbitrary-sized Arrays
```cpp
#include <cstdio>

struct Battery {
	int power;
	static thread_local int totalPower;
};
thread_local int Battery::totalPower{200};

int main() {
	printf("How many Batteries do you want? [0-9]\n");
	fflush(stdin);
	char answer = getchar();
	int answerNum = answer - '0';
	Battery** batteries {new Battery*[answerNum]};
	for(int i = 0; i < answerNum; i++) {
		batteries[i] = new Battery();
    batteries[i].power = 10;
    Battery::totalPower += 10;
	}
	for(int i = answerNum-1; i > -1; i--) {
		delete batteries[i];
	}
	delete[] batteries;

	printf("In the end, we have a power of: %d\n",Battery::totalPower);
}
```

## Memory Leaks
- If you allocate memory
- But forget to de-allocate it
- Resources are lost FOREVER
- Or until your program exits (whichever happens sooner)

## Tracing Object Life Cycle
This example uses Object-Oriented-Code, but no worries, it stays simple enough.
```cpp
#include <cstdio>

class Tracer {
  const char* const name;
public:
  Tracer(const char* name) : name{ name }➋ {
    printf("%s()\n", name);
  }
  ~Tracer() {
    printf("~%s()\n", name);
  }
};

static Tracer t1{ "Static" };
thread_local Tracer t2{ "Thread-local" };

int main() {
  printf("Step 1\n");
  Tracer t3{ "Automatic" };
  printf("Step 2\n");
  const auto* t4 = new Tracer{ "Dynamic" };
  printf("Step 3\n");
}
```

## EXERCISE
- Ask the user, how many People he wants to create.
- Create an array of that function.
- Write a function that can create a person.
  - It should return a pointer to the created Person to avoid unnecessary copies.
  - It should return a Person `struct` where the name has been assigned.
- In the main function, use that function as many times as necessary to fill the array with people.
- After that, do a for-loop to print the names of all the people separated by comma as well as the total amount of people:

```
Output:How many people do you want to create?
Input:3
Output:What's the next person supposed to be called?
Input:Marc
Output:What's the next person supposed to be called?
Input:Alex
Output:What's the next person supposed to be called?
Input:Sarah
Output:Marc, Alex, Sarah
```