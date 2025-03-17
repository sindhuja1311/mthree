# Angular vs React and DSA

## Angular vs React

### Model-View-Controller (MVC)
MVC is a software design pattern used to organize code in a structured way, especially in web applications. It separates an application into three interconnected components:

1. **Model** – Manages data and logic.
2. **View** – Represents the user interface.
3. **Controller** – Handles user input and acts as a middleman between Model and View.

#### How MVC Works (Step-by-Step):
1. User interacts with the application (e.g., clicks a button).
2. Controller processes the request and retrieves data from the Model.
3. Model fetches the data (e.g., from a database).
4. Controller sends the data to the View.
5. View updates and displays the new UI to the user.

### Data Binding
Data binding connects the UI (what the user sees) with the data behind it (stored in variables or a database). It ensures that when the data changes, the UI updates automatically, and vice versa.

### What is the DOM (Document Object Model)?
The **DOM** is a programming interface that represents a webpage as a tree structure, allowing JavaScript and other languages to interact with HTML & CSS dynamically.

#### Steps of DOM Processing:
1. The browser loads an HTML page.
2. It creates a **DOM tree** representing all HTML elements as objects.
3. JavaScript can modify, add, or remove elements dynamically.
4. Changes in the DOM reflect instantly on the webpage.

### Virtual DOM in React
The **Virtual DOM** is a lightweight copy of the Real DOM that helps React optimize updates and improve performance.

#### Steps of Virtual DOM:
1. **Render the Virtual DOM** – React creates an in-memory representation of the UI.
2. **Detect Changes** – When state or props change, React generates a new Virtual DOM.
3. **Compare Virtual DOMs** – React finds differences between the old and new Virtual DOM (diffing).
4. **Update the Real DOM Efficiently** – React updates only the changed elements.
5. **Commit & Repaint** – The browser updates the UI smoothly.

## Data Structures in Python

1. **Stack**
2. **Binary Tree and Traversals**
3. **Binary Search Trees (BSTs)**
4. **Using Tries to Search for a Word in a Database**

---

## Stack Implementation in Python
```python
class Stack:
    def __init__(self):
        self.items = []

    def is_empty(self):
        return len(self.items) == 0

    def push(self, item):
        self.items.append(item)

    def pop(self):
        return self.items.pop()

    def peek(self):
        return self.items[-1]

    def size(self):
        return len(self.items)

    def __str__(self):
        return str(self.items)
```

---

## Tree Implementation in Python
```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.children = []

    def add_child(self, child):
        self.children.append(child)

    def remove_child(self, child):
        self.children.remove(child)
```

---

## Binary Search Tree (BST) Implementation in Python
```python
class BSTNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

    def insert(self, value):
        if value < self.value:
            if self.left is None:
                self.left = BSTNode(value)
            else:
                self.left.insert(value)
        elif value > self.value:
            if self.right is None:
                self.right = BSTNode(value)
            else:
                self.right.insert(value)

    def inorder_traversal(self):
        if self.left:
            self.left.inorder_traversal()
        print(self.value, end=" ")
        if self.right:
            self.right.inorder_traversal()
```

---

## Tries Implementation in Python
```python
import time
from nltk.corpus import words
import random

class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
    
    def search(self, word):
        start_time = time.perf_counter()
        node = self.root
        for char in word:
            if char not in node.children:
                return "Not Found", time.perf_counter() - start_time
            node = node.children[char]
        return ("Found" if node.is_end_of_word else "Not Found"), time.perf_counter() - start_time

# Sample data for testing
word_list = words.words()
keywords = random.sample(word_list, 200000)
search_word = random.choice(keywords)

# Using Trie
tree = Trie()
for word in keywords:
    tree.insert(word)
result, trie_time = tree.search(search_word)
print(f"Trie Search: {result}, Time: {trie_time:.6f} sec")
```


