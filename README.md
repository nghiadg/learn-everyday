`23/06/2023`
<details>
  <summary>To print a PDF a solution is: Frontend call API -> Backend will get data, file template html -> Another Server run browser with SolidJS (or another framework,..etc to create a web page) to render content with this data and template.</summary>

- Use headless browser such as `puppeteer` to access this web page to render file PDF from page in Printer Server.
- Send this file for Main Server and return this resource for user.
</details>

<details>
  <summary>I'm always thought someone else's flow code previous is good. So I try convert it and without believe in my code. `Solution`: Believe my code, if resolve problem, it's well.</summary>
</details>

<details>
  <summary>Remmember domain bussiness is really hard with me.</summary>

  - I'm focusing code logic and skip domain bussiness. `Solution`: Understand domain bussiness and temporarily ignore logic code. Back to think logic code after.
</details>

`22/06/2023`
<details>
  <summary>Use `Ctr + Shift + c` in Window to open new terminal outsite VS Code</summary>

  - Run project in terminal outsite to avoid VS Code crash -> project break down.
</details>

<details>
  <summary>One-Way binding and Two-Way binding in React (Updating...)</summary>
  
</details>
<details>
  <summary>Use [nvm-windows](https://github.com/coreybutler/nvm-windows) to install Node Version for Window 10</summary>

  - `nvm install latest` to install latest node version.
  - `nvm install <version>` to install specified version.
  - `nvm use <version>` switch to use the specified version.
</details>

<details>
  <summary>When use Object.assign in Typescript. Typescript parsed type for object</summary>

  ```
  import * as React from 'react';

  // React.ForwardRefExoticComponent<React.RefAttributes<unknown>>
  export const Modal = React.forwardRef(() => { 
    return <div>Modal here</div>;
  });

  // () => React.JSX.Element
  const ModalTitle = () => {
    return <span>Modal title here</span>;
  };

  // React.ForwardRefExoticComponent<React.RefAttributes<unknown>> & {
  //  Title: () => React.JSX.Element;
  // }
  export const AppModal = Object.assign(Modal, {
    Title: ModalTitle,
  });
  ```
</details>

<details>
  <summary>`Modal.Title` is called `Compound Component`</summary>

  ```
  <Modal>
    <Modal.Title>Title here</Modal.Title>
  </Modal>
  ```
</details>

`21/06/2023`
<details>
  <summary>I can use `details` tag to collapse contents in markdown</summary>
  
  ## Rules
  1. Have an empty line after `summary` tag or markdown/code blocks will not render.
</details>

<details>
  <summary>Use AbortController to abort fetch or asynchronous tasks</summary>
  
  #### Create a controller:
  `const controller = new AbortController()`
  
  A controller have a single method `abort()` and a single property `signal` to set event listeners on it.
  
  When `abort()` is called
  
  1. `controller.signal` emits the `abort` event.
  2. `controller.signal.aborted` property becomes `true`
  
  To be able to cancel `fetch`, pass the `signal` property of an `AbortController` as a `fetch` option
  
  ```
const controller = new AbortController();
  fetch(url, {
    signal: controller.signal
  });
```
  
  The `fetch` knows how to work with `AbortController`. It will listen to abort events on signal. 
  And to abort `fetch`, call `controller.abort()`
  
  `fetch` get event from `signal` and abort the request.
  When `fetch` is aborted,  its promise rejects with an error `AbortError`, so we should handle it, e.g in `try catch`
  
  More details, visit post at https://javascript.info/fetch-abort
  
</details>
