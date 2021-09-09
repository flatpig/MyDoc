# 尝试NestJS Day 1

## 环境

- OS : Windows 10 Pro
- Yarn : 1.22.5
- Node : v14.15.2

## 安装（有坑）

- 官网:[NextJS](https://nestjs.com/)
- [中文文档](https://docs.nestjs.cn/)

安装Nest CLI （官方）（安装没成功！！！what a fuck！！！）

```bash
npm i -g @nestjs/cli
nest new project-name
```

此处有一个坑，由于本人比较喜欢用Yarn，使用Yarn安装时就发生了以下错误

```bash
yarn global add @nestjs/cli
nest new project-name
⚡  We will scaffold your app in a few seconds..

Error: Collection "@nestjs/schematics" cannot be resolved.
    at NodeModulesEngineHost.resolve (C:\Users\DFG\AppData\Local\Yarn\Data\global\node_modules\@angular-devkit\schematics\tools\node-module-engine-host.js:75:19)
    at NodeModulesEngineHost._resolveCollectionPath (C:\Users\DFG\AppData\Local\Yarn\Data\global\node_modules\@angular-devkit\schematics\tools\node-module-engine-host.js:80:37)
    at NodeModulesEngineHost.createCollectionDescription (C:\Users\DFG\AppData\Local\Yarn\Data\global\node_modules\@angular-devkit\schematics\tools\file-system-engine-host-base.js:118:27)
    at SchematicEngine._createCollectionDescription (C:\Users\DFG\AppData\Local\Yarn\Data\global\node_modules\@angular-devkit\schematics\src\engine\engine.js:162:40)
    at SchematicEngine.createCollection (C:\Users\DFG\AppData\Local\Yarn\Data\global\node_modules\@angular-devkit\schematics\src\engine\engine.js:155:43)
    at NodeWorkflow.execute (C:\Users\DFG\AppData\Local\Yarn\Data\global\node_modules\@angular-devkit\schematics\src\workflow\base.js:101:41)     
    at main (C:\Users\DFG\AppData\Local\Yarn\Data\global\node_modules\@angular-devkit\schematics-cli\bin\schematics.js:254:14)
    at Object.<anonymous> (C:\Users\DFG\AppData\Local\Yarn\Data\global\node_modules\@angular-devkit\schematics-cli\bin\schematics.js:361:5)       
    at Module._compile (internal/modules/cjs/loader.js:1063:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1092:10)

Failed to execute command: node @nestjs/schematics:application --name=project-name --directory=undefined --no-dry-run --no-skip-git --package-manager=undefined --language="ts" --collection="@nestjs/schematics"
```

于是使用npm安装然而问题依旧，查看GitHub issue里面尝试清空yarn缓存

```bash
yarn cache clean
yarn global remove @nestjs/cli
yarn global add @nestjs/cli
```

可是仍不能解决 what ??? 之后再尝试安装@nestjs/schematics，问题终于解决。

```bash
yarn global add @nestjs/schematics
```

## 安装总结（成功安装）

```bash
yarn global add @nestjs/cli
yarn global add @nestjs/schematics
nest new project-name
```

## 安装中

直接工程名project-name 😓懒得改了，本来想来个HelloWorld的。

```bash
$ nest new project-name
⚡  We will scaffold your app in a few seconds..

CREATE project-name/.eslintrc.js (631 bytes)
CREATE project-name/.prettierrc (51 bytes)        
CREATE project-name/nest-cli.json (64 bytes)      
CREATE project-name/package.json (1968 bytes)     
CREATE project-name/README.md (3339 bytes)        
CREATE project-name/tsconfig.build.json (97 bytes)
CREATE project-name/tsconfig.json (546 bytes)     
CREATE project-name/src/app.controller.spec.ts (617 bytes)
CREATE project-name/src/app.controller.ts (274 bytes)
CREATE project-name/src/app.module.ts (249 bytes)
CREATE project-name/src/app.service.ts (142 bytes)
CREATE project-name/src/main.ts (208 bytes)
CREATE project-name/test/app.e2e-spec.ts (630 bytes)
CREATE project-name/test/jest-e2e.json (183 bytes)
```

选择包管理工具，此处当然是选择yarn了。

```bash
? Which package manager would you ❤️  to use? (Use arrow keys)
> npm
  yarn
  pnpm
```

然后就是漫长的安装等待，竟然还提示我去喝杯咖啡。

```bash
▹▹▹▹▹ Installation in progress... ☕
```

## 安装完成

等了5-6分钟终于安装好了，可能是我的系统太差了？？？此处应该有黑人问号脸

```bash
🚀  Successfully created project project-name
👉  Get started with the following commands:

$ cd project-name
$ yarn run start


                          Thanks for installing Nest 🙏
                 Please consider donating to our open collective
                        to help us maintain this package.


               🍷  Donate: https://opencollective.com/nest


```

## 查看项目文件

```bash
$ ls
nest-cli.json  node_modules/  package.json  README.md  src/  test/  tsconfig.build.json  tsconfig.json  yarn.lock
```

终于知道了为什么第一次这么慢，原来是一并安装依赖了

## 启动

```bash
$ cd project-name
$ yarn run start
yarn run v1.22.5
$ nest start
[Nest] 23344  - 2021/09/03 上午10:25:54     LOG [NestFactory] Starting Nest application...
[Nest] 23344  - 2021/09/03 上午10:25:54     LOG [InstanceLoader] AppModule dependencies initialized +33ms
[Nest] 23344  - 2021/09/03 上午10:25:54     LOG [RoutesResolver] AppController {/}: +5ms
[Nest] 23344  - 2021/09/03 上午10:25:54     LOG [RouterExplorer] Mapped {/, GET} route +5ms
[Nest] 23344  - 2021/09/03 上午10:25:54     LOG [NestApplication] Nest application successfully started +2ms
```

启动了？？？ 端口是啥？ 一脸懵逼，打开项目，查看文件，找到src/main.ts

```ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

好吧，直接写在代码里面的。
