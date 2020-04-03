# AppProg
Notes for Applications Programming

# Study 3

## Procedural Programming

A stepping stone to Object-Oriented (OO) programming...

### Break down a program into procedures

* A small program has one goal = one method
* A large program has many sub-goals = many methods.

### Show a diamond: main procedure

**Specification:** Read in a size. Show a diamond of that size.

```java
public static void main(String[] args) {
    System.out.print("Size: ");
    int size = In.nextInt();
    showDiamond(size);
}
```

Output:

```
Size: 4
   *
  * *
 * * *
* * * *
 * * *
  * *
   *
```
Judging from the output, it seems like `showDiamond()` would be a complex method, right?

Well, it isn't:

```java
public static void showDiamond(int size) {
    showTop(size);
    showMiddle(size);
    showBotton(size);
}
```

This method takes one parameter, which breaks down different sections of the diamond and assigns that output to different methods.

### Show a diamond: showTop procedure

```java
public static void showTop(int size) {
    for (int length = 1; length < size; length++)
        showLine(length, size);
}
```

e.g. Input: `size = 4`

```
  *     length = 1
 * *    length = 2
* * *   length = 3
```

As it turns out, even showing a single line as a complicated process, so that is abstracted away into another method: `showLine`.

### Show a diamond: showLine procedure

```java
public static void showLine(int howManyStars, int size) {
    int howManySpace = size - howManyStars;
    repeat(howManySpaces, " ");
    repeat(howManyStars, "* ");
}
```

e.g. Input: `size = 4`

```
  *     howManyStars = 1    howManySpaces = 3
 * *    howManyStars = 2    howManySpaces = 2
* * *   howManyStars = 3    howManySpaces = 1
```

As you may have noticed, there is *another* layer of abstraction beneath this, in the method `repeatLine`.

### Show a diamond: repeat procedure

```java
public static void repeat(int howMany, String what) {
    for (int i = 0; i < howMany; i++)
        System.out.print(what);
}
```

And this is the lowest level of abstraction.

### Show a diamond: showMiddle and showBottom

And now for the remaining methods. Implementing them is quite easy as we have already created methods which perform similar tasks to what we need.

```java
public static void showMiddle(int size) {
    showLine(size, size);
}
```
The `showMiddle` method uses the `showLine` method.

```java
public static void showBottom(int size) {
    for (int length = size - 1; length >= 1; length--)
        showLine(length, size);
}
```

The `showBottom` method peforms a similar task to `showTop`, hence we can use `showLine` in the same way.

## Key/framework approach

How to find a solution when you don't have a pattern

### Goal, Plan and Key

1. **Goal**: what your program should achieve
2. **Plan**: a series of steps to achieve the goal
3. **Key**: the key line of code that achieves the goal

#### Example:

**Goal**: Show the total rainfull for a period.

Start with the key.

The "key" line of code

```java
total += rainfull[i];
```

After this is established, **build the solution around the key**.

```java
double[] rainfall = { 97.0, 112.0, ...
int total = 0;
for (int i = 0; i < rainfall.length; i++) {
    total += rainfall[i];
}

System.out.println("Total = " + total);
```

The code that supports the key is called the **framework**.

The framework for a solution can be different and may vary according to the data source.

* From an array
* From user input
* From a file...

But the **key** remains the same.

For example one solution can be:

```
<array loop> {
    total += rainfall;
}
```

And another solution can be

```
<read loop> {
    total += rainfall;
}
```

## Incremental goals

How to takle a large problem!

* Sometimes the goal is too difficult
* Start with a simplified goal and devise a plan for that
* Gradually add more, until the complete goal is achieved

Simplify your goal in *increments* until it gets bigger and bigger and eventually you have solved the whole problem.

1. Simplified Goal
2. Intermediate Goal
3. Complete Goal

e.g.

### Specification

Read integers less than 100 until the user enters -1. Show the frequency of integers in each group: 0-9, 10-19, 20-29, 30-39, ..., 90-99.

#### Input:
```
Number: 17
Number: 19
Number: 45
Number: 87
Number: 49
Number: -1
```

#### Output:
```
00's: 0
10's: 2
20's: 0
30's: 0
40's: 2
50's: 0
60's: 0
70's: 0
80's: 1
90's: 0
```

### Incremental goal #1

**Goal**: Show the frequency of integers in **one group**: 0-9

Patterns/key code:
* read loop
* output
* count
* if (value < 10)

#### Solution:

```java
int count = 0;

System.out.print("Integer: ");
int value = In.nextInt();
while (value != -1) {
    if (value < 10) 
        count++;
    System.out.print("Integer: ");
    value = In.nextInt();
}
System.out.println("00's: " + count);
```

### Incremental goal #2

**Goal**: Show the frequency of integers in **two groups**: 0-9 and 10-19.

Patterns/key code:
* read loop
* output
* count 1
* count 1
* if (value < 10)
* else if (value < 20)

#### Solution:

```java
int count0 = 0, count1 = 0;

System.out.print("Integer: ");
int value = In.nextInt();
while (value != -1) {
    if (value < 10) 
        count0++;
    else if (value < 20)
        count1++;
    System.out.print("Integer: ");
    value = In.nextInt();
}
System.out.println("00's: " + count0);
System.out.println("10's: " + count1);
```

### Incremental goal #3: Complete

**Goal**: show the frequency of integers in **many groups**: 0-9, 10-19, ... 90-99.

* Design question: how can we store the count for many groups?
    * Solution: an array.
* How big is the array?
    * 10 groups. Positions start from zero. [0]-[9]
* Read integer N. How do we decide which group to count?
    * e.g. 95 goes into [9]. 47 goes into [4]. 31 goes into [3].
    * **Divide by 10**: 95 / 10 is 9, 47 / 10 is 4, 31 / 10 is 3 (using integer divsion)
* **What is the "key" code?**
    * `count[value/10]++;`

#### Complete Solution

```java
int[] count = new int[10];

System.out.print("Integer: ");
int value = In.nextInt();
while (value != -1) {
    count[value / 10]++;
    System.out.print("Integer: ");
    value = In.nextInt();
}
for (int i = 0; i < count.length; i++) {
    System.out.println(i + "0's: " + count[i]);
}
```

## Extracting digits from a number

The following can be done with integer division and the modulus (%) operator

* The last digit of a number is: number % 10
    * e.g. What is the last digit of 92873
    * 92873 % 10 -> 3
* number / 10 will remove a digit from the right
    * 92872 / 10 -> 9287
* To obtain the first digit on an N-digit number, divide by 10 N-1 times.
    * e.g. What is the first digit of 92873?
    * 92873 / 10 / 10 /10 / 10 = 9 (in other words, 92873 / 10000 = 9)
* To obtain a digit in the middle, combine / 10 and % 10:
    * e.g. What is the middle digit of 92873?
    * Divide by 10 two times: 92873 -> 9287 -> 928.
    * Now the digit we want is the last digit.
    * 928 % 10 = 8.

### The modulo operator: %

* Numbers wrap around a "modulus". e.g. 12 hour time is modulo 12.
* (11 o'clock + 2 hours) modulo 12 = 1 o'clock
    * (11 + 2) % 12 = 1

## Testing

### How to test

* Don't test every possible input
* Devise a finite set of test cases taht are representative of all scenarios

e.g. Your program stores values into this array


| <mark>[0]| </mark> [1] | [2] | [3] | <mark>[4]</mark> | [5] | [6] | [7] | [8] | <mark>[9]</mark> |
|---| ---|---|---|---|---|---|---|---|---|

1. Middle case: Test storing into position [4]
    * No need to also test [5] and [6]... etc. One middle value is representative
2. Edge case: Test storing into position [0]
3. Edge case: Test storing into position [9]

### Off-by-one error

A common programming mistake is to be "off by one"

#### Loops from [0] to [9]

```java
for (int i = 0; i < 10; i++)
```

#### Loops from [1] to [10]

```java
for (int i = 1; i <= 10; i++)
```

* The left loop works
* The right loops gives ArrayIndexOutOfBoundException: 10
* This error is picked up by testing edge cases

It is more likely for something to go wrong in edge cases than in middle cases.

**NOTE:** This subject doesn't require testing for invalid inputs unless explicitly stated.

## Debugging

Add breakpoints to your code to pause your program once it reaches that line.

You can then step through each step of the program and then see how to local variables change and see if everything is running like how it's supposed to.

# Study 4

## Methods

### Functions vs Procedures

There are two different kinds of methods:

* A procedure does something. It's name is a verb
* A function does something. It's name is a noun

#### Procedures

* A procedure is a method that does an action/has some "effect.
    * e.g. prints a value, changes a value
* A procedure may take parameters, but should return nothing
* The name of a procedure is a verb describing the goal

```java
public static void showCircleArea(double radius) {
    double area = Math.PI * radius * radius;
    System.out.println("The area of the circle is " + area);
}
```

* A procedure may use local variables. A local variable is temporary. It is deleted when the method exits

#### Functions

* A function is a method that returns a value
* A funciton should not have any side effects
    * It should not print a value or change a value
* A function may take parameters
* The name of a function is a noun describin what it is returning

```java
public static double circleArea(double radius) {
    double area = Math.PI * radius * radius;
    return area;
}
```

* A function may also use local variables

#### Side effects

* Function design rule
    * A function returns a value and changes nothing
* If a function changes something, this is called a "side effect"
* Side effects are bad:
    * the reader assumes the function changes nothing
    * the read does not look inside the funciton
    * because a function changes nothing
* Avoid programming by side effect
    * Unless it is a known pattern

#### Interaction between procedures and functions

* A procedure can call a function
* A function can call a function
* But a function should not call a procedure

Functions should not have side effects
 
Calling a procedure may introduce side effects

### String functions and patterns

In Java, String is a class, providing a set of useful functions

| Functions | Description |
| ---- | --- |
| `int length()` | returns the length of the string |
| `char charAt(int i)` | returns the character at position i |
| `String[] split(String operator)` | returns an array of substring split by the separator |

e.g.

```java
String s = "hello world";
System.out.println(s.length());     // prints 11
System.out.println(s.charAt(1));    // prints 'e'
String[] words = s.split(" ");      // returns {"hello", "world"}
```

*To see patterns, go to the patterns book*

#### Looping over words in a string using "split"

**Program:**

```java
String sentence = "Eat your vegatables";
for (String word : sentence.split(" "))
    System.println("Next word = " + word);
```

**Output:**

```
Next word = Eat
Next word = your
Next word = vegatables
```

#### Split by one or more spaces

* If you have a string with extra spaces between words:
    * <pre>String sentence = "Eat          your         vegetables"</pre>
* Use the regular expression <pre>" +"</pre> as the separator

```java
for (String word : sentence.split(" +"))
    System.out.println("Next word = " + word);
```

**Output:**

```
Next word = Eat
Next word = your
Next word = vegatables
```

### Functional patterns

*Go to the patterns book*

### Boolean function

* Bad: `if (matches == true)`

* Good: `if (matches)`
    * There is no need to comparae a boolean to true or false
    * A boolean *is* true or false
<br>
<br>
* A boolean function returns a boolean value:
    ```java
    boolean isDry(int rain) {
        return rain == 0;
    ```

* The name of a boolean is an adjectival phrase
    * e.g. `boolean dry(int rain)`, `boolean isDry(int rain)`, `boolean hasDry()`

### Break it down, build it up

#### Break a program into functions

**Specification:** Read in a sentence. Show the number of words that contains a lowercase vowel.

**Remember:** Each goal goes in a separate method.

| Level | Goal | Pattern | Form |
| --- | --- | --- | --- |
| *Sentence level* | How many matching words in this sentence? | *count* | *param:* String sentence, *result:* int
| *Word level* | Are there any lowercase vowels in this word? | *any* | *param:* String word, *result:* boolean
| *Character level* | Is this character any of these: a/e/i/o/u? | *any* | *param:* char c, *result:* boolean

#### Implementing the solution

When implementing the solution, start at the lowest level first. In this case, the lowest level would be  *Character level* as this is the lowest level that doesn't depend on anything, then work your way up.


