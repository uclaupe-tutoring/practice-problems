### Word reversal

*Contributed by Abdulaziz Siddiqi*.

Given a string that contains words separated by single space, reverse the words in the string. You can assume no trailing spaces or leading spaces.

##### Example

For the input `Main Bites dog`, we would print `dog Bites Main`.

#### Solution

This solution is also available through [codepad](https://code.hackerearth.com/52a5a8P).

```cpp
string reverseWords(string value) {
  string newString = "";
  string tempWord = "";
  for (int i = 0; i < value.length(); i++) {
    if (value[i] != ' ')
         tempWord += value[i];
    else {
      if (newString.length() != 0)
        newString = tempWord + " " + newString;
      else
        newString = tempWord;
      tempWord = "";
    }
  }
  if (tempWord != "")
    newString = tempWord + " " + newString;
  return newString;
}
```

---

### Palindromes

*Contributed by Abineet Das Sharma*.

Check if a string can be reordered to make a palindrome, ignore spaces, case and punctuation. You do not need to re-order, simply check whether it is possible.

#### Solution

We use the fact that in a palindrome, there can be at most one character that occurs just once. Other than that, all characters need to occur an even number of times. We use a fixed length boolean array, where each index maps to a certain character in the alphabet. We initialize these to false and flip it every time we encounter the corresponding character in the string. In the end we can just count the number of `true` indices left, and if this number is less than or equal to one, we can convert the string into a palindrome.

This solution is also available through [codepad](https://code.hackerearth.com/75b8fdH).

```cpp
bool isPalindromable(string s) {
  bool Alpha[26];
  int count_true = 0;

  for (int i = 0; i < 26; i++) {
    Alpha[i] = false;
  }

  for (int i = 0; i < s.length(); i++) {
    if (isalpha(s[i])) {
      s[i] = tolower(s[i]);
      if (Alpha[s[i] - 'a'] == false) {
        Alpha[s[i] - 'a'] = true;
      }
      else {
        Alpha[s[i] - 'a'] = false;
      }
    }
  }

  for (int i = 0; i < 26; i++) {
    if (Alpha[i] == true) {
      count_true++;
    }
  }

  if (count_true <= 1)
    return true;
  else
    return false;
}
```

---

### Code tracing 1

*Contributed by Aditya Raju*.

What does the following code output?

```cpp
int main() {
  int x = 10;
  while(x > 0) {
    x = x - 3;
    for (int i = 1; i <= x; i++) {
      if (x % i == 0)
        cout << "DOG" << endl;
      else if (x % 2 == 0)
        cout << "CAT" << endl;
      else
        cout << "SMALLBERG" << endl;
    }
  }
  return;
}
```

#### Solution

This solution is also available through [codepad](https://code.hackerearth.com/47c0e8T).

```
DOG
SMALLBERG
SMALLBERG
SMALLBERG
SMALLBERG
SMALLBERG
DOG
DOG
DOG
CAT
DOG
DOG
```

---

### Code tracing 2

*Contributed by Anderson Huang*.

What does the following code output?

```cpp
int foo(int *a, int &b) {
  if (*a > b)
    b = 2 * (*a) + 1;
  else
    *a = b + 1;
  return (*a * b);
}

int bar(int a, int *b) {
  if (a > *b)
    a = *b - 1;
  else
    *b = 2 * a - 1;
  return (a / *b);
}

int main() {
  int a = 3, b = 2;
  int c = foo(&a, b);
  cout << a << " " << b << endl;
  int d = foo(&b, a);
  cout << c << " " << d << endl;
  cout << bar(c, &d) << endl;
  cout << a << " " << b << endl;
  cout << c << " " << d << endl;
}
```

#### Solution

It’s easy to make simple mistakes, so pay careful attention to whether the argument is being passed by value, passed by reference, or as a pointer. With this in mind, notice that function `foo` passes its first argument as a pointer, and its second as a reference. Function bar passed its first argument by value, and its second as a pointer. In total, there are three function calls. Start off by setting aside space for the output; then, evaluate each function call.

##### First call to foo:

At this point, `a = 3`, `b = 2`. The variable `c` is assigned to the return value of `foo(&a, b)`. If any changes occur to `a` or `b`, they will be seen in the `main` function. In the function `foo`, the value pointed to by `a` is compared with the value of `b`. Because 3 is greater than 2, the statement following the conditional if is executed, and variable `b` is set to `2 * 3 + 1`, or 7. Finally, the return statement executes `3 * 7`, giving the variable `c` a value of 21.

##### First output:

```
3 7
```

##### Second call to `foo`:

At this point, `a = 3`, `b = 7`. However, the arguments are called in reverse order. Again, changes to any variables will be seen in the main function. The value being pointed to by the first argument (7 in this case), is greater than 3. As a result, `a` is set to `2*(7) + 1 = 15`. Take care, because the naming convention is confusing. The return value is `(7 * 15)`, which is 105. At this point, `a = 15`, `b = 7`.

##### Second output:

```
21 105
```

##### Call to `bar`:

`bar` is called with the variables `c` and `d`. The return value is not assigned to any variable, but is outputted. The first argument is passed by value, and the second is passed as a pointer. Only modifications to the second value will be seen in the `main` function. The value pointed to by the second argument, `d`, is greater than the value of `c`. The statement following the else statement is executed, and the value being pointed to by `d` is set to `(2 * 21) - 1`, which is 41. The return value is `21 / 105`, which reduces to 0, by the rules of integer division. To summarize, `c = 21`, `d = 41`.

##### Third output:

```
0
```

At this point, no more functions are called.

##### Fourth and fifth output:

```
15 7
21 41
```

---

### Maximizing with pointers

*Contributed by Andrew Lee.*

Given an array of `int`s, return a pointer to the largest `int`. If the array is empty (according to the `size` variable), return `nullptr`. If the maximum appears in more than one place, return the pointer whose index is the lowest.

##### Function declaration

```cpp
int *getMax(int *arr, int size);
```

#### Solution

This solution is also available through [codepad](https://code.hackerearth.com/e574a2G).

```cpp
int *getMax(int *arr, int size) {
  if (arr == nullptr)
    return nullptr;

  int *max = &arr[0];
  for (int i = 1; i < size; i++) {
    if (arr[i] > *max)
      max = &arr[i];
  }

  return max;
}
```

---

### Right rotation

*Contributed by Bradley Zhu.*

Given an array, the size of said array, and an integer `k` greater than zero, rotate the array right `k` times.  For example, if the array is `[0, 1, 2, 3, 4, 5]` and `k = 2`, then return `[4, 5, 0, 1, 2, 3]`, or if `k = 4`, return `[2, 3, 4, 5, 0, 1]`.


#### Solution

An easy way to solve this is to first store the last `k` values into a temporary array to ensure we don’t lose their values.  After doing so, we can loop starting from the end and directly modify the values in the array to be what they are supposed to be (the value stored in an index `k` less that its own index).  At the end, we can copy the saved values into the beginning of the array.

This solution is also available through [codepad](https://code.hackerearth.com/8b0d30Z).

```cpp
void rotate_right (int arr[], int size, int k) {
  k = k % size;
  int *temp_arr = new int[k];
  for (int i = size - k; i < size; i++) {
    temp_arr[i - (size - k)] = arr[i];
  }

  for (int i = size - 1; i >= k; i--) {
    arr[i] = arr[i - k];
  }

  for (int i = 0; i < k; i++) {
    arr[i] = temp_arr[i];
  }

  delete[] temp_arr;
}
```

---

### Code tracing 3

*Contributed by Cameron Ito.*

What does the following code output?

```cpp
int main() {
  for (int i = 1; i <= 10; i++) {
    for (int j = 1; j <= 10; j++) {
      if (j > 10 - i) {
        cout << "*";
      } else {
        cout << " ";
      }
    }
    cout << endl;
  }
  return 0;
}
```

#### Solution

The output can be described as a staircase made up of asterisks. The staircase has ten steps.

This solution is also available through [codepad](https://code.hackerearth.com/552836y).

```
*
**
***
****
*****
******
*******
********
*********
**********
```

---

### Left shifting

*Contributed by Danny Nguyen.*

Implement a function `shift_left` that takes in three parameters: a pointer to an array, the position in consideration, and a number of interesting elements to look at. Rotate all elements leftward one position beginning at `a[i]`, making sure to wrap the leftmost element to the appropriate position at the end of the array. You may assume that `pos < n`.

For example, if my array is originally `3 4 5 6 7`, and the function parameters are `pos = 1` and `n = 5`, my array will become the following: `3 5 6 7 4`.

#### Solution

You first have to store the original value at `arr[pos]` because we will begin overwriting all values within the array beginning at index pos and ending at index `n - 2` by simply moving values from the rightmost cell over. Lastly, remember to restore the original value at `arr[pos]` to the end of the array, `arr[n - 1]`.

This solution is also available through [codepad](https://code.hackerearth.com/9d7cb1S).

```cpp
int shift_left(int *arr, int pos, int n) {
  int originalValue = arr[pos];

  for (int i = pos; i < n - 1; i++) {
      arr[i] = arr[i + 1];
  }

  arr[n - 1] = originalValue;
  return pos;
}
```

---

### Code tracing 4

*Contributed by Eugene Liu.*

1. Assuming that the program does not print anything else, what is output to the console when `print_stuff()` is called?
2. What is the new output if `k++` is changed to `*(i + 1) = i[1] + 1` and the `break` under `case 14:` is removed?

```cpp
void print_stuff() {
  int i[2] = { 14, 13 };
  int k = *i;

  if (k % 2 == 0)
    k++;
  else
    k--;

  switch (*(i + 1)) {
  case 13:
    cout << *i << " stuff";
  case 12:
    cout << "stuff\n";
    break;
  case 14:
    cout << *i << endl;
    break;
  default:
    cout << "case 12";
  }
  cout << k;
}
```

#### Solution

##### Output 1

```
14 stuffstuff
15
```

The variable `i` is an array initialized with two values, 14 and 13. `k` is a copy of the first element in `i`. The if-else statement checks that `k` is even, so it is incremented to 15. The switch statement looks at the value `i[1]`, which is still 13 because `k` is only a copy of `i[0]` that gets changed, so the program jumps to case 13. It prints out the value `i[0]` (since an array variable is really just a pointer to the first value in the array), followed by `" stuff"`. Since there is no `break` after that, it continues executing the code under `case 12:` and then reaches the `break` after that. It then prints out the value of `k` after exiting the `switch` statement, and the function returns.

##### Output 2

```
14
case 1214
```

The value of `i[1]` gets changed to 14 in this scenario, so the `switch` statement jumps to the `case 14:` code instead. Without the `break` afterwards, the program continues down to the `default:` case, which prints `"case 12"`. The value of `k`, which stays at 14, is then printed at the end.

This solution is also available through [codepad](https://code.hackerearth.com/f56a84M).

---

### Pointer sum

*Contributed by Greg Giebel.*

Write a function that sums the items of an array of `n` `int`s using only pointers to traverse the array.

#### Solution

Simple array traversal, but with pointers. To get to item `i`, add `i` to your head pointer then dereference. This works because the compiler knows the size of an item in your array in C++. Sum values as you go by adding them to a `total` value declared outside of the for loop.

This solution is also available through [codepad](https://code.hackerearth.com/2cd6bdH).

```cpp
int sum(int *head, int n) {
  int total = 0;
  for (int i = 0; i < n; i++) {
    int temp = *(head + i);
    total += temp;
  }
  return total;
}
```

---

### Counting vowels

*Contributed by Hsien-Chih Hung.*

Write a function `count_vowel` that takes a string as input and returns the number of vowels in the string. Given the following code, use a nested loop to complete the `count_vowel` function.

```cpp
int count_vowel(string text) {
  int count = 0;
  char vowel[10] = {'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'};

  // Your code here ...

  return count;
}
```

#### Solution

We can use a for-loop to check if the current character of the text string is a vowel in the `vowel` array.

This solution is also available through [codepad](https://code.hackerearth.com/b4612ep).

```cpp
int count_vowel (string text) {
  int count = 0;
  char vowel[10] = {'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'};
  for (int i = 0; i != text.size(); i++) {
    for (int j = 0; j != 10; j++) {
      if (text[i] == vowel[j])
        count ++;
    }
  }
  return count;
}
```

---

### Code tracing 5

*Contributed by Isaac Yeo En Jie.*

What is the output of the following function?

```cpp
void ptr_function(int *ptr, int &a) {
  int *ptr2 = &a;
  *ptr2 = 50;
  ptr = ptr2;
  A = 1000;
}

int main() {
  int *ptr = new int;
  *ptr = 10;
  int a = 25;
  ptr_function(ptr, a);
  cout << "Pointer value is: " << *ptr << endl << "a's value is: " << a << endl;
}
```

#### Solution

The important thing here is to note that `ptr` is passed by value and not by reference.

This solution is also available through [codepad](https://code.hackerearth.com/159707f).

```
Pointer value is: 10
a's value is: 1000
```

---

### Something with pointers, structs, and arrays

*Contributed by Iza Say.*

Given an array of the following struct, write a function that calculates and prints the average test grade, along with the name and grade of the person with the highest test grade. If the function receives an empty array, you should just return after printing an error message. You may assume any necessary libraries are already included.

Your output should follow the following format (number of decimal places doesn’t matter):

```
Average Test Grade: 99.0090
Top Student: Yu Narukami
<name of top student>’s Test Grade: 100.0
```

```cpp
// With the typedef, you don’t need to use the struct keyword when declaring.
typedef struct aStudent {
  string name;
  double test_grade;
} Student;

void print_average_and_highest(const Student arr[], const size_t sz) {
  // Your code here!
}
```

#### Solution

This solution is also available through [codepad](https://code.hackerearth.com/1bbcb5U).

```cpp
void print_average_and_highest(const Student arr[], const size_t sz) {
  if (sz <= 0 || arr == nullptr) {
    cout << "Error: empty array.\n";
    return; // Exit if invalid input
  }

  // Variables and pointers to keep track of what we want.
  const Student* topStudent = &arr[0];
  double sum, average;
  double size = sz; // Here, we have to do this in order
                    // to avoid integer division later on.

  for (unsigned int i = 0; i < sz; i++) {
    // For each Student, add their grade to sum.
    sum += arr[i].test_grade;

    // Check if this Student has a higher grade than previous Student.
    if (arr[i].test_grade > topStudent->test_grade) {
      topStudent = &arr[i];
    }
  }

  // Calculate average.
  average = sum / size;

  cout << "Average Test Grade: " << average << "\n";
  cout << "Top Student: " << topStudent->name << "\n"; //or endl;
  cout << topStudent->name << "’s Test Grade: "
       << topStudent->test_grade << "\n";
}
```

---

### Even odds

*Contributed by Jerin Tomy.*

Suppose you have an array `a` of 10 integers, which have all been initialized with positive numbers. 

Create a new array `b` that is double the size of `a`, and initialize all of its integers to 0. Then, copy the elements of `a` over to `b` such that the values are placed in every even index of `b`. (e.g. first element in `a` copied to first element in `b`, second element in a copied to third element in `b`, third element in `a` copied to fifth element of `b`, etc.)

#### Solution

Note that the problem stated that every even index of b would be filled. This would mean that `a[0]` would be copied to `b[0]`, `a[1]` would be copied to `b[2]`, `a[2]` would be copied to `b[4]`. You can begin to see the relationship here: the value in `a[i]` gets copied to `b[2 * i]`.

This solution is also available through [codepad](http://codepad.org/8IZkEMdE).

```cpp
int b[20];
for (int i = 0; i < 20; i++) {
  b[i] = 0;
}

for (int i = 0; i < 10; i++) {
  b[2 * i] = a[i];
}
```

---

### An elaborate triangle

*Contributed by Jerry Li.*

Write the function `createPattern()` that takes in a positive integer `n`, and displays the following pattern to standard output, continued for `n` rows:

```
**
**$*
**$**$
**$**$**
**$**$**$*
**$**$**$**$
...
```

The first row always contains `**`, and every subsequent row after that adds two additional characters. Every third character in each row is a `$` character. 

You are not allowed to use strings to solve this problem.

#### Solution

This problem tests your knowledge of nested for loops, conditional statements, and the modulus `%` operator. 

We first loop through each of the n rows to generate them. For each line, we loop from `0` to `2 * (row + 1)`, as the index starts at `0` and each new row is 2 characters longer than the previous one. This outer loop will generate newlines between each row of the pattern. In this nested loop, where we print the contents of each row of the pattern, we check if the number is divisible by 3 using the modulus `%` operator, and print out a `$` if it is, `*` if it isn’t. One must be careful to notice that the `$` characters should be printed when the current character index of the row is `2` (mod 3) instead of `0` (mod 3), if you started your indexing at `0`.

This solution is also available through [codepad](https://code.hackerearth.com/72cc00u).

```cpp
void createPattern(int n) {
  for(int row = 0; row < n; row++) {
    for(int j = 0; j < 2 * (row + 1); j++) {
      if(j % 3 == 2)
        cout << '$';
      else
        cout << '*';
    }
    cout << endl;
  }
}
```

---

### Game development

*Contributed by Joanne Park.*

You are writing code for a game. A user inputs three numbers between 1-5, which you store into an array. All error checking has been done so that you can assume that the array input is of correct format. A different action will occur depending on the input array. What will be the value of the variable HP at the end of this code? Assume all appropriate headers are given.

```cpp
int main() {
  int HP = 100;
  int input[3] = {1, 3, 4};
  for (int i = 0; i < 3; i++) {
    switch (input[i]) {
      case 1:
      case 2:
        printf("Welcome!\n");
        HP *= 2;
        break;
      case 3:
        printf("Obtained a poisonous mushroom\n");
        HP /= 2;
        break;
      case 4:
        printf("Encountered a troll! HP -5\n");
        HP -= 5;
      case 5:
        printf("Got a potion! HP +5\n");
        HP += 5;
        break;
      default:
        printf("Invalid move\n");
    }
  }
}
```

#### Solution

The HP will still be 100. Input 1 will be double `HP` and input 3 will halve it. Notice that for input 4, the case statement does NOT end with a `break` and will also go into case 5. So an input of 4 will -5 to `HP` and then +5 to `HP`.

This solution is also available through [codepad](https://code.hackerearth.com/d6f800a).

---

### Filtering

*Contributed by Jonathan Chu.*

Write a function that removes all characters `c` from a c-string and returns the number of characters removed.

##### Example

For the input `"She sells sea shells by the sea shore"` and character `'e'`, we would expect the output `"Sh slls sa shlls by th sa shor`, with 7 characters being removed.

##### Function declaration

```cpp
int remove_char(char *string, char c)`
```

#### Solution

For this solution, we will iterate through each character of the string, and if the current character is the selected character `c`, then we will shift all successive characters down one byte, including the terminating nullbyte.

This solution is also available through [codepad](https://code.hackerearth.com/6f9633R).

```cpp
int remove_char(char *string, char c) {
  int count = 0;
  int i = 0;
  while (string[i]) {
    if (c == string[i]) {
      int j = i;
      while(string[j]) {
        string[j] = string[j + 1];
        j++;
      }
      count++;
    } else {
      i++;
    }
  }
  return count;
}
```

---

### Code tracing 6

*Contributed by Jorge Fuentes.*

Will the following code compile? If not, explain why. If it does, what does it output?

```cpp
int main() {
  int a[] = {1, 2, 3, 4, 5};
  int *b;
  int *c;
  b = &*(a + 1);
  c = b - 1;
  c[0] = 6;
  b += 2;
  c = b + 1;
  cout << *a << *b << *c;
}
```

#### Solution

We can trace through the code snippet like so:

```cpp
int main() {
  int a[] = {1, 2, 3, 4, 5};
  int *b;
  int *c, d;      // c is a int ptr, d is an int
  b = &*(a + 1);  // 1. b points to a[1]
  c = b - 1;      // 2. c points to a[0]
  *b = *c;        // 3. a[1] = a[0](1)
  d = *c;         // 4. d = a[0](1)
  c[0] = 6;       // 5. a[0] = 6 (notice d doesn’t change)
  b += 2;         //6. b points to  a[1+2] or a[3]
  cout << *a << *b << *c << d;
}
```

In which case, the output is `6461`.

This solution is also available through [codepad](https://code.hackerearth.com/cd322fn).

---

### Tongue twisters

*Contributed by Joshua Sambasivam.*

Let's define a tongue twister as a sentence in which more than half of the words begin with the same letter as the first letter of the first word. Write a function that returns a boolean value for whether a function is a tongue twister.

Assume that strings passed in begin with a word and each word is separated by one space character `' '`.

#### Solution

The very restrictive definition of a tongue twister given allows us to approach the problem through the lens of the first letter of the first word. Since we need to compare the number of words that have this letter as their first to the total number of words, we need to keep running counts of both and perform the comparison at the end. Due to the fact that words are guaranteed to be separated by a single whitespace character, we may use those to aid us in differentiating words from one another.

This solution is also available through [codepad](https://code.hackerearth.com/8563c0T).

```cpp
bool is_tongue_twister(string s) {
  char first_letter = tolower(s[0]);
  int num_words = 1;
  int num_match = 1;
  for(int i = 1; i < s.size(); i++) {
    if (s[i] == ' ') {
      num_words++;
        if (i < s.size() - 1 && tolower(s[i + 1]) == first_letter)
          num_match++;
    }
  }
  
  return ((double)(num_match) >= 0.5 * (double)(num_words));
}
```

---

### The flattening

*Contributed by Keshav Tadimeti.*

Write a function `two2One` that converts a two-dimensional array of integers into a one-dimensional array of integers. You are given the 2-D array and the dimensions of that 2-D array. 

The function prototype for `two2One` is as follows:

```cpp
void two2One(int [][20] twoD, int[] oneD, int numRows, int numCols); 
```

- `twoD`: the two-dimensional array to convert to the one-dimensional array
- `oneD`: the one-dimensional array we want to populate with the values of `twoD` 
- `numRows`: the number of ‘rows’ in the two-dimensional array
- `numCols`: the number of ‘columns’ in the two-dimensional array

You can assume that `oneD` will have sufficient enough space to fit all the elements of `twoD`. You can also assume that `numRows` and `numCols` are in fact the total number of rows and columns in `twoD`, and are thus non-negative. Finally, you can also assume that `numCols` is not greater than 20.

#### Solution

What really helps in solving this problem is drawing a picture. Being able to visualize how the 2-D array can be stored in one dimension can be tricky at first, but some quick sketches will show that it ultimately boils down to some simple math.

Say we have a 2-D array of dimensions 2 x 2, so 2 elements in the ‘row’ and 2 elements in the ‘column’. This would look something like this:

| 0 | 1
--- | --- | ---
0 | 27 | 33
1 | -15 | 1223

If we want to convert this 2-D array into a 1-D array, we can basically cut the 2-D array above in half and connect the ends, forming the array:

 0 | 1 | 2 | 3
 --- | --- | --- | ---
 27 | 33 | -15 | 1223
 
Another way of creating the 1-D array would be like this:

0 | 1 | 2 | 3
 --- | --- | --- | ---
 27 | -15 | 33 | 1223

Though there are two solutions, the underlying math is still the same:
 
1. `indexNumber = (rowNumber * numberOfRows) + columnNumber`
2. `indexNumber = (columnNumber * numberOfColumns) + rowNumber`

Hence, the two possible implementations of `two2One` are:

```cpp
// Using the 'row' implementation
void two2One_row(int twoD[][20], int oneD[], int numRows, int numCols) {
  for (int rowNum = 0; rowNum < numRows; rowNum++) {
    for (int colNum = 0; colNum < numCols; colNum++) {
      int index = (rowNum * numRows) + colNum;
      oneD[index] = twoD[rowNum][colNum];
    }
   }
 }

// Or, using the 'column' implementation ...
void two2One_col(int twoD[][20], int oneD[], int numRows, int numCols) {
  for (int colNum = 0; colNum < numCols; colNum++) {
    for (int rowNum = 0; rowNum < numRows; rowNum++) {
      int index = (colNum * numCols) + rowNum;
      oneD[index] = twoD[rowNum][colNum];
    }
  }
}
```

This solution is also available through [codepad](https://code.hackerearth.com/dd2d43J).

---

### Banking on it

*Contributed by Mahir Eusufzai.*

Implement a class `BankAccount`. This class will have the following member variables and functions:

- `int m_current_balance`, private member variable that represents the amount currently present in the bank account. 
- `BankAccount(int initial_deposit)`, a constructor that takes one parameter, `initial_deposit`, and assigns it to the `m_current_balance` variable.
- `withdraw(int request)`, a public function that takes in one parameter, `request`, and updates the `m_current_balance`. Returns the updated amount in the bank account.
- `deposit(int request)`, a public function that takes in one parameter, `request`, and updates the `m_current_balance`. Returns the updated amount in the bank account.

#### Solution

This solution is also available through [codepad](https://code.hackerearth.com/0ecdd3Z).

```cpp
class BankAccount {
private: 
  int m_current_balance;
public:
  BankAccount(int initial_deposit) {
    m_current_balance = initial_deposit;
  }
  int withdraw(int request) {
    m_current_balance -= request;
    return m_current_balance;
  }
  int deposit(int request) {
    m_current_balance += request;
    return m_current_balance;
  }
};
```

---

### It's a palindrome, but is it the longest one?

*Contributed by Megan Williams.*

##### Part A

Write a function `is_palindrome` that takes a string as an argument and returns `true` if the string is a palindrome and `false` if it is not. A palindrome is a string that is read backwards the same way as it is forwards. For example `"racecar"` backwards is `"racecar"`.

##### Part B

Using the function `is_palindrome`, write a function `longest_palindrome` that takes a string as an argument and returns the longest palindrome substring. For example, if the input is `"abadcabba"`, the function should return `"abba"`.

#### Solution

##### Part A

Start by checking that the first and the last letters in the string are equal, then move inward until we reach the middle of the string. Note: This only requires `n / 2` iterations of the for-loop. Also, this code works even when `n` is odd. Trace through an example by hand to see why!

```cpp
bool is_palindrome(string s) {
  int n = s.size();
  for (int i = 0; i < n / 2; i++) {
    int j = (n - 1) - i;
    if (s[i] != s[j])
      return false;
    }
  return true;
}
```

##### Part B

Use a nested for-loop to iterate over each possible substring of `s`. To keep track of the longest substring palindrome, we record the starting index `pal_index` and the length `pal_length` of each palindrome we encounter. In the code below, `i` and `j` represent the index of the start and index of the end of the substring in `s` we are considering. To get all the possible substrings, we vary the start index `i` from 0 to the end of string, and for each `i` we vary the end index `j` from `i` to the end of the string. For each substring, we check if it is a palindrome using `is_palindrome(...)`. If it is, we see if it is the longest palindrome encountered so far, and update `pal_length` and `pal_index` accordingly. To extract a substring with start index `i` and end index `j` from `s`, we use the function `substr(int start, int length)`.

```cpp
string longest_palindrome(string s) {
  int pal_length = 0;
  int pal_index = 0;
  int n = s.size();

  // Iterate over all substrings
  for (int i = 0; i < n; i++) {
    for (int j = i; j < n; j++) {
      // Set tmp equal to the substring in s starting 
      // at index i and ending at index j.
      string tmp = s.substr(i, j - i + 1);
      if (is_palindrome(tmp)) {
        // Tmp is a palindrome, check if it is the longest.
        if(tmp.size() > pal_length) {
          pal_length = tmp.size();
          pal_index = i;
        }
      }
    }
  }
  return s.substr(pal_index, pal_length);
}
```

This solution is also available through [codepad](https://code.hackerearth.com/874e28Z).

---

### Code tracing 7

*Contributed by Michael Zhang.*

What does the following code snippet output?

```cpp
int main() {
  int arr[3] = {1, 4, 5};
  for (int i = 0; i < 3; i++) {
    for (int j = 3; j > 1; j--) {
      cout << arr[i] * (j - i);
    }
  }
}
```

#### Solution

The purpose of this exercise is to test your code tracing abilities. For each element in the array (which is what the outer loop controls), the console will print the value of that element multiplied by 3 minus the position of that element in the array, as well as the value of that element multiplied by 2 minus the position of that element in the array.

This solution is also available through [codepad](https://code.hackerearth.com/e0fdd8C).

```cpp
3
2
8
4
5
0
```

---

### Code tracing 8

*Contributed by Nathan Zhang.*

What does the following code snippet output?

```cpp
int a(int b) {
  for(int x = 0; x < b; x+= 2) {
    if(x + 3 > b)
      cout << b++;
    else
      cout << x;
  }
  return b;
}

int main(int argc, const char * argv[]) {
  cout << a(8) + 1 << endl;
}
```

#### Solution

We expect the output to be `0248911`. Below are the values of `x`, `b`, and the number printed as we iterate through the loop:

| `x` | `b` | prints
--- | --- | --- | ---
1 | 0 | 8 | 0
2 | 2 | 8 | 2
3 | 4 | 8 | 4
4 | 6 | 8 | 8
5 | 8 | 9 | 9

Notice that the fourth iteration is the first time the `if` statement is `true` since `6 + 3 > 8`. Moreover, 8 is printed because `b` is incremented *after* the print.

Then, `x = b = 10` so the loop ends and we return 10. The `main` function will print `10 + 1 = 11`. Therefore, we have `0248911` as our final output.

This solution is also available through [codepad](https://code.hackerearth.com/2ec5d8B).

---

### `strcpy`

*Contributed by Ray Zhang.*

Implement the function `strcpy` that takes a c-string `str1` and character array `str2` and fills up `str2` with the contents of `str1`. Assume that `str2` is sufficiently large to contain all the contents of `str1`.

#### Solution

The gist of this solution is to copy each character from `str1` until we reach the nullbyte.

This solution is also available through [codepad](https://code.hackerearth.com/a0df20X).

```cpp
void strcpy(char *str1, char *str2) {
  int ctr = 0;
  while (true) {
    str2[ctr] = str1[ctr];
      if (str1[ctr] == 0)
        Break;
      ctr++;
  }
}
```

---

### Capitalization

*Contributed by Richard Sun.*

Write a function `are_words_capitalized` that takes a string and returns `true` if the string is correctly capitalized, and `false` if it is not. A string is correctly capitalized if the first letter of each word is capitalized and the rest of the letters are lowercase. For example, `are_words_capitalized("I Like Cats")` should return `true`. The only characters in the string are letters and spaces.

#### Solution

We know that the first character in each word must be uppercase. A character is the first letter in a word if it is the first character in the string or if it comes right after a space. All characters that are not the first in a word must be lowercase. If either of these conditions is violated, we return false. If we make it to the end of the for loop, then the string is correctly capitalized, so we return true.

This solution is also available through [codepad](https://code.hackerearth.com/f9554eN).

```cpp
bool are_words_capitalized(string s) {
  bool first_letter = true;

  for (size_t i = 0; i < s.size(); i++) {
    if (isspace(s[i])) {
      first_letter = true;
    } else if (first_letter) {
      first_letter = false;
      if (!isupper(s[i])) {
        return false;
      }
    } else if (!islower(s[i])) {
      return false;
    }
  }
  return true;
}
```

---

### Matching characters

*Contributed by Rishi Bhargava*.

Given two C-strings, count the number of letters that match in both without any regard of the case of the letters.

#### Solution

C-strings can be iterated through using the square bracket operator `[]` and a for loop. We know that c-strings end with a null character so we check that first. We then check if both c-strings have a letter present using the `isalpha(char c)` function. We can then check for equality without case by using the `isupper(char c)` function on that character for both c-strings. Increment `count` if equal. Return `count`.

This solution is also available through [codepad](https://code.hackerearth.com/60926bm).

```cpp
int matchingLetters (const char *str1, const char *str2) {
  int count = 0;
  // C-strings end will a null byte so this keeps counting till one c-string ends.
  while (*str1 != '\0' && *str2 != '\0') {
  // isalpha returns whether the character is alphabetical.
  // toupper returns corresponding uppercase characters.
    if (isalpha(*str1) && isalpha(*str2)) {
      if (toupper(*str1) == toupper(*str2))
        count++;
    }
    // Iterates through both c-strings
    str1++;
    str2++;
  }
    
  // Returns total number of matches
  return count;
}
```

---

### Sorted similarity

*Contributed by Russell Jew.*

Given two arrays sorted in increasing order, determine whether the two arrays contain an element that is the same.

##### Examples

```
Array 1: 1 4 5 9 20
Array 2: 2 3 6 7 8 9 10
Returns true (Both contain 9)

Array 1: 1 4 5 9 20
Array 2: 2 3 6 7 8 10
Returns false (no common elements)
```

#### Solution

We can start by maintaining two counters, one at the first element in `array1` and the second one at the first element in `array2`. Since both arrays are sorted in increasing order, we can compare the two elements that the counters are at. 

If the elements the counters are pointing to are equal, then we are done and return `true`. If the element at `array1` is less than the element at `array2`, then we increment the counter of `array1` (since the element is smaller, we increment it to get a larger number that might be equal to the element at `array2`). If the element at `array1` is greater than the element at `array2`, then we increment the counter of `array2` (same reasoning reversed). 

We continue this process until we either get an element that is equal (in this case, we return `true`) or until we reach the end of an array (in this case, we return `false` as there are no more elements in that array which can equal the other one).

This solution is also available through [codepad](https://code.hackerearth.com/563f72G).

```cpp
bool contains_same_element(int a1[], int size1, int a2[], int size2) {
  int i = 0; //counter for a1
  int j = 0; //counter for a2
	
  while (i < size1 && j < size2) {
    if (a1[i] == a2[j])
      return true;
    else if (a1[i] < a2[j])
      i++;
    else		//a1[i] > a2[j]
      j++;
  }
  return false;
}
```

---

### String reversal

*Contributed by Thomas Garcia.*

Implement the function `printReverse` which takes a c-string as input and prints out the string in reverse. If the string is empty, you should just print out a newline. When implementing this function, you have a few restrictions: you may only use one local variable, which must be a char pointer, and you may not use any `*`, except for when declaring your one local variable.

#### Solution

Because we cannot declare any int variables, we have to traverse the string by incrementing the pointer itself. We also cannot use `*` to dereference the pointer, but that is okay because `s[0]` is the same thing as `*s`. The trick to this function is to find the end of the string (the null byte), and then increment backwards while printing the characters, until you reach the start of the string.

This solution is also available through [codepad](https://code.hackerearth.com/e5fcecq).

```cpp
void printReverse(const char s[]) {
  const char *end = s;
  while(end[0] != '\0') {
    end++;
  }
  end--;
  while(end >= s) {
    cout << end[0];
    end--;
  }
  cout << endl;
}
```

---

### Subset

*Contributed by Vincent Siu.*

Write a function called subset which, when given an array and its length, will print out all the [proper subsets](http://mathworld.wolfram.com/ProperSubset.html) of the array. Note that proper subsets are subsets that do not include the original set. Assume that the array has no duplicates. For additional practice, write a function that handles arrays with duplicates.

##### Example

Given an array `[3, 1, 2]`, we would expect the following output:

```
""
"3"
"1"
"2"
"31"
"32"
"12"
```

#### Solution

You want to be careful with array indexing for this problem.

First, we want to consider the case of an empty set. Then, we will construct three for loops. The outer for loop iterates through all the sizes of subsets (which run from 1 to `length - 1`). The middle for loop iterates through all the possible subsets of that chosen length. The inner for loop prints out the characters inside the current subset.

In order to handle the case where the array has duplicates, the solution is to create a function called `remove_all_duplicates` which removes all the duplicates from the array. Then, we can call our original subset function on this array which has all duplicates removed.

This solution is also available through [codepad](https://code.hackerearth.com/78d3c9c).

```cpp
void subset(int input[], int length) {
  cout << "\"\"" << endl;
  for (int i = 1; i < length; i++) {
    for (int j = 0; j <= length - i; j++) {
      cout << "\"";
      for (int k = j; k < j + i; k++) {
        cout << input[k];
      }
      cout << "\"" << endl;
    }
  }
}
```

---

### Printing backwards

Write a function `printBackwards` that takes as input an array of c-strings and the size of this c-string array. The function then prints each c-string backwards, with a newline in between each reversed c-string.

#### Solution

This solution is also available through [codepad](https://code.hackerearth.com/b88f55A).

```cpp
void print_cstring(char *str) {
  for (int i = strlen(str) - 1; i >= 0; i--) {
    cout << str[i];
  }
  cout << endl;
}
void print_backwards(char *ary[], int len) {
  for (int i = 0; i<len; i++) {
    print_cstring(ary[i]);
  }
}
```
