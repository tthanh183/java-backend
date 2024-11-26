### Array

# Arrays in Java

An array is a data structure that allows you to store multiple values of the same type in a single variable.
1. **Declaring an Array**

To declare an array in Java, you define the type of the elements followed by square brackets `[]`:
```
// Declare an array of integers
int[] numbers;

// Declare an array of strings
String[] fruits;
```
2. **Creating an Array**

After declaring an array, you need to allocate memory for the array using the new keyword:
```
// Creating an array of 5 integers
numbers = new int[5];

// Creating an array of 3 strings
fruits = new String[3];
```
You can also initialize the array at the time of creation:
```
// Initialize an array with values
int[] numbers = {1, 2, 3, 4, 5};

// Initialize an array of strings
String[] fruits = {"Apple", "Banana", "Cherry"};
```
3. **Accessing Array Elements**

Array elements are accessed using an index, starting from 0 for the first element:
```
// Accessing the first element of the numbers array
System.out.println(numbers[0]);  // Output: 1

// Accessing the second element of the fruits array
System.out.println(fruits[1]);  // Output: Banana
```
4. **Modifying Array Elements**

You can modify an element of the array by accessing it via its index:
```
// Modifying the value of the first element
numbers[0] = 10;
System.out.println(numbers[0]);  // Output: 10
```
5. **Looping through Arrays**

You can loop through arrays using different types of loops, such as for, for-each, or while loops.
```
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}
```
```
for (int number : numbers) {
    System.out.println(number);
}
```
6. **Multidimensional Arrays**

Java also supports multidimensional arrays. A two-dimensional array can be thought of as an array of arrays.
- Declaring a 2D array:
```
int[][] matrix = new int[3][3];  // 3x3 matrix
```
- Initializing a 2D array:
```
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```
- Accessing elements in a 2D array:
```
System.out.println(matrix[0][0]);  // Output: 1
System.out.println(matrix[2][2]);  // Output: 9
```
7. **Array Length**
```
int length = numbers.length;
System.out.println(length);  // Output: 5
```
8. **Array Methods**

- Sorting an array:
```
import java.util.Arrays;

int[] numbers = {5, 2, 8, 1, 3};
Arrays.sort(numbers);
System.out.println(Arrays.toString(numbers));  // Output: [1, 2, 3, 5, 8]
```
- Copying an array:
```
int[] copy = Arrays.copyOf(numbers, numbers.length);
System.out.println(Arrays.toString(copy));  // Output: [1, 2, 3, 5, 8]
```
- Searching for an element:
```
int index = Arrays.binarySearch(numbers, 3);
System.out.println(index);  // Output: 2
```