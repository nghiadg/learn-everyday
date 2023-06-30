`30/06/2023`

<details>
  <summary>Setup config SSH GitHub to use another git account the same machine Window 10</summary>
  
  `Step 01: Generate a key with ed25519` 
  
  - Access ./ssh directory and run command `ssh-keygen -t ed25519 -C "<email>". It will show a question this mean the file name you want to generate.`
    
  `Step 02: Open SSH Setting GitHub: Settings -> SSH and GPG keys -> Click button New SSH key`
  
  - Copy content of file `.pub` generated at `./ssh` directory and paste in `Key` form then click button `Add SSH key`
    
  `Step 03: Config git for new SSH key`
  
  - Create a new file with name `config`. Edit this file with template like that
  ```
  Host <username> github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/<name of file key generated>
  ```

  `Step 04: Config user.name and user.email if you want to special profile for this project when push commit to GitHub`

  - Run command `git config user.name <username>` and `git config user.email "<email>"` to config special username and email push to GitHub repository. You have to run `git init` before.

    References
    - https://www.youtube.com/watch?v=vSeYsk4WYvg
    
</details>

`28/06/2023`

<details>
  <summary>window.opener is magic. I can call fuction of window from other window</summary>

  window.opener return a reference to the window that opened the window, either with `open()`, or by navigating a link with a `target` attribute.

  In other words, if window `A` open window `B`, `B.opener` return `A`.

  If you want to call a function at window `A` from window `B`. you can defined this function at window `A` and use `window.opener.<this function>()` at window `B` to call it. Let see example:

  Defined `customFunc` at window `A`

  ```
  const customFunc = () => {
    // Logic code here
  };

  window.customFunc = customFunc;
  ```

  Trigger call it from window `B`

  ```
  window.opener.customFunc();
  ```
</details>

`27/06/2023`

<details>
  <summary>To review code. Let overview all files changed, after that, review related logic file changes.</summary>
</details>

<details>
  <summary>When using fetch to call API. Fetch promise does not reject on HTTP errors (404, ...etc). Instead, a then handler must check Response.ok and/or Response.status.</summary>

  References:

  - https://developer.mozilla.org/en-US/docs/Web/API/fetch
</details>

`25/06/2023`

<details>
  <summary>Bach Mai Hospital don't allow guests use motorbike enter by main gate. Instead of, enter by gate 3 or 4 in Phương Mai street.</summary>
</details>

`24/06/2023`
<details>
  <summary>The Javascript `Number` is a `double precision 64 bits format IEEE 754`. I will explain why 0.2 + 0.1 not equal 0.3</summary>

  ## Why 0.2 + 0.1 not equal 0.3 in Javascript?
  ### What is IEEE 754?
  #### What is `sign` bit?
 `sign bit` is the first bit of the binary representation. `sign` bit define the number is positive or negative. If `0` the number is positve and `1` is negative. Default is IEEE 754 64 bits is `0`.
  `sign` have `1 bit`.
  
  <em>Read more: https://c-for-dummies.com/blog/?p=4685</em>
  #### What is `exponent` bits?
  The number in scientific notation is $Number = (-1)^{sign}.mantissa * 2^{exponent - bias}$ in binary
  
  Understand simply `Exponent bits` to define where is `.` position at `mantissa`. Let explain it.

  Look at the number $1.23 * 10^{-3}$ in decimal. It equal `0.00123`. In this example. 1.23 is `mantissa` and -3 is `exponent`. The same is true with binary number.

  Let try it out with convert `12.5` to binary and format it to `IEEE 754 32 bits`. `bias` is `127`

 ![IEEE 754 32 bits](https://raw.githubusercontent.com/nghiadg/Today-I-Learnt/e64a57559eeac2b1bccfcb7bab459298ac97243b/IEEE%20754%2032bits.svg)

 IEEE 754 32 bits:

 `1` bit for `sign`

 `8` bits for `exponent`

 `23` bits for `mantissa` or called `fraction`

 The `fraction` or `mantissa` part is converted into decimal by summing all $bit value * 2^{-n}$ with `n` is index of bit.

 For example: convert `12.25` to binary IEEE 754 32 bits

 Convert `12` to binary:

 | 12/2 	| 6 	| 0 	|
|------	|---	|---	|
| 6/2  	| 3 	| 0 	|
| 3/2  	| 1 	| 1 	|
| 1/2  	| 0 	| 1 	|

 So `12` to binary is `1100`

 Convert `0.25` to binary:

 | 0.25*2 	| 0.5 	|
|--------	|-----	|
| 0.5*2  	| 1   	|

So `0.25` to binary is `01`

`12.25` to binary is `1100.01`

Let format to IEEE 754 32bits

`1100.1` to scientific notation $1.1001 * 2^3$ -> $(-1)^0.1001 * 2^3$ -> So `Exponent` is `130`, `Mantissa` is `1001` and `sign` is `0`.

`1` bit for `sign` -> Because `12.25` is positive number so `sign` is `0`.

`8` bits for `exponent` -> Exponent is `3` -> `10000010`

`23` bits for `mantissa` -> `10001000000000000000000`

`12.25` to IEEE 754 32 bits is `0 10000010 10001000000000000000000`. The true value is stored is `1 * 2^3 * (1 + 2^{-1} + 2^{-5}) = 1 * 2^3 * 1.53125 = 12.25`

  #### `0.1` to IEEE 754 64 bits in JavaScript
  
  `0.1` to binary:

  | 0.1*2 	| 0.2 	| 0 	|
|-------	|-----	|---	|
| 0.2*2 	| 0.4 	| 0 	|
| 0.4*2 	| 0.8 	| 0 	|
| 0.8*2 	| 1.6 	| 1 	|
| 0.6*2 	| 1.2 	| 1 	|
| 0.2*2 	| 0.4 	| 0 	|

So `0.1` to binary is `0.00011001100110011...`

To format to IEEE 64 bits:
`0.00011001100110011...` -> $1.1001100110011... * 2^-4$

`1` bit for `sign` -> `0`

`11` bits for `exponent` with `bias` is `1023` -> `01111111011`

`52` bits for `mantissa` -> `1001100110011001100110011001100110011001100110011001`

So `0.1` to IEEE 754 64 bits is `0 01111111011 10011001100110011001100110011001100110011001100110011`

The actual value is stored is $1 * 2^-4 * (1 + 2^{-1} + 2^{-4} + 2^{-5} + 2^{-8} + 2^{-9} + 2^{-12} + 2^{-13} + 2^{-16} + 2^{-17} + 2^{-20} + 2^{-21} + 2^{-24} + 2^{-25} + 2^{-28} + 2^{-29} + 2^{-32} + 2^{-33} + 2^{-36} + 2^{-37} + 2^{-40} + 2^{-41} + 2^{-44} + 2^{-45} + 2^{-48} + 2^{-49} + 2^{-52}) = 1 * 2^{-4} * 1.5999999999999998667732370449812151491641998291015625 = 0.09999999999999999167332731531132594682276248931884765625$

So `0.1` is stored < `0.1` 


  References:

  - https://mathcenter.oxford.emory.edu/site/cs170/ieee754/#:~:text=The%20sum%20of%20the%20bias,power%20%3D%20exponent%2Dbias

</details>

`23/06/2023`
<details>
  <summary>To print a PDF a solution is: Frontend call API -> Backend will get data, file template html -> Another Server run browser with SolidJS (or another framework,..etc to create a web page) to render content with this data and template.</summary>

- Use headless browser such as `puppeteer` to access this web page to render file PDF from page in Printer Server.
- Send this file for Main Server and return this resource for user.

References: https://medium.com/compass-true-north/go-service-to-convert-web-pages-to-pdf-using-headless-chrome-5fd9ffbae1af
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
