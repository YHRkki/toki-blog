## npm
```js
npm install => npm i
// 项目（运行时、发布到生产环境时）依赖；例：antd , element,react...
// 对应的是 package.json 里 dependencies 里的依赖
// dependencies -> 运行时的依赖，发布后，即生产环境下还需要用的模块
npm install --save <packName>  => npm i <packName> -S
// 工程构建（开发时、“打包”时）依赖 ；例：xxx-cli , less-loader , babel-loader...
// 对应的是 package.json 里 devDependencies 里的依赖
// devDependencies -> 开发时的依赖。里面的模块是开发时用的，发布时用不到它。
npm install --save-dev <packName>  => npm i <packName> -D
npm run start => npm start
// 仅会拉取dependencies中的依赖
npm i --production => npm i --prod
// 跳过提问阶段，一路 yes 默认创建
npm init -y
// 当前项目安装的所有模块
// 可简写为 npm ls
npm list
```

### npm install 安装模块
```js
// 读取package.json里面的配置单安装
$ npm install
//可简写成 npm i

// 默认安装指定模块的最新(@latest)版本
$ npm install [<@scope>/]<name>
//eg:npm install gulp

// 安装指定模块的指定版本
$ npm install [<@scope>/]<name>@<version>
//eg: npm install gulp@3.9.1

// 安装指定指定版本范围内的模块
$ npm install [<@scope>/]<name>@<version range>
//eg: npm install vue@">=1.0.28 < 2.0.0"

// 安装指定模块的指定标签 默认值为(@latest)
$ npm install [<@scope>/]<name>@<tag>
//eg:npm install sax@0.1.1

// 通过Github代码库地址安装
$ npm install <tarball url>
//eg:npm install git://github.com/package/path.git
```

### npm uninstall 卸载模块
```js
// 卸载当前项目或全局模块
$ npm uninstall <name> [-g]
// eg: npm uninstall gulp --save-dev
// npm i gulp -g

// 卸载后，你可以到 /node\_modules/ 目录下查看包是否还存在
// npm ls 查看安装的模块
```

### npm update 更新模块
```js
// 升级当前项目或全局的指定模块
$ npm update <name> [-g]
// eg: npm update express
// npm update express -g
```