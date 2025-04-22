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

# Documentation

## Responding to events

* You can handle events by passing a function as a prop to an element like `<button>`.
* Event handlers must be passed, not called.
* You can define an event handler function separately or inline.
* Event handlers are defined inside a component, so they can access props.
* You can declare an event handler in a parent and pass it as a prop to a child.
* You can define your own event handler props with application-specific names.
* Events propagate upwards. Call `e.stopPropagation()` on the first argument (the event) to prevent that.
* Events may have unwanted default browser behavior. Call `e.preventDefault()` on the first argument to prevent that.
* Explicitly calling an event handler prop from a child handler is a good alternative to propagation. Because in this case, the control flow goes from child to parent to child.

## State: A component's memory

The `useState` Hook provides,

1. A state variable that retains data between renders.
2. A state setter function to update the variable and trigger React to render the component again.

*Hooks* are special functions that are only available while React is rendering. They let you "hook into" different React features.

# Book

Start from page 82.

### Callback handler

We pass a function from a parent component to a child component via props; we call this function in the child component, but have the actual implementation of the called function in the parent component.
So, when an event handler is passed on props from a parent component to it's child component, it becomes a **Callback Handler**. React props are always passed down the component tree and therefore functions that are passed down as callback handlers in props can be used to communicate up the component tree. Because when a function is passed down the component tree, as `props` are immutable, only the reference of the function is passed and not the actual implementation along with it. When this passed down function is called, the control flow actually goes up the component tree.

### Lifting state

![[lifting state up in React.png]]

**Rule of thumb:** 

* Always manage state at the component level where every component that's interested in it is one that either manages the state, or a component below the state managing component.
* If a component below needs to update the state, pass a callback handler down to it which allows this particular component to update the state above in the parent component.
* If a component below needs to use the state, pass it down as a prop.

HTML elements come with their internal state which is not coupled to React. HTML should know about the React state.

Even though both have the same syntax (three dots), the rest operator shouldn't be mistaken with the spread operator. Whereas the rest operator happens on the left side of an assignment, the spread operator happens on the right side. The rest operator is always used to separate an object from some of its properties. Below is an example of rest operator,

```JavaScript
const List = ({ list }) => (
  <ul>
    {list.map(({ objectID, ...item }) => (
      <Item key={objectID} {...item} />
    ))}
  </ul>
)
```

In this variation, the rest operator is used to destructure the `objectID` from the rest of the item object. Afterward, the `item` is spread with its key/value pairs into the Item component.
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

**Q30**: How does lifting state contribute to better component re-usability? 
**Answer**: Lifting state allows stateful logic to be concentrated in a common ancestor, making components more reusable.

Q31: What is a controlled component in React?
Answer: A controlled component is a component whose form elements are controlled by React state.

Q32: How do you create a controlled input in React?
Answer: Set the input value attribute to a state variable and provide an `onChange` handler to update the state.

Q33: What is the role of the value prop in a controlled input element?
Answer: The value prop sets the current value of the input, making it a controlled component.

Q34: How do you handle a controlled checkbox in React?
Answer: Use the `checked` attribute and provide an `onChange` handler to update the corresponding state.

Q35: How do you clear the value of a controlled component?
Answer: Set the state variable to an empty or null value to clear the value of a controlled component.

Q36: What are the potential downsides of using controlled components?
Answer: Controlled components can lead to verbose code, especially in forms with many input elements.

Q37: How do you destructure props in a function component's parameters?
Answer: You can destructure props directly in the function parameters like
`MyComponent({ prop1, prop2}) { .... }`

Q38: Can you provide a default value while destructuring props?
Answer: Yes, you can provide default values during destructuring such as, `{ prop1 = 'default', prop2 }`

Q39: Is it necessary to destructure all props, or can you choose specific ones?
Answer: You can choose to destructure specific props based on your component's needs, leaving others untouched.

Q40: How is the spread operator used in React props?
Answer: The spread operator is used to pass all properties of an object as separate props to a React component, like `<MyComponent {...obj} />`.

Q41: Can you use the spread operator to combine props with additional ones?
Answer: Yes, you can combine existing props with additional ones using the spread operator, like `<MyComponent {...props} newProp={value} />`

Q42: Does the spread operator create a shallow or deep copy of an object?
Answer: The spread operator create a shallow copy of an object, meaning nested objects are still references to the original.

Q43: What is the purpose of the rest operator in React?
Answer: The rest operator is used to collect remaining properties into a new object, often used in combination with props destructuring.

Q44: Why is array destructuring used for React Hooks like `useState` and object destructuring for props?
Answer: A React Hook like `useState` returns an array whereas props are an object; hence we need to apply the appropriate operation for the underlying data structure. The benefit of having an array returned from `useState` is the the values can be given any name in the destructuring operation.

Q45: What is prop drilling in React?
Answer: Prop drilling is the process of passing props through multiple layers of components to reach a deeply nested child component.
