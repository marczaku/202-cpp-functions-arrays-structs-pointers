# 6 References
Safer, more convenient version of pointers
- can pass on references/pointers to values (no copy)
- cannot be assigned `nullptr`
- cannot be reseated (reassigned)
- no member-of-pointer operator needed `->`

```cpp
#include <cstdio>

struct DateTime {
	int year{1990};
};

void modify(DateTime& time){
	time.year = 2022;
}

int main() {
	DateTime time;
	modify(time);
	printf("Value of time: %d\n", time.year);
}
```

Cannot be reseated:

```cpp
#include <cstdio>

struct DateTime {
	int year{1990};
};

int main() {
	DateTime time{2004};
	DateTime time2{2012};
	DateTime& timeRef = time;
	timeRef = time2; // the value of time2 gets copied to time
	time2.year = 12; // 12 gets only assigned to time2
	printf("Value of time: %d\n", time.year);
	printf("Value of time2: %d\n", time2.year);
	printf("Value of timeRef: %d\n", timeRef.year);
}
```

## Reference vs. Pointer
Use Pointers only, when you need to reseat them or because you need a nullable type ()`nullptr`). In all other cases, References should be preferred.

## EXERCISE
Rewrite the Swap function from the Pointers Chapter to use References
- Is it easier to use?