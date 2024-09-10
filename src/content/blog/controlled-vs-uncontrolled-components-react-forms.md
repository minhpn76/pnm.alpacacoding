---
author: Minh PN
pubDatetime: 2024-08-20T03:58:53Z
title: "[Frontend] Controlled vs Uncontrolled Components in React Forms"
slug: "controlled-vs-uncontrolled-components-react-forms"
featured: true
draft: false
tags:
  - react
  - react hook form
  - frontend
  - controlled
  - uncontrolled
description: A guide to understanding controlled vs uncontrolled inputs in React and how to create reusable forms using React Hook Form.
---

In this article, we’ll explore the differences between controlled and uncontrolled components in React. Understanding these concepts is key to building reusable and maintainable forms. Additionally, we will lay the groundwork for building a reusable React Hook Form in this multi-part series.

## Table of Contents

- [Introduction to Controlled and Uncontrolled Components](#introduction-to-controlled-and-uncontrolled-components)
- [Understanding Controlled Components](#understanding-controlled-components)
- [Understanding Uncontrolled Components](#understanding-uncontrolled-components)
- [Benefits and Use Cases](#benefits-and-use-cases)
- [Conclusion](#conclusion)

## Introduction to Controlled and Uncontrolled Components

When working with forms in React, you often encounter two main approaches to handling input elements: **controlled** and **uncontrolled** components. These two paradigms define how React manages form data.

- **Controlled components**: React handles the state of the form elements, and every update is passed through React's `setState`.
- **Uncontrolled components**: The DOM itself manages the form state, and you access the data via references.

Understanding when and how to use each approach is crucial to building robust, reusable forms, particularly in complex applications.

## Understanding Controlled Components

In controlled components, form elements derive their values from the component state. This means that every time the user enters data into the input, React updates its internal state to reflect that change.

Here’s an example of a controlled input:

```jsx
import { useState } from "react";

const ControlledInput = () => {
  const [value, setValue] = useState("");

  return (
    <input type="text" value={value} onChange={e => setValue(e.target.value)} />
  );
};

export default ControlledInput;
```

## Understanding Uncontrolled Components

Uncontrolled components work differently: the form element keeps track of its own state, and React interacts with the DOM only when necessary. This is often done through refs to access form values.

Here’s an example of an uncontrolled input:

```jsx
import { useRef } from "react";

const UncontrolledInput = () => {
  const inputRef = useRef();

  const handleSubmit = () => {
    alert(`Input Value: ${inputRef.current.value}`);
  };

  return (
    <>
      <input type="text" ref={inputRef} />
      <button onClick={handleSubmit}>Submit</button>
    </>
  );
};

export default UncontrolledInput;
```

Key Features of Uncontrolled Components:

Minimal state management: Since React doesn’t monitor changes, you don’t need to constantly update the component state.
Access via refs: You access the value of the input only when needed by using ref, thus keeping the state handling outside of React's state mechanism.
Simplicity: For simple forms or when you don’t need to process the input value immediately, uncontrolled components are a good fit.

When to Use:
When you don’t need real-time validation.
For small forms or one-off inputs where managing state through React may be overkill.
When working with libraries like React Hook Form, which provides convenient access to the form state using refs.

## Benefits and Use Cases

### Controlled Components

Advantages:

Full control over form elements.
Real-time validation and conditional rendering based on input data.

Use cases:

Complex forms where inputs impact other parts of the UI.
Scenarios requiring input validation while typing.

### Uncontrolled Components

Advantages:

Minimal interaction with React's state.
Great for performance in large forms where constant re-rendering is not ideal.

Use cases:

Simple, one-off form inputs.
Integrations with third-party libraries like React Hook Form.

## Conclusion

Both controlled and uncontrolled components have their place in React applications. Controlled components offer more flexibility and real-time control, while uncontrolled components simplify form management when you don’t need constant validation or control over the input state.

In the next part of this series, we’ll explore how to use React Hook Form to manage forms efficiently, allowing us to combine the best of both worlds.
