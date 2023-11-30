# TypeScript React

This repository contains TypeScript solutions to various React tasks. It covers typing components,
props, and states, utilizing React's type definitions effectively. Each task demonstrates different
use cases and scenarios commonly encountered while working with React and TypeScript.

## Task 1

You have a `React component` that uses `useRef` and `IntersectionObserver` to determine when the
user reaches the end of the content. Your task is as follows:

Set the correct prop types for this component. It has two properties: `children` and
`onContentEndVisible`. `Children` can be any valid `React node`, and `onContentEndVisible` is a
function without arguments that returns `void`.

Set the correct type for useRef. The reference endContentRef is used for a div that is located at
the end of the content.

Set the correct type for options (a class can also be a type for options).

```ts
import React, { useEffect, useRef } from "react";

// Describe Props
export function Observer({ children, onContentEndVisible }: Props) {
  // Specify the correct type for useRef, paying attention to the DOM element we are passing it to
  const endContentRef = useRef(null);

  useEffect(() => {
    // Specify the correct type for options, hint: you can also specify a class as a type
    const options = {
      rootMargin: "0px",
      threshold: 1.0,
      root: null,
    };

    const observer = new IntersectionObserver(entries => {
      entries.forEach(entry => {
        if (entry.intersectionRatio > 0) {
          onContentEndVisible();
          observer.disconnect();
        }
      });
    }, options);

    if (endContentRef.current) {
      observer.observe(endContentRef.current);
    }

    return () => {
      observer.disconnect();
    };
  }, [onContentEndVisible]);

  return (
    <div>
      {children}
      <div ref={endContentRef} />
    </div>
  );
}
```

## Task 2

Your task is to add types for the following elements of the code:

`RequestStep`: It is a string literal.

`State`: This type is an object with two properties: `isRequestInProgress` and `RequestStep`.

`Action`: This type represents possible actions that can be dispatched to the reducer.

Take a look at the code and describe the correct types for it.

```ts
import React, { useReducer } from "react";

const initialState: State = {
  isRequestInProgress: false,
  requestStep: "idle",
};

function requestReducer(state: State, action: Action): State {
  switch (action.type) {
    case "START_REQUEST":
      return { ...state, isRequestInProgress: true, requestStep: "start" };
    case "PENDING_REQUEST":
      return { ...state, isRequestInProgress: true, requestStep: "pending" };
    case "FINISH_REQUEST":
      return { ...state, isRequestInProgress: false, requestStep: "finished" };
    case "RESET_REQUEST":
      return { ...state, isRequestInProgress: false, requestStep: "idle" };
    default:
      return state;
  }
}

export function RequestComponent() {
  const [requestState, requestDispatch] = useReducer(requestReducer, initialState);

  const startRequest = () => {
    requestDispatch({ type: "START_REQUEST" });
    // Simulate a request to the server
    setTimeout(() => {
      requestDispatch({ type: "PENDING_REQUEST" });
      // Simulate receiving a response from the server
      setTimeout(() => {
        requestDispatch({ type: "FINISH_REQUEST" });
      }, 2000);
    }, 2000);
  };

  const resetRequest = () => {
    requestDispatch({ type: "RESET_REQUEST" });
  };

  return (
    <div>
      <button onClick={startRequest}>Start request</button>
      <button onClick={resetRequest}>Reset request</button>
      <p>Request state: {requestState.requestStep}</p>
    </div>
  );
}

export default RequestComponent;
```

## Task 3

You are creating a form component in `React`. You have an input field where you want to track
changes.  
For this purpose, you're using the `onChange` event handler.  
Your task is to correctly type the event being passed to this function.

```ts
import React, { useState } from "react";

export function FormComponent() {
  const [value, setValue] = useState("");

  const handleChange = event => {
    setValue(event.target.value);
  };

  return <input type="text" value={value} onChange={handleChange} />;
}
```

## Task 4

You've decided to apply context to the menu and now you need to type it.

Describe the type `SelectedMenu`: It should be an object containing an `id` of type `MenuIds`.

Describe the type `MenuSelected`: This type is an object that contains `selectedMenu`.

Describe the type `MenuAction`: This type is an object with a method `onSelectedMenu` that takes an
object of type `SelectedMenu` as an argument and returns `void`.

Describe the type `PropsProvider`: Provide the correct type for children.

Describe the type `PropsMenu`: Define the type for `menus`; it should be derived from the type
`Menu`.
