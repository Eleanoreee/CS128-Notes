
### BSTs( binary search trees)

1. Intro
	1. Let x be a node in the BST,  If y is a node in the left subtree of x,  then y.key < x.key
	2. height: From 0 to n
	3. depth: index from 0
2. BST traversals
	1. inorder traversal: ascending order
	2. preorder traversal: process the root before go to subtrees 
	3. postorder traversal: process the root after go to subtrees
3. BST construction
4. BST insertion and destruction
5. BST queries



6. code practice: double AverageValue()
```
double CountSum(Node<int>* node) {
	if (node == nullptr) {
		return 0.0;
	}
	return node->value + CountSum(node->left) + CountSum(node->right);
}

int CountNumber(Node<int>* node) {
	if (node == nullptr) {
		return 0;
	}
	return 1 + CountNumber(node->left) + CountNumber(node->right);
}

double IntegerTree::AverageValue() {
	if (root_ == nullptr) {
		throw std::runtime_error("Nullptr");
	}
	double sum = CountSum(root_);
	int number = CountNumber(root_);
	return sum / number;
}
```

2. code practice: void Insert(const T& value), destructor
```
#ifndef BST_HPP
#define BST_HPP

#include "bst_node.hpp"

template <typename T>
class Bst {
public:
	void Insert(const T& value);
	~Bst();
	Bst() = default;

private:
	Node<T>* root_ = nullptr;
	
	void InsertHelper(Node<T>*& root, const T& value) {
		if (root == nullptr) {
			root = new Node<T>(value);
		} else if (value < root->value) {
			InsertHelper(root->left, value);
			root->left->parent = root;
		} else {
			InsertHelper(root->right, value);
			root->right->parent = root;
		}
	}
	
	void DeleteHelper(Node<T>* root) {
		if (root == nullptr) {
			return;
		}
		DeleteHelper(root->left);
		DeleteHelper(root->right);
		delete root;
	}
	
};

template <typename T>
void Bst<T>::Insert(const T& value) {
	InsertHelper(root_, value);
}

template <typename T>
Bst<T>::~Bst() {
	DeleteHelper(root_);
}


#endif

```

3. code practice: FindNode(T value)
```
#ifndef BST_HPP
#define BST_HPP

#include "bst_node.hpp"

template <typename T>
class Bst {
public:
  // assigned
	Node<T>* FindNode(T value);
  // provided
	Bst(T root_value);

private:
	Node<T>* root_ = nullptr;
	
	Node<T>* FindHelper(Node<T>* node, T value) {
		if (node == nullptr || node->value == value) {
			return node;
		}
		if (value < node->value) {
			return FindHelper(node->left, value);
		}
		return FindHelper(node->right, value);
	}
};

template <typename T>
Bst<T>::Bst(T root_value) {
	root_ = new Node<T>(root_value);
}

template <typename T>
Node<T>* Bst<T>::FindNode(T value) {
	return FindHelper(root_, value);
}

#endif

```

4. code practice: SuccessorNode
```
#ifndef BST_HPP
#define BST_HPP

#include "bst_node.hpp"

template <typename T>
class Bst {
public:
  // assigned
  Node<T>* SuccessorNode(Node<T>* node);
  
  // provided
  Bst(T root_value);

private:
  Node<T>* root_ = nullptr;
  
  Node<T>* SuccessorHelper(Node<T>* node) {
    if (node == nullptr) {
	    return nullptr;
    }
    if (node->right != nullptr) {
	    Node<T>* successor = node->right;
	    while (successor->left != nullptr) {
		    successor = successor->left;
	    }
	    return successor;
    }
    Node<T>* parent = node->parent;
    while (parent != nullptr && node == parent->right) {
	    node = parent;
	    parent = parent->parent;
    }
    return parent;
  }
};

template <typename T>
Bst<T>::Bst(T root_value) {
  root_ = new Node<T>(root_value);
}

template <typename T>
Node<T>* Bst<T>::SuccessorNode(Node<T>* node) {
  return SuccessorHelper(node);
}

#endif

```