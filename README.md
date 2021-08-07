# setup-react
Configuração básica para rodar React sem o CRA

## Estrutura inicial e Instalação de dependências
```bash
# criar o arquivo `.gitignore` preenchendo com `node_modules`
echo node_modules >> .gitignore

# iniciar o arquivo `package.json`
yarn init -y

# instalar `react` e `react-dom`
yarn add react react-dom

# instalar as dependencias relacionadas ao `webpack`
yarn add webpack webpack-cli webpack-dev-server html-webpack-plugin -D

# instalar as dependencias relacionadas ao `babel`
yarn add @babel/core @babel/preset-env @babel/preset-react babel-loader -D

# instalar as dependencias para utilizar css e sass
yarn add style-loader css-loader sass-loader node-sass -D
```

## Configuração webpack
```js
const path = require('path');
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: path.resolve(__dirname, 'public/index.html'),
      filename: 'index.html'
    })
  ],
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env', '@babel/preset-react']
          }
        }
      },
      {
        test: /\.css?$/,
        use: [
          { loader: "style-loader" },
          { loader: "css-loader" },
        ]
      },
      {
        test: /\.scss$/,
        use: [
          { loader: "style-loader" },
          { loader: "css-loader" },
          { loader: "sass-loader" }
        ]
      },
    ]
  }
};
```

## Criar scripts no `package.json`
```json
"scripts": {
  "build:dev": "webpack --mode='development'",
  "build:prod": "webpack --mode='production'",
  "start": "webpack server --mode development --env development --hot --port 3000"
},
```