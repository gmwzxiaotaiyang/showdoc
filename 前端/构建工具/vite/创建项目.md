
[TOC]

# 创建项目

## 搭建vite 项目

参考网址 https://cn.vitejs.dev/guide/

## 添加又名
在vite.config.js 中的defineConfig 中添加
```
import path from 'node:path';

    resolve: { alias: {
      '@': path.resolve(__dirname, 'src'),
      '#': path.resolve(__dirname, 'src/types'),
    } },
```

新建 jsconfig.json   或者 tsconfig.json
```
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

#### IDE 配置

配置文件为 .editorconfig ，通常情况下无需做任何修改。

```
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

#### node 版本
```
echo "18.20.3" > .node-version
```

#### eslint

```
npm i eslint -D
npm i @antfu/eslint-config -D
npx eslint --init  |  npm init @eslint/config@latest |pnpm create @eslint/config@latest
npm i unocss -D

```

修改工作区配置
```
@'
{
  "editor.formatOnSave": false,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit",
    "source.fixAll.stylelint": "explicit"
  },
  "stylelint.validate": [
    "css",
    "scss",
    "vue"
  ],
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
    "yaml"
  ]
}
'@ | Out-File -Encoding UTF8 .\.vscode\settings.json
```

替换 eslint.config.js
```
import antfu from '@antfu/eslint-config'

export default antfu(
  {
    unocss: true,
    ignores: ['public', 'dist*'],
  },
  {
    rules: {
      'eslint-comments/no-unlimited-disable': 'off',
      'curly': ['error', 'all'],
      'antfu/consistent-list-newline': 'off',
      // 'no-console': ['error', { allow: ['warn', 'error'] }], // 添加 no-console 规则
      'no-console': 'off', // 允许使用 console
    },
  },
  {
    files: ['src/**/*.vue'],
    rules: {
      'vue/block-order': [
        'error',
        {
          order: ['route', 'script', 'template', 'style'],
        },
      ],
    },
  },
)

```

创建 uno.config.js
```
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

```
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

在 package.json 的 script 中添加 
```
    "lint:eslint": "eslint . --cache --fix",
```

#### Stylelint

```
npm init stylelint  |  pnpm create stylelint

# @stylistic/stylelint-config
# stylelint-config-recess-order css排序
# stylelint-config-standard 提供了一套全面的、标准化的 CSS 代码风格规则
# stylelint-config-standard-scss 专门针对 SCSS（Sassy CSS）语法
# stylelint-config-standard-vue 添加了特定于 Vue 单文件组件 (SFC) 的规则
# stylelint-scss  # 处理 SCSS（Sassy CSS）语法
npm install --save-dev stylelint  stylelint-config-standard-scss stylelint-config-standard-vue stylelint-config-recess-order @stylistic/stylelint-config stylelint-scss
```

修改 .stylelintrc.json 文件
```
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
```
    "lint:stylelint": "stylelint \"src/**/*.{css,scss,vue}\" --cache --fix",
```

#### npm-run-all
```
npm i npm-run-all2 -D
```
在 package.json 的 script 中添加 
```
    "lint": "npm-run-all -s lint:eslint lint:stylelint",
```


#### simple-git-hooks 和 lint-staged

```
npm i simple-git-hooks -D
npm i lint-staged -D     

```

### 必备框架

#### elementui
```
npm install element-plus --save
```
参考如下代码修改 man.js
```
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'

const app = createApp(App)

app.use(ElementPlus)
app.mount('#app')
```

安装自动导入插件
```
npm install -D unplugin-vue-components unplugin-auto-import
```

#### sass
```
npm install --save-dev sass

```

### vite plugin

新建目录 vite/plugins
新建文件 index.js
```
import vue from '@vitejs/plugin-vue'


export default function createVitePlugins(viteEnv, isBuild = false) {
  const vitePlugins = [vue()]

  return vitePlugins
}

```
修改 vite.config.js
```import process from 'node:process'
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
    // resolve: { ... },
    // css: { ... },
    // 等等
  })
}
```

#### 开发服务器启动时,在终端中输出信息

```
npm i boxen -D
```

新建app-info.js
```
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
          `${bold(green(`由 ${bgGreen('Fantastic-admin')} 驱动`))}\n\n${underline('https://fantastic-admin.github.io')}\n\n当前使用：${cyan('基础版')}`,
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

```
npm install vite-plugin-banner --save-dev   || yarn add vite-plugin-banner --dev

```

#### vite-plugin-fake-server 模拟后端服务器的响应

```
npm install vite-plugin-fake-server --save-dev
```

新建 mock.js 文件
```
import vue from '@vitejs/plugin-vue'

export default function createVitePlugins(viteEnv, isBuild = false) {
  const vitePlugins = [vue()]
  vitePlugins.push(createBanner())
  return vitePlugins
}

```

安装mock.js
```
npm i mockjs
```

新建mock 文件夹
创建user.js 
```
import { defineFakeRoute } from 'vite-plugin-fake-server/client'
import Mock from 'mockjs'

export default defineFakeRoute([
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

### axios

安装
```
npm i axios
```

新建文件夹 api
新建文件index.js
```

```


### pinia

安装

参考官网 

```
npm install pinia
```

