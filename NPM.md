# 基本介绍

## Node.js

- 基于chrome v8的，一个js的运行时环境，可以用来做后端开发

![image-20250414105929313](https://skillset.oss-cn-shanghai.aliyuncs.com/image-20250414105929313.png)

## 安装

### MAC

- [官网下载](https://nodejs.org/en): 一键安装
- 版本：v20.13.1

```bash
# 目录
- Node.js v22.14.0 to /usr/local/bin/node
- npm v10.9.2 to /usr/local/bin/npm

# 查看版本号
- node -v
# 执行对应的js代码
- node erick.js

# 卸载
sudo rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}
```

## NPM

- Node Package Manager，node的包管理工具
- [npm第三方依赖](https://www.npmjs.com/)
- 随着node的安装，会一起安装在客户机上

# 初始化

## 项目初始化

```bash
# package.json
- 定义依赖的包，类似maven中的pom.xml
# npm init -y
- 在执行命令的目录下，创建一个package.json的文件，来统一管理项目依赖
```

```json
{
  "name": "replau-ui-service",
  "version": "5.3.1",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": ""
}
```

## 镜像配置

- 默认npm包是从海外服务器拉的：https://registry.npmjs.org
- 第一次执行任何一个config set的命令后，就会在本地/Users/shuzhan/.npmrc创建一个配置文件

```bash
npm config list

# 使用淘宝镜像
npm config set registry https://registry.npmmirror.com/

#  切回默认地址
npm config set registry https://registry.npmjs.org/

# 验证是否成功
npm config get registry
```

# 依赖

## 安装

### 依赖安装

- 不要上传node_moudles模块代码，体积太大，pipeline中有node环境，会自动进行下载

```bash
# npm install lodash

# node_moudles模块
- 项目跟路径下，创建node_moudles模块，并存储第三方包

# package.lock.json
- 生成对应package.lock.json，详细定义具体的依赖版本，组织信息

# npm install/i
- 当node_moudles模块，package.lock.json，package.lock.json数据不一致时，就会更新package.json重新更新
```

![image-20250414132937130](https://skillset.oss-cn-shanghai.aliyuncs.com/image-20250414132937130.png)

### 语义化版本(SemVer)

- node中的三方包的依赖，采用这种模式
- 允许升级到对应的版本

```bash
# major.minor.patch

#  ^ 5.3.2      指定版本安装时候的默认方式
- 允许升级minor和patch版本             5.3.2 <= version < 6.0.0 

# ~ 5.3.2
- 允许升级patch版本                    5.3.2 <= version < 5.4.0

# 无符号
- 固定版本，禁止升级                     只能是5.3.2
```

```bash
# 特殊场景
   # 主版本为0     ^0.2.3
   - 表示为初始开发阶段，   ^0.2.3===~0.2.3, 仅允许修改patch版本
   # 主版本大于1， 
   - 遵循上面的语义化规范
```

```bash
# 设计原因
- 兼容性： 主版本升级表示破坏性更新，因此默认禁止升级
- 灵活性： 允许自动升级minor和patch版本，减少手动维护

# 应用建议
- ^        : 希望依赖自动升级minor和patch
- ~        : 仅希望升级漏洞修复
- 固定版本  : 在关键生产环境中，可直接写死版本，避免意外升级
```

### package.json

```json
{
  "name": "replau-ui-service",
  "version": "5.3.1",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": "",
  "dependencies": {
    "lodash": "^4.17.21"
  }
}
```

### package-lock.json

- 通过npm install后产生的文件，一般必须提交到git上

```bash
# 锁定整个依赖树
- 不仅记录直接依赖的版本，还精确记录所有间接依赖（嵌套依赖）的版本和下载地址
- 不要手动修改package-lock.json

# 确保环境一致性
- 无论何时何地运行 npm install，只要 package-lock.json 存在，npm 会严格按照锁文件中的版本和依赖关系安装
# 完全复现开发环境
```

- 必须提交到git上
- npm 的依赖解析逻辑可能因缓存、镜像源或网络环境不同，导致实际安装的子依赖版本存在差异

```bash
# 不提交的风险：     "lodash": "4.17.21"
- 实际安装时，如果全局缓存中存在lodash@4.17.21， 则会使用
- 如果缓存被清除或镜像源更新，npm可能会重新下载（看似没问题）
- 如果lodash的某个子依赖有安全更新，npm可能会自动升级子依赖，导致行为不一致
```

```json
{
  "name": "replau-ui-service",
  "version": "5.3.1",
  "lockfileVersion": 3,
  "requires": true,
  "packages": {
    "": {
      "name": "replau-ui-service",
      "version": "5.3.1",
      "license": "ISC",
      "dependencies": {
        "lodash": "^4.17.21"
      }
    },
    "node_modules/lodash": {
      "version": "4.17.21",
      /*镜像地址*/
      "resolved": "https://registry.npmmirror.com/lodash/-/lodash-4.17.21.tgz",
      "integrity": "sha512-v2kDEe57lecTulaDIuNTPy3Ry4gLGJ6Z1O3vE1krgXZNrsQ+LFTGHVxVjcXPs17LhbZVGedAJv8XZ1tvj5FvSg==",
      "license": "MIT"
    }
  }
}
```

## 分类

### 项目依赖

- 只会将依赖安装在本项目的node_moudles模块

```bash
npm install/i lodash               # 默认最新版本
npm i lodash@4.17.21               # 指定版本
npm i lodash axios                 # 多个同时安装

# 卸载, 会删除package.json，自动更新package-lock.json和项目的node_moudles模块
npm uninstall/uni lodash           
```

```bash
# 只有在开发时候用到的依赖
npm install moment --save-dev
npm install moment -D
```

### 全局依赖

- 全局依赖默认安装位置：/usr/local/lib/node_modules
- 只有工具一类的包，才需要安装全局包

```bash
# 更改目录所有权（假设用户名为 your_username）
sudo chown -R $(whoami) /usr/local/lib/node_modules

# 安装全局依赖：因为mac中对文件保护机制
npm install lodash -g
npm uninstall lodash -g
```

### 依赖过期

```bash
npm outdated
```
