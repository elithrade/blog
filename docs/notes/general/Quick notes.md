# Quick notes

Ideas or definitions that are heard but not sure what they mean.

## Imperative vs declarative
* With imperative programming, you tell the compiler what you want to happen, step by step.
    ```csharp
    List<int> collection = new List<int> { 1, 2, 3, 4, 5 };
    List<int> results = new List<int>();
    foreach(var num in collection)
    {
        if (num % 2 != 0)
            results.Add(num);
    }
    ```
* With declarative, you write code that describes what you want but not necessarily how to get it (declare your desired result, but now the step-by-step).
    ```csharp
    var results = collection.Where( num => num % 2 != 0);
    ```
* LINQ operations are considered declarative, here we say "give me everything that is odd", not "step through the collection, check this item, if it is odd then add it to the collection".
* A simple example in python.
  ```python
  # Declarative
  small_nums = [x for x in range(20) if x < 5]

  # Imperative
  small_nums = []
  for i in range(20):
      if i < 5:
          small_nums.append(i)
  ```

## [Nullish assignments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators)

The nullish coalescing operator is evaluated left to right, it is tested for possible short-circuit evaluation using the following rule:
`(some expression that is neither null nor undefined) ?? expr` is short-circuit evaluated to the left-hand side expression if left-hand side proves to be either `null` or `undefined`.
It is equivalent to `x ?? (x = y);`.
```
x = 10
> 10
y = 20
> 20
x ?? y
> 10
x = undefined
> undefined
x ??= y
> 20
x ??= 30
> 20
```
