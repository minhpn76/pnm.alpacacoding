---
author: Minh PN
pubDatetime: 2024-08-26T02:00:53Z
title: "[Frontend] Creating Reusable Form Controllers with React Hook Form Part 1"
slug: "creating-reusable-form-controllers-react-hook-form-part-1"
featured: true
draft: false
tags:
  - react
  - react hook form
  - frontend
description: A guide to understanding controlled vs uncontrolled inputs in React and how to create reusable forms using React Hook Form.
---

When working with React Hook Form, we often find ourselves repeatedly writing similar code to register form fields and handle validation. This repetitive code can quickly grow cumbersome as our forms become more complex.

In this article, we'll explore how to convert these repetitive input fields into a reusable component using InputTextController. By the end of this guide, you'll have a streamlined approach for managing form inputs, reducing code duplication, and improving maintainability.

## Table of contents

## Problem: Repetitive Input Fields

Here's a common pattern when using React Hook Form for multiple inputs:

```jsx
import React from "react";
import { useForm } from "react-hook-form";

const SimpleForm = () => {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = data => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Input for Name */}
      <input {...register("name", { required: "Name is required" })} />
      {errors.name && <span>{errors.name.message}</span>}

      {/* Input for Email */}
      <input
        {...register("email", {
          required: "Email is required",
          pattern: {
            value: /^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$/,
            message: "Invalid email address",
          },
        })}
      />
      {errors.email && <span>{errors.email.message}</span>}

      <button type="submit">Submit</button>
    </form>
  );
};

export default SimpleForm;
```

In this example, the register function and validation logic are repeated for every input field. While this works fine for small forms, it quickly becomes cumbersome when you have many inputs. Also, if you need to change validation logic or add additional behavior, you have to update each field individually.

## Solution: Creating a Reusable InputTextController Component

Instead of repeating the same logic for each input, we can extract the input field into a reusable component that wraps around Controller from React Hook Form. This allows us to handle validation and props in a consistent way across all inputs.

Here’s how to implement the InputTextController component:

Step 1: Create the InputTextController Component

```jsx
import React from 'react';
import { Controller, FieldValues, FieldPath } from 'react-hook-form';
import { TextField, TextFieldProps } from '@mui/material';

interface InputTextControllerProps<TFieldValues extends FieldValues, TName extends FieldPath<TFieldValues>> {
  name: TName;
  control: any;
  rules?: object;
  defaultValue?: string;
  type?: string;
  label?: string;
}

const InputTextController = <TFieldValues extends FieldValues, TName extends FieldPath<TFieldValues>>({
  name,
  control,
  rules,
  defaultValue = '',
  type = 'text',
  label,
}: InputTextControllerProps<TFieldValues, TName>) => {
  return (
    <Controller
      name={name}
      control={control}
      defaultValue={defaultValue}
      rules={rules}
      render={({ field, fieldState: { error } }) => (
        <TextField
          {...field}
          label={label}
          type={type}
          error={!!error}
          helperText={error ? error.message : ''}
        />
      )}
    />
  );
};

export default InputTextController;

```

Step 2: Use the InputTextController in Your Form
Now, you can use the reusable InputTextController in place of individual register calls:

```jsx
import React from "react";
import { useForm } from "react-hook-form";
import InputTextController from "./InputTextController"; // Import the reusable component

const SimpleForm = () => {
  const {
    control,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = data => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <InputTextController
        name="name"
        control={control}
        label="Name"
        rules={{ required: "Name is required" }}
      />

      <InputTextController
        name="email"
        control={control}
        label="Email"
        type="email"
        rules={{
          required: "Email is required",
          pattern: {
            value: /^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$/,
            message: "Invalid email address",
          },
        }}
      />

      <button type="submit">Submit</button>
    </form>
  );
};

export default SimpleForm;
```

Advantages of This Approach

1. Code Reusability: By creating a reusable InputTextController, you reduce the amount of repeated code in your forms. This makes the code more maintainable and easier to extend.

2. Consistency: Validation, error handling, and other logic can now be handled in a consistent way across all inputs.

3. Scalability: If you need to add more fields to your form, you only need to instantiate the InputTextController with the required props instead of rewriting validation logic.

## Conclusion

Refactoring repetitive form inputs into reusable components like InputTextController is a powerful way to make your React forms more maintainable and easier to work with. As your forms grow in complexity, this approach will save you time and ensure consistency across all form fields.

By leveraging Controller from React Hook Form, you can easily abstract away common form logic and keep your components clean and reusable. This is especially useful when working with large forms or when multiple forms share similar validation rule
