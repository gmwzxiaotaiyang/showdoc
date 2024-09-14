
## 顺序

- 基础
  - [使用Vite创建vue项目](#使用Vite创建vue项目)
  - [创建 .editorconfig](#创建-editorconfig)
  - [创建 .nvmrc](#创建nvmrc)
  - [安装 eslint @antfu/eslint-config](#eslint-antfueslint-config) 
  - [安装 Stylelint
  ](#stylelint)
  - [安装 npm-run-all](#npm-run-all)
  - [创建vite-plugin-文件修改-基础配置](#创建vite-plugin-文件修改-基础配置)  （包含添加别名）
  - [安装 elementui](#elementui)
  - [安装 auto-import](#auto-import)
  - [安装 components](#components)
- 可选
  - [axios](#axios)
  - [vue-router](#router)

## 搭建项目

### 使用Vite创建vue项目

参考官方指南: [Vite 官方文档](https://cn.vitejs.dev/guide/#scaffolding-your-first-vite-project)

```bash
npm create vite@latest my-vue-app -- --template vue 
npm i

```

### 添加别名

在`vite.config.js`的`defineConfig`中添加:

```javascript
import path from 'node:path';

    resolve: { alias: {
      '@': path.resolve(__dirname, 'src'),
      '#': path.resolve(__dirname, 'src/types'),
    } },
```

创建`jsconfig.json`或`tsconfig.json` :
```bash
@'
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  },
  "include": ["src"]
}
'@ | Out-File -Encoding UTF8 jsconfig.json
```


## 安装框架

### 代码规范

#### 创建 `.editorconfig`

参考 [官方配置](https://github.com/editorconfig/editorconfig/blob/master/.editorconfig)

>  .editorconfig 是一种文件格式和一组文本编辑器插件，用于在不同的编辑器和集成开发环境（IDE）之间保持一致的编码风格。它用在不同开发人员之间维护一致的编码风格。

```bash
@'
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
'@ | Out-File -Encoding UTF8 .editorconfig
```

#### 创建`.nvmrc`

> nvm 配置文件

参考 [nvm github](https://github.com/nvm-sh/nvm?tab=readme-ov-file#nvmrc)

> .nvmrc 是一个用于指定 Node.js 版本的文件，通常放置在项目的根目录中。这个文件的主要目的是帮助开发者在不同的项目之间切换 Node.js 版本时更加方便和一致。.nvmrc 文件的内容非常简单，通常只包含一个版本号，例如：

```bash
echo "18.20.3" > .nvmrc
```

#### eslint @antfu/eslint-config

[参考网址](https://www.npmjs.com/package/@antfu/eslint-config/v/2.21.2)

##### 安装依赖:

```bash
npm i -D eslint @antfu/eslint-config@^2.21.2
```

##### 创建 `eslint.config.js`
```bash
@'
import antfu from '@antfu/eslint-config'

export default antfu({
  rules: {
    'unused-imports/no-unused-vars': 'off',
  },
},
)

'@ | Out-File -Encoding UTF8 .\eslint.config.js
```

##### 添加 package.json 脚本

```json
{
  "scripts": {
    "lint": "eslint .",
    "lint:eslint": "eslint . --cache --fix",
    "lint:fix": "eslint . --fix"
  }
}
```

##### VS Code 支持（保存时自动修复）

安装 [VS Code ESLint](https://cn.vitejs.dev/guide/) 插件

将以下设置添加到您的`.vscode/settings.json`：


##### 配置VSCode:

```json
{
  // 禁用默认的格式化程序，使用 eslint 代替
  "prettier.enable": true,
  "editor.formatOnSave": false,

  // 自动修复
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit",
    "source.organizeImports": "never"
  },

  // 在 IDE 中静默样式规则，但仍然自动修复它们
  "eslint.rules.customizations": [
    { "rule": "style/*", "severity": "off" },
    { "rule": "format/*", "severity": "off" },
    { "rule": "*-indent", "severity": "off" },
    { "rule": "*-spacing", "severity": "off" },
    { "rule": "*-spaces", "severity": "off" },
    { "rule": "*-order", "severity": "off" },
    { "rule": "*-dangle", "severity": "off" },
    { "rule": "*-newline", "severity": "off" },
    { "rule": "*quotes", "severity": "off" },
    { "rule": "*semi", "severity": "off" }
  ],

  // 为所有支持的语言启用 eslint
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact",
    "vue",
    "html",
    "markdown",
    "json",
    "jsonc",
    "yaml",
    "toml",
    "xml",
    "gql",
    "graphql",
    "astro",
    "css",
    "less",
    "scss",
    "pcss",
    "postcss"
  ]
}

```

```bash
npm run eslin:fix

```

#### Stylelint

安装依赖:

```bash
# 1.
npm init stylelint  |  pnpm create stylelint
# 2. （推荐）
# @stylistic/stylelint-config
# stylelint-config-recess-order css排序
# stylelint-config-standard 提供了一套全面的、标准化的 CSS 代码风格规则
# stylelint-config-standard-scss 专门针对 SCSS（Sassy CSS）语法
# stylelint-config-standard-vue 添加了特定于 Vue 单文件组件 (SFC) 的规则
# stylelint-scss  # 插件  强制执行 SCSS（Sassy CSS）语法
npm install --save-dev stylelint  stylelint-config-standard-scss stylelint-config-standard-vue stylelint-config-recess-order @stylistic/stylelint-config stylelint-scss
```

修改`.stylelintrc.json`文件
```json
{
  "extends": [
    "stylelint-config-standard-scss",
    "stylelint-config-standard-vue/scss",
    "stylelint-config-recess-order",
    "@stylistic/stylelint-config"
  ],
  "plugins": [
    "stylelint-scss"
  ],
  "overrides": [
    {
      "files": [
        "*.vue",
        "**/*.vue"
      ],
      "customSyntax": "postcss-html"
    }
  ]
}

```


在 package.json 的 script 中添加 
```json
    "lint:stylelint": "stylelint \"src/**/*.{css,scss,vue}\" --cache --fix",
```

#### npm-run-all
```bash
npm i npm-run-all2 -D
```
在 package.json 的 script 中添加 
```bash
    "lint": "npm-run-all -s lint:eslint lint:stylelint",
```

#### simple-git-hooks 和 lint-staged

```bash
npm i simple-git-hooks -D
npm i lint-staged -D     

```

### 必备框架

#### axios

参考 [axios 官网](https://axios-http.com/docs/intro)

安装
```bash
npm i axios
```

新建文件夹 src\api\index.js
```bash
 New-Item -Path ".\src\api\index.js" -ItemType File -Force
```


```bash
@'
import axios from 'axios'

const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000 * 10,
})

// 添加请求拦截器
axios.interceptors.request.use((config) => {
  // 在发送请求之前做些什么
  return config
}, (error) => {
  // 对请求错误做些什么
  return Promise.reject(error)
})

// 添加响应拦截器
axios.interceptors.response.use((response) => {
  // 2xx 范围内的状态码都会触发该函数。
  // 对响应数据做点什么
  return response
}, (error) => {
  // 超出 2xx 范围的状态码都会触发该函数。
  // 对响应错误做点什么
  return Promise.reject(error)
})

export default instance

'@ | Out-File -Encoding UTF8 .\src\api\index.js
```

[配置 mock](#vite-plugin-fake-server-模拟后端服务器的响应)


#### pinia

安装

参考官网 

```bash
npm install pinia
```

#### elementui

[参考](https://element-plus.org/zh-CN/guide/quickstart.html)

安装依赖

```bash
npm install element-plus --save

```
参考如下代码修改 man.js
```js
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'

const app = createApp(App)

app.use(ElementPlus)
app.mount('#app')
```

##### 安装插件

[auto-import](#auto-import)
[components](#components)



#### router

[参考](https://router.vuejs.org/zh/)

```bash
npm install vue-router@4
```

创建文件 src\router\index.js
```bash
 New-Item -Path ".\src\router\index.js" -ItemType File -Force
```

```js
import { createRouter, createWebHistory } from 'vue-router'
import index from '@/views/index.vue'
import test from '@/views/test/index.vue'

const routes = [
  { path: '/', component: index },
  { path: '/test', component: test },
]

const router = createRouter({
  history: createWebHistory(),
  routes,
})

export default router

```

修改 App.vue
```vue

```

修改 main.js
```js
import router from './router'
app.use(router)
```

修改 app.js
```js
  <main>
    <RouterView />
  </main>
```

#### bootstrap

```bash
npm i --save bootstrap @popperjs/core
```

修改 vite.config.js

```js
alias: {
+      '~bootstrap': path.resolve(__dirname, 'node_modules/bootstrap'),
    }
```

创建文件 src\assets\styles\styles.scss
```bash
 New-Item -Path ".\src\assets\styles\styles.scss" -ItemType File -Force
```
```bash
// Import all of Bootstrap's CSS
@import "~bootstrap/scss/bootstrap";
```

修改 main.js

```js
// Import our custom CSS
import '../scss/styles.scss'

// Import all of Bootstrap's JS
import * as bootstrap from 'bootstrap'
```

#### sass
```bash
npm install --save-dev sass

```

#### unocss

安装依赖

```bash
npm i unocss -D
```

创建`uno.config.js`和主题文件 

```bash
@'
import {
  defineConfig,
  presetAttributify,
  presetIcons,
  presetTypography,
  presetUno,
  transformerCompileClass,
  transformerDirectives,
  transformerVariantGroup,
} from 'unocss'
import { entriesToCss, toArray } from '@unocss/core'
import { darkTheme, lightTheme } from './themes'

export default defineConfig({
  content: {
    pipeline: {
      include: [
        /\.(vue|svelte|[jt]sx|mdx?|astro|elm|php|phtml|html)($|\?)/,
        'src/**/*.{js,ts}',
      ],
    },
  },
  shortcuts: [
    {
      'flex-center': 'flex justify-center items-center',
      'flex-col-center': 'flex flex-col justify-center items-center',
    },
  ],
  preflights: [
    {
      getCSS: () => {
        const returnCss = []
        // 明亮主题
        const lightCss = entriesToCss(Object.entries(lightTheme))
        const lightRoots = toArray([`*,::before,::after`, `::backdrop`])
        returnCss.push(lightRoots.map(root => `${root}{${lightCss}}`).join(''))
        // 暗黑主题
        const darkCss = entriesToCss(Object.entries(darkTheme))
        const darkRoots = toArray([`html.dark,html.dark *,html.dark ::before,html.dark ::after`, `html.dark ::backdrop`])
        returnCss.push(darkRoots.map(root => `${root}{${darkCss}}`).join(''))

        return returnCss.join('')
      },
    },
  ],
  theme: {
    colors: {
      'ui-primary': 'rgb(var(--ui-primary))',
      'ui-text': 'rgb(var(--ui-text))',
    },
  },
  presets: [
    presetUno(),
    presetAttributify(),
    presetIcons({
      extraProperties: {
        'display': 'inline-block',
        'vertical-align': 'middle',
      },
    }),
    presetTypography(),
  ],
  transformers: [
    transformerDirectives(),
    transformerVariantGroup(),
    transformerCompileClass(),
  ],
  configDeps: [
    'themes/index.ts',
  ],
})


'@ | Out-File -Encoding UTF8 uno.config.js

```

```bash
@'
import { hex2rgba } from '@unocss/preset-mini/utils'

export const lightTheme = {
  'color-scheme': 'light',
  // 内置 UI
  '--ui-primary': hex2rgba('#0f0f0f').join(' '),
  '--ui-text': hex2rgba('#fcfcfc').join(' '),
  // 主体
  '--g-bg': '#f2f2f2',
  '--g-container-bg': '#fff',
  '--g-border-color': '#f2f2f2',
  // 头部
  '--g-header-bg': '#fff',
  '--g-header-color': '#0f0f0f',
  '--g-header-menu-color': '#0f0f0f',
  '--g-header-menu-hover-bg': '#dde1e3',
  '--g-header-menu-hover-color': '#0f0f0f',
  '--g-header-menu-active-bg': '#0f0f0f',
  '--g-header-menu-active-color': '#fff',
  // 主导航
  '--g-main-sidebar-bg': '#f2f2f2',
  '--g-main-sidebar-menu-color': '#0f0f0f',
  '--g-main-sidebar-menu-hover-bg': '#dde1e3',
  '--g-main-sidebar-menu-hover-color': '#0f0f0f',
  '--g-main-sidebar-menu-active-bg': '#0f0f0f',
  '--g-main-sidebar-menu-active-color': '#fff',
  // 次导航
  '--g-sub-sidebar-bg': '#fff',
  '--g-sub-sidebar-logo-bg': '#0f0f0f',
  '--g-sub-sidebar-logo-color': '#fff',
  '--g-sub-sidebar-menu-color': '#0f0f0f',
  '--g-sub-sidebar-menu-hover-bg': '#dde1e3',
  '--g-sub-sidebar-menu-hover-color': '#0f0f0f',
  '--g-sub-sidebar-menu-active-bg': '#0f0f0f',
  '--g-sub-sidebar-menu-active-color': '#fff',
  // 标签栏
  '--g-tabbar-dividers-bg': '#a3a3a3',
  '--g-tabbar-tab-color': '#a3a3a3',
  '--g-tabbar-tab-hover-bg': '#e5e5e5',
  '--g-tabbar-tab-hover-color': '#0f0f0f',
  '--g-tabbar-tab-active-color': '#0f0f0f',
}

export const darkTheme = {
  'color-scheme': 'dark',
  // 内置 UI
  '--ui-primary': hex2rgba('#e5e5e5').join(' '),
  '--ui-text': hex2rgba('#0f0f0f').join(' '),
  // 主体
  '--g-bg': '#0a0a0a',
  '--g-container-bg': '#141414',
  '--g-border-color': '#15191e',
  // 头部
  '--g-header-bg': '#141414',
  '--g-header-color': '#e5e5e5',
  '--g-header-menu-color': '#a8a29e',
  '--g-header-menu-hover-bg': '#141414',
  '--g-header-menu-hover-color': '#e5e5e5',
  '--g-header-menu-active-bg': '#e5e5e5',
  '--g-header-menu-active-color': '#0a0a0a',
  // 主导航
  '--g-main-sidebar-bg': '#0a0a0a',
  '--g-main-sidebar-menu-color': '#a8a29e',
  '--g-main-sidebar-menu-hover-bg': '#141414',
  '--g-main-sidebar-menu-hover-color': '#e5e5e5',
  '--g-main-sidebar-menu-active-bg': '#e5e5e5',
  '--g-main-sidebar-menu-active-color': '#0a0a0a',
  // 次导航
  '--g-sub-sidebar-bg': '#141414',
  '--g-sub-sidebar-logo-bg': '#0f0f0f',
  '--g-sub-sidebar-logo-color': '#e5e5e5',
  '--g-sub-sidebar-menu-color': '#a8a29e',
  '--g-sub-sidebar-menu-hover-bg': '#0a0a0a',
  '--g-sub-sidebar-menu-hover-color': '#e5e5e5',
  '--g-sub-sidebar-menu-active-bg': '#e5e5e5',
  '--g-sub-sidebar-menu-active-color': '#0a0a0a',
  // 标签栏
  '--g-tabbar-dividers-bg': '#a8a29e',
  '--g-tabbar-tab-color': '#a8a29e',
  '--g-tabbar-tab-hover-bg': '#1b1b1b',
  '--g-tabbar-tab-hover-color': '#e5e5e5',
  '--g-tabbar-tab-active-color': '#e5e5e5',
}

'@ | Out-File -Encoding UTF8 .\themes\index.js
```

### vite plugin

#### 创建vite plugin 文件，修改 基础配置

新建文件 vite/plugins/index.js

```bash
# mac
mkdir -p vite/plugins
touch vite/plugins/index.js
# win
New-Item -Path ".\vite\plugins\index.js" -ItemType File -Force
```

```js
import vue from '@vitejs/plugin-vue'


export default function createVitePlugins(viteEnv, isBuild = false) {
  const vitePlugins = [vue()]

  return vitePlugins
}

```

修改 vite.config.js
```js
import process from 'node:process'
import path from 'node:path'
import { defineConfig, loadEnv } from 'vite'
import createVitePlugins from './vite/plugins'

// https://vitejs.dev/config/
export default async ({ mode, command }) => {
  // 加载环境变量
  const env = loadEnv(mode, process.cwd())

  // 返回 Vite 配置
  return defineConfig({
    // 配置插件
    // createVitePlugins 函数用于创建一组 Vite 插件
    // 传入环境变量和一个布尔值，表示是否为生产构建
    plugins: createVitePlugins(env, command === 'build'),

    // 这里可以添加其他 Vite 配置选项
    // 例如：
    // base: '/',
    // server: { ... },
    // build: { ... },
    // 设置别名
    resolve: {
      alias: {
        '@': path.resolve(__dirname, 'src'),
        '#': path.resolve(__dirname, 'src/types'),
      },
    },
    // css: { ... },
    // 等等
  })
}
```
#### auto import

创建文件 `auto-import`
```bash
@'
import autoImport from 'unplugin-auto-import/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default function createAutoImport() {
  return autoImport({
    resolvers: [ElementPlusResolver()],
    imports: [
      'vue',
      'vue-router',
      'pinia',
    ],
    dts: './src/types/auto-imports.d.ts',
    dirs: [
      './src/utils/composables/**',
    ],
  })
}

'@ | Out-File -Encoding UTF8 .\vite\plugins\auto-import.js
```

添加至 `vite\plugins\index.js`

```js
import vue from '@vitejs/plugin-vue'
+import createAutoImport from './auto-import'
import createComponents from './components'

export default function createVitePlugins(viteEnv, isBuild = false) {
  const vitePlugins = [vue()]
+  vitePlugins.push(createAutoImport())
  vitePlugins.push(createComponents())

  return vitePlugins
}

```

#### components

创建文件 `components.js`

```bash
@'
import components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default function createComponents() {
  return components({
    resolvers: [ElementPlusResolver()],
    dirs: [
      'src/components',
      'src/layouts/ui-kit',
    ],
    include: [/\.vue$/, /\.vue\?vue/, /\.tsx$/],
    dts: './src/types/components.d.ts',
  })
}

'@ | Out-File -Encoding UTF8 .\vite\plugins\components.js
```

添加至 `vite\plugins\index.js`

```js
import vue from '@vitejs/plugin-vue'
import createAutoImport from './auto-import'
+import createComponents from './components'

export default function createVitePlugins(viteEnv, isBuild = false) {
  const vitePlugins = [vue()]
  vitePlugins.push(createAutoImport())
+  vitePlugins.push(createComponents())

  return vitePlugins
}

```

#### 开发服务器启动时,在终端中输出信息

```bash
npm i boxen -D
```

新建app-info.js
```js
// 引入 boxen 库，用于在终端中创建带边框的文本框
import boxen from 'boxen'
// 引入 picocolors 库，用于给终端文本添加颜色和样式
import picocolors from 'picocolors'

// 定义并导出一个名为 appInfo 的 Vite 插件
export default function appInfo() {
  return {
    // 插件的名称
    name: 'appInfo',
    // 插件的应用阶段，这里设为 'serve'，即仅在开发服务器启动时应用
    apply: 'serve',
    // 钩子函数，在构建开始时执行
    async buildStart() {
      // 从 picocolors 中解构出一些常用的样式函数
      const { bold, green, cyan, bgGreen, underline } = picocolors
      // 使用 console.log 输出格式化的文本，注意这里为了避免 ESLint 的警告，使用了注释

      console.log(
        // 使用 boxen 创建一个带双重边框的文本框
        boxen(
          // 格式化输出的内容
          `${bold(green(`由 ${bgGreen('vite vue')} 驱动`))}\n\n${underline('http://www.yznature.com/')}\n\n当前使用：${cyan('基础版')}`,
          {
            // 设置文本框的内边距
            padding: 1,
            // 设置文本框的外边距
            margin: 1,
            // 设置文本框的边框样式为双重边框
            borderStyle: 'double',
            // 设置文本框内部文字居中
            textAlignment: 'center',
          },
        ),
      )
    },
  }
}

```

#### banner 用于在生成的文件顶部添加自定义注释

```bash
npm install vite-plugin-banner --save-dev   || yarn add vite-plugin-banner --dev
```

#### vite-plugin-fake-server 模拟后端服务器的响应

参考 [vite-plugin-fake-server github](https://github.com/condorheroblog/vite-plugin-fake-server)

##### 安装依赖

```bash
npm install vite-plugin-fake-server --save-dev
```

##### 新建 `.\vite\plugins\mock.js` 文件
```bash
@'
import { vitePluginFakeServer } from 'vite-plugin-fake-server'

export default function createMock(env, isBuild) {
  const { VITE_BUILD_MOCK } = env
  return vitePluginFakeServer({
    logger: !isBuild,
    include: 'src/mock',
    enableProd: isBuild && VITE_BUILD_MOCK === 'true',
  })
}
'@ | Out-File -Encoding UTF8 .\vite\plugins\mock.js
```

##### 添加至 `vite\plugins\index.js`

```js
import vue from '@vitejs/plugin-vue'
import createAutoImport from './auto-import'
import createComponents from './components'
+import createMock from './mock'

export default function createVitePlugins(viteEnv, isBuild = false) {
  const vitePlugins = [vue()]
  vitePlugins.push(createAutoImport())
  vitePlugins.push(createComponents())
+  vitePlugins.push(createMock(viteEnv, isBuild))

  return vitePlugins
}

```

##### 安装`mock.js @faker-js/faker`

```bash
npm i mockjs @faker-js/faker
```

##### 新建 mock 文件（如 user）

```bash
 New-Item -Path ".\src\mock\user.fake.js" -ItemType File -Force
```

```js
import { defineFakeRoute } from 'vite-plugin-fake-server/client'
import Mock from 'mockjs'
import { faker } from '@faker-js/faker'

export default defineFakeRoute([
  {
    url: '/mock/get-user-info',
    response: () => {
      return Mock.mock({
        id: '@guid',
        username: '@first',
        email: '@email',
        avatar: '@image("200x200")',
        role: 'admin',
      })
    },
  },
  {
    url: '/fake/get-user-info',
    response: () => {
      return {
        id: faker.string.uuid(),
        avatar: faker.image.avatar(),
        birthday: faker.date.birthdate(),
        email: faker.internet.email(),
        firstName: faker.person.firstName(),
        lastName: faker.person.lastName(),
        sex: faker.person.sexType(),
        role: 'admin',
      }
    },
  },
  {
    url: '/mock/user/login',
    method: 'post',
    response: ({ body }) => {
      return {
        msg: '',
        code: 0,
        data: Mock.mock({
          account: body.account,
          token: `${body.account}_@string`,
          avatar: 'https://fantastic-admin.github.io/logo.png',
        }),
      }
    },
  },
  {
    url: '/mock/user/permission',
    method: 'get',
    response: ({ headers }) => {
      let permissions = []
      if (headers.token?.indexOf('admin') === 0) {
        permissions = [
          'permission.browse',
          'permission.create',
          'permission.edit',
          'permission.remove',
        ]
      }
      else if (headers.token?.indexOf('test') === 0) {
        permissions = [
          'permission.browse',
        ]
      }
      return {
        msg: '',
        code: 0,
        data: {
          permissions,
        },
      }
    },
  },
  {
    url: '/mock/user/password/edit',
    method: 'post',
    response: () => {
      return {
        msg: '',
        code: 0,
        data: {
          isSuccess: true,
        },
      }
    },
  },
])

```
