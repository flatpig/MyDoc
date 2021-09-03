# 尝试NestJS Day 2

续上一篇，今天进入项目文件ls一下

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
