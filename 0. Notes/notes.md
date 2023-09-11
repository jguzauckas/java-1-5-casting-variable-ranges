# Casting and Ranges of Variables

We discussed in previous sections about how values of the type `int` and `double` can interact automatically in java. We had rules about how whenever a `double` is involved in a mathemtical operation, the output value must be a `double`. This is how Java will sometimes automate the process of **casting** values for us.

---

## Casting

Casting a value tells Java to reinterpret a value from one type as another. This is most often done with our two main types of numbers: `int` and `double`. Let's look at this sample from `NotesCasting1.java` to see where this is applicable:

```java
System.out.println(1 + 2.0);
```

This is a situation where Java automatically will cast value data types to make sure it can do the math. Despite it seeming so, Java cannot actually do math between an `int` and a `double`. It can, however, convert the `int` to a double and then do the math between two `double` values to produce an output. So when Java sees `1 + 2.0` it actually reinterprets it as `1.0 + 2.0`, which it then knows is `3.0`. This process of temporarily turning a value into a different type is called **casting**.

While Java will do it automatically in these cases, there are sometimes where we want to be able to cast our values manually. In this course we will only do this for types `int` and `double`. To cast a value that was already a `double` into an `int`, we use the casting operator `(int)` before the value we want to be casted. This sample from `JavaCasting2.java` shows the same problem from before with this casting involved:

```java
System.out.println(1 + (int) 2.0);
```

If we run this file we'll notice the output is now just an integer `3`, since `(int) 2.0` casts the value to be `2` so the computer does `1 + 2` instead of the `1.0 + 2.0` we saw earlier. It is important to note that Java converts a `double` to an `int` using **truncation**. Truncating a `double` means to just remove the decimal point, no rounding, just delete it. So while we might sometimes feel like `(int) 2.9` should convert it to `3`, it actually just converts it to `2`.

While much less common due to its dominance over `int` types in operations, we can cast a type to `double` by using `(double)` before the value we want to be casted. This sample from `JavaCasting2.java` shows the same problem from before with this casting involved:

```java
System.out.println((double) 1 + 2.0);
```

This casting will turn `(double) 1` into `1.0`, which is identical to what we saw should happen in `NotesCasting1.java` because this is essentially what Java did in the background to do the operation with `double` values. Java converts an `int` to a `double` by adding `.0` to the end of the number, which we know is equivalent to the whole number anyways, so this type of conversion doesn't lose any information.

---

## Rounding Numbers

While **truncating** a `double` may lose us some information, if we are smart about it, we can force this truncation to do the rounding for us. Thinking back to my example of `2.9`, if we just add a little bit to the value before truncating, it will be at least `3.0` before we truncate and will become `3`, which makes more sense for us. So how much do we add?

The first thing we have to consider is what is the smallest decimal that should round up. Normally this would be the half, so in our example, `2.5`. We need to add enough to make sure that `2.5` becomes at least `3.0` but not so much that `2.49` would hit `3.0`, lest it round up as well when it shouldn't. It seems obvious then, that the correct answer would be to add `0.5` to our number. Therefore then, this sample from `NotesRounding1.java` shows us the standard way to round a `double` to an `int`:

```java
double decimalNumber = 2.6;;
System.out.println((int) (decimalNumber + 0.5));
decimalNumber = 2.4;
System.out.println((int) (decimalNumber + 0.5));
```

Note here that by putting `decimalNumber + 0.5` in parentheses, we force it to add them together first before casting to an `int`. If we hadn't put the parentheses, it would have cast `2.6` to `2` and then added `0.5` to make `2.5`, which is the opposite of what we wanted. With these parentheses, `2.6 + 0.5` becomes `3.1` and then casting to an `int` truncates the value to `3`, which is successfully rounded up. `2.4`, on the other hand, still gets added, so we get `2.9`, which is then truncated to `2`, successfully rounding down.

There is a problem with this strategy, however, which is negative numbers. Let's say we wanted to round `-2.6`, which should round (technically down) to `3`. `-2.6 + 0.5` becomes `-2.1`, which then would truncate to `2`. So how do we fix this? We reverse the operation, so instead of adding `0.5`, we subtract it. `-2.6 - 0.5` becomes `-3.1` which successfully rounds down by truncating to `3`. Therefore then, this sample from `NotesRounding2.java` shows us the standard way to round a negative `double` to an `int`:

```java
double decimalNumber = -2.6;
System.out.println((int) (decimalNumber - 0.5));
decimalNumber = -2.4;
System.out.println((int) (decimalNumber - 0.5));
```

Here, again the parentheses around `decimalNumber` save us by forcing the subtraction before truncating. So unfortunately, there is not one strategy to round with casting, but two for when you have either positive or negative numbers. How do we know whether we are doing this with positives or negatives? We don't always know, but we often will know the context of a variable. For example, a variable storing age would never be negative, so we know our positive rounding strategy would always be valid.

---

## Minimum and Maximum Integer Values

In our section on data types, we mentioned the limitations of numbers we can store with the `int` data type. We know we can store a value no smaller than `-2,147,483,648` and no larger than `2,147,483,647`. While we don't need them often, these values can be difficult to remember exactly, and so we have a couple of constants saved in Java we can always call on to get them in the form of `Integer.MIN_VALUE` and `Integer.MAX_VALUE`. This sample from `NotesMinMax1.java` shows these two constants at work:

```java
int largestNumber = Integer.MAX_VALUE;
System.out.println(largestNumber);
int smallestNumber = Integer.MIN_VALUE;
System.out.println(smallestNumber);
```

Advanced Note: `Integer.MAX_VALUE` and `Integer.MIN_VALUE` show two important concepts at work that will become relevant later this year. `Integer` is the **wrapper class** that describes all of the important behavior for our `int` data type, and `MAX_VALUE` and `MIN_VALUE` are class constants from the `Integer` class. We can tell they are constants based on the style of naming. The all-capital-letters and underscores-between-words conventions are used for constants in Java. These values will never change. The period between `Integer` and `MAX_VALUE` or `MIN_VALUE` shows the same idea as `System.out.println`, which is that the dot operator tells it to look inside the `Integer` class for `MAX_VALUE` or `MIN_VALUE` to use.

---

## Integer Overflow

Interestingly, Java will not throw an error when a value too large (or too small) is saved to an `int` type, instead it **overflows** or **underflows**. The way to think about this goes way back to our traditional number line, where at the left end we'd have our `Integer.MIN_VALUE` of `-2,147,483,648` and at the right end we'd have our `Integer.MAX_VALUE` of `-2,147,483,647`. Java doesn't view this as a line though, it views it as a circle, where the next number after the maximum value of `2,147,483,647` is our minimum value of `-2,147,483,648`. We typically refer to this behavior more simply as **wrapping**, where once it hits one end of the number line, it *wraps around* to the other end of the line.

When we go above our maximum value of `2,147,483,647` and it wraps around to the positive side, we refer to this as **overflow**, whereas when we go below our minimum value of `-2,147,483,648` and it wraps around to the positive side, we refer to this as **underflow**. We can see both of these occur in this sample from `JavaOverUnderFlow1.java`:

```java
int largestNumber = Integer.MAX_VALUE;
System.out.println(largestNumber + 1);
int smallestNumber = Integer.MIN_VALUE;
System.out.println(smallestNumber - 1);
```

This produces the following output:

```
-2147483648
2147483647
```

Taking our maximum value and adding 1 results in overflow, wrapping it around to the negative end, and taking our minimum value and subtracting 1 results in underflow, wrapping it around to the positive end.

While it's great that this doesn't cause any errors, and instead wraps around, this is always something to keep in mind as being the source of odd behavior in code that is doing arithmetic on large `int` values.

---

## Final Notes

Taking our very first example from `NotesCasting1.java`, the whole file looks like this:

```java
public class NotesCasting1 {
    public static void main (String[] args){
        System.out.println(1 + 2.0);
    }
}
```

We only honed in on the middle line with our variables initially, but every piece of this file is important to the program running and doing what we want. Taking a look at our second example from `NotesCasting2.java`, that whole file looks like this:

```java
public class NotesCasting2 {
    public static void main (String[] args){
        System.out.println(1 + (int) 2.0);
    }
}
```

Comparing the first two lines of code from both files should feel extremely similar, with only a slight variation. All of our Java files for the foreseeable future will have this structure. The first line is `public class NameOfFileHere{`, where you'll notice that in both examples, it utilizes the name of the file without the `.java` extension. The second line is identical in both cases as `public static void main (String[] args){`.

Throughout the year we will end up unpacking these lines and better understand how they work to better use them to our advantage. For the time being though, we will let Visual Studio Code autofill those lines when we create a Java file and understand that it's on the third line where we put our code using things like our variable statements.

---

## Assignment

Now that you have gone through the notes for this section, you can check out the `Try.md` and `Try.java` files to try a short assignment using this material.
