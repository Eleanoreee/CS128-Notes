
###  Hello, World!
```
#include <iostream> // include the library

int main() {
	std::cout << "Hello, World!" << std::endl;
	// std::cout is output defined in lib iostream
	// << is the insertion operator to insert objs
	return 0;
}
```

### Objects, types, and values
1. Useful Primitive Types
	1. **unsigned int**: A positive whole number from 0 to 4,294,967,295
2. Constants: value cannot changed after assign
	1. `constexpr`: value is known at compile time
	2. `const`: value is unknown at compile time
	3. The convention is to start the variable name of a constant with k.
3. Lifetime and Scope
	1. Lifetime: span of time when the program has allocated (set aside) memory for a particular object to be stored
	2. Scope: the area of code that has access to a variable. A variable's scope is within the curly braces it is defined in and any nested curly braces within it.
		1. 看declare type的大括号范围
4. Conversions:  a value may convert from one type to another
	1. Implicit Type Conversions: automatic conversion of values from one type to another, called coercion
		1. examples: `int i = 9.0 / 5.0; // The result 1.8 is truncated to 1; i is initialized as 1`
	2. Explicit Type Conversions: written out conversion of values from one type to another, called casts
		1. `static_cast<type_to_cast_to>(value_to_cast)`
		2. example: `static_cast<double>(556); // results in 556.0`
		3. some conversions aren't safe. Information may be lost_

### Functions
1. C++ Function Syntax
	1. `RETURN_TYPE FUNCTION_NAME(PARAMETERS){ BODY }`
	2. return type
	3. function name: ExampleFunctionName
	4. parameters: type + variable name
2. Declaration or Definition
	1. Declaration
		1. specifies the return type, name, and parameters of a function, but NOT the body
		2. in .hpp files
	2. Definition
		1. specifies the return type, name, parameters of a function, AND the body
		2. in .cc files


### Selection
1. Else If Statements: 
`if (CONDITION) { BODY }`  
`else if (CONDITION2) { BODY2 }`
2. Short Circuit: if the first part of the AND statement evaluates to `false` the program does not evaluate the second part
3. Switch Statement
```
switch (EXPRESSION) {  //evaluated once into a value, has to be int, character, or enum.
  case x:  
   //CODE_BLOCK  
   break;  
  case y:  
   //CODE_BLOCK  
   break;  
  default:  //runs no matter the expression val
   //CODE_BLOCK  
   break;  
}
```

### Iteration
1. For loop
	1. `for (VARIABLE; CONDITION; CHANGE) { BODY }`
	2. are best when we know how many times we want to loop
2. While Loop
	1. `while (CONDITION) { BODY }`
	2. are best when we are waiting for some condition to turn true.
3. Do While Loop
	1. `do { BODY } while (CONDITION);`
	2. when are waiting for some condition to turn true BUT we want to do the action at least once before checking the condition.


### Vector & Strings
1. Vector:  a data structure that holds any number of values
```
#include <iostream>
#include <vector>int main() {
  std::vector<int> vect{1, 2, 3, 4, 5};
  std::vector<int> even_values;
  for (unsigned int i = 0; i < vect.size(); ++i) {
    if (vect.at(i) % 2 == 0)
      even_values.push_back(vect.at(i));
  }
  std::cout << "Even values found: " << std::endl;
  for (unsigned int i = 0; i < even_values.size(); ++i) {
    std::cout << even_values.at(i) << std::endl;
  }
}
```
2.  String: a data structure that hold a series of characters
```
std::string input = "Howdy world.";
std::string revised;
for(unsigned int i = 0; i < input.size(); ++i) {
  switch(input.at(i)) {
    case '.' :
      break;
    default :
      revised += input.at(i);
  }
}
 
std::cout << "input: " << input << std::endl;
std::cout << "revised: " << revised << std::endl;

```

### Basic compilation
1. Terminology
	1. Root Directory: The main folder of your project
	2. Makefile: A file that allows you to easily compile your code with a single command.
		1. `make exec`: compiles your code with your main function
		2. `make tests`: compiles your code with the tests
		3. Both create a corresponding executable file in the bin folder.
2. Compilation Command
	1. format: `<compiler> <flags> <files to compile and link> <output executable location and name>`
	2. compiler:  `clang++`
	3. flags: 
		1. grading your code and so recommend for testing locally: `-std=c++20 -Wall -Wextra -Werror -O1 -gdwarf-4`
	4. output location: `bin/exec`
	5. signify you are specifying the output executable location and name: `-o`
	6. specify that your code should look in the includes folder for included .hpp files: `-Iincludes`
	7. Note you only include .cc files in the compiling command
3. Compilation Process
	1. At a high level the process involves two steps
		1. Compiling each individual file
			1. look at .cc files and firstly resolve `#include` 
			2. at this stage, syntax and declaration is checked
		2. Linking together the compiled files
			1. look for definitions of all functions declared and used
			2. looks for a single main function (throw a linker error and fail otherwise)
	2. After both of these steps you will now have an executable file you can run
		1. type `./bin/exec` into the terminal from the root directory