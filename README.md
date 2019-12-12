## Create Your Own React Component as Npm Package

### Setting up environment
#### 1. Create empty folder, Name it as you wish.
#### 2. Run `npm init`. Just keep pressing ENTER until it disappears. 
#### 3. Now install dependencies:
`npm install --save react webpack `
#### 4. Now install dev dependencies
`npm install -D babel-cli babel-core babel-loader babel-plugin-transform-object-rest-spread babel-plugin-transform-react-jsx babel-preset-env webpack-cli`
#### 5. Change `package.json` a little. We need to:
- Provide build and start script
- Set entry point to build/index.js
```json
{
"name": "npm_package_dolt",
  "version": "1.0.0",
  "description": "Publish Your Own React Component as Npm Package",
  "main": "build/index.js",
  "scripts": {
    "start": "webpack --watch",
    "build": "webpack"
  },
}
```
#### 6. Configure webpack to transpile our code
Create 2 file:

`webpack.config.js`
```typescript
var path = require('path');
module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'index.js',
    libraryTarget: 'commonjs2'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        include: path.resolve(__dirname, 'src'),
        exclude: /(node_modules|bower_components|build)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['env']
          }
        }
      }
    ]
  },
  externals: {
    'react': 'commonjs react' 
  }
};
```
`.babelrc`
```typescript
{
    "presets": ["env"],
    "plugins": [
        "transform-object-rest-spread",
        "transform-react-jsx"
    ]
}
```
#### 7. Create src directory and inside it create file index.js
This will be entry point for the webpack. Hence our component will be located in this file.
In this example we’re gonna write very simple component.
`index.js`
```typescript
import React from "react";

const ReactColorSquare = props => {
  const { width, height, color, text } = props;
  return (
    <div
      style={{
        width: width || 100,
        height: height || 100,
        backgroundColor: color || "blue"
      }}
    >
      {text}
    </div>
  );
};
export default ReactColorSquare;
```

#### 8. Build package
run `npm run build`

### Use package in other project but only on our computer
run `npm link <package_name>`
<package_name> is full patch of project package.

- Ex: `npm link /Users/letrungdo/React_Projects/npm_package`

### - Publishing the package to npmjs.com
Create account at https://www.npmjs.com/signup
Then inside component project directory run `npm login` and sign in with your credentials. 
After that run `npm publish`.

## * Publishing the package to Github repo
Github package can use as private package for team.

- Step 1: Change `package.json` a little:

Add `publishConfig`

Edit `name`: @github_username/repo
  
```json
{
  "name": "@letrungdo/react-create-npm-package",
  "version": "1.0.0",
  "description": "Publish Your Own React Component as Npm Package",
  "main": "build/index.js",
  "scripts": {
    "start": "webpack --watch",
    "build": "webpack"
  },
  "publishConfig": { "registry": "https://npm.pkg.github.com/" },
}
```
- Step 2: Authenticate

`npm login --registry=https://npm.pkg.github.com/`

Note: Password is Github `Personal access tokens`

- Step 3: Publish

```npm publish```

=> Result: https://github.com/letrungdo/react-create-npm-package/packages/79168

### Ref:
- https://medium.com/quick-code/publish-your-own-react-component-as-npm-package-under-5-minutes-8a47f0cb92b9
- https://docs.npmjs.com/cli/link.html
- https://docs.npmjs.com/misc/developers.html
- https://help.github.com/en/github/managing-packages-with-github-packages/configuring-npm-for-use-with-github-packages
- https://medium.com/better-programming/build-your-very-own-react-component-library-and-publish-it-to-github-package-registry-192a688a51fd
