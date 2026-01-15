---
name: auto-vitepress
description: 自动为当前项目生成或更新 VitePress 文档网站。
---

# 自动生成 VitePress 文档

## 目标
为当前项目在 `/docs` 目录下创建或更新一个 VitePress 文档网站。此过程应具备智能化，能够处理依赖安装、目录扫描、配置生成和文件创建等任务。

## 指令

### 步骤 1: 环境准备

1.  **检查 `package.json`**: 读取项目根目录下的 `package.json` 文件。如果文件不存在，则终止任务并告知用户。
2.  **检查并安装依赖**:
    *   分析 `package.json` 的 `devDependencies` 字段。
    *   检查 `vitepress` 和 `vue` 是否已存在。
    *   如果任一依赖缺失，使用 `npm install -D vitepress vue` 命令进行安装。
3.  **创建目录结构**:
    *   检查项目根目录下是否存在 `/docs/.vitepress` 目录。
    *   如果不存在，请创建它。

### 步骤 2: 智能扫描与侧边栏生成

1.  **扫描 `/src` 目录**:
    *   递归地遍历项目根目录下的 `/src` 目录。如果 `/src` 目录不存在，可以跳过此步骤，生成一个空的侧边栏。
    *   在遍历时，忽略 `node_modules`, `__tests__`, `*.spec.js`, `*.test.js` 等非业务代码文件和目录。
2.  **构建侧边栏对象**:
    *   将子目录映射为可折叠的分组，格式为 `{ text: '目录名', collapsed: false, items: [...] }`。
    *   将 `.js`, `.ts`, `.vue`, `.jsx`, `.tsx` 文件映射为链接项，格式为 `{ text: '文件名（无后缀）', link: '/目录/文件名' }`。
    *   将这个过程的产物构建成一个符合 VitePress `sidebar` 格式的 JavaScript 数组。

### 步骤 3: 生成或更新配置文件

1.  **检查 `config.js`**: 检查 `/docs/.vitepress/config.js` 文件是否存在。
2.  **如果 `config.js` 不存在**:
    *   读取 `package.json` 的 `name` 和 `description` 字段。
    *   创建一个新的 `config.js` 文件，内容如下模板所示，并将上一步生成的 `sidebar` 对象填充进去。
      ```javascript
      import { defineConfig } from 'vitepress'

      export default defineConfig({
        title: '项目名称',
        description: '项目描述',
        themeConfig: {
          sidebar: [/* 在这里填充侧边栏对象 */]
        }
      })
      ```
3.  **如果 `config.js` 已存在**:
    *   读取文件内容。
    *   使用正则表达式或字符串替换的方式，安全地更新 `themeConfig` 中的 `sidebar` 字段。**注意：只更新 `sidebar`，不要覆盖用户的其他配置。**

### 步骤 4: 创建文档页面和 NPM 脚本

1.  **创建占位 `.md` 文件**:
    *   遍历 `sidebar` 对象中的所有链接。
    *   对于每一个链接（如 `/components/Button`），检查对应的文件（`/docs/components/Button.md`）是否存在。
    *   如果不存在，则创建该文件，并写入一个简单的中文标题，例如 `# Button`。
2.  **创建首页**:
    *   检查 `/docs/index.md` 是否存在。
    *   如果不存在，则创建一个，并使用 `package.json` 的 `name` 和 `description` 作为内容。
3.  **添加 NPM 脚本**:
    *   读取 `package.json`。
    *   检查 `scripts` 字段中是否已存在 `docs:dev` 和 `docs:build`。
    *   如果不存在，则添加它们：
      ```json
      "scripts": {
        "docs:dev": "vitepress dev docs",
        "docs:build": "vitepress build docs"
      }
      ```
    *   将更新后的内容写回 `package.json`。

## 总结
完成以上所有步骤后，向用户报告任务完成，并提示他们可以通过运行 `npm run docs:dev` 来启动文档服务器。
