# React + TypeScript + Storybook Configuration and Setup

> A guide that will help you to setup and run React + TypeScript + Storybook configuration.

I will take a `React + Storybook` config described above as a reference for this one.

- Configure TypeScript with Storybook

  - Create 2 new files: `webpack.config.js` and `tsconfig.json`

  ```sh
  touch .storybook/webpack.config.js tsconfig.json
  ```

  - Add `TypeScript` `dev` dependencies: the types definition for react, typescript, and the typescript loader.

  ```sh
  yarn add -D @types/react typescript awesome-typescript-loader
  ```

  - Setup a usual `TS` config in `tsconfig.json`:

  ```json
  {
    "compilerOptions": {
      "outDir": "build/lib",
      "module": "commonjs",
      "target": "es5",
      "lib": ["es5", "es6", "es7", "es2017", "dom"],
      "sourceMap": true,
      "allowJs": false,
      "jsx": "react",
      "moduleResolution": "node",
      "rootDir": "src",
      "baseUrl": "src",
      "forceConsistentCasingInFileNames": true,
      "noImplicitReturns": true,
      "noImplicitThis": true,
      "noImplicitAny": true,
      "strictNullChecks": true,
      "suppressImplicitAnyIndexErrors": true,
      "noUnusedLocals": true,
      "declaration": true,
      "allowSyntheticDefaultImports": true,
      "experimentalDecorators": true
    },
    "include": ["src/**/*"],
    "exclude": ["node_modules", "build", "scripts"]
  }
  ```

  - Heads up to `webpack.config.js`. For ts and tsx files, we're going to require that it use TypeScript loader and also make it recognize ts and tsx files

  ```javascript
  const path = require("path");

  module.exports = (baseConfig, env, defaultConfig) => {
    // config
    defaultConfig.module.rules.push({
      test: /\.(ts|tsx)$/,
      loader: require.resolve("awesome-typescript-loader")
    });
    defaultConfig.resolve.extensions.push(".ts", ".tsx");
    return defaultConfig;
  };
  ```

  - Edit Button component:

  ```
  import * as React from "react";
  import "./Button.css";

  export interface Props {
  children: React.ReactNode;
  onClick: () => void;
  disabled?: boolean;
  }

  const noop = () => {};

  export const Button = (props: Props) => {
  const { children, onClick, disabled = false } = props;
  const disabledclass = disabled ? "Button_disabled" : "";
  return (
      <div
      className={`Button ${disabledclass}`}
      onClick={!disabled ? onClick : noop}
      >
      <span>{children}</span>
      </div>
  );
  };
  ```

      - Add `Button.css` file:

      ```css
      .Button span {

          margin: auto;
          font-size: 16px;
          font-weight: bold;
          text-align: center;
          color: #fff;
          text-transform: uppercase;
          }
          .Button {
          padding: 0px 20px;
          height: 49px;
          border-radius: 2px;
          border: 2px solid var(--ui-bkgd, #3d5567);
          display: inline-flex;
          background-color: var(--ui-bkgd, #3d5567);
          }

          .Button:hover:not(.Button_disabled) {
          cursor: pointer;
          }

          .Button_disabled {
          --ui-bkgd: rgba(61, 85, 103, 0.3);
      }
      ```

  - Edit `Button.story.js` file:

  ```javascript
  import React from "react";
  import { wInfo } from "./../.storybook/utils";

  import { storiesOf } from "@storybook/react";
  import { Button } from "./Button";
  import { text, boolean } from "@storybook/addon-knobs/react";

  storiesOf("Button", module)
    .addWithJSX(
      "with background",
      wInfo(`
      description
      
      ~~~js
      <Button>slkdjslkdj</Button>
      ~~~
      `)(() => <Button bg="palegoldenrod">Hello world</Button>)
    )
    .addWithJSX("with background 2", () => (
      <Button disabled={boolean("disabled", false)}>
        {text("text", "Hello world")}
      </Button>
    ));
  ```

## References:

- [Storybook Docs](https://storybook.js.org/basics/guide-react/)
