## npm
```js
npm install => npm i
// 项目（运行时、发布到生产环境时）依赖；例：antd , element,react...
// 对应的是 package.json 里 dependencies 里的依赖
npm install --save <packName>  => npm i <packName> -S
// 工程构建（开发时、“打包”时）依赖 ；例：xxx-cli , less-loader , babel-loader...
// 对应的是 package.json 里 devDependencies 里的依赖
npm install --save-dev <packName>  => npm i <packName> -D
npm run start => npm start
// 仅会拉取dependencies中的依赖
npm i --production => npm i --prod
// 表示一路 yes 默认创建
npm init -y
```