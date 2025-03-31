React is a JavaScript library for building interactive user interfaces.

**User Interface (UI)** - the elements that users see and interact with on-screen.

![[React.js - user interface.png]]

**Library** - React provides helpful functions (APIs) to build UI, but leaves it up to the developer where to use those functions in their application.

Imperative programming is like giving a chef step-by-step instructions on how to make a pizza. Declarative programming is like ordering a pizza without being concerned about the steps it takes to make the pizza. React is a declarative library that can be used to build user interfaces. Because, as a developer, I can tell React what I want to happen to the UI, and React will figure out the steps of how to update the DOM on my behalf.

## React core concepts

1. Components
2. Props
3. State
## Three rules of JSX

1. Return a single root element.
2. Close all the tags.
3. camelCase most of the things.

Use a JSX Converter to translate existing HTML and SVG to JSX.

## Essential JavaScript for React

* Functions and Arrow Functions
* Objects
* Arrays and array methods
* De-structuring
* Template literals
* Ternary Operations
* ES Modules and Import / Export Syntax

# Book

Completed till page 55.

### Callback handler

We pass a function from a parent component to a child component via props; we call this function in the child component, but have the actual implementation of the called function in the parent component.
So, when an event handler is passed on props from a parent component to it's child component, it becomes a **Callback Handler**. React props are always passed down the component tree and therefore functions that are passed down as callback handlers in props can be used to communicate up the component tree. Because when a function is passed down the component tree, as `props` are immutable, only the reference of the function is passed and not the actual implementation along with it. When this passed down function is called, the control flow actually goes up the component tree.

### Lifting state

![[lifting state up in React.png]]

**Rule of thumb:** 

* Always manage state at the component level where every component that's interested in it is one that either manages the state, or a component below the state managing component.
* If a component below needs to update the state, pass a callback handler down to it which allows this particular component to update the state above in the parent component.
* If a component below needs to use the state, pass it down as a prop.

### Interview questions

**Q1**: What is JSX in React?
**Answer**: JSX is a syntax extension for JavaScript recommended by React for describing what the UI should look like.

**Q2**: Can JSX be directly rendered by browsers?
**Answer**: No, browsers can't understand JSX. It needs to be transpiled to regular JavaScript using tools like Babel.

**Q3**: Is JSX mandatory in React?
**Answer**: No, JSX is not mandatory, but it's widely used and convenient way to write React components.

**Q4**: How do you render a variable in JSX?
**Answer**: Use curly braces to embed variables in JSX, like`{myVariable}`.

**Q5**: How to render a list of items in JSX?
**Answer**: Use `map()` to iterate over the array and return JSX elements for each item.

**Q6**: What happens if you return `null` instead of the JSX?
**Answer**: Returning `null` instead of JSX is allowed.

**Q7**: What does the term "JSX expression" refer to?
**Answer**: JSX expressions are JavaScript expressions embedded within curly braces in JSX, allowing dynamic content.

**Q8**: Can you embed HTML directly within JSX?
**Answer**: Yes, you can embed HTML directly within JSX, but it's generally discouraged due to security risks. Use `dangerouslySetInnerHTML` cautiously.

**Q9**: How do you comment in JSX?
**Answer**: Use curly braces for JavaScript comments `{/* comment */}`.

**Q10**: Why is it beneficial to extract components in React?
**Answer**: Extracting components promotes reusability, maintainability, and a cleaner component structure.

**Q11**: How do you decide when to extract a component?
**Answer**: Extract a component when you find repeated UI patterns or functionality within your code.

**Q12**: Is it possible to extract components across different files?
**Answer**: Yes, extracting components into separate files promotes better file organization and modularity.

**Q13**: What is a callback handler in React?
**Answer**: A callback handler is a function passed as a prop to a child component, allowing the child to communicate with the parent.

**Q14**: How do you pass a callback handler to a child component?
**Answer**: Include it as a prop, like `<ChildComponent callback={handleCallback} />` 

**Q15**: How do you define a callback handler in a parent component?
**Answer**: Create a function in the parent component.

**Q16**: Call a callback handler receive parameters?
**Answer**: Yes, callback handlers can receive parameters passed by the child component.

**Q17**: Can callback handlers be asynchronous?
**Answer**: Yes, callback handlers can be asynchronous, allowing for handling asynchronous operations.

**Q18**: Can you pass a callback handler through multiple layers of components?
**Answer**: Yes.

**Q19**: Can a child component have multiple callback handlers from the same parent?
**Answer**: Yes.

**Q20**: Is it common to use callback handlers for form submissions in React?
**Answer**: Yes, callback handlers are commonly used for handling form submissions and updating state in parent components.

**Q21**: What is lifting state in React?
**Answer**: Lifting state refers to the practice of moving the state from a child component to its parent component.

**Q22**: Why would you lift state in React?
**Answer**: To share and manage state at a higher level, making it accessible to multiple child components.

**Q23**: How do you lift state in React?
**Answer**: Move the state and related functions to a common ancestor (usually a parent) component.

**Q24**: Can multiple child components share the same lifted state?
**Answer**: Yes, lifting state allows multiple child components to share the same state.

**Q25**: What's the advantage of lifting state over using local state in a component?
**Answer**: Lifting state promotes sharing state among components.

**Q26**: What is the role of callbacks in lifting state?
**Answer**: Callback functions are used to pass data from child to parent components when lifting state.

**Q27**: What is the role of callbacks in lifting state?
**Answer**: Callback functions are used to pass data from child to parent components when lifting state.

**Q28**: Can a child component modify the state of a parent component directly through a callback handler?
**Answer**: No, the child component can invoke the callback to notify the parent, and the parent can decide how to update its state.

**Q29**: Is it necessary to lift all state to the top-level parent component?
**Answer**: No, only lift state to a level where it needs to be shared among multiple components.

**Q30**: How does lifting state contribute to better component reusability? 
**Answer**: Lifting state allows stateful logic to be concentrated in a common ancestor, making components more reusable.


