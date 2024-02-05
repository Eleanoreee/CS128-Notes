### Debugger
1. continue
2. step over
3. step into
4. step out

### Structs
1. def: an aggregate of elements of nearly arbitrary types
	1. members are public by default

### Introduction to classes
1. def: representation and operation for a user-defined type
```
#include <iostream>
class Rectangle {
private: 
	double length; 
	double width;
public:
	Rectangle(double len, double wid) : length(len), width(wid) {} 
	double area() { return length * width; }
};

int main() {
	Rectangle rectangle1(5.0, 3.0);
	std::cout << "Area of rectangle1: " << rectangle1.area() << std::endl;
	retrun 0;
}
// A `static` member variable is shared among all instances of the class.

```
2. Structs Vs. Classes
	1.  Class members are private by default.
	2. Class used for more complex data structures where encapsulation and information hiding are emphasized.
3. Class
	1. Accessor (Getter) Methods
		1. read-only access to the private members
		2. They typically return the current value of a private attribute.
	2. Mutator (Setter) Methods
		1. provide a way to modify the private members of a class.
		2. They typically take a parameter and update the value of a private attribute.


### Introduction to classes cont
1. Constructors and Destructors
	1. Constructors are special member functions that get called when an object is created.
	2. Destructors, on the other hand, are called when an object goes out of scope or is explicitly deleted.
```
// Constructor
Rectangle(double width, double height) : width_(width), height_(height) {
	if (width_ <= 0 || height_ <= 0) {
		throw std::invalid_argument("Width and Heights should be greater than 0"); } 
	std::cout << "Constructor called!" << std::endl; 
} 

// Destructor 
~Rectangle() { 
	std::cout << "Destructor called!" << std::endl; 
}
```
2.  In-class member initializers
3. Public / private distinction
	1. Public Access Specifier
		1. accessible from outside the class.
		2. can be accessed by objects of the class, as well as external functions.
	2. Private Access Specifier
		1. not accessible from outside the class
		2. only be accessed by member functions of the same class
4. Interfaces
	1. abstract class
```
class Shape {
public: virtual double calculateArea() const = 0; 
}; 

class Circle : public Shape {
private: 
	double radius_;
public: 
	Circle(double radius) : radius_(radius) {} 
	double calculateArea() const override {
	return 3.14 * radius_ * radius_;
	} 
};
```
Here, `Shape` serves as an "interface" with a pure virtual function `calculateArea()`, and `Circle` is a concrete class that implements this interface.
```
#include "running-total.hpp"
#include "running-total-history-entry.hpp"
#include <vector>
#include <string>

RunningTotal::RunningTotal():total_(0) {
}

int RunningTotal::CurrentTotal() const {
  return total_;
}

void RunningTotal::Add(int value) {
  total_ += value;
  RunningTotalHistoryEntry entry;
  entry.operation = '+';
  entry.value = value;
  history_.push_back(entry);
}

void RunningTotal::Subtract(int value) {
  total_ -= value;
  RunningTotalHistoryEntry entry;
  entry.operation = '-';
  entry.value = value;
  history_.push_back(entry);
}

std::string RunningTotal::History() const {
  std::string str = "0";
  for (size_t i = 0; i < history_.size(); ++i) {
    const RunningTotalHistoryEntry& entry = history_[i];
    str += entry.operation + std::to_string(entry.value);
  }
  return str;
}
```
5. non-member helper function
	1. Instead of being a member of a class, it is declared as a non-member function and often marked as a `friend` of the class if it needs access to private members.
	2. particularly useful when overloading operators or providing utility functions that are related to a class but don't need direct access to its private members

### Function overloading
1. define multiple functions with the same name but with different parameter
2. in .hpp in class Point
```
Point operator+(const Point& other) const {
    return Point(x + other.x, y + other.y);
}
```
1. `operator+`:  function name
2. `(const Point& other)`:  declares the function to take a const reference to a`Point` object (`other`) as its parameter. `const` ensures that the `other` object won't be modified.
3. `{ return Point(x + other.x, y + other.y); }`: returns a new `Point` object whose `x` coordinate is the sum of the `x` coordinates of the current object and the `other` object, and similarly for the `y` coordinateP
```
// Overloading operator<< as a non-member function
friend std::ostre& operator<<(std::ostream& os, const Point& point); 

// Definition of the overloaded operator<< std::ostream& operator<<(std::ostream& os, const Point& point) {
	os << "(" << point.x << ", " << point.y << ")"; 
	return os;
}
```