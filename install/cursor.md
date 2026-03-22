# Cursor / Windsurf 安装

## 方式 1：项目 Rules

将 `SKILL.md` 内容复制到项目规则文件：

```bash
# Cursor
cp SKILL.md .cursor/rules/stop-slop.md

# Windsurf
cp SKILL.md .windsurf/rules/stop-slop.md
```

如果需要参考文件（禁用短语表等），在同目录下创建 `references/` 并复制进去。

## 方式 2：全局 Rules

在 Cursor Settings > Rules 中粘贴 `SKILL.md` 的内容。

## 注意

Cursor 的 rules 文件会自动加载到上下文中。如果参考文件太大影响性能，可以只放 `SKILL.md`，核心规则已经自包含。需要深度改写时再手动引用具体的 `references/` 文件。
