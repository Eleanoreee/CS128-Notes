
### Array

The size of an array is fixed once it is declared, and you cannot change it during runtime
```
int numbers[5] = {1, 2, 3, 4, 5};  // Initializes an array of 5 integers
int firstElement = numbers[0]; // Accesses the first element
numbers[1] = 10; // Modifies

int matrix[3][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
```

```
bool ContainsDuplicate(int array[], unsigned int array_size) {
  for (unsigned int i = 0; i < array_size - 1; ++i) {
    for (unsigned int j = i + 1; j < array_size; ++j) {
      if (array[i] == array[j]) {
        return true;
      }
    }
  }
  return false;
}
```


### Stack
1. Last-In-First-Out (LIFO)
2. named objects (not declared as static/global) have their memory allocated in
3. stored inside a function's stack frame
	1. Arguments passed by value to the function.
	2. Housekeeping information
	3. Local variables to the function.
4. growing and shrinking array of heterogeneous objects.

# Dynamic allocation of objects on the free store
1. deallocates the memory pointed to by `ptr`
```
int* ptr = new int{7};
delete ptr;

int* ptr = new int[7];
delete[] ptr;

```

### Testing and debugging
1. Unit tests : verifies the behavior of low-level code in isolation from the rest of the system, Automated tests that exercise and evaluate individual units of source code.
2. C0 (statement coverage)
2. C1 (conditionals coverage): It measures the percentage of decision points (branches) in the code that have been executed at least once.
3. C2 (paths coverage)
4. DU-coverage (all pairs of define X/use X for every variable X)

### Pointers
```
int a = 2;
int *ptr = &a; // Declare a pointer

a += 1;
std::cout << *ptr << std::endl; // dereference
//output will be 3, the value of a
```