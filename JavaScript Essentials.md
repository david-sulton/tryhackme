# JavaScript Essentials
![image](https://github.com/user-attachments/assets/b6e63c21-bb45-47d8-b7e8-903f4764d6f0)

JavaScript (JS) is a popular scripting language that allows web developers to add interactive features to websites containing HTML and CSS (styling). Once the HTML elements are created, you can add interactiveness like validation, onClick actions, animations, etc, through JS. Learning the language is equally important as that of HTML and CSS. The JS scripts are used primarily with HTML.

This room is designed as an introductory overview of JS, specifically tailored for beginners with limited JS experience. The primary focus is on teaching the fundamentals of JS from a cyber perspective and how hackers utilise legitimate functionalities to achieve malicious results.

## Variables

Variables are containers that allow you to store data values in them. Like any other language, variables in JS are similar to containers to store data. When you store something in a bucket, you also need to label it so that it can be referenced later on easily. Similarly, in JS, each variable has a name; when we store a certain value in a variable, we assign a name to it to reference it later. image of var, let and const typesThere are three ways to declare variables in JS: var, let, and const. While var is function-scoped, both let, and const are block-scoped, offering better control over variable visibility within specific code blocks.

## Data Types

In JS, data types define the type of value a variable can hold. Examples include string (text), number, boolean (true/false), null, undefined, and object (for more complex data like arrays or objects).

## Functions

A function represents a block of code designed to perform a specific task. Inside a function, you group code that needs to perform a similar task. For example, you are developing a web application in which you need to print students' results on the web page. The ideal case would be to create a function PrintResult(rollNum) that would accept the roll number of the user as an argument.

   <script>
        function PrintResult(rollNum) {
            alert("Username with roll number " + rollNum + " has passed the exam");
            // any other logic to display the result
        }

        for (let i = 0; i < 100; i++) {
            PrintResult(rollNumbers[i]);
        }
    </script>
        

So, instead of writing the same print code for all the students, we will use a simple function to print the result.

## Loops

Loops allow you to run a code block multiple times as long as a condition is true. Common loops in JS are for, while, and do...while, which are used to repeat tasks, like going through a list of items. For example, if we want to print the results of 100 students, we can call the PrintResult(rollNum) function 100 times by writing it 100 times, or we can create a loop that will be iterated through 1 to 100 and will call the PrintResult(rollNum) function as shown below.

        // Function to print student result
        function PrintResult(rollNum) {
            alert("Username with roll number " + rollNum + " has passed the exam");
            // any other logic to the display result
        }

        for (let i = 0; i < 100; i++) {
            PrintResult(rollNumbers[i]); // this will be called 100 times 
        }
    

## Request-Response Cycle

In web development, the request-response cycle is when a user's browser (the client) sends a request to a web server, and the server responds with the requested information. This could be a webpage, data, or other resources.
