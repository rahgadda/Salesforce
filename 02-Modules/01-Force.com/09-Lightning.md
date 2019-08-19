## Lightning Web Components

### Overview

- In this section we will go through one of the UI development model highlighted below of Salesforce.com.
  - Visual Force
  - Lightning Components: We can build build Lightning components using two programming models as given below. Both these models can coexist and interoperable on a Salesforce page.
    - Aura Components:
      - As of March 2019, this component development is marked as read-only as Salesforce has archived.
      - The reason for this is, aura component does not follows W3C standards for developing components. Its has Salesforce proprietary recipes to create Web Components.
      - This require resources to know Salesforce specific development model. This makes harder to get resources in the market.
      - Aura components can contain Lightning web components (though not vice-versa).
    - **Lightning Web Components**
      - These components are also referred as **LWC**.
      - These are based on W3C specifications with minimal Salesforce proprietary recipes like project layout, web components, directives, decorators etc..
      - Salesforce has also opensourced [LWC](https://lwc.dev). This has everything except Salesforce proprietary web components.
      - You can view the library and custom components through your org’s instance, too, at `http://<instance>.lightning.force.com/docs/component-library`.

#### Web Components

- Web Components are used define Custom Elements that allow developers to implement reusable components with only HTML, CSS and JavaScript.
- Advantages of Web Components
  - WebComponents are based on below:
    - **HTML Templates:**
      - These are used to hold HTML tags without displaying.
      - The content can be made visible and rendered later by using JavaScript.
      - The **\<template\>** and **\<slot\>** elements enable developers to write markup templates that are not displayed in the rendered page.
    - **Custom Elements:**
      - Allows developers to create HTML native developer specific elements.
      - Developer can perform all native activities on these elements like attributes, properties and methods.
    - **Shadow DOM:**
      - This is different from Virtual DOM.
      - Address DOM encapsulation problem by allowing custom elements to have its on DOM Subtree.
      - These are rendered separately from parent DOM to prevent collision of script and styling.
  - These components are framework independent and can be used in any framework like React, Angular, Oracle JET etc..
  - Cross functional teams can create a UI components based on companies style guide.

#### Lightning Web Components Opensource

- To understand LWC, lets start working with bare bones LWC Opensource.
- To work with LWC, we require below:

  - Node.js (LTS 10.15 version is used in this demo)
  - Navigate to your preferred folder and run below commands

    ```script
    npm i -g yarn
    npm i -g lwc-create-app
    cd D:\01-MyDev\01-Salesforce\01-LWC\00-Demo
    lwc-create-app my-app
    cd my-app
    yarn watch

    ```

    ![](../../01-Images/15-LWCOpenSourceInstall.png)

  - Open browser http://localhost:3001, this will display below Salesforce hello scaffolding app.
    ![](../../01-Images/16-LWCHello.png)

- Building blocks of LWC

  - Creating HelloWorld WebComponent

    - Navigate to `scr/modules` and create a new folder `learn/hello`
    - Create new files `hello.hmtl` and `hello.js` as below

      ```html
      <template>
        Hello, {name}
      </template>
      ```

      ```javascript
      import { LightningElement, api } from "lwc";

      export default class hello extends LightningElement {
        @api name = "World!";
      }
      ```

    - Update `src/index.html` and `src/index.js` with below

      ```html
      <!DOCTYPE html>
      <html lang="en">
        <head>
          <meta charset="utf-8" />
          <title>Hello World</title>
          <meta name="viewport" content="width=device-width,initial-scale=1" />
          <link rel="shortcut icon" href="/resources/favicon.ico" />
        </head>
        <body>
          <learn-hello></learn-hello>
        </body>
      </html>
      ```

      ```javascript
      import { buildCustomElementConstructor } from "lwc";
      import LearnHello from "learn/hello";

      customElements.define(
        "learn-hello",
        buildCustomElementConstructor(LearnHello)
      );
      ```

    - Delete folder `scr/modules/my`

  - Data Binding

    - To bind a property in a component’s template to a property in JavaScript class, in the template, surround the property with curly braces: `{name}`
    - The property in `{ }` must be a valid JavaScript identifier or member expression.
    - In HTML page, Don’t add spaces around the property, for example, `{ name }` is not valid HTML.
    - It's illegal to compute a value in an expression, like `data[2].name['John']`.

  - HelloWorld WebComponent To Take User Input

    - Update files `hello.hmtl` and `hello.js` as below

      ```
      <template>
          Hello, {name} <br>
          Name: <input type="text" value={name} oninput={handleInput} />
      </template>
      ```

      ```javascript
      import { LightningElement, track } from "lwc";

      export default class hello extends LightningElement {
        @track name = "World!";

        handleInput(event) {
          let name = event.target.value;
          this.name = name;
        }
      }
      ```

  - Decorator

    - @api

      - Add reactive nature to a property. Any change to the variable in HTML/JS will automatically update other part. It will re-render the HTML component on change.
      - This decorator makes the `name` property public so that other components can set it.
      - Removing this decorator makes `name` property as private variable and not reactive.

    - @track

      - Add reactive nature to a property. Any change to the variable in HTML/JS will automatically update other part. It will re-render the HTML component on change.
      - This decorator makes the `name` property private so that other components cannot set it.
      - Removing this decorator makes `name` property not reactive.

  - Composition

    - It enables you to build complex components by adding components within the body of another component.
    - Composite components will have **owner**(that owns the template) and **container**(other components within the owner component).
      | Owner | Container |
      |:------|:----------|
      | The owner controls all the composed components that it contains | A container is less powerful than the owner|
      | Can set public properties on owner and container | Can set public properties only on container. Can read public properties from owner|
      | Call public methods on owner and container | Call public methods on owner an container|
      | Listen for events dispatched by owner and container | Listen for some events from owner and all from container|

### References

- [Sample Gallery](https://trailhead.salesforce.com/sample-gallery)
- [Design System](https://lightningdesignsystem.com/)
