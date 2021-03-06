# ๐ป ๋ฆฌ์กํธ ์ปค์คํ ์ํ ๋ณด์ผ๋ฌ ํ๋ ์ดํธ
### React18 + TypeScript + Webpack5 + Babel + Eslint + prettier ํธํ ํ๊ฒฝ ์ํ ๋ณด์ผ๋ฌ ํ๋ ์ดํธ

<br />

## ๐ ๊ธฐ๋ณธ์ ์ธ Webpack, Babel, ESLint ์ฐธ๊ณ  ์ ์ฅ์
- [ํ๋ก ํธ์๋ ๊ฐ๋ฐํ๊ฒฝ ์ดํด](https://github.com/ssi02014/front_development_environment)

<br />

## ๐ ๊ทธ์ธ ์ฐธ๊ณ  ์ฌํญ 1. ๋น๋ ํ์ผ ์น์๋ฒ ์คํ
### serve
```
yarn add -D serve
```
```json
{
  "scripts": {
    "start:build": "serve -s build -l 8080",
  }
}
```
- `serve`๋ ํ๋ก ํธ ๊ฐ๋ฐ์ ํ๋ค๋ณด๋ฉด ๊ฐ๋จํ๊ฒ ์น์๋ฒ ์์ ์คํ์ํฌ ๋ ์ฌ์ฉํ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ด๋ค.
- webpack-dev-server๋ฅผ ํตํด ๊ฐ๋ฐ ์๋ฒ๋ ํ์ธํ  ์ ์์ง๋ง ๋น๋๋ ํ์ผ์ ์คํ์ํฌ ๋๋ ์ฌ์ฉํ  ์ ์๊ธฐ์ ๋ณ๋์ serve ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ค์นํ๋ค.
- package.json์๋ค ๋ค์๊ณผ ๊ฐ์ ์์ฑ์ ์ฃผ๋ฉด ๋น๋๋ ํ์ผ์ ์น์๋ฒ๋ก ์คํ์ํฌ ์ ์๋ค.

<br />

## ๐ ๊ทธ์ธ ์ฐธ๊ณ  ์ฌํญ 2. webpack
###  webpack-merge
- [Wepback ๊ณต์ ์ฌ์ดํธ - Webpack Merge](https://joshua1988.github.io/webpack-guide/advanced/webpack-merge.html)
- webpack merge๋ ๋จ์ด ๊ทธ๋๋ก ์ฌ๋ฌ ๊ฐ์ ์นํฉ ์ค์  ํ์ผ์ ํ๋๋ก ๋ณํฉํด์ฃผ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ด๋ค.
- ์ผ๋ฐ์ ์ผ๋ก ์น ์ ํ๋ฆฌ์ผ์ด์์ ์ ์ํ  ๋๋ ์นํฉ ์ค์  ๊ฐ๋ฐ(development)์ ๋ฐฐํฌ(production)์ฉ์ผ๋ก ๋๋์ด์ ์ ์ฉํ๋ค.
- ์ฌ์ค ์กฐ๊ฑด๋ฌด๋ฅ๋ก ์ค์ ์ ๊ตฌ๋ถํ  ์๋ ์์ง๋ง ์ค์ ๋ก ํ์ผ์ ์์ ๋๋ ๋๋๊ฒ ๊ถ์ฅ๋๋ ๋ฐฉ์์ด๋ค.

```
yarn add -D webpack-merge
```
```js
// webpack.dev.js
const { merge } = require('webpack-merge');
const webpack = require('webpack');
const common = require('./webpack.common.js');

require('dotenv').config({ path: './.env.development' });

module.exports = merge(common, {
  mode: 'development',
  devtool: 'source-map',
  devServer: {
    // ...
  },
  plugins: [
    new webpack.EnvironmentPlugin({
      ...process.env,
    }),
  ],
});

```

### ์ ๋ ๊ฒฝ๋ก
- webpack์์ `resolve`๋ ๋ชจ๋ ํด์์ ๋ํ ์ค์ ์ผ๋ก ํน์  ๋ชจ๋์ ํธ์ถํ  ๋ ๋ชจ๋์ ์ฐพ๋ ์์น๋ฅผ ๋ณ๊ฒฝํ  ์ ์๋ค.
- `alias`๋ฅผ ์ด์ฉํด ๋ณ์นญ์ ์ถ๊ฐํด์ค์ผ๋ก์จ ์๋ ๊ฒฝ๋ก๋ฅผ ์ ๋ ๊ฒฝ๋ก๋ก ์ค์ ํด ์ค ์ ์๋ค.
- `extensions`๋ ์ต์ ๋ด์ ๋ฐฐ์ด๋ก ํ์ฅ์๋ฅผ ์ ์ธํ๋ฉด ํ์ฅ์๋ฅผ ์ผ์ชฝ๋ถํฐ ์์๋๋ก ํด์ํ๋ค. ์ฌ๋ฌ ํ์ผ์์ ์ด๋ฆ์ด ๋์ผํ์ง๋ง ๋ค๋ฅธ ํ์ฅ์๋ฅผ ๊ฐ์ง ๊ฒฝ์ฐ webpack์ ๋ฐฐ์ด์ ์์์๋ถํฐ ํ์ผ์ ํด์ํ๊ณ  ๋จ์ ๊ฒ์ ํด์ํ์ง ์๋๋ค.

```js
module.exports = {
  // ...
  resolve: {
    extensions: ['.ts', '.tsx', '.js'],
    alias: {
      '@components': path.resolve(__dirname, 'src/components/'),
      '@pages': path.resolve(__dirname, 'src/pages/'),
    },
  },
}
```

- Typescript์์๋ ์ฐธ๊ณ ๋ก `tsconfig.json`๋ ์์ ํด์ฃผ์ด์ผ ํ๋ค.
```json
{
  "compilerOptions": {
    "baseUrl": "./",
    // ...
  },
  "include": [
    "src"
  ],
  "extends": "./tsconfig.paths.json"
}
```
- baseUrl์๋ `./` extends์๋ `tsconfig.paths.json`์ ์ถ๊ฐํ๋ค.

```json
{
  "compilerOptions": {
    "paths": { 
      "@pages/*": ["src/pages/*"],
      "@components/*": [ "src/components/*"],
    },
  }
}
```

- `tsconfig.paths.json`์ ๋ด์ฉ์ ๋ค์๊ณผ ๊ฐ๋ค.

<br />

### ForkTsCheckerWebpackPlugin ํ๋ฌ๊ทธ์ธ
- webpack.common.js์ plugins์ ๋ณด๋ฉด ForkTsCheckerWebpackPlugin์ด ์ถ๊ฐ๋ ๊ฒ์ ํ์ธํ  ์ ์๋ค.
- ForkTsCheckerWebpackPlugin ๊ฐ๋จํ๊ฒ ๋งํ๋ฉด ๋น๋ ์ ํ์์คํฌ๋ฆฝํธ์ ํ์ ์ฒดํฌ ์๋๋ฅผ ๋นจ๋ฆฌ ํด์ฃผ๋ ํ๋ฌ๊ทธ์ธ์ด๋ค.

```js
const ForkTsCheckerWebpackPlugin = require('fork-ts-checker-webpack-plugin');

module.exports = {
  // ...
  module: {
    // ...
  },
  plugins: [
    // ...
    new ForkTsCheckerWebpackPlugin({
      async: false,
    }),
  ],
};
```

<br />

## ๐ ๊ทธ์ธ ์ฐธ๊ณ  ์ฌํญ 3. babel
```js
// babel.config.js
module.exports = {
  presets: [
    '@babel/preset-env',
    '@babel/preset-typescript',
    '@babel/preset-react',
  ],
  plugins: [],
};
```
### @babel/preset-typescript
- `@babel/preset-typescript`์ Typescript๋ฅผ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ ์ฌ์ ์ ์ถ๊ฐํด์ฃผ๋ฉด ์ข๋ค. ์ด ํ๋ฌ๊ทธ์ธ์ TypeScript๋ฅผ JavaScript๋ก ๋ณํํด์ฃผ๋ ํ๋ฌ๊ทธ์ธ์ด ๋ด์ฅ๋์ด ์๋ค.

<br />

### @babel/preset-react
- `@babel/preset-react`๋ JSX๋ก ์์ฑ๋ ์ฝ๋๋ค์ createElement ํจ์๋ฅผ ์ด์ฉํ ์ฝ๋๋ก ๋ณํํด ์ฃผ๋ ๋ฐ๋ฒจ ํ๋ฌ๊ทธ์ธ์ด๋ค.


<br />

## ๐ ๊ทธ์ธ ์ฐธ๊ณ  ์ฌํญ 4. eslint
```js
module.exports = {
  // .eslintrc.js
  env: {
    browser: true,
    es2021: true,
    node: true,
  },
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint', 'react', 'react-hooks'],
  extends: [
    'plugin:react/recommended',
    'plugin:@typescript-eslint/recommended',
    'prettier',
  ],
  rules: {
    // rules
  },
};
```
### @typescript-eslint
- [typescript-eslint](https://github.com/typescript-eslint/typescript-eslint)๋ ESLint์ TypeScript๋ฅผ ์ง์ํ๊ธฐ ์ํ ํจํค์ง์ MonoRepository
- `@typescript-eslint/parser`๋ `typescript-estree`๋ฅผ ํ์ฉํ ํ์์คํฌ๋ฆฝํธ์ฉ ESLint ํ์์ด๋ค. ์ฐธ๊ณ ๋ก `typescript-estree`๋ ESTree(์๋ฐ์คํฌ๋ฆฝํธ AST์คํ) ํธํ AST๋ฅผ ์์ฑํ๋ ํ์์คํฌ๋ฆฝํธ ํ์์ด๋ค.
- `@typescript-eslint/eslint-plugin`๋ Typescript ๊ด๋ จ linting ๊ท์น์ ์ฒ๋ฆฌํ๋ ํ๋ฌ๊ทธ์ธ์ด๋ค.

<br />

### eslint-plugin-react
- `eslint-plugin-react`๋ ๋ฆฌ์กํธ์ ๊ด๋ จ๋ ๊ท์น์ ์ ์ํ ํจํค์ง์ด๋ค.

<br />

### eslint-plugin-react-hooks
- `eslint-plugin-react-hooks`๋ React ๋ด์ ํ์ ๋ํ ๊ท์น์ด์๋ค. ์ต์์์์๋ง Hook์ ํธ์ถํด์ผ๋ง ํ๊ณ , ์ค์ง React์์๋ง Hook์ ํธ์ถํด์ค์ผํ๋๋ฐ ์ด๋ฌํ ๊ท์น์ ๊ฐ์ ํด์ฃผ๋ ESLint ํ๋ฌ๊ทธ์ธ์ด๋ค.

<br />
