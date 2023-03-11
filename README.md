# Lint Configuration Guide

## 1. ESLint initialization: [eslint.org]("https://www.eslint.org/")

    npm init @eslint/config

Go through the basic setup.

## 2. Configuring `.eslintrc.json`

- To initialize jest in eslint, add `"jest": true` under the `env` object in `.eslintrc.json`. An example is given below:

```json
{
"env": {
    "browser": true,
    "es2021": true,
    "jest": true    <---
}
```

- Also, include `"project": "./tsconfig.json"` under `"parserOptions"` to ensure proper typescript transpiling.

```json
{
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module",
    "project": "./tsconfig.json"  <---
  },
```

- Finally, specify the target react version

```json
"settings": {
    "react": {
      "version": "detect"
    }
  }
```

## 3. Enable eslint linting in `package.json`

```json
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "lint": "eslint .",             <---
    "lint:fix": "eslint --fix ."    <---
  },
```

Here, the `.` refers to the `.eslintrc.json` file in the root directory.

## 4. Install and enable eslint-plugin-react-hooks

```
npm install eslint-plugin-react-hooks --save-dev
```

Now, go to `eslintrc.json` and add `react-hooks` to `plugins`:

```json
"plugins": ["react", "react-hooks", "@typescript-eslint"],
```

We also need to add some rules in `eslintrc.josn`

Here are some of my custom rules

```json
  "rules": {
    "indent": ["error", "tab"],
    "linebreak-style": ["error", "unix"],
    "quotes": ["error", "double"],
    "semi": ["error", "always"],
    "react/react-in-jsx-scope": "off",
    "react-hooks/rules-of-hooks": "error", // Checks rules of Hooks
    "react-hooks/exhaustive-deps": "warn" // Checks effect dependencies
  },
```


## 5. Prettier integration with eslint

Install required packages:

    npm i prettier eslint-config-prettier eslint-plugin-prettier --save-dev

Adding prettier to `.eslintrc.json`

```json
	"extends": [
		"eslint:recommended",
		"plugin:react/recommended",
		"plugin:@typescript-eslint/recommended",
		"plugin:prettier/recommended"        <---
	],
```

Also, add prettier to the last of the plugins list to give it highest priority

```json
"plugins": ["react", "react-hooks", "@typescript-eslint", "prettier"],
```

Now, create `.prettierrc` file in the root directory of your project and add the following

```json
{
	"tabWidth": 2,
	"useTabs": true,
	"semi": true,
	"singleQuote": false,
	"trailingComma": "all",
	"printWidth": 80,
	"bracketSpacing": true
}
```

Install `Prettier` in `VSCode extensions`. To apply the prettier rules on save, go to `settings` and enable the following:

    Default Formatter: Prettier
    Format on Save: Ticked


Now we can check for errors using the lint script.

    npm run lint

To fix the issues

    npm run lint:fix



## 6. Visualizing eslint configuration

- Download and install the `Lintel` extension for vscode.

- Now, just hit `Ctrl + Shift + P` and run `Lintel` to view Lintel for current project.

## 7. Husky - enable pre-commit [typicode.github.io/husky]("https://typicode.github.io/husky")

Installation

    npx husky-init && npm install

This will create `.husky` on the root and in there, the `pre-commit` file will have all the pre-commit commands that will be executed before git commit. We can manually add/edit the pre-commit commands in that file.

Alternatively, we can use the `npx husky add .husky/pre-commit "CUSTOM_COMMAND"` to `add` commands OR `npx husky set .husky/pre-commit "CUSTOM_COMMAND"` to `set` through the `terminal`. `add` will append commands to already available commands whereas `set` will discard previous commands and only include the new commands.

For our case, we will `set` the linting command

    npx husky set .husky/pre-commit "npm run lint"


## 8. Tailwind Fix
If you are using tailwind, you'll get an error related to `tailwind.config.js`. To fix this, create a file `.eslintignore` and add `tailwind.config.js` to ignore list.

## 9. Typescript Type Declarations for images, css files, ...
Create a `custom.d.ts` file in the `/src` directory and add all the required type declarations. Here is an example:

```
declare module "*.png";
declare module "*.svg";
declare module "*.css";

```


## 10. VSCode ESLint in subdirectory
By default, it'll throw an error like: <span style="color:#db4f4a">Parsing error: Cannot read file ......./tsconfig.json</span>

Just add this to settings file. Here, Eslint will assume `./client` as root dir.
```javascript
"eslint.workingDirectories": [
    "./client"
]
```


## 11. Node Express Backend 
`.eslintrc` file for **express backend**
```json
{

    "env": {
        "node": true,
        "es2021": true
    },
    "extends": ["eslint:recommended", "plugin:prettier/recommended"],
    "overrides": [],
    "parserOptions": {
        "ecmaVersion": "latest",
        "sourceType": "module"
    },
    "plugins": ["prettier"],
    "rules": {
        "indent": ["error", "tab"],
        "linebreak-style": ["error", "unix"],
        "quotes": ["error", "double"],
        "semi": ["error", "always"]
    }
}
```


#guides #react #typescript #eslint #prettier #husky #lint
