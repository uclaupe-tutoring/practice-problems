## CS 32 Practice Problems

### Balanced Tree

*Contributed by Abdulaziz Siddiqi*.

Implement a function to check if a tree is balanced.
For the purposes of this question, a balanced tree is defined to be a tree such that no two leaf nodes differ in distance from the root by more than one.

#### Solution

The solution is also available through [codepad](https://code.hackerearth.com/e0f034G).

```cpp
struct Node {
  int m_data;
  Node *leftChild = nullptr, *rightChild = nullptr;
  Node(int data) {
    m_data = data;
  }
};
int helper(Node* topNode) {
  int sum = 1;
  if (topNode == nullptr)
    return 0;
  Node* leftChild = topNode->leftChild;
  Node* rightChild = topNode->rightChild;
  sum += helper(leftChild);
  sum += helper(rightChild);
  return sum;
}

bool isBalancedTree(Node* root) {
  if (root == nullptr)
    return true;
  Node* leftChild = root->leftChild;
  Node* rightChild = root->rightChild;
  bool isLeftBalanced = isBalancedTree(leftChild);
  bool isRightBalanced = isBalancedTree(rightChild);
  if (isLeftBalanced && isRightBalanced) 
    return ((helper(leftChild) - helper(rightChild)) < 2) ? true : false;
  else
    return false;
}
```

---

### In-order Binary Tree Traversal

*Contributed by Abineet Das Sharma*.

Given a binary tree, perform an in-order traversal iteratively.

![In-order](https://upload.wikimedia.org/wikipedia/commons/7/77/Sorted_binary_tree_inorder.svg)

In-order traversal: `A, B, C, D, E, F, G, H, I`.

#### Solution

We use the stack library in order to keep track of our traversal deep into the left side of the binary tree. Once we reach the end we start outputting and visiting the right nodes from bottom upward.

The solution is also available through [codepad](https://code.hackerearth.com/b1a510U).

```cpp
#include<iostream>
#include<stack>

using namespace std;

struct Node {
  Node* left;
  Node* right;
  int data;
};

void inorder(Node* p) {
  stack<Node*> MyStack;

  do {
    while (p != nullptr) {
      MyStack.push(p);
      p = p->left;
    }

    if (!MyStack.empty()) {
      p = MyStack.top();
      MyStack.pop();

      cout << p->data << endl;

      p = p->right;
    }

  } while(!MyStack.empty() || p != nullptr);
}
```

---

### Reverse a Linked List

*Contributed by Aditya Raju*.

Given the head pointer of a linked list, write a function, reverse(Node *head), to reverse the linked list, and return the new head. You are given the following struct:
```cpp
typedef struct Node {
  int value; 
  Node* next;
} Node;
```

#### Solution

The solution is also available through [codepad](https://code.hackerearth.com/976133a).

```cpp
Node *reverse(Node *head) {
  Node * temp;
  Node* ptr = head;
  Node * previous = NULL;
  while(ptr != NULL) {
    temp = ptr->next;
    ptr->next = previous;
    previous = ptr;
    ptr = temp;
  }
  return previous;
}
```

---

### Rerverse Recursively

*Contributed by Algan Rustinya*.

Implement a function `reverseRecursively` that takes a string as an argument and returns the reversed string. The function must be implemented using recursion. The keywords `do`, `while`, and `for` are not allowed in the function.

#### Solution
In this solution we need to first determine the base case. The base case is when the string that need to be reverse has a length of zero or one which we do not need to reverse. For the inductive step, we take the last character of the string and put it to the front of the string. We do this inductive step until we reach the base case. 

The solution is also available through [codepad](https://code.hackerearth.com/d0b8c4l).

```cpp
string reverseRecursively(string s) {
  if (s.length() <= 1)
    return s;

  return s[s.length()-1] + reverseRecursively(s.substr(0, s.length()-1));
}
```

---

### Linked List Merging

*Contributed by Andreson Huang.*

Linked lists are constructed using the following node structure. 
```cpp
struct ListNode(int value = 0) {
  int val;
  ListNode *next;
  ListNode(int value) : val(value), next(nullptr) {}
}
```
Write a function, mergeSortedLists, that takes two sorted linked lists as parameters and returns a combined, sorted list of the two. The new list should rearrange the existing nodes of the two lists. That is, no new nodes need be created. The function prototype is given. 
Assume the lists are sorted for you in non-increasing order.

For example, if you are given the following two lists:

`1->4->8->10->NULL`

`3->4->6->9->NULL`

You should generate and return a list of the form 

`1->3->4->4->6->8->9->10->NULL`

#### Solution
Start with the base cases. What happens when either `list1` is empty, `list2` is empty, or both are empty? We simply return the other list. Note that we may return a `NULL` value if both lists are empty.

Now to the interesting stuff. The important point to realize is that the separate lists are already in sorted order. Let’s think high-level. All we should need to walk element by element in both lists, compare and determine the smaller of the two elements, and stuff that node into a new list. As it turns out, that is exactly what we need to do. Now, the recursion part. After setting the current node in the new list, the next step is to determine what the current node should point to. But to decide on the next node, we must compare two elements. The two elements to be compared are the element that was greater in the last comparison, and the next node in the list with the element that was lesser in the last comparison. 

We are now ready to translate this into C++. Each chain of recursive calls will eventually terminate when the list attempts to access the last element’s next pointer (which is `NULL`). When all nodes have been processed, the head of the list is returned.

The solution is also available through [codepad](https://code.hackerearth.com/f963abX?key=f3045195352f253e8db112b89a996f18).

```cpp
ListNode* mergeSortedLists(ListNode* l1, ListNode* l2) { 
  // Base cases
  if(list1 == nullptr)
    return list2; 
  if(list2 == nullptr)
    return list1; 
  
  ListNode* head;
  if(list1->val < list2->val) {
    head = list1; 
    head->next = mergeSortedLists(list1->next, list2); 
  }
  else{
    head = list2; 
    head->next = mergeSortedLists(list1, list2->next);
  }
  return head;    
}
```

---

### Balanced Brackets

*Contributed by Andrew Lee.*

Given a sequence of only brackets, determine whether the sequence is balanced. A sequence is balanced when every open bracket can be matched uniquely with a closing bracket of the same type. The empty string is considered balanced. The types of brackets are as follows: `( [ {`. 

Balanced expression: `{{[(())]}}`

Unbalanced expression: `{{[(])}}  // you can't have [(])`

Hint: Consider stacking up the braces.


#### Solution

Our solution is based on the observation that in order to be balanced, for every open bracket, you must have a closing bracket. Furthermore, if you see a closing bracket, the nearest open bracket must be of the same type. 
The way we would implement it is as follows: If we see an open bracket, push it onto the stack, else, it’s a close bracket, so we check the top to see whether it is an open bracket of the same type. If it is, continue; else, return false. By the time you’ve gone through the entire string, if the stack were balanced, the stack would be empty.

The solution is also available on [codepad](https://code.hackerearth.com/358f8cM?key=0517d477bebfe0ac3909cb9d199d8e2b).

```cpp
#include <stack>

bool isBalanced(string &s) {
  stack<char> stk;

  // consider the ith char of the string
  for (int i = 0; i < s.size(); i++) {
    // If the ith char of a string is an open bracket, add it to stack.
    if (s[i] == '(' || s[i] == '{' || s[i] == '[') {
      stk.push(s[i]);
    }

    // else it's a close bracket 
    // edge case: if it's a close bracket and stack is empty, it's unbalanced
    else if (stk.empty())  
      return false;
    else {
      // check whether the top is a matching open bracket 
      // if true, keep going; else, not balanced, return false
      bool balanced = true;
      switch (s[i]) {
      case ')':
        if (stk.top() != '(') balanced = false; break;

      case '}':
        if (stk.top() != '{') balanced = false; break;

      case ']':
        if (stk.top() != '[') balanced = false; break;
      }

      if (!balanced)
        return false;
      else
        stk.pop();
    }
  }

  return stk.empty();
}
```

---

### Undirected Graph Counting

*Contributed by Andy Shih.*

Determine the number of connected components in an undirected graph
  1. using a stack
  2. using recursion

#### Solution
This is a basic graph traversal problem. We can use depth-first search to tackle
this problem because we do not need to know the distances between each vertex
(what breadth-first search is good at).

We simply start from vertex 0 and perform DFS, marking all the vertices that we
touch during the traversal. At the end of the first DFS, we would have traversed
one connected component, so we increase our result counter by one.

We continue this process for every vertex in the graph. If the vertex has
already been marked as seen, then we know that we have already counted that
connected component, so we do not increase the counter. Else, if this is a new
vertex, we perform DFS on it, mark all the vertices we traverse to, and increase
the result counter by one.

Note that one and only one vertex in each connected component will have DFS
performed on it. This is because after we perform DFS on that vertex, every
other vertex in its connected component will have been marked as seen.
Therefore, our final count must be the number of connected components in the
undirected graph. We are done!

The solution is also available on [codepad](https://code.hackerearth.com/17cec8Q).

```cpp
////////////////////
//Stack Based Solution
////////////////////

#include <stack>
#include <vector>
#include <iostream>
using namespace std;
int num_connected_components_stack(const vector< vector<int> > &adj_graph) {
int n = adj_graph.size();
vector<bool> seen(n, false);

int count = 0;
  
  stack<int> s;
  
  for (int a = 0; a < n; a++) {
    if (!seen[a]) {
      s.push(a);
      
      while (!s.empty()) {
        int cur = s.top();
        s.pop();
        for (int i = 0; i < adj_graph[cur].size(); i++) {
          int next = adj_graph[cur][i];
          if (!seen[next]) {
            s.push(next);
            seen[next] = true;
          }
        }
      }
      count++;
    }
  }
  
  return count;
}
//Complexity: O(V+E)

////////////////////
//Recursive Solution
////////////////////

#include <stack>
#include <vector>
using namespace std;
void dfs(int u, const vector< vector<int> > &adj_graph, vector<bool> &seen) {
  seen[u] = true;
  for (int i = 0; i < adj_graph[u].size(); i++) {
    int next = adj_graph[u][i];
    if (!seen[next]) {
      dfs(next, adj_graph, seen);
    }
  }
}


int num_connected_components_recur(const vector< vector<int> > &adj_graph) {
  int n = adj_graph.size();
  vector<bool> seen(n, false);
  
  int count = 0;
  
  for (int i = 0; i < n; i++) {
    if (!seen[i]) {
      dfs(i, adj_graph, seen);
      count++;
    }
  }

  return count;
}
//Complexity: O(V+E)
```

---

### Rotate Array

*Contributed by Bradley Zhu.*

Implement a recursive function that rotates an array left until a given value is in the first slot of the array and returns the number of rotations it took.  Assume the given value is always inside the array.

#### Solution
We can write a function that first checks if the value is in the first slot.  If it is, then it returns 0, but if it’s not, it then rotates the array once to the left, and returns one plus the function called again on the newly rotated array.  Assuming the value is in the array, the ones will be recursively added after the function stops calling itself (when it returns 0). This final sum of ones is the number of rotations it took.

This solution is also available through [codepad](https://code.hackerearth.com/300a98y).

```cpp
int recursive_rotate (int arr[], int size, int value) {
  if (value == arr[0]) {
    return 0;
  }

  int temp = arr[0];
  for (int i = 0; i < size - 1; i++) {
    arr[i] = arr[i + 1];
  }
  arr[size - 1] = temp;

  return 1 + recursive_rotate(arr, size, value);
}
```

---

### Recursive Even Detection

*Contributed by Cameron Ito.*

Write a function bool `is_even(int number)` that takes an integer as a parameter. This function should use recursion to test whether or not `number` is even. It should return `true` if `number` is even, and `false` otherwise.

#### Solution
To check if `number` is even recursively, we need to call our function on itself. We need to change `number` in some way each time we call our function again. The best way to change `number` is to increment or decrement it by a value of 2. This way `number` will remain even or odd. Now, we need to figure out how our recursive calls will terminate. A good base case is when `number` becomes 0 or 1. When `number` becomes 0, the original value of `number` must have been even, so we can return true here. When `number` becomes 1, the original value of `number` must have been odd, so we can return false here. Now, we need our recursive calls to eventually change `number` into these values. If `number` is positive, we want to decrement it by 2 in our recursive call. If `number` is negative, we want to increment it by 2 in our recursive call. This will cause `number` to have less and less magnitude until it reaches 0 or 1.

This solution is also available through [codepad](https://code.hackerearth.com/4cba94v).

```cpp
bool is_even(int number) {
  if (number == 0) {
    return true;
  } else if (number == 1) {
    return false;
  } else if (number > 0) {
    return is_even(number - 2);
  } else {
    return is_even(number + 2);
  }
}
```

---

### Recursive Any-True

*Contributed by Danny Nguyen.*

Given the following condition, implement a function `any_true` that uses recursion to traverse an array of integers to determine whether or not at least one of the elements within the array will return true if passed as a parameter into `givenCondition`.
```cpp
bool given_condition(int n) {
  if (n % 2 == 0)
    return true;
  else
    return false;
}
```

#### Solution
We leverage the fact that we can simply pass the value of the next array element using pointer arithmetic, so we need not worry about physically looping through the contents of the entire array. This enables us to traverse through the array and process an individual element within a separate function call. As each recursive call is made, we simply decrement n, the number of elements remaining for examination and pass through the appropriate pointer to the next value within the array. Furthermore, given the requirements of the question, we can terminate early if our program detects that a certain element passes `givenCondition`.

This solution is also available through [codepad](https://code.hackerearth.com/e7b30fY).

```cpp
bool any_true(const int a[], int n) {
  if (n <= 0)
    return false;
    
  if (givenCondition(a[0]))
    return true;
  else
    return any_true(a + 1, n - 1);
}
```

---

### Tree Traversal

*Contributed by Darin Truong.*

Given the following binary tree, write recursive functions that print out the tree’s values using pre-order, in-order, and post-order traversal.  
![Tree](http://i.imgur.com/tkipN7c.png)

#### Solution
Recursion makes the task of printing out a tree’s elements a relatively easy task; using recursion, we can reuse our function to sequentially print out elements from the current node’s left and right subtrees.  Once we have our function, alter the body to create three versions that print the tree’s nodes in the proper order.

This solution is also available through [codepad](https://code.hackerearth.com/bd6872y).

```cpp
void pre(Node* head) {
  if(head == nullptr)
    return;
  std::cout << head->val << “ ”;
  pre(head->left);
  pre(head->right);
}
void in(Node* head) {
  if(head == nullptr)
    return;
  in(head->left);
  std::cout << head->val << “ ”;
  in(head->right);
}
void post(Node* head) {
  if(head == nullptr)
    return;
  post(head->left);
  post(head->right);
  std::cout << head->val << “ ”;
}
```

---

### Remove All in a Linked List

*Contributed by Eugene Liu.*

Implement the function `remove_all_occurrences` that removes every occurrence of the given value in a singly-linked list, and returns the new head of the list. Remember to free any memory used by removed nodes. Use the following node structure:
```cpp
struct Node {
  int val;
  Node *next;
}
```
#### Solution

This solution is also available through [codepad](https://code.hackerearth.com/a5d2ddT).

```cpp
Node* remove_all_occurrences(Node *list, int value) {
  //check if the value is stored at the head of the list
  while (list != nullptr && list->val == value) {
    Node *to_delete = list;
    list = list->next;
    delete to_delete;
  }
  
  //return null if the whole list got removed
  if (list == nullptr)
    return nullptr;
  
  Node *head = list;
  Node *current = list; //added variable for clarity
  Node *next = list->next;
  
  //go through the linked list
  while (next != nullptr) {
    if (next->val == value) { //found a matching node, time to remove it
      //reconnect the surrounding nodes, keep current where it is
      //in case there is another matching node after this one
      current->next = next->next;
      delete next;
    } else { //no match found, continue searching
      current = current->next; 
    }
    next = current->next;
  }
  
  return head;
}
```

---

### Binary Tree Sum

*Contributed by Greg Giebel.*

Write a recursive function that returns the sum of all the items in a binary tree.
Explanation: First, check for empty tree. If non-empty, we will have to call sum on each of the existing children that the node has and add these into a total that includes the current node’s value. Returns the sum of its own value and the sums of each of its children. This assumes a `Node` struct with public members `data` (holds integer data value), `L` (pointer to L child) and `R` (pointer to R child).

#### Solution

This solution is also available through [codepad](https://code.hackerearth.com/cfefcbp).

```cpp
struct Node {
    int data;
    Node* R;
    Node* L;
};
int sum(struct Node* current) {
  if (current == NULL)
    return 0;
  int total = current->data;
  if(current->L != NULL)
    total += sum(current->L);
  if(current->R != NULL)
    total += sum(current->R);
  return total;
}
```

---

### Insertion Sort

*Contributed by Isaac Yeo En Jie.*

Implement the function `insertion_sort` that takes an array and returns a sorted array using the insertion sort algorithm.  Assume you are given the function `swap`. The function `swap` takes in 2 parameters passed by reference, and swaps the values that they hold.
#### Solution
The insertion sort algorithm always maintains a sorted sublist in the lower positions of the list. Each new item is then “inserted” back into the previously sorted sublist such that the sorted sublist is one item larger.

This solution is also available through [codepad](https://code.hackerearth.com/5d75a3Y).

```cpp
void swap(int &a, int &b) {
  int temp = a;
  a = b;
  b = temp;
}

void insertion_sort(int arr[], int size) {
  for (int i = 1; i < size; i++) {
    int j = i;
    while (j > 0 && arr[j] < arr[j - 1]) {
      swap(arr[j], arr[j - 1]);
      j--;
    }
  }
}
```

---

### Inheritance Practice 

*Contributed by Iza Say.*

Your job is to design the following three classes: `Clothes`, `Shirt`, and `Pants`. The classes `Shirt` and `Pants` should inherit from `Clothes` publicly, as seen in the skeleton we have provided.
 
You will also implement the function `count_complete_outfits` that takes a vector of `Clothes*`, counts the number of shirts and pants, and returns the number of complete outfits in the vector. A complete outfit is defined as a pair of shirt and pants. 

For example, if you had 3 shirts and 3 pants, you would have 3 complete outfits. If you had 4 shirts and 7 pants, you would only have 4 complete outfits and 3 leftover pants.
 
Your function should also print a message telling the user whether the vector contained only complete outfits or not.

Design the classes in such a way that allows you to implement the `count_complete_outfits` function.

You may define any necessary member functions and member variables to achieve your goal. In the case of any errors, invalid input, etc., just return -1.

Your output should follow this format:
  
```cpp
They were all complete outfits!

There were some lonely shirts/pants. :(
```

Skeleton code:

```cpp
class Clothes {

};

class Shirt : public Clothes {

};

class Pants : public Clothes {

};

int count_complete_outfits(const vector<Clothes*>& vec) {

} 
```

#### Solution
This problem can be implemented in two different ways: can either specialize the `Shirt` and `Pants` constructors OR have a pure virtual function that returns a different string for each `Shirt` and `Pants`. The first method is probably simpler.

This solution is also available through [codepad](https://code.hackerearth.com/e33695t?key=cea328937bdf89adca7f7c2cbec2265b).

```cpp
class Clothes {
  private:
    string type; //could also work as an int, char, etc.

  public:
    Clothes(string s): type(s) { }
    string GetType() const { return type; } 
};

class Shirt : public Clothes {
  public:
    Shirt(): Clothes(“shirt”) { }
};

class Pants : public Clothes {
  public:
    Pants(): Clothes(“pants”) { }
};

int count_complete_outfits(const vector<Clothes*>& vec) {
  int numShirts = 0, numPants = 0;
  int numOutfits = 0;
  string cType;

  for (unsigned int i = 0; i < vec.size(); i++) {
    //for every piece of clothing
    if(vec[i] != nullptr)
      cType = vec[i]->GetType();
    else
      return -1;
    
    if (cType == “shirt”)
      numShirts++;
    else if (cType == “pants”)
      numPants++;
  }
  numOutfits = (numShirts == numPants) ? numShirts : 
               (numShirts < numPants ? numShirts : numPants);
  if(numShirts == numPants)
    cout << “They were all complete outfits!\n”;
  else
    cout << “There were some lonely shirts/pants. :(\n”;

  return numOutfits;
}

```

---

### Max Heap Insertion

*Contributed by Jai Srivastav.*

Write a function that inserts an element into a max heap.  The max heap is implemented with an array with the root node at index 0.  The function prototype is given below.  Also change the passed in `size` variable to its correct value after the element is added.  You may write as many helper functions as necessary.  Assume there is enough space in the max heap to insert one more element.

```cpp
void addElement(int* max_heap, int& size, int element);
```
#### Solution
We use the traditional method of putting the element at the end of our heap and then heapifying upwards starting from the newly inserted element.  Heapifying upwards consists of looking at an element’s parent. If the element should be above its parent, we swap the values and then heapify the parent.  If the element and its parent are in the right places we return from the function.  Note that we find the parent of an element at position pos at `(pos - 1 ) / 2`. 

This solution is also available through [codepad](https://code.hackerearth.com/f53411X).

```cpp
int parent(int pos) { return (pos - 1) / 2; }

void heapify(int* max_heap, int pos) {
  if (max_heap[parent(pos)] < max_heap[pos]) {
    std::swap(max_heap[parent(pos)], max_heap[pos]);
    heapify(max_heap, parent(pos));
  }
}

void addElement(int* max_heap, int& size, int element) {
  max_heap[size] = element;
  heapify(max_heap, size);
  ++size;
}
```

---

### 

*Contributed by Jerin Tomy.*
Consider the following series of classes:
```cpp
class Animal {
public:
  Animal() {
    cout << "I am an animal." << endl;
  }
  virtual ~Animal() {
    cout << "This animal is leaving." << endl;
  }
  virtual void speak() = 0;
};

class Toy {
public:
  Toy(string name) {
    m_name = name;
    cout << "This is a " << name << endl;
  }
  ~Toy() {
    cout << "This " << m_name << " is gone!" << endl;
  }
private:
  string m_name;
};

class Dog: public Animal {
public:
  Dog()
  :m_toy("Tennis Ball") {
    cout << "I am a dog." << endl;
  }
  virtual ~Dog() {
    cout << "This dog is leaving." << endl;
  }
  virtual void speak() {
    cout << "WOOF!";
  }
private:
  Toy m_toy;
};
class Puppy: public Dog {
public:
  Puppy() {
    cout << "I am a puppy." << endl;
  }
  virtual ~Puppy() {
    cout << "This puppy is leaving." << endl;
  }
  virtual void speak() {
    cout << "woof!" << endl;
  }
};
```
a. If your main function is as follows:
```cpp
int main() {
  Animal* a = new Puppy();
  delete a; 　
}
```
What will be printed out from the execution of this program?

b. Suppose the following code was added to the previous statement:
```cpp
int main() {
  Animal* a = new Puppy();
  a->speak();
  delete a; 　
}
```
What new output would be the result of this new line? 


#### Solution
a. Before `Puppy`’s constructor is called, it must first call its superclass’ constructor. Before `Dog`’s constructor is called, it must first call its superclass’s constructor. Once `Animal’`s constructor is called, then the embedded object variable’s constructor is called. Only then is `Dog`’s constructor called, followed by `Puppy`’s Constructor. The reverse process happens during destruction.
```cpp
I am an animal.  
This is a Tennis Ball.
I am a dog.
I am a puppy.
This puppy is leaving.
This dog is leaving
This Tennis Ball is gone!
This animal is leaving.
```
b. Because of polymorphism, and due to the fact that speak is virtual and redefined in `Puppy`, when speak is called, it will go to `Puppy`’s code. The remainder of the lines are unchanged.
```cpp
I am an animal. 
This is a Tennis Ball.
I am a dog.
I am a puppy.
woof!
This puppy is leaving.
This dog is leaving
This Tennis Ball is gone!
This animal is leaving.
```

This solution is also available through [codepad](http://codepad.org/Xyw2iwBr).

---

### Valid Parenthesis

*Contributed by Jerry Li.*

Write a function that takes an input string of parentheses and returns `true` or `false` on whether it is a valid grouping, i.e. every left parenthesis ( must be matched with an appropriate right parenthesis ).

For example:

`validGrouping(“()(()())”)` will return true

`validGrouping(“())()(”)` will return false

`validGrouping(“)”)` will return false

`validGrouping(“()()”)` will return true

`validGrouping(“”)` will return true

#### Solution
For a string to be a valid grouping, it must satisfy two properties:

It must have the same number of left parentheses and right parentheses. 
As we traverse the string from left to right, there cannot have been fewer left parentheses seen than right parentheses. If there were, then some number of right parentheses will not have a matching left parenthesis before it to pair up with.

We keep track of the left parentheses in a stack, and traverse the string from left to right. When we see a left parenthesis, we will push it onto the stack. When we see a right parenthesis, we match it with the most recent left parenthesis seen (i.e. the one on the top of the stack), popping it off the top of the stack to signify that it has been matched. However, if the stack is empty when we see a right parenthesis, it means that we have violated property 2 above; there are no available left parenthesis to be matched with that right parenthesis, so this is not a valid grouping and we return `false`.

By the end of the string traversal, the stack should be empty. If it is not empty, then that means there are still some number of unmatched left parenthesis, so this is not a valid grouping and we return `false`. Finally, if we reached the end of the function, the grouping is valid so we return `true`.

This solution is also available through [codepad](https://code.hackerearth.com/9a8f65m).

```cpp
#include <string>
#include <stack>
using namespace std;

bool validGrouping(string str) {
  stack<char> parenStack;
    
  for(int i = 0; i < str.size(); i++) {
    if(str[i] == '(')
      parenStack.push(str[i]);
    else {
      if(parenStack.empty())
        return false;
      parenStack.pop();
    }
  }
  
  if(!parenStack.empty())
    return false;
  
  return true;
}
```

---

### Integer Binary Tree

*Contributed by Jonathan Chu.*

Create an integer binary tree with two methods, an `insert()` method to add an integer to the tree, and a `print()` method to print all values currently in the tree in sorted order, along with the number of times that value appears in the tree.
```cpp
class IntTree {
public:
 IntTree() {
  //your code here
 }

 ~IntTree() {
  //your code here
 }

 void insert(int val) {
  //your code here
 }

 void print_tree(int val) {
  //your code here
 }

private:
 //your code here
};

int main() {
  IntTree tree;
  tree.insert(2);
  tree.insert(3);
  tree.insert(2);
  tree.insert(3);
  tree.insert(2);
  tree.insert(1);
  tree.insert(5);
  tree.insert(2);
  tree.insert(3);
  tree.insert(2);
  tree.insert(6);
  tree.print_tree();
  /*
  Expected output: 
  1 appears 1 times.
  2 appears 5 times.
  3 appears 3 times.
  5 appears 1 times.
  6 appears 1 times.
*/
}
```

#### Solution
For this solution, we create a binary search tree with only two methods, `insert()` and `print_tree()`, outside of the constructor and destructor.  Upon insertion of any value, we do a tree traversal, starting from the head, to find the node with the selected value. If the current node is it, we can increment the count of this node and end the insertion; otherwise, we either check the left node (if it’s less than the current value) or the right node (if it’s greater than the current node), and set that as the new current, or create a new node their if there is none. To print the values, we do an inorder tree traversal, and print each node’s value with its current count. Something to note is that the nodes in this binary tree are self-destructing, in the sense that they delete their children before destroying themselves, so that the binary tree destructor can be simpler.

This solution is also available through [codepad](https://code.hackerearth.com/2aea75u).

```cpp
class Node
{
public:
 Node(int v)
 :val(v), count(1), left(nullptr), right(nullptr) {
 }
 ~Node() {
  if (nullptr != left)
   delete left;
  if (nullptr != right)
   delete right; 
 }

 int val;
 int count;
 Node* left;
 Node* right;
};

class IntTree
{
public:
 IntTree()
 :head(nullptr) {
 }

 ~IntTree() {
  delete head;
 }

 void insert(int val) {
  if (nullptr == head) {
   head = new Node(val);
  }
  else {
   Node* current = head;

   while (1) {
    if (val == current->val) {
     current->count++;
     break;
    }
    else if (val > current->val) {
     if (nullptr == current->right) {
      current->right = new Node(val);
      break;
     }
     else {
      current = current->right;
     }
    }
    else if (val < current->val) {
     if (nullptr == current->left) {
      current->left = new Node(val);
      break;
     }
     else {
      current = current->left;
     }
    }
   }
  }
 }

 void print_tree() {
  print_node(head);
 }

private:
 void print_node(Node* node) {
  if (nullptr == node)
   return;

  print_node(node->left);
  std::cout << node->val << " appears " << node->count << " times." << std::endl;
  print_node(node->right);
 }

 Node* head;
};
```

---

### Tree from Heap

*Contributed by Jorge Fuentes.*

Create a function called `create_tree` that returns a `Node` pointer. Given a heap (stored as an array) and its size `n` as parameters, return the head of the corresponding binary tree. 
```cpp
struct Node {
  Node* next;
  Node* prev;
  int val;
}
Heap_size = n;
1 | 2 | 3 | 4 | 5  ->  1
					 2    3
				    4 5

```
#### Solution
The most important thing to understand is that if a number at position `i` has children, then they will be found at `i*2+1` and `i*2+2`. You could always check if such children exist by comparing `i` to `n`. It is difficult to implement the tree iteratively because of the array-index to tree-position mapping." However, with recursion, the entire tree can be created one `Node` at a time.

This solution is also available through [codepad](https://code.hackerearth.com/2b136eG).

```cpp
Node* helper_creator(int a[], int n, int level) {
  if (n <= level)
	return nullptr;
  Node* head = new Node;
  head->val = a[level];
  if (n > 2 * level + 1)
	head->left = helper_creator(a, n, 2 * level + 1);
  if (n > 2 * level + 2)
	head->right = helper_creator(a, n, 2 * level + 2);
  return head;
}

Node* create_tree(int a[], int n) {
  return helper_creator(a, n, 0);
}
```

---

### C-String Merging

*Contributed by Joshua Sambasivam.*

Implement a recursive function that takes in two pointers to C-strings and prints out the merging of the two. Given `p = “p1p2...pn”` and `q = “q1q2...qn”`, print out `"p1q1...pnqn”`. If either parameter is longer than the other, print out its remaining characters after the merged ones. Print out a new line once all necessary characters have been printed.
#### Solution
We will use the fact that C-strings terminate on the null byte for all of our base cases and conditions. We print out the characters and recursively call ourselves using pointer arithmetic in order to reach the end of the strings. The difficulty in this problem is handling remaining characters if one string is longer than the other. Because of this, if we reach only one of the strings’ null bytes, print out the other’s current character and perform the function call without moving the string that is already finished.

This solution is also available through [codepad](https://code.hackerearth.com/6c9000l).

```cpp
void print_merged(char* a, char* b) {
  if (*a == '\0') {
    if (*b == '\0') {
      cout << endl;
      return;
    } else {
      cout << *b;
      print_merged(a, b + 1);
    }
  }
  
  else if (*b == '\0') {
    cout << *a;
    print_merged(a + 1, b);
  }

  else {
    cout << *a << *b;
    print_merged(a + 1, b + 1);
  }
}
```

---

### Jump Array

*Contributed by Keshav Tadimeti.*

**Background**

Suppose we have an array where each element is the index of another element in that same array. Let’s call this data structure a jump array. For example, a jump array of size 3 could be: 

![jumparray](http://i.imgur.com/lsm7Y77.png)

Here, the first element has the value 1, which is a valid index in the array. The second has the value 2, which is also a valid index. Finally, the last element has the value 0, which is again a valid index in the array. 

Jumping through a jump array means starting at the first element (index 0) and using that element as the index for the next element in the array. Following that, you would use the element of the next index as the index for the next element, and so on. For example, this is how you would traverse through the following jump array:

![traverse](http://i.imgur.com/N8voVoM.png)

A valid jump array is one where you can jump through it, as shown above, and reach each of its elements before returning back to the first element (index 0). 

**Question**

Write a recursive function `countElements` that determines the number of elements in a given jump array if the size is not given. Your implementation may alter the inputted array during runtime, but once it returns, the array must be identical to the one that was inputted.

You can assume that the inputted arrays will be valid jump arrays with no repeating elements and no other irregularities. In addition, feel free to use a helper function, if needed.

Here is the function prototype:

```cpp
int countElements(int jumpArr[]);
```

#### Solution
The key to this problem is realizing that a jump array is like a cycle, where jumping through it will eventually take you back to the first element of the array. While it may seem confusing at first as to how to keep track of which elements we’ve already visited while ‘jumping’, we can exploit the fact that the problem allows us to modify the array, so long as we return it back to what it originally was once we return out of the function.

For example, consider the example from the problem.

As we can see, after 3 ‘jumps’, or movements of the arrow, we’ve returned back to the first element, which is where we first began. Hence, for any jump array of size `n`, if we do `n` jumps, we will return back to the first element. This is really useful.

We can use this property by modifying each of the elements we visit to the value `-1`, so as to ‘mark’ that we’ve already visited it. In addition, `-1` isn’t a valid array index, so it wouldn’t interfere with any sort of jumping. Using this, we could write a helper function called `countHelper` to `countElements`.

This solution is also available through [codepad](https://code.hackerearth.com/6b93acj).

```cpp
int countHelper(int jumpArr[], int startIndex) {
  if (jumpArr[startIndex] == -1)
    return 0;

  int nextIndex = jumpArr[startIndex];
  jumpArr[startIndex] = -1;

  int numJumps = countHelper(jumpArr, nextIndex);

  jumpArr[startIndex] = nextIndex;
  return numJumps + 1;
}
int countElements(int jumpArr[]) {
  return countHelper(jumpArr, 0);
}
```

---

### Substitution Ciphers

*Contributed by Kevin Hsieh.*

A monoalphabetic simple substitution cipher is an encoding technique in which each character of a text is substituted for another specified character. Such a cipher is performed using a ciphertext alphabet, which maps each character to its encoded version; characters not in the alphabet are not transformed. Here is a popular example, known as ROT13. Note that ROT13 does not transform space and punctuation characters, but other ciphers may.

Alphabet: `ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz`

Ciphertext alphabet: `NOPQRSTUVWXYZABCDEFGHIJKLMnopqrstuvwxyzabcdefghijklm`

Sample input: `The quick brown fox jumps over the lazy dog.`

Sample output: `Gur dhvpx oebja sbk whzcf bire gur ynml qbt.`

**(a)** Implement the following class, which can be used to perform monoalphabetic simple substitution ciphers given a ciphertext alphabet. You may include other STL structure(s) to meet the complexity requirements.
```cpp
#include <string>  // you may include other STL structure(s)

class MonoalphabeticSSC {
public:
  // Input: an alphabet and a corresponding ciphertext alphabet; assume that 
  //   each string does not contain duplicate characters, and that the
  //   alphabets have the same length
  // Complexity: O(alphabet.length()) or better
  MonoalphabeticSSC(std::string alphabet, std::string ciphertext_alphabet);
  virtual ~MonoalphabeticSSC() { }
  
  // Input: decoded text
  // Output: encoded text
  // Complexity: O(input.length()) or better
  std::string encode(std::string input);
  
  // Input: encoded text
  // Output: decoded text
  // Complexity: O(input.length()) or better
  std::string decode(std::string input);
  
private:  // you may add private members if you wish
};
```

**(b)** The Caesar cipher is a monoalphabetic simple substitution cipher in which each letter of a text is replaced by the corresponding letter n letters to its left or right in the alphabet ('a' and 'z' loop back to each other). Use inheritance to implement a CaesarCipher class without overriding the encode or decode functions of MonoalphabeticSSC (they are not virtual). Assume that your MonoalphabeticSSC class works as intended.

Parameters: `n = 7`

Sample input: `The quick brown fox jumps over the lazy dog.`

Sample output: `Aol xbpjr iyvdu mve qbtwz vcly aol shgf kvn.`

```cpp

const std::string uppercase_alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
const std::string lowercase_alphabet = "abcdefghijklmnopqrstuvwxyz";

class CaesarCipher : public MonoalphabeticSSC {
public:
  // Input: the number of letters to shift, -26 < n < 26 (a negative n 
  //   indicates a shift to the left)
  CaesarCipher(int n);
  virtual ~CaesarCipher() { }

private:  // you may add private members if you wish
};
```


**(c)** A polyalphabetic simple substitution cipher is more difficult to break because it uses different ciphertext alphabets at different locations in the text. (The Enigma machine, used by Germany during World War II, uses this type of cipher.) Implement a basic polyalphabetic simple substitution cipher that uses a Caesar cipher with n = odd_shift at odd character positions (the 1st character is at an odd position) and n = even_shift at even character positions. Assume that your CaesarCipher class works as intended.

Parameters: `odd_shift = 13, even_shift = 7`

Sample input: `The quick brown fox jumps over the lazy dog.`

Sample output: `Gor dbvjx oybda svk wbzwf bcry aul sngl qvt.`

```cpp
class PolyalphabeticSSC {
public:
  // Input: -26 < odd_shift < 26, the shift to be used at odd indices;
  //   -26 < even_shift < 26, the shift to be used at even indices
  PolyalphabeticSSC(int odd_shift, int even_shift);

  // Input: decoded text
  // Output: encoded text
  // Complexity: O(input.length()) or better
  std::string encode(std::string input);

  // Input: encoded text
  // Output: decoded text
  // Complexity: O(input.length()) or better
  std::string decode(std::string input);

private:  // you may add private members if you wish
};
```

#### Solution
**(a)** We use unordered_map's so that encoding/decoding each character is approximately constant time. From there, the encode and decode functions are just a matter of looping through the input.

This solution is also available through [codepad](https://code.hackerearth.com/26da6dp).

```cpp
#include <string>
#include <unordered_map>

class MonoalphabeticSSC {
public:
  MonoalphabeticSSC(std::string alphabet, std::string ciphertext_alphabet) {
    for (int i = 0; i < alphabet.length(); i++) {
      encoding_map[alphabet[i]] = ciphertext_alphabet[i];
      decoding_map[ciphertext_alphabet[i]] = alphabet[i];
    }
  }

  virtual ~MonoalphabeticSSC() { }
  
  std::string encode(std::string input) {
    std::string output;
    for (char c : input)
      if (encoding_map.count(c))
        output += encoding_map[c];
      else
        output += c;
    return output;
  }
  
  std::string decode(std::string input) {
    std::string output;
    for (char c : input)
      if (decoding_map.count(c))
        output += decoding_map[c];
      else
        output += c;
    return output;
  }

private:
  std::unordered_map<char, char> encoding_map;
  std::unordered_map<char, char> decoding_map;
};
```

**(b)** Since we inherit from MonoalphabeticSSC, there is no need to re-implement the encode and decode functions. We simply need to give MonoalphabeticSSC's constructor the right alphabets. To do so, we implement a private helper function rotate that moves letters in an alphabet to the left or right by the specified number of characters (looping around as necessary). We must be careful to rotate the uppercase and lowercase letters separately and then concatenate them so that uppercase letters do not become lowercase letters and vice-versa.

This solution is also available through [codepad](https://code.hackerearth.com/26da6dp).

```cpp
const std::string uppercase_alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
const std::string lowercase_alphabet = "abcdefghijklmnopqrstuvwxyz";

class CaesarCipher : public MonoalphabeticSSC {
private: 
  // We declare this first because it is used in the constructor.
  std::string rotate(std::string str, int n) {
    if (n < 0)
      n += str.length();  // change left rotation into equivalent right rotation
    std::string output;
    for (int i = n; i < str.length(); i++)
      output += str[i];
    for (int i = 0; i < n; i++)
      output += str[i];
    return output;
  }

public:
  CaesarCipher(int n) :
    MonoalphabeticSSC(uppercase_alphabet + lowercase_alphabet,
      rotate(uppercase_alphabet, n) + rotate(lowercase_alphabet, n)) { }

  virtual ~CaesarCipher() { }
};
```

**(c)** We implement this class using two private members, both CaesarCipher's. One is used with odd indices, and the other is used with even indices. The encode and decode functions are similar to those of MonoalphabeticSSC except for the additional conditional to differentiate between odd and even indices.

This solution is also available through [codepad](https://code.hackerearth.com/26da6dp).

```cpp
class PolyalphabeticSSC {
public:
  PolyalphabeticSSC(int odd_shift, int even_shift)
    : odd_cipher(odd_shift), even_cipher(even_shift) { }

  virtual ~PolyalphabeticSSC() { }
  
  std::string encode(std::string input) {
    std::string output;
    for (int i = 0; i < input.length(); i++)
      if (i % 2 == 0)  // even i => odd character position
        output += odd_cipher.encode(std::string(1, input[i]));
      else
        output += even_cipher.encode(std::string(1, input[i]));
    return output;
  }
  
  std::string decode(std::string input) {
    std::string output;
    for (int i = 0; i < input.length(); i++)
      if (i % 2 == 0)
        output += odd_cipher.decode(std::string(1, input[i]));
      else
        output += even_cipher.decode(std::string(1, input[i]));
    return output;
  }

private:
  CaesarCipher odd_cipher;
  CaesarCipher even_cipher;
};
```

---

### Shallowest Common Ancestor

*Contributed by Mahir Eusufzai.*

Implement a function that takes a pointer to the root of a binary tree along with two values and returns a pointer to the shallowest common ancestor of the two values in the tree.  The function declaration is listed below, along with the implementation of the `Node` struct.

```cpp
Node* findLCA(Node* root, int n1, int n2)

struct Node {
  int key;
  Node *left, *right;
}
```

#### Solution

This solution is also available through [codepad](https://code.hackerearth.com/2000bdi?key=ed3480af7200ac7090e07c63142d8a6e).

```cpp
Node* findLCA(Node* root, int n1, int n2) {
  //Base case
  if (root == NULL) {
    Return NULL;
  }
  // If either n1 or n2 matches with root’s key, report the presence by 
  //returning root
  if (root->key == n1 || root->key == n2) {
    return root; 
  } 
  //Look for keys in the left and right subtrees
  Node *left_lca = findLCA(root->left, n1, n2);
  Node *right_lca = findLCA(root->right, n1, n2);

  //If both of the above calls return Non-NULL, then one key is present in one      
  //subtree and the other is present in the other, so this node must be the LCA
  if (left_lca && right_lca) {
    return root;
  }
  //Otherwise both lie in the same subtree. Check if the left subtree or right        
  //subtree is LCA
  return (left_lca != NULL) ? left_lca : right_lca; 
}
```

---

### Constructor Destructor Practice

*Contributed by Marek Pokorny.*

Without using a compiler, determine the output of the following program.
```cpp
#include <iostream>
using namespace std;

class A {
public:
  A() {
    cout << "A() ";
  }
  ~A() {
    cout << "~A() ";
  }
};

class B : public A {
public:
  B() {
    cout << "B() ";
  }
  ~B() {
    cout << "~B() ";
  }
};

class C : public B {
public:
  C() {
    cout << "C() ";
  }
  ~C() {
    cout << "~C() ";
  }
  B b;
  A a;
};

int main() {
  C c;
}
```

#### Solution
`A() B() A() B() A() C() ~C() ~A() ~B() ~A() ~B() ~A()`

When constructing a class, the constructor for that class’s base class will be called first. So, when we construct `C`, we first call the constructor for `B`, which in turn first calls the constructor for `A`, therefore `A() B()` will be printed initially. Next, before `C` is constructed, its members are constructed in order of declaration. Therefore, `A() B()` will be printed for `b`, followed by `A()` for `a`. Finally, `C`’s constructor is called, which prints `C()`.

Destructors are called in the reverse order of the constructors. We first call `C`’s destructor, printing `~C()`. We then call the destructors for each of the members in reverse order of their declaration, printing `~A() ~B() ~A()`. We then call the destructor for `C`’s base class `B`, which is run and then calls the destructor for its base class `A`, resulting in `~B() ~A()`.

This solution is also available through [codepad](https://code.hackerearth.com/da7b92b).

---

### Doubly-Linked List Code Fixing

*Contributed by Megan Williams.*

Given below is a partial implementation of a doubly-linked list. Each list has a pointer to the head node. Each node in the list has a pointer to the next node, a pointer to the previous node, and a value. The `LinkedList` class has a function `erase_node` that will delete the first node in the list that has a value of `val`, and return `true` if such a node is successfully deleted. There are at least three runtime bugs in the implementation of `erase_node`. Find them and fix them. 

```cpp
struct Node {
  int value;
  Node* next;
  Node* prev;
};

class LinkedList {
  public:
    bool erase_node(int val);
    ...
  private:
    Node* m_head;
};

bool LinkedList::erase_node(int val) {
  //return false if the list is empty
  if (m_head == nullptr)
    return false;
    
  //find the node with value equal to val
  Node* p;
  for (p = m_head; p != nullptr; p = p->next) {
    if (p->value == val)
      break;
  }
    
  //remove p
  if (p == m_head) {
    //check special case where we remove the head node
    m_head = p->next;
    m_head->prev = nullptr;
  }
  else {
    p->prev->next = p->next;
    p->next->prev = p->prev;
  }   
  delete p;
  return true;
}
```
#### Solution

The code above never checks the case in which a node with a value of val isn’t found in the list. If there isn’t a node with value equal to `val` then after the execution of the for-loop, p will be set to a null pointer. When we try to access `p->prev`, we will have invalid pointer access. We can fix this simply by adding an if-statement to check if `p` is null after the for-loop.
Another issue is created by the situation where the list only has one node. We set `m_head` to `p->next` and then access `m_head->prev`. But `p->next` might be null if there is only one node. We fix this with an if-statement to check if it is null.
The last bug is that this code does not properly handle the situation where the node we are removing is the last node in the list. In this case, the program will crash when we try to access `p->next->prev`. We can fix this by adding in an extra else if-statement to check if `p->next` is null.

This solution is also available through [codepad](https://code.hackerearth.com/fbd077M).

Corrected Code:
```cpp
bool LinkedList::erase_node(int val) {
  //return false if the list is empty
  if (m_head == nullptr)
    return false;
    
  //find the node with value equal to val
  Node* p;
  for (p = m_head; p != nullptr; p = p->next) {
    if (p->value == val)
      break;
  }
  if (p == nullptr) {
    //no node with value of “val” is found
    return false;
  }
  //remove p
  if (p == m_head) {
    //check special case where we remove the head node
    m_head = p->next;
    if (m_head != nullptr)
      m_head->prev = nullptr;
  }
  else if (p->next == nullptr) {
    //removing last node
    p->prev->next = nullptr;
  }
  else {
    p->prev->next = p->next;
    p->next->prev = p->prev;
  }   
  delete p;
  return true;
}
```

---

### Queue from Stacks

*Contributed by Michael Wang.*

Implement a queue using one or more stack(s). The queue should have at least `push()`, `pop()`, and `front()`.
#### Solution
In a queue, the order of elements is FIFO (first-in first-out). In a stack, the order of elements is FILO (first-in last-out). If we use two stacks, we “reverse” the reversed order and obtain the elements in order again. Call one of the stacks `in`, and the other stack `out`.

`push()`: push element into `in`

`pop()`: if there are no elements in `out`, pop all elements from `in` and push them into `out`; pop the `out` stack

`front()`: if there are no elements in `out`, pop all elements from `in` and push them into `out`; return the top of `out`


This solution is also available through [codepad](https://code.hackerearth.com/d9e30fz).

```cpp
template <class T>
class MyQueue {
private:
  stack<T> in;
  stack<T> out;
public:
  void pop() {
    if (out.empty()) {
      while (!in.empty()) {
        T& t = in.top();
        in.pop();
        out.push(t);
      }
    }
    out.pop();
  }
  void push(const T& val) {
    in.push(val);
  }
  T& front() const {
    if (out.empty()) {
      while (!in.empty()) {
        T& t = in.top();
        in.pop();
        out.push(t);
      }
    }
    return out.top();
  }
};
```

---

### Binary Tree Swap Rightmost

*Contributed by Michael Zhang.*

Given a binary tree, implement swap_rightmost that swaps the rightmost node with the head node. The function declaration and Node struct is listed below.
```cpp
struct Node {
    int value;
    Node* right;
    Node* left;
};

swap_rightmost(Node* head)
```
#### Solution
The basic idea behind the approach to the solution goes like this: we’ll continuously follow the right pointer for each node until we reach the end of the tree. Then we’ll swap the pointer values for each node so that the last pointer is the new head, and head is the new last node. In order to do this, we’ll need to include a node pointer that keeps track of the previous node visited, that way once we reach the last node, we can point the previous node’s right to the new head.

This solution is also available through [codepad](https://code.hackerearth.com/99f706e).

```cpp
#include <iostream>
using namespace std;

void swap_rightmost (Node* head)  {
  if (head == NULL || head -> right == NULL)
    return;
  Node* current = head;
  Node* previous = NULL;
  while (current -> right != NULL) {
    previous = current;
    current = current -> right;
  }
  int prevValue = current -> value;
  Node* prevLeft = current -> left;
    
  current -> right = head -> right;
  current -> left = head -> left;
  current -> value = head -> value;
    
  previous -> right = head;
  head -> value  = prevValue;
  head -> left = prevLeft;
  head = current;
}
```

---

### Array Permutations

*Contributed by Patrick Lai.*

Implement the function `permutation` that takes an array and prints out all possible permutations of the elements in the array recursively. You may use as many helper functions as you need. Assume that the size of the array is given.
#### Solution
We can start by creating a helper function that generates all permutations of an array from element `a` to element `b`. For example, say we wanted to generate permutations of the array `[0, 1, 2]` from element 0 to 2 (i.e. all elements). To accomplish this, we can fix the 0th element, 0, in place and call the function to generate permutations of the last two elements (element 1 to 2). In other words, we want to find all orderings `[0, _, _]`. Similarly, we can now fix the 1st element , 1, and call the function to generate permutations of the last element. When `a == b`, or the last element is reached, we recognize that this indicates the completion of one possible ordering and the function prints the array `[0, 1, 2]`.
Having reached the end of the array, we can then move back a position, swap the elements at positions 1 and 2, and repeat the process. This prints out the array [0, 2, 1] and gives us the final ordering of the form `[0, _, _]`. We can then move back another position and swap the element at position 0 with each of the following elements to generate all permutations of the forms [1, _, _] and `[2, _, _]` in similar fashion.
This process of repeated substitution and size reduction allows all permutations of the original array to be printed recursively.

This solution is also available through [codepad](https://code.hackerearth.com/2ddf67i).

```cpp
void swap_elements(int* v, int i1, int i2) {
  int temp = v[i1];
  v[i1] = v[i2];
  v[i2] = temp;
}

void permutation_helper(int* v, int size, int a, int b) {
  if (a == b) {
    for (int i = 0; i < size; i++) {
      printf("%d ", v[i]);
    }
    printf("\n");
   }
   //otherwise, for each element i after a,
   //swap a with i and find all permutations of remaining elements
   else{
     for (int i = a; i <= b; i++) {
       //swap a and i
       swap_elements(v, i, a);
       //find all permutations with elements before 'a' fixed
       permutation_helper(v, size, a + 1, b);
       //swap back
       swap_elements(v, i, a);
     }
  }
}

void permutation(int* v, int size) {
  permutation_helper(v, size, 0, size - 1);
}
```

---

### Catalan Numbers

*Contributed by Ray Zhang.*

Implement the function `catalan` that takes an `int n` and returns the nth Catalan number.
A Catalan number is in the form of `C_n = sum from i=0 to n-1: C_i*C_(n-1-i)`.

#### Solution
The catalan number is a classic example of recurrence relations with more than one variable. We just have to loop through all possible catalan numbers which is the sum from `i=0` to `n-1` of `catalan(i)*catalan(n-i-1)`.

This solution is also available through [codepad](https://code.hackerearth.com/a565c7C).

```cpp
int catalan(int n) {
  if (n == 0 || n == 1) return 1;
  
  int res = 0;
  for (int i = 0; i < n; i++)
    res += catalan(i) * catalan(n - i - 1);
 
  return res;
}
```

---

### Sorted Linked List Insertion

*Contributed by Rishi Bhargava.*

Given a Sorted Linked List, write a function where you can insert a new Node into the Linked List while keeping it sorted.

#### Solution
First check if the Linked List is empty or if the first entry in the list has a value greater than the node you are inserting. If either is true, the new node is inserted at the top and is the new head of the Linked List. 
Otherwise you traverse the Linked List till you find the node that is greater than or equal to the to-be-inserted node or reach the end. Then you insert the node by changing around the pointers of the current and next nodes.

This solution is also available through [codepad](https://code.hackerearth.com/c900a8L).

```cpp
#include <iostream>
#include <stdio.h>
#include <stdlib.h>

struct Node {
  int data;
  struct Node* next;
};

// Uses special case code for the head end
void sortedInsert(struct Node** head, struct Node* newNode) {
  // Special case for the head end
  if (*head == NULL || (*head)->data >= newNode->data) {
    newNode->next = *head;
    *head = newNode;
  } else {
  // Locate the node before the point of insertion
    struct Node* current = *head;
    while (current->next != NULL && current->next->data < newNode->data) {
      current = current->next;
    }   
    newNode->next = current->next;
    current->next = newNode;
  }
}
```

---

### Binary Tree Reverse

*Contributed by Russell Jew.*

Reverse a binary tree by swapping the left and right children of each parent. For example,

![example](http://i.imgur.com/7wMgDHJ.png)
#### Solution
We can solve this problem using recursion. If you look at the example, you can see that we need to swap each node’s left and right children if they exist. Recall that recursion is used to solve a large problem by the combining solutions to its sub-problems. In order to use recursion we need a simplifying step and a base case. 

If you look at the code below, the first step that is done is checking the base case. The base case checks to see if the root node is equal to `nullptr`. If it is, then it immediately returns. This covers all cases. It covers the case when the root node is nullptr and it also works when the root node has no children or one child since swapping two children that are `nullptr` works. The next step is completing the swap of the left and right children. Then the simplifying step is recursively calling `reverse_binary_tree` on the left and right children.

This solution is also available through [codepad](https://code.hackerearth.com/75c545Q).

```cpp
void reverse_binary_tree(Node* root) {
  if (root == nullptr)
    return;

  Node* temp = root->left;
  root->left = root->right;
  root->right = temp;

  reverse_binary_tree(root->left);
  reverse_binary_tree(root->right);
}
```

---

### Sorting C-Strings

*Contributed by Srikanth Ashtalakshmi V Rajendran.*

Write a function that takes in an array of C strings and sorts it in ascending order. You may use any sorting algorithm to solve this problem. Before you start writing this function, create a helper function to compare two C strings. You may use the strcpy library function to copy one C string into another (but you must write your own strcmp function!) The interface of the functions are provided for you. The C strings in the array are all null terminated. 

```cpp
bool str_cmp(char* string1, char* string2); // return true if string1 < string2

void sort_array(char array[][100], int size);
```
#### Solution
The `str_cmp` function compares both strings lexicographically. The first letter in the string which is different from the corresponding letter in the second string is used to decide if `string1` is smaller `string2`. If one of the strings is a prefix of the other, then the `str_cmp` function returns true only if `string1` is smaller.

The `sort_array` function uses a selection sort algorithm. The str_cmp function is used to compare two C strings in the array. If a C string in a larger index of the array is smaller than (<) a C string in a smaller index of the array, these two C strings are swapped. The C strings are swapped using the `strcpy` library function and a temporary C string.

This solution is also available through [codepad](https://code.hackerearth.com/87c5bdU).

```cpp
bool str_cmp(char* string1, char* string2) {
  int i;
  for (i = 0; string1[i] != 0 && string2[i] != 0; i++) {
    if (string1[i] == string2[i])
      continue;
    if (string1[i] < string2[i])
      return true;
    else
      return false;
  }
  
  //if string1 is shorter then string1 < string2
  if (string1[i] == 0)
    return true; 
  else
    return false;
}


void sort_array(char array[][100], int size) {
  for (int i = 0; i < size; i++) {
      for (int j = i + 1; j < size; j++) {
          if (str_cmp(array[j], array[i])) {
              //swapping c strings in index i and j
              char temp[100];
              strcpy(temp, array[i]);
              strcpy(array[i], array[j]);
              strcpy(array[j], temp);
          }
      }
  }
}
```

---

### Max in a K-ary Tree

*Contributed by Srikanth Ashtalakshmi V Rajendran.*

A K-ary tree is a tree data structure in which each node has at most K children nodes. Write a recursive function that takes in the root node and the value of K as parameters and returns the maximum value in the K-ary tree. The struct defining a node is shown below.
```cpp
struct Node {
  int value;
  vector<Node*> child_ptrs;
};
```

You may assume that the `child_ptrs` vector will consist of K pointers, starting from the 0th index. If a child doesn’t exist, the pointer will have a null value. Additionally, you may also assume that all data values in the tree are non-negative. The interface of the function you will write is given below.
```cpp
int max_value(Node* root, int K);
```
#### Solution
Use recursion to obtain the maximum value from each child pointer’s sub tree. This is done by passing the child pointer as the root node to the `max_value` function. Then compare these max values to a temporary maximum value, and update the temporary maximum when necessary. We also need to check if the root is null, to terminate the recursion at leaf nodes.

This solution is also available through [codepad](https://code.hackerearth.com/754859y).

```cpp
int max_value(Node* root, int K) {
  if (root == NULL)
    return -1;
  int max = -1;  
  if (root->value > max)
    max = root->value;    
  for (int i = 0; i < K; i++) {
    int temp_max = max_value(root->child_ptrs[i], K);
    if (temp_max > max)
      max = temp_max;
  }  
  return max;
}
```

---

### Level Order Tree Traversal

*Contributed by Thomas Garcia.*

Implement the function `levelOrder` which takes a pointer to the root of a binary tree and prints out the tree in level order. Tree nodes are represented by the struct:
```cpp
struct Tree {
  int val;
  Tree *left;
  Tree *right;
};
```
![levelorder](https://upload.wikimedia.org/wikipedia/commons/d/d1/Sorted_binary_tree_breadth-first_traversal.svg)

Level Order: `F, B, G, A, D, I, C, E, H`
#### Solution
The easiest way to solve this problem is using a queue. Because a queue is a First In, First Out data structure, we know that all the elements in one row will be printed before the elements in the next row. You have to remember to push the left child before the right child, otherwise the row will be printed out in reverse.

This solution is also available through [codepad](https://code.hackerearth.com/aecebfS).

```cpp
void levelOrder(Tree *root) {
  queue<Tree *> nodes;
  nodes.push(root);
  while(!nodes.empty()) {
    Tree *ptr = nodes.front();
    nodes.pop();
    if(ptr == nullptr) {
      continue;
    }
    cout << ptr->val << endl;
    nodes.push(ptr->left);
    nodes.push(ptr->right);
  }
}
```

---

### LinuxBerg

*Contributed by Vincent Jin.*

A file path in LinuxBerg has the following format:

**/directory1/directory2/directory3/moredirectories/filename.ext**

For example, the following is a valid LinuxBerg file path:

**etc/bin/usr/nachenberg/cs32/final_answers.txt**

There are also several special characters:

* The dot (./) represents the current directory.
 * For example, **/etc/bin/./usr/answers.txt** translates to **etc/bin/usr/answers.txt**
 * Here, the ./ does not move into a different directory folder.

* The double dot (../) represents the previous directory.
 * For example, **/etc/bin/../file_in_etc.txt** represents **etc/file_in_etc.txt**
 * Here, the ../ moves out of the etc/bin/ folder and back into the etc/ folder.

Given a file path in LinuxBerg, with ./ and ../ characters, output the shortest possible file path that represents the same file.

For example:

**/directory1/a.txt** becomes **/directory1/a.txt**

**/directory1/./a.txt** becomes **/directory1/a.txt**

**/directory1/directory2/../directory2/a.txt** becomes **/directory1/directory2/a.txt**

**/directory1/./directory2/../directory3/b.txt** becomes **/directory1/directory3/b.txt**

**/./././././././file.txt** becomes **/file.txt**

**/../** is defined to be **/** - the root directory. That is, if there are more double dots than actual directories, return the root directory.

Skeleton code is provided below.
```cpp
#include <vector>
#include <string>
using namespace std;
vector<string> shortestFilePath(vector<string> longFilePath) {
/*	  longFilePath is a vector of strings. Each string either contains a folder
  name, a filename, a dot (.) or a double dot (..) - you must return a vector
  that contains strings, all folder names, with the last string being the
  filename. All dots and double dots should be processed and removed.      
For example, the vector {"abc", "def", ".", "..", "g.txt"} represents the file path /abc/def/./../g.txt which should be shortened to /abc/g.txt via your program.										*/

  vector<string> answer;
  //your code here.

  return answer;
}
```
#### Solution
We can use a stack to store our current directory path. By traversing the original vector, we add directory paths to our stack. When we encounter a dot, we simply ignore it. When we encounter a double dot, we simply pop off our stack. At the end, we return our stack in backwards format.

This solution is also available through [codepad](https://code.hackerearth.com/6c4609n).

```cpp
vector<string> shortestFilePath(vector<string> longFilePath){
  vector<string> answer;
  stack<string> filePath;
  for (vector<string>::iterator it = longFilePath.begin(); 
                              it != longFilePath.end(); it++) {
    if (*it == ".." && !filePath.empty()) {
      filePath.pop();
    } else if (*it != ".") {
      filePath.push(*it);
    }
  }
	
  while (!filePath.empty()){
    answer.emplace(answer.begin(), filePath.top());
    filePath.pop();
  }
  return answer;
}
```
---