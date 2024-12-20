### move semantics
1. rule of three: destructor, copy constructor, copy assignment operator should be defined at the same time
2. C++11 has: 
	1. move constructor
		1. ```Type(Type<T>&& rhs)```
	2. move assignment operator
		1. ```Type& operator=(Type<T>&& rhs)```
	3. then it becomes rule of five
	4. allow to define the semantics of a move operation for user-defined types
3. TypeName&&
	1. rvalue reference, use to identity objects that been moved
	2. rvalue vs. lvalue
		1. lvalue: an object occupies a identifiable location in memory that lives beyond expression (can be modified)
		2. rvalue: not lvalue (temporary)
4. object ownership: Transferring ownership explicitly with ```std::move()```
	1.  ```std::move()``` cast argument to rvalue
	2. allowed to invoke move constructor and move assignment operator explicitly!
5. summary
	1. ![[Screenshot 2024-04-27 at 1.42.19 PM.png]]

### smart pointers
1. Raw pointers are difficult to love
	1. do not state it points to a single obj or array
2. smart pointers provides:
	1. behaviors of raw pointers
	2. safeguarding
3. 4 smart pointers with ```#include<cmemory>```
	1. ```std::unique_ptr<T>```
		1. exclusive-ownership resource manage
	2.  ```std::shared_ptr<T>```
		1. shared-ownership resource manage
		2. no specific ```std::shared_ptr<T>``` will own the obj. the last ```std::shared_ptr<T>``` obj standing destroys the obj pointed to
	3.  ```std::weak_ptr<T>```
		1. dosen't participate in the shared ownership
	4.  ```std::auto_ptr<T>``` with c++98 
	5. ```std::make_unique``` and ```std::make_shared```
		1. used to initialize ```std::unique_ptr<T>``` and ```std::shared_ptr<T>```

### undirected graph type using adjacency lists
1. use std::map to map a vertex to all its neighbors(std::list)
	1. ```std::map<std::string, std::list<std::string>> graph_```
	2. ```size_t no_verticies_``` and ```size_t no_edges_``` no refers to number
2. constructor
	1. ```UndirectedGraph() = default;```
3. member function
 ```void AddVertex(const std::string& vertex) {
		if (VertexInGraph(vertex)) {
			throw std::runtime_error("vertex already in graph.");
		}
		graph_.insert(std::pair(vertex, std::list<std::string>()));
	}
	
	void AddEdge(const std::string& v1, const std::string& v2) {
		if (!VertexInGraph(v1) || !VertexInGraph(v2)) {
			throw std::invalid_argument("vertex not in graph.");
		}
		auto& adj_list_v1 = GetAdjacencyList(v1);
		auto& adj_list_v2 = GetAdjacencyList(v2);
		if (std::find(adj_list_v1.begin(), adj_list_v1.end(), v2) == adj_list_v1.end()) {
			adj_list_v1.push_back(v2);
		}
		if (std::find(adj_list_v2.begin(), adj_list_v2.end(), v1) == adj_list_v2.end()) {
			adj_list_v2.push_back(v1);
		}
	}
	
	bool AreAdjacent(const std::string& v1, const std::string& v2) {
		if (!VertexInGraph(v1) || !VertexInGraph(v2)) {
			return false;
		}
		const auto& adj_list_v1 = GetAdjacencyList(v1);
		const auto& adj_list_v2 = GetAdjacencyList(v2);
		if (std::find(adj_list_v1.begin(), adj_list_v1.end(), v2) != adj_list_v1.end()) { //std::find return last if no
			return true;
		}
		if (std::find(adj_list_v2.begin(), adj_list_v2.end(), v1) == adj_list_v2.end()) {
			return true;
		}
		return false; 
	}
```

### Breadth-first search
```
unsigned int NoConnectedComponents() {
	std::map<std::string, bool> visited;
	unsigned int components = 0;
	
	// initilize all as unvisited
	for (const auto& vertex : graph_) {
		visited[vertex.first] = false;
	}
ws	
	// iterate
	for (const auto& vertex : graph_) {
		if (!visited[vertex.first]) {
			components++;
			// Perform BFS
			std::queue<std::string> queue;
			queue.push(vertex.first); // unvisited added to queue
			visited[vertex.first] = true; // make it visited
			while (!queue.empty()) {
				std::string current = queue.front();
				queue.pop();
				for (const auto& neighbor : graph_[current]) {
					if (!visited[neighbor]) {
						visited[neighbor] = true; //mark as visited
						queue.push(neighbor); // add back to queue
					}
				}
			}
		}
	}
	return components;
}
```

### design patterns
1. essential elements:
	1. name
	2. the problem it solves
	3. the solution
	4. the consequences: results and trade-off
2. "Micro-patterns"
	1. Most-wanted holder: compare all and identity best one
	2. One-way flag: identity if a property is true or false
	3. Follower: compare adjacent elements

### iterators
1. Basic STL model:
	1. algorithm: manipulate data
	2. iterator
	3. containers: store data
		1. end of container: One-past the last item.
2. read/write iterator: .begin(), .end()
3. read-only: .cbegin()
4. An iterator is an abstraction of a pointer