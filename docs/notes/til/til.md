# Today I Learned

**05/03/2022** - Today I was doing a [leetcode question](https://leetcode.com/problems/can-make-arithmetic-progression-from-sequence/), and I couldn't get one of the tests to pass, and eventually I figured out that JavaScript's `array.sort()` method by default sorts the elements alphabetically. `[-68, -96, -12, -40, 16].sort()` will give you `[ -12, -40, -68, -96, 16 ]`. Today I learned you have to pass in a comparer function if you want to sort numerically. `arr.sort((a, b) => a - b);`

**16/03/2022** - I alway forget how to property use the spread operator `...` in Java/TypeScript. The following is a good example of how to replace an specific field of an array of objects. The `...` will spread the objects in the array and replace with `name` property with the specified value.

```ts
// ✅ Updating properties in multiple objects
const arr1 = [
  { id: 1, name: "Alice" },
  { id: 1, name: "Bob" },
  { id: 3, name: "Charlie" },
];
// Returns a new array
// [
//   { id: 1, name: "Alfred" },
//   { id: 1, name: "Bob" },
//   { id: 3, name: "Charlie" },
// ]
const newArr = arr1.map((obj) => {
  if (obj.id === 1) {
    return { ...obj, name: "Alfred" };
  }
  return obj;
});
```
