---
title: "Angel or demon: styled-components"
date: 2023-02-17 20:00
categories: [English, Programming]
keywords: styled-components, react, css, css-in-js
author: Özgür Adem Işıklı
lang: en
---

## What is styled-components?

[styled-components](https://styled-components.com/) is a solution that lets you write actual CSS in your JavaScript. But what does that mean? Why we should write CSS in JavaScript codes?

In this article, I am going to discuss styled-components and CSS in JS solutions.

## Getting started with styled-components

This is the first and a very simple example of styled-components;

```js
import styled from "styled-components";

const Button = styled.button`
  background: red;
  color: white;
`;
```

As you can see, this is a simple JS code. We created a constant called `Button` and after that did something weird. Let's take a look closer at the following syntax;

```js
styled.button`
  background: red;
  color: white;
`;
```

This is called as [Tagged templates](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates). In here, `styled.button` is actually a JS function. You can pass string parameters to a function like this. In this example, we are passing our CSS codes to the `styled.button()` function.

As a result of that code, you are going to have a styled component, which is called `Button` in this example. You can use this component in your applications such as React application.

## What is the benefit of using styled-components over using CSS?

The first time I saw the CSS-in-JS solution, I thought it was a new toy for a bored JavaScript developer. I was sure that it doesn't solve any problem since we are free to write CSS codes like the following example;

```html
<head>
  <style>
    button.red-button {
      background: red;
      color: white;
    }
  </style>
</head>
<body>
  <button class="red-button">My Button</button>
</body>
```

I ignored all CSS-in-JS solutions for a long time. Then, I understood the benefits of it in a project I had to use styled-components.

We can add more items to the list, but the most important ones;

- It solves class name issues.
- It makes your styled/components extendable.
- It helps to add logical expressions to your styles.

## Goodbye to class names

By creating a new component, you will already have a meaningful name for the component in your code. It means that you don't need to define many class names for an HTML element.

## Extendable components

Styled-components helps you to organize your styles in an inheritance. Let's assume that you need different kinds of buttons in your application. But you are sure about one thing; every button should have the same `border-radius` value.

```js
const BaseButton = styled.button`
  border-radius: 5px;
`;

const RedButton = styled(Button)`
  background-color: red;
`;

const GrayButton = styled(Button)`
  background-color: gray;
`;
```

## Smart Styles

Adding some logical expression to your styles is easier. You don't need to manage classes in your code.

```js
const Text = styled.div`
  color: ${(props) => (props.primary ? "white" : "palevioletred")};
`;

<Text primary></Text>;
```

## Does styled-components developer-friendly?

Yes! It is the best CSS writing solution that I have ever enjoyed in my career. Creating styled elements is so easy. The code is more readable. I would love to write more CSS since I met with styled-components. I don't think we will not see any other tool or library in the near future that will be close to styled-components.

## Does styled-components user-friendly?

No! It is not a good tool. Here, I mean the end-user by using user-friendly. Let's talk about why it is not.

You need to understand that CSS-in-JS is a buzzword. It is not a CSS. They are pure JS functions and string variables. We are passing string variables to the JS functions. It means that all of your CSS has to be built in JS execution time. By default, there is no `pre-built` way for styled-components. It should be generated in the execution time.

It causes slow rendering. You need to use the client's CPU to build those styles over and over again.

If you use SSR with Next.js, you can render the styles on the server. But it will have some cost. All styles should be rendered at runtime due to it is a pure JS code. In the classical CSS writing techniques, styles would be ready to serve the clients even though SASS, because we can build before to serve them.

## Is there any prebuild CSS-in-JS solution?

Yes! There is an option that you can use as a zero-runtime CSS-in-JS library; [linaria](https://github.com/callstack/linaria). But it is not mature as styled-components. styled-components has more stars. Also, styled-components have many many more users than **lineria**.

## Will styled-components add zero-runtime support in the future?

I couldn't find any plan for it. But I think they will add it at some point. Generating styles on runtime is an important drawback. The competitors of styled-components are focusing on this feature.

## Should I use styled-components?

After many years I can clearly see one thing; every advantage is another disadvantage.

It is very developer-friendly. It wouldn't steal the time of the developer. But it doesn't mean that it is also user-friendly. styled-components is a decision about where you would waste the time. You can use CSS modules, SASS, or pure CSS files. But the rendering time on the client side would be better. As a developer, you can gain more time, increase your productivity, and create beautiful screens quickly by using styled-components. But browsers would need more time to render styles.

You can decide to use zero-runtime CSS-in-JS solutions. I believe that that kind of solution would be the future even though I think they are not mature enough yet.

So, you should decide about the application's priorities.
