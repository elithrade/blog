# Today I Learned

05/03/2022 - Today I was doing a [leetcode question](https://leetcode.com/problems/can-make-arithmetic-progression-from-sequence/), and I couldn't get one of the tests to pass, and eventually I figured out that JavaScript's `array.sort()` method by default sorts the elements alphabetically. `[-68, -96, -12, -40, 16].sort()` will give you `[ -12, -40, -68, -96, 16 ]`. Today I learned you have to pass in a comparer function if you want to sort numerically. `arr.sort((a, b) => a - b);`
