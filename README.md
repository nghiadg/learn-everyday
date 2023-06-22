`22/06/2023`
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
