# JavaScript Essentials - TryHackMe

> Learn how to use JavaScript to add interactivity to a website and understand associated vulnerabilities

**Room Link:** https://tryhackme.com/room/javascriptessentials

<img width="1534" height="784" alt="image" src="https://github.com/user-attachments/assets/b9dc354a-a715-4e7e-a890-c38e205d2af1" />

---

## Task 1: Introduction

**No answer needed**

---

## Task 2: Essential Concepts

### Key JavaScript Concepts

**Variables**
- Containers for storing data
- Declared with `var`, `let`, or `const`
- `var` is function-scoped
- `let` and `const` are block-scoped

**Data Types**
- `string` - Text values
- `number` - Numeric values
- `boolean` - true/false
- `null` - Intentionally empty
- `undefined` - Not assigned
- `object` - Arrays, objects, etc.

**Functions**
- Code blocks for specific tasks
- Reusable instead of repeating code
- Example: `PrintResult(rollNum)`

**Loops**
- Run code multiple times
- Types: `for`, `while`, `do...while`
- Execute while condition is true

**Request-Response Cycle**
- Client sends request
- Server responds with resources

### Questions

**What term allows you to run a code block multiple times as long as it is a condition?**
- Answer: `Loop`

---

## Task 3: JavaScript Overview

### What is JavaScript?

JavaScript (JS) is an **interpreted language** that runs directly in the browser.

### Key Features

- **Client-side execution** - Runs in the browser
- **Interpreted** - No compilation needed
- **Dynamic** - Can change content in real-time
- **Console access** - Test code in Chrome Console

### Example Program

```javascript
let x = 5;
let y = 10;
let result = x + y;
console.log("The result is: " + result);
```

**Output:** `The result is: 15`

### Basic Program Structure

**Hello, World!**
```javascript
console.log("Hello, World!");
```

**Variables and Data Types**
```javascript
let age = 25;
let name = "John";
let isStudent = true;
```

**Control Flow**
```javascript
if (age >= 18) {
    console.log("Adult");
} else {
    console.log("Minor");
}
```

**Functions**
```javascript
function greet(name) {
    console.log("Hello, " + name);
}
```

### Questions

**What is the code output if the value of x is changed to 10?**

If `x = 10` and `y = 10`, then:
```javascript
let result = 10 + 10; // result = 20
console.log("The result is: " + result);
```
- Answer: `The result is: 20`

---

**Is JavaScript a compiled or interpreted language?**
- Answer: `Interpreted`

---

## Task 4: Integrating JavaScript in HTML

### Two Ways to Add JavaScript

**Internal JavaScript**
- Code embedded directly in HTML
- Uses `<script>` tags
- Good for small scripts and learning

```html
<!DOCTYPE html>
<html>
<head>
    <title>Internal JS</title>
</head>
<body>
    <script>
        console.log("Hello from internal JS!");
    </script>
</body>
</html>
```

**External JavaScript**
- Code stored in separate `.js` file
- Linked with `src` attribute
- Keeps HTML clean
- Easy to reuse across pages

```html
<!DOCTYPE html>
<html>
<head>
    <title>External JS</title>
    <script src="script.js"></script>
</head>
<body>
</body>
</html>
```

### Verifying Integration Type

**Check page source:**
- **Internal JS** - `<script>` tags without `src`
- **External JS** - `<script src="filename.js">`

### Questions

**Which type of JavaScript integration places the code directly within the HTML document?**
- Answer: `Internal`

---

**Which method is better for reusing JS across multiple web pages?**
- Answer: `External`

---

**What is the name of the external JS file that is being called by external_test.html?**

Check the source code for:
```html
<script src="thm_external.js"></script>
```
- Answer: `thm_external.js`

---

**What attribute links an external JS file in the <script> tag?**
- Answer: `src`

---

## Task 5: Abusing Dialogue Functions

### JavaScript Dialogue Functions

**alert()** - Shows message with "OK" button
```javascript
alert("Hello THM");
```

**prompt()** - Asks for user input
```javascript
let name = prompt("What is your name?");
// Returns input or null
```

**confirm()** - Shows "OK" and "Cancel"
```javascript
let proceed = confirm("Do you want to proceed?");
// Returns true or false
```

### Exploitation Example

Malicious code can abuse these functions:
```javascript
alert("Hacked");
alert("Hacked");
alert("Hacked");
```

Multiple alerts can disrupt user experience!

### Questions

**In the file invoice.html, how many times does the code show the alert Hacked?**
- Answer: `3`

---

**Which of the JS interactive elements should be used to display a dialogue box that asks the user for input?**
- Answer: `prompt`

---

**If the user enters Tesla, what value is stored in the carName variable?**
```javascript
carName = prompt("What is your car name?");
```

When user types "Tesla", the variable stores that exact value.

- Answer: `Tesla`

---

## Task 6: Bypassing Control Flow Statements

### Control Flow in JavaScript

Control flow directs code execution based on conditions.

**Conditional Statements**
```javascript
if (age >= 18) {
    console.log("You are an adult");
} else {
    console.log("You are a minor");
}
```

**Loops**
```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}

while (condition) {
    // code
}
```

### Simple Authentication Bypass

Some developers implement basic JS-based login forms that can be tested:

```javascript
if (username === "admin" && password === "ComplexPassword") {
    alert("Login successful!");
}
```

### Questions

**What is the message displayed if you enter the age less than 18?**

For `age < 18`, the else block executes:
```javascript
console.log("You are a minor.");
```
- Answer: `You are a minor.`

---

**What is the password for the user admin?**

Check the JavaScript code for hardcoded credentials.

- Answer: `ComplexPassword`

---

## Task 7: Exploring Minified Files

### Minification vs Obfuscation

**Minification**
- Removes unnecessary characters
- Reduces file size
- Improves load times
- Still somewhat readable

**Obfuscation**
- Hides code logic
- Renames variables
- Inserts junk code
- Hard to read but functions the same

### Practical Steps

1. Create `hello.html` with JS code
2. Use online tool to minify/obfuscate
3. Code becomes unreadable but works the same
4. Use deobfuscation tools to revert

### Questions

**What is the alert message shown after running the file hello.html?**
- Answer: `Welcome to THM`

---

**What is the value of the age variable in the following obfuscated code snippet?**

Look through the obfuscated code to find where age is defined.

- Answer: `21`

---

## Task 8: Best Practices

### Security Best Practices

**1. Avoid Client-Side Validation Only**
- Always perform server-side validation
- Users can bypass client-side checks
- Never trust client input

**2. Avoid Untrusted Libraries**
- Only include libraries from trusted sources
- Verify library integrity
- Check for known vulnerabilities

**3. Avoid Hardcoded Secrets**
- Never hardcode API keys
- Don't store passwords in JS
- Use environment variables

**4. Minify and Obfuscate Code**
- Improves load time
- Adds protection layer
- Makes reverse engineering harder


### Questions

**Is it a good practice to blindly include JS in your code from any source (yea/nay)?**
- Answer: `nay`

---

## Task 9: Conclusion

**No answer needed**

---

## Quick Reference

### Variable Declaration

| Keyword | Scope | Reassignable | Redeclarable |
|---------|-------|--------------|--------------|
| `var` | Function | Yes | Yes |
| `let` | Block | Yes | No |
| `const` | Block | No | No |

### Data Types

| Type | Example | Description |
|------|---------|-------------|
| String | `"Hello"` | Text |
| Number | `42` | Numeric |
| Boolean | `true` | True/False |
| Null | `null` | Empty value |
| Undefined | `undefined` | Not assigned |
| Object | `{name: "John"}` | Complex data |

### Control Flow

| Statement | Purpose |
|-----------|---------|
| `if...else` | Conditional execution |
| `switch` | Multiple conditions |
| `for` | Loop with counter |
| `while` | Loop with condition |
| `do...while` | Execute then check |

### Dialogue Functions

| Function | Purpose | Returns |
|----------|---------|---------|
| `alert()` | Show message | Nothing |
| `prompt()` | Get input | String or null |
| `confirm()` | Yes/No choice | Boolean |

### Integration Methods

| Method | Code Location | Best For |
|--------|---------------|----------|
| Internal | Inside HTML | Small scripts, learning |
| External | Separate file | Reusable code, production |

### Security Practices

| Practice | Why |
|----------|-----|
| Server-side validation | Client-side can be bypassed |
| Trusted libraries only | Prevent malicious code |
| No hardcoded secrets | Easily accessible in JS |
| Minify/obfuscate | Reduce size, add protection |

---

## Key Takeaways

- **JavaScript is interpreted** - Runs directly in browser
- **Client-side execution** - Users can view and modify code
- **Variables** store data with `var`, `let`, or `const`
- **Functions** make code reusable
- **Loops** automate repetitive tasks
- **Internal JS** - Code in HTML file
- **External JS** - Code in separate `.js` file
- **Dialogue functions** - `alert()`, `prompt()`, `confirm()`
- **Control flow** - `if`, `else`, loops direct execution
- **Minification** reduces file size
- **Obfuscation** hides code logic
- **Never trust client-side** - Always validate server-side
- **Never hardcode secrets** - Use secure methods
- **Use trusted libraries** - Avoid malicious code
- **Security first** - Follow best practices

JavaScript is powerful but can be abused. Understanding both functionality and security is essential for web development.
