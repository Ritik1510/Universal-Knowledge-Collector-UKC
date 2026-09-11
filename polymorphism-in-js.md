# Polymorphism in js

polymorphism is completely possible in JavaScript. Because JavaScript is a dynamically typed, prototype-based language, polymorphism manifests differently than it does in statically typed languages like Java or C++. [1, 2, 3]  
JavaScript primarily implements runtime (dynamic) polymorphism through two mechanisms: Method Overriding and Duck Typing. [1, 3, 4]  
1. Method Overriding (Subtype Polymorphism) 
When a child class extends a parent class, it can redefine a method to provide its own specific behavior. When you call that method, JavaScript automatically determines at runtime which version to execute based on the object's type. [1, 5]  
2. Duck Typing 
JavaScript doesn't have strict type safety or formal interfaces. Instead, it relies on a philosophy called Duck Typing: "If it walks like a duck and quacks like a duck, it's a duck." [3, 4, 6]  
As long as different objects share the same method name, a function can treat them interchangeably without them needing to share a common parent class. [1, 4]  
What about Method Overloading? (Static Polymorphism) 
In languages like Java, you can have multiple functions with the exact same name but different argument counts or types. JavaScript does not support true method overloading natively. If you declare two functions with the same name, the second one will simply overwrite the first. [1, 7]  
However, you can easily simulate method overloading by utilizing default parameters, type checking, or checking the number of arguments inside a single function: [1, 8]  
Would you like to see how to implement polymorphism using older ES5 prototype syntax, or are you looking to design a specific real-world feature (like a notification or payment system) using these principles? [2, 7, 9]  

AI can make mistakes, so double-check responses

[1] https://www.geeksforgeeks.org/javascript/polymorphism-in-javascript/
[2] https://prototypr.io/post/unlocking-the-power-of-polymorphism-in-javascript-a-deep-dive
[3] https://namastedev.com/blog/practical-polymorphism-in-javascript/
[4] https://javascript.plainenglish.io/polymorphism-in-javascript-6571c9c000ac
[5] https://www.youtube.com/watch?v=YkhLw5tYR6c
[6] https://viktor-kukurba.medium.com/object-oriented-programming-in-javascript-3-polymorphism-fb564c9f1ce8
[7] https://www.codewithharry.com/tutorial/js-polymorphism
[8] https://codesignal.com/learn/courses/revisiting-software-design-patterns-in-javascript/lessons/polymorphism-in-javascript-harnessing-the-power-of-oop
[9] https://coddy.tech/learn/javascript/object_oriented_programming/what_is_polymorphism

