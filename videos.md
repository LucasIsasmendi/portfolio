# Videos
## Performance
### Big O Notation
[FreeCodeCamp Video Challenges](https://www.freecodecamp.com/videos/big-o-notation-what-it-is-and-why-you-should-care)  
Time complexity. 
* How long algorithms take. 
* Forward looking for larger scale site. 
* worst case scenario.
* Helps explain Why your code runs well and is efficient.

Time complexity is commonly estimated by counting the number of elementary operations (elementary operation = an operation that takes a fixed amount of time to preform) performed in the algorithm.
Time complexity is classified by the nature of the function T(n). `O` represents the function, and `(n)` represents the number of elements to be acted on.
Worst-case time complexity, the longest it could possibly take with any valid input, is the most common way to express time complexity.
When you discuss *Big-O notation*, that is generally referring to the worst case scenario.
For example, if we have to search two lists for common entries, we will calculate as if both entries would be at the very end of each list, just to be safe that we don't underestimate how long it could take.
* `O(1)` - determining if a number is odd or even. O(1) is a static amount of time, the same no matter how much information is there or how many users there are.
* `O(log N)` finding a word in the dictionary (using binary search). Binary search is an example of a type of 'divide and conquer' algorithm.
* `O(N)` reading a book
* `O(N log N)` sorting a deck of playing cards (using merge sort)
* `O(N^2)` checking if you have everything on your shopping list in your cart
* `O(infinity)` tossing a coin until it lands on heads
As a rule of thumb, anything with `N^2` or any other exponent is NOT a good algorithm for a site with multiple users.
If your algorithm slows down exponentially with the input, you're going to want to look for a more efficient way to solve that problem.
Whenever you’re coding loops within loops, you want to be especially mindful of time complexity.

Big O Cheat Sheet is the place to look once you can classify your algorithm, like as a 'merge-sort' or a 'quick-sort'.
bigocheatsheet.com/

Princeton Coursera course is NOT for the faint of heart. With examples and practice in Java, this course will cover iterating over data specifically with Java, sorting, and searching algorithms.
coursera.org/course/algs4partI

## LearnCode.academy
### [2016/2017 MUST-KNOW WEB DEVELOPMENT TECH - Watch this if you want to be a web developer] (https://www.youtube.com/watch?v=sBzRwzY7G-k)
FE: precompiler SASS, npm, Webpack, React,flux,redux.
DevOps: *Docker*, Kubernetes, Continuous Testing and Deployment (Jenkins, SemaphoreCI, CircleCI, Codeship)

### [EXTREMELY FAST & SIMPLE CI TESTING - SemaphoreCI](https://www.youtube.com/watch?v=OQAifaOEmVo)
push to github, semaphoreci performs the test after create a new environment, comments in readme are related with semaphoreci status.