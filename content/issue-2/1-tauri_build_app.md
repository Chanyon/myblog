##### 一、Tauri介绍

What is Tauri? \
Tauri is a toolkit that helps developers make applications for \
the major desktop platforms - using virtually any frontend framework \
in existence. The core is built with Rust, and the CLI leverages Node.js \
making Tauri a genuinely polyglot approach to creating and maintaining great apps. \
[Tauri](https://tauri.app)


##### 二、快速开始

[HTML/CSS/JS](https://tauri.app/zh-cn/v1/guides/getting-started/setup/html-css-js)


##### 三、初始化项目

```
使用 pnpm init 初始化
安装vite pnpm add vite
修改script
"scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
  },
```

##### 四、其他插件

```
用来复制需要的文件到public文件夹vite打包时会一起打包到dist目录
pnpm add rollup-plugin-copy

vite.config.js中引入
import copy from 'rollup-plugin-copy'
export default defineConfig({
  plugins: [
    copy({
      targets: [
        { src: "./assets/*", dest: "./public/assets" }
      ],
      verbose: true,
      copyOnce: true,
    })
  ]
})
```
