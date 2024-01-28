### Writing Good Code
1. what is good code
	1. achieve its purpose, readable, reusable
2. how to write good code
	1. comments, variable names, and constants to improve the readability
	2. sing functions and multiple files
3. why use function
4. why use multiple files：good solutions
	1. Put the function declarations in a header file (.hpp)
	2. and the definitions in a corresponding .cc file

### Basic input/output
1. Writing to standard out and to files
	1. standard output
		1. include `#include <iostream>`
		2. simply insert objects and literals into the stream in the desired format: `std::cout << "" << std::endl;`
	2. files
		1.  include `include <fstream>`
		2.      ```std::ofstream ofs {"filename.ext"};
			if (!ofs.is_open()) {
			// do sth.
			}
			std::string name = "Amy";
			int num = 1
			ofs << "name: " << name << "; numebr: " <<  std::endl; ``` 
		3.  Be careful
			1. building a new file-stream to an existing file will overwrite its contents
			2. to append: `std::ofstream ofs {"filename.ext", std::ofstream::app}`
2. Reading from standard in and from files
	1. standard input
		1. include `#include <iostream>`
		2. this will be input entered from terminal
		3.     ```std::string first_name;
			std::string last_name;
			std::cout << "Enetr your full name: ";
			std::cin >> first_name >> last_name;
			// "</> is pointing info flow"
			// reads using std::cin whitespace delimited ```
		4.     ```std::string full_name;
			std::cout << "Enter full name: ";
			std::getline(std::cin, full_name);
			//first: the string to get the line from
			//second: where to put the line ```
	2. files
		1. include `#include <fstream>`
		2.     ```std::ifstream ifs {"filename.ext"};
			if (!ifs.is_open()) {
			// do sth.
			}
			int i = 0;
			double j = 0;
			std::string str;
			ifs >> i >> j >> str;

### Navigating the command line
1. Basic Command
	1. `pwd` : absolute path to current directory, the location of a file or directory from the root directory, `cd` in windows
	2. relative path: the location of a file or directory relative to the current working directory
	3. `cat` : show the contents of a file
	4. `ls`: hows the contents of the current working directory, `dir` in windows
	5. `mv`: moves the file from its current name to the new name
### Command line arguments
1. In C++, when you execute a program from the command line and pass arguments, `argv[0]` typically holds the name of the program itself.
2. `argc` represents the total number of command-line arguments passed to the program, including the program name
### Two-dimensional vectors
1. Declaration and initialization
	1. `std::vector<std::vector<int>> matrix;`
2. Adding rows to the matrix
	1. `matrix.push_back({1, 2, 3});`
3. Accessing elements in the 2D vector
	1. `std::cout << "Element at matrix[1][2]: " << matrix[1][2] << std::endl;`
4. Iterating through the 2D vector
```
for (size_t i = 0; i < matrix.size(); ++i) {
	for (size_t j = 0; j < matrix[i].size(); ++j) {
		std::cout << matrix[i][j] << " ";
	}
}
```


### Maps
1. include `#include <iostream>` and `#include <map>`
2. declaration `std::map<KeyType, ValueType> myMap;`
3. elements
	1. insert element: `myMap[key1] = value1;` and `MAP_NAME.insert({KEY, VALUE});`
	2. remove elemene: 
	3. access element: `ValueType value = myMap[key];`
	4. Checking if a Key is in a Map: `MAP_NAME.contains(KEY)`
4. iteration: 
```
for (const auto& [key, value] : myMap) { 
}
```
5. notes
	1.  Each key maps to exactly one value.
	2. Every key in a map is unique and cannot change.
	3. Values do not have to be unique and can change.
	4. The keys and values each have their own type.



### References and argument passing
1. Initialization of objects and function calls
	1. when the parameter is an object, we say that the argument is passed by value because a value is copied into the parameter object for initialization
2. References & Pass by reference
	1. references provide a way to pass variables to functions without making a copy
	2. When you pass a variable by reference, any changes made to the parameter inside the function affect the original variable outside the function.
3. 