# askills

![NPM Version](https://img.shields.io/npm/v/askills?style=flat-square)

可通过 askills 命令安装内置 skills 到当前项目或者全局目录。

- [Github](https://github.com/anghunk/askills)
- [文档地址](https://askills.pages.dev)
- [NPM Package](https://www.npmjs.com/package/askills)

## 1. 安装

```bash
# npm install
npm install -g askills

# 或使用 npx 直接运行（无需安装）
npx askills ls
```

## 2. 使用方法

### (1) 通过 Cli 安装 Skills

```bash
# 列出所有可用 Skills
askills ls

# 安装 Skills 到当前项目
askills install <name>

# 全局安装（所有项目可用 ~/.claude）
askills install <name> -g

# 卸载Skills
askills uninstall <name>
```

### (2) 手动安装

将 `skills` 目录下你需要的 Skills 工具放在项目或者全局的 `.claude/skills/` 路径下即可生效。

参考文档：[Claude Code Skills 官方文档](https://code.claude.com/docs/zh-CN/skills)

## 3. 命令说明

| 命令                          | 说明                           |
| ----------------------------- | ------------------------------ |
| `askills ls` / `askills list` | 列出所有可用 Skills            |
| `askills install <name>`      | 安装 Skills 到当前项目         |
| `askills install <name> -g`   | 全局安装到 `~/.claude/skills/` |
| `askills uninstall <name>`    | 卸载 Skills                    |

## 4. 可用 Skills

详情请参考每个 skill 的 `SKILL.md`：

| Skills               | 说明                                    |
| -------------------- | --------------------------------------- |
| `git-commit-message` | 自动生成符合 Git 提交规范的提交信息     |
| `auto-vitepress`     | 自动为当前项目生成或更新 VitePress 文档 |

## 5. License

Apache License 2.0
