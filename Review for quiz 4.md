
###  Allocation of memory to variables on the stack
1. Code/Static region: actual program's code + static variables
2. Free store/Heap: dynamic memory allocation (when use `new`)
	1. allows us to create an object in a function and have that object's lifetime extend beyond that function
3. Stack:  static memory allocation:  
	1. includes: local variables, function parameters, and return addresses
	2. growing and shrinking array of heterogeneous objects

### Dynamic allocation of objects on the free store
1. `std::string* ptr = new std::string("Howdy");`
	1. `ptr` is allocated in stack
	2. object pointed to by `ptr`  is allocated in free store
```
void PushBackToIntArray(int*& array, unsigned int& size, unsigned int& capacity, int value) {
	if (array == nullptr ** capacity == 0) {
		capacity = 1;
		array[0] = value;
	}
	if (size == capacity) {
		unsigned int new_capacity = 2 * capacity; // new capacity
		int* new_array = new int[new_capacity]; // new array ptr
		for (unsigned int i = 0; i < size; ++i) {
			new_array[i] = array[i];
		}
		delete[] array; // delete array
		array = new_array; // update array
		capacity = new_capacity; // update capacity
	}
	array[size] = value;
	size++;
}
```

### An implicit contract with the free store manager
1. Clause 1: Memory Leaks
	1. Memory is allocated using `new`, but there is no corresponding `delete` statement.
2. Clause 2: Dangling Pointers
	1. A pointer is not set to `nullptr` after deleting the memory it points to.
	2. A pointer is used after the object it points to has been deleted.
	3. ``` int* danglingPointerExample() {
		    int* dynamicValue = new int(42);  // Allocate memory for an integer
		    int* anotherPointer = dynamicValue;  // Copy the pointer
		    delete dynamicValue;  // Deallocate the memory
		    return anotherPointer;  // Returning a dangling pointer
		}
3. Clause 3: Runtime Errors
	1. index outside its bounds
	2. Division by zero
	3. Dereferencing null or dangling pointers
	4. Incorrect use of functions or algorithms.

### Two-dimensional arrays on the free store
```
int** Empty2dArray(unsigned int num_rows, unsigned int num_cols) {
  int** array = new int*[num_rows];
  for (unsigned int i = 0; i < num_rows; ++i) {
	  array[i] = new int[num_cols]();
  }
  return array;
}

void Init2dArray(int src[], int** dest, unsigned int num_rows, unsigned int num_cols) {
  for (unsigned int i = 0; i < num_rows; ++i) {
	  for (unsigned int j = 0; j < num_cols; ++j) {
		  dest[i][j] = src[i * num_rows + j];
	  }
  }
}

void Print2dArray(int** arr, unsigned num_rows, unsigned num_cols, std::ostream& os) {
  for (unsigned int i = 0; i < num_rows; ++i) {
	  for (unsigned int j = 0; j < num_cols; ++j) {
		  os << arr[i][j];
		  if (j < num_cols - 1) {
			  os << " ";
		  }
	  }
	  if (i < num_rows - 1) {
		  os << "\n";
	  }
  }
}
```

### Tools for testing and debugging memory issues
1. Dereferencing the nullptr results in well-defined behavior
2. Tools
	1. Asan (AddressSanitizer)
		1. Detects memory errors such as out-of-bounds accesses, use-after-free, and memory leaks.
	2. Clang-tidy (Clang Static Analyzer)
		1. Performs static analysis on the source code to find potential bugs, security vulnerabilities, and coding style issues.
	3. UBSan (UndefinedBehaviorSanitizer)
		1. Detects undefined behavior in the program, such as signed integer overflow, null pointer dereference, and other undefined operations.
	4. Valgrind
		1. Detects memory-related issues, including memory leaks, use of uninitialized memory, and incorrect memory access.

### RAII and dynamic memory in classes
1. RAII: Resource Acquisition Is Initialization
	2. key idea: **represent a resource by a local object.**
	3. Allocate memory in the class constructor
	4. Deallocate memory in the class destructor
	5. If your class manages dynamic memory, it's a good idea to define or disable the copy constructor and copy assignment operator
	6. Implementing these functions can help prevent unintentional resource sharing
```
MatrixInt::MatrixInt(unsigned int rows, unsigned int cols) : rows_{rows}, cols_{cols}, matrix_{nullptr} {
	if (rows_ <= 0 || cols_ <= 0) {
		throw std::invalid_argument("Invalid rows or cols");
	}
	matrix_ = new int*[rows_]; // firstly declear rows
	for (unsigned int i = 0; i < rows_; ++i) {
		matrix_[i] = new int[cols_](); // initilize all to 0
	}
}

MatrixInt::~MatrixInt() {
	if (matrix_ != nullptr) { // firstly check if null
		for (unsigned int i = 0; i < rows_; ++i) {
			delete[] matrix_[i]; // first delete inside
		}
		delete[] matrix_; // then delete outside
	}
}

void MatrixInt::Resize(unsigned int new_rows, unsigned int new_cols) {
	if (new_rows <= 0 || new_cols <= 0) {
		throw std::invalid_argument("Invalid new rows or cols");
	} 
	// declear new_matrix
	int** new_matrix = new int*[new_rows]
	for (unsigned int i = 0; i < new_rows; ++i) {
		new_matrix[i] = new int[new_cols](); // initilize to 0
	}
	// add old elements
	for (unsigned int i = 0; i < std::min(rows_, new_rows); ++i) {
		for (unsigned int j = 0; j < std::min(cols_, new_cols); ++j) {
			new_matrix[i][j] = matrix_[i][j];
		}
	}
	// delete
	for (unsigned int i = 0; i < rows_; ++i) {
		delete[] matrix_[i];
	}
	delete[] matrix_;
	//update
	matrix_ = new_matrix;
	rows_ = new_rows;
	cols_ = new_cols;
}
```