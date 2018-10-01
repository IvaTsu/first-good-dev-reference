# React, TypeScript, Storybook config

1.  Create a needed folder
    `mkdir folder_name`
2.  Inside that folder run `yarn init -y`. This creates a default `package.json` file.
3.  Let's start with adding dependencies:
    - `yarn add -D @storybook/react babel-core` - it adds _@storybook/react_ and _babel-core_ to your _dev dependencies_.
    - `yarn add react react-dom` - it adds your _core dependencies_, e.g. _react_ and _react-dom_.
4.  Create 2 additional folders `.storybook` and `src` by running `mkdir .storybook src` command.
5.  Create an empty config file for the _storybook_: `touch .storybook/config.js`. This file is a **required** one for storybook to run.
6.  Add script for further convenient use of storybook in the futute.
    - Go to the `package.json` file.
    - Add "scripts" there in the following way:
    ```
    "scripts": {
        "storybook": "start-storybook -p 6006 -c .storybook"
    }
    ```
    - Here `-p` flag means port you want to run server on, `-c` flag means folder that contains config file.
    - Now wen you run command `yarn run storybook`, it starts a server at localhost:6006 and you can navigate there to see the results.
7.  Add first _React Story_ to _Storybook_:

    - Go to the `.storybook/config.js` and import _configure_ method from _storybook_.

    ```
    import { configure } from '@storybook/react';
    ```

    - Define utility function that will look through everything in `./src` folder and use a RegExp to find files that will ends with `.stories.js`

    ```
    const req = require.context("../src/", true, /.stories.js$/);
    ```

    - Define a function `loadStories` in the following way:

    ```
    function loadStories() {
        req.keys().forEach(file => req(file));
    }
    ```

    - for the every file that is matched, it is needed to require the file to build the story.
    - Configure current function with the current module:

    ```
    configure(loadStories, module);
    ```

    - Create an example component: - Create `Button.js` file inside `src` folder: `touch ./src/Button.js`. - Create a basic component setup:

      ```
      import React from 'react';

      export default ({ bg, title }) => (
        <button style={{ backgroundColor: bg }}>{ title }</button>
      );
      ```

    - Create a story file for the component:

      - Create file `Button.stories.js` right near with `Button.js`.

      ```
      import React from "react";
      import { storiesOf } from "@storybook/react";
      import Button from "./Button";

      storiesOf("Button", module).add("with background", () => (
          <Button bg="red" title="Button" />
      ));
      ```
