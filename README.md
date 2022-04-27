# NPM Project Quickstart Guide

This guide follows a quickstart tutorial on how to setup and configure a NPM project, including:
- convenience NPM / YARN commands to deal with common and frequent development lifecycle workflows
- TypeScript (TS)
- Webpack including default plugins and a configuration file (`webpack.config.js`)
- Babel including presets for environment and typescript

## Initializing project source

### Project root
```bash
mkdir <app> && cd <app>
```

## Initializing NPM project

### Generation of `package.json`
```bash
npm init -y
```

### Customization of `package.json`
After initial generation of the package configuration file, adjust should be adjusted to the specific requirements of
to the project requirements. For convenience, it is recommended to add commands to deal with the development lifecycle
of the project:

(Replace in `package.json`)
```json
"scripts": {
  "build": "webpack",
  "start": "webpack serve",
  "clean": "rm -rf build/*"
},
```

## Initializing project source (`/src`)
```bash
mkdir src/ && cd src/
touch index.html
touch index.ts
touch App.ts
```

### `/src/index.html`
```html
<!doctype html>
<html lang='en'>
  <head>
    <meta charset='UTF-8' />
    <meta name='viewport' content='width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0' />
    <title>APP</title>
  </head>
  <body>
    <noscript>JavaScript is required to execute this app</noscript>
    <div id='root'></div>
  </body>
</html>
```

## Webpack

### Installation
```bash
npm install -D webpack webpack-cli webpack-dev-server
npm install -D path html-webpack-plugin style-loader css-loader sass-loader node-sass file-loader @svgr/webpack
```

### `webpack.config.js`
```bash
touch webpack.config.js && nano webpack.config.js
```

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: path.join(__dirname, 'src', 'index.tsx'),
  output: {
    path: path.join(__dirname, 'build'),
    filename: 'index.bundle.js',
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js'],
  },
  devServer: {
    static: {
      directory: path.join(__dirname, 'public'),
    },
    compress: true,
    port: 9000,
  },
  mode: process.env.NODE_ENV || 'development',
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: ['babel-loader'],
      },
      {
        test: /\.(ts|tsx)$/,
        exclude: /node_modules/,
        use: ['ts-loader'],
      },
      {
        test: /\.(css|scss)$/,
        use: ['style-loader', 'css-loader', 'sass-loader'],
      },
      {
        test: /\.(jpg|jpeg|png|gif|mp3)$/,
        use: ['file-loader'],
      },
      {
        test: /\.svg$/,
        use: ['@svgr/webpack'],
      }
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, 'src', 'index.html'),
    }),
  ],
};
```

## Babel

### Installation
```bash
npm install -D @babel/core @babel/node @babel/preset-env @babel/preset-typescript babel-loader
```

### `.babelrc`
```bash
touch .babelrc && nano .babelrc
```

```json
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-typescript"
  ]
}
```

## TypeScript

### Installation
```bash
npm install typescript
npm install -D ts-loader
```

### `tsconfig.json`
```bash
touch tsconfig.json && nano tsconfig.json
```

```bash
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "sourceMap": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": false,
    "jsx": "react-jsx"
  },
  "include": [
    "src"
  ]
}
```

## Project structure
- The project is built under `<app>/build/`. Run `npm run clean` or `yarn clean` to clean up the build.
- Run `npm run build` or `yarn build` to build to application to `<app>/build/`. Webpack determines the build mode
  (development / production) depending on the env variable `NODE_ENV`, otherwise it will assume that webpack runs in
  development mode. For further information, see `webpack.config.js`.
- Run `npm run start` or `yarn start` to serve webpack in dev mode. The webpack dev server will launch at
  `localhost:9000`.