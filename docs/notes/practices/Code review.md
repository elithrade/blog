# Code review

[What to look for in a code review](https://google.github.io/eng-practices/review/reviewer/looking-for.html)

## Design
* Do the interactions of various code changes make sense?
* Does it integrate well with the rest of the system? For example, a piece of code can be extracted to allow it to be used by other developers?
* Does the changes break certain design principles such as SOLID?

## Functionality
* As a code reviewer, always think of edge cases, look for concurrency issues, try to think like a end user, and making sure there is no bugs just by reading the code.
* Validate UI changes.
* See if there is any sort of parallel programming going on can cause deadlocks or race conditions.

## Complexity
* Is the changes too hard to understand for code reviewer?
* Avoid over-engineering, where developers make changes more generic than it needs to be.
* Functionalities that not presently needed by the system.
* Encourage developers they know that needs to be solved now rather than the problem developers speculate might need to be solved in the future.

## Test
* Ask for unit, integration, end-to-end tests for the changes. Tests should be added unless it is emergency problem.
* Make sure the tests are sensible and useful.
* Remember tests are also part of code has to be maintained, avoid complexity in tests.

## Naming
* Good naming for everything?
* A good name is long enough to fully communicate what it is or does.

## Comments
* Are comments clear in understandable english?
* Comments are useful to explain why some code exists, and should not explain what the code is doing.
* Exceptions include regular expressions and algorithms.
* Comments are different to documentation.

## Style
* Make sure certain style is followed.
* Style changes (fix formatting)0 should not mix with other changes.

## Documentation
* If code change how users build, test, interact or release code, check if appropriate documentation, including READMEs also need updating.

## Context
* It is helpful to look at the changes in a broad context, not just the changed lines.
* Think about the changes in the context of the system as a whole. Does it improve the code health of the whole system or does it make it more complex, less tested etc.?

## Good things
* If you see something nice in the changes, especially they addressed one of your comments in a great way, tell the developer.
* Code reviews mostly focus on bad things, but they should offer encouragements and appreciation for good practices.

## Summary

* The code is well-designed.
* The functionality is good for the users of the code.
* Any UI changes are sensible and look good.
* Any parallel programming is done safely.
* The code isn’t more complex than it needs to be.
* The developer isn’t implementing things they might need in the future but don’t know they need now.
* Code has appropriate unit tests.
* Tests are well-designed.
* The developer used clear names for everything.
* Comments are clear and useful, and mostly explain why instead of what.
* Code is appropriately documented (generally in g3doc).
* The code conforms to our style guides.
