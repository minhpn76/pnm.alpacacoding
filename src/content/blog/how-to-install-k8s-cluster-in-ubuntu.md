---
author: Minh PN
pubDatetime: 2024-09-20T13:20:53Z
title: "[K8s] How to install k8s cluster in ubuntu"
slug: "how-to-install-k8s-cluster-in-ubuntu"
featured: true
draft: true
tags:
  - Kubernetes cluster
  - ubuntu
  - docker
description: A comprehensive guide on how to write unit tests for a React app using Jest, React Testing Library, and MSW to achieve high test coverage.
---

Kubernetes is an open-source platform for managing containers like Docker. It acts as an orchestration system that automates deployment, scaling, and management of containerized applications. With Kubernetes, you can seamlessly manage hybrid, on-premise, and public cloud infrastructure for your deployment tasks.

Docker allows you to create containers from pre-configured images and applications, while Kubernetes takes container management to the next level by balancing loads between containers and enabling you to run multiple containers across several systems.

## Table of contents

## Prerequisites

- 2 or more Linux servers running Ubuntu 20.04
  root privileges
- 3 or more of Ram
- 2 CPUs or more
- Full network connectivity between all machines in the cluster (public or private network is fine)
- Unique hostname, MAC address, and product_uuid for every node.
- Certain ports are open on your machines.
- Swap disabled. You MUST disable swap for the kubelet to work properly.

## Step 1: Install ubuntu server via VMWare

```jsx
//Login page
import {
  Box,
  Button,
  FormControl,
  FormErrorMessage,
  FormLabel,
  Heading,
  Input,
  Stack,
  Text,
  useColorModeValue,
} from "@chakra-ui/react";
import React from "react";
import { useForm } from "react-hook-form";
import { useNavigate } from "react-router-dom";
import { useLogin } from "../../services/index";

const Login = () => {
  const navigate = useNavigate();

  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const { mutateAsync, error, isPending } = useLogin(navigate);

  const onSubmit = ({ email, password }) => {
    mutateAsync({
      username: email,
      password,
    });
  };

  const navigateRegister = () => {
    navigate("/register");
  };

  return (
    <Box
      minH="100vh"
      display="flex"
      alignItems="center"
      justifyContent="center"
    >
      <Box
        maxW="md"
        w="full"
        p={8}
        borderWidth={1}
        borderRadius="lg"
        boxShadow="lg"
        bg={useColorModeValue("white", "gray.700")}
      >
        <Stack spacing={4}>
          <Heading fontSize="2xl" textAlign="center">
            Log in to your account
          </Heading>
          <form onSubmit={handleSubmit(onSubmit)}>
            <FormControl isInvalid={errors.email}>
              <FormLabel>Email address</FormLabel>
              <Input
                type="text"
                data-testid="email"
                {...register("email", { required: "Email is required" })}
              />
              {errors.email && (
                <FormErrorMessage>{errors.email.message}</FormErrorMessage>
              )}
            </FormControl>
            <FormControl isInvalid={errors.password} mt={4}>
              <FormLabel>Password</FormLabel>
              <Input
                type="password"
                data-testid="password"
                {...register("password", { required: "Password is required" })}
              />
              {errors.password && (
                <FormErrorMessage>{errors.password.message}</FormErrorMessage>
              )}
            </FormControl>
            <Stack spacing={6} mt={4}>
              <Button
                type="submit"
                colorScheme="blue"
                size="lg"
                fontSize="md"
                isLoading={isPending}
              >
                Log in
              </Button>
            </Stack>
          </form>
          {error && <Text color="tomato">{error.message}</Text>}
        </Stack>
        <Text mt={4} textAlign="center">
          Don't have an account?{" "}
          <Button variant="link" onClick={navigateRegister}>
            Sign up
          </Button>
        </Text>
      </Box>
    </Box>
  );
};

export default Login;
```

```jsx
//Login unit test
import { fireEvent, screen, waitFor } from "@testing-library/react";
import { renderWithRouterReactQuery } from "../../helpers/tests/utils";
import * as Service from "../../services/index";
import Login from "./index";

describe("Login form", () => {
  test("Should be display validate message", async () => {
    renderWithRouterReactQuery(<Login />);
    const loginBtn = screen.getByText("Log in");
    fireEvent.click(loginBtn);

    const emailError = await screen.findByText("Email is required");
    expect(emailError).toBeInTheDocument();

    const passwordError = await screen.findByText("Password is required");
    expect(passwordError).toBeInTheDocument();
  });

  test("Should be redirect to home page", async () => {
    const { history } = renderWithRouterReactQuery(<Login />);

    const inputText = screen.getByTestId("email");
    const passwordText = screen.getByTestId("password");
    fireEvent.input(inputText, { target: { value: "admin" } });
    fireEvent.input(passwordText, { target: { value: "admin" } });

    const loginBtn = screen.getByText("Log in");
    fireEvent.click(loginBtn);

    await waitFor(() => {
      expect(history.location.pathname).toBe("/");
    });
  });

  test("Should be display error message", async () => {
    const jestSpy = jest.spyOn(Service, "useLogin").mockImplementation(
      jest.fn().mockReturnValue({
        error: { message: "Error message" },
        isPending: false,
        mutateAsync: jest.fn(),
      })
    );

    renderWithRouterReactQuery(<Login />);

    const inputText = screen.getByTestId("email");
    const passwordText = screen.getByTestId("password");
    fireEvent.input(inputText, { target: { value: "admin" } });
    fireEvent.input(passwordText, { target: { value: "admin" } });

    const loginBtn = screen.getByText("Log in");
    fireEvent.click(loginBtn);

    const errorMessage = await screen.findByText("Error message");
    expect(errorMessage).toBeInTheDocument();
    jestSpy.mockRestore();
  });

  test("Should be redirect to register page", async () => {
    const { history } = renderWithRouterReactQuery(<Login />);

    const signUpBtn = screen.getByText("Sign up");
    fireEvent.click(signUpBtn);
    expect(history.location.pathname).toBe("/register");
  });
});
```

2. Register Page: The Register Page allows users already have an account, they should be able to navigate to the Login Page by clicking the "Log in" button.

Here's the breakdown of test cases for the Register page:

- **Test Case**: When a user clicks the "Log in" button on the Register Page, the app should navigate to the Login Page.

```jsx
// Register page
import React from "react";
import {
  Box,
  Button,
  Stack,
  Heading,
  Text,
  useColorModeValue,
} from "@chakra-ui/react";
import { useNavigate } from "react-router-dom";

const Register = () => {
  const navigate = useNavigate();

  const navigateLogin = () => {
    navigate("/login");
  };

  return (
    <Box
      minH="100vh"
      display="flex"
      alignItems="center"
      justifyContent="center"
    >
      <Box
        maxW="md"
        w="full"
        p={8}
        borderWidth={1}
        borderRadius="lg"
        boxShadow="lg"
        bg={useColorModeValue("white", "gray.700")}
      >
        <Stack spacing={4}>
          <Heading fontSize="2xl" textAlign="center">
            Create a new account
          </Heading>
        </Stack>
        <Text mt={4} textAlign="center">
          Already have an account?{" "}
          <Button variant="link" onClick={navigateLogin}>
            Log in
          </Button>
        </Text>
      </Box>
    </Box>
  );
};

export default Register;
```

```jsx
// Register unit test
import { fireEvent, screen } from "@testing-library/react";
import { renderWithRouterReactQuery } from "../../helpers/tests/utils";
import Register from "./index";

describe("Register form", () => {
  test("Should be redirect to login page", async () => {
    const { history } = renderWithRouterReactQuery(<Register />);

    const loginBtn = screen.getByText("Log in");
    fireEvent.click(loginBtn);
    expect(history.location.pathname).toBe("/login");
  });
});
```

3. Home Page: The Home Page displays a welcome message to the user after a successful login. If not successfully will be display welcome message exclude user name

Here's the breakdown of test cases for the Home page:

- **Test Case 1**: Ensure the correct welcome message is shown when the user is logged in.

- **Test Case 2**: Ensure the default message is displayed when no username is stored.

```jsx
// Home page
import { Box, Heading, Stack, useColorModeValue } from "@chakra-ui/react";
import React from "react";

const Home = () => {
  const name = localStorage.getItem("username") || "";

  let textWelcome = "Welcome to Jest Demo";
  if (name) {
    textWelcome += `, ${name}!`;
  }

  return (
    <Box
      minH="100vh"
      display="flex"
      alignItems="center"
      justifyContent="center"
    >
      <Box
        maxW="md"
        w="full"
        p={8}
        borderWidth={1}
        borderRadius="lg"
        boxShadow="lg"
        bg={useColorModeValue("white", "gray.700")}
      >
        <Stack spacing={4}>
          <Heading fontSize="2xl" textAlign="center">
            {textWelcome}
          </Heading>
        </Stack>
      </Box>
    </Box>
  );
};

export default Home;
```

```jsx
// Home unit test
import { render, screen } from "@testing-library/react";
import Home from "./index";

describe("Home page", () => {
  test("Should be display full welcome message", async () => {
    jest.spyOn(Storage.prototype, "getItem").mockImplementation(key => {
      if (key === "username") {
        return "MinhPN";
      }
      return null;
    });
    render(<Home />);
    const welcomeText = screen.getByText(/Welcome to Jest Demo, MinhPN!/i);
    expect(welcomeText).toBeInTheDocument();
  });

  test("Should be display welcome message without name", async () => {
    jest.spyOn(Storage.prototype, "getItem").mockReturnValue(null);
    render(<Home />);
    const welcomeText = screen.getByText(/Welcome to Jest Demo/i);
    expect(welcomeText).toBeInTheDocument();
  });
});
```

## Unit Test Coverage

When you write unit tests like in the examples above, the application can achieve **50 - 60%** test coverage. However, to reach **90%** coverage, you need to follow these steps:

- **Run Test Coverage**: Use tools like Jest with the coverage option (`yarn test:coverage`). This will help identify functions or code sections that haven't been covered by the current unit tests.

- **Handle Uncovered Functions**: : Review the test coverage report and write additional test cases for parts of the code that are missing coverage, including: **services folder, helpers, layouts,...**

- **Learn More**: Refer to the [Jest test demo](https://github.com/minhpn76/pnm.jest-test-demo) repo mentioned above for detailed implementation and optimizing test coverage for a React application.

- **Tables test coverages**
  | File | Statements | Branches | Functions | Lines |
  | -------------- | ------------ | ---------- | ---------- | ------------ |
  | config | 100% (8/8) | 100% (3/3) | 100% (2/2) | 100% (7/7) |
  | helpers/tests | 100% (4/4) | 100% (1/1) | 100% (0/0) | 100% (4/4) |
  | layouts | 100% (2/2) | 100% (1/1) | 100% (0/0) | 100% (2/2) |
  | pages/Home | 100% (6/6) | 100% (1/1) | 100% (4/4) | 100% (6/6) |
  | pages/Login | 100% (9/9) | 100% (3/3) | 100% (6/6) | 100% (9/9) |
  | pages/Register | 100% (5/5) | 100% (2/2) | 100% (0/0) | 100% (5/5) |
  | services | 100% (15/15) | 100% (8/8) | 100% (2/2) | 100% (13/13) |

## Best Practices to Improve Test Coverage

To achieve high test coverage (up to 90%), ensure you follow these practices:

- **Test Core Logic**: Focus on key business logic and critical components that drive the app's functionality.

- **Mock API Calls**: Use tools like MSW to mock API requests in tests and avoid relying on external servers.

- **Simulate User Interactions**: Use React Testing Library's fireEvent and userEvent utilities to simulate user actions such as clicking buttons and typing in inputs.

- **Test Edge Cases**: Write tests for edge cases, such as empty input submissions, failed API requests, and invalid user input.

- **Generate a Coverage Report**: Use Jest's coverage report to identify areas that lack sufficient test coverage and ensure critical parts of your app are tested.

## Conclusion

Writing unit tests for your React app is crucial for ensuring it works as intended, especially as it grows.Let's Focus on testing core logic, key user interactions, and mocking server responses to ensure your app is reliable and resilient.
