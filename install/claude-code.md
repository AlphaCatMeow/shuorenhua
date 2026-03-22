# Claude Code 安装

## 方式 1：项目级（推荐）

将 skill 文件复制到项目的 `.claude/` 目录：

```bash
# 在项目根目录下执行
mkdir -p .claude/skills/stop-slop-zh
cp SKILL.md .claude/skills/stop-slop-zh/
cp -r references/ .claude/skills/stop-slop-zh/
```

然后在项目的 `CLAUDE.md` 中添加引用：

```markdown
## 写作风格
对外文本遵循 `.claude/skills/stop-slop-zh/SKILL.md` 的反模式规则。
```

## 方式 2：全局

将 skill 复制到全局配置：

```bash
mkdir -p ~/.claude/skills/stop-slop-zh
cp SKILL.md ~/.claude/skills/stop-slop-zh/
cp -r references/ ~/.claude/skills/stop-slop-zh/
```

## 使用

安装后 Claude Code 会自动识别 skill。你可以：

- 在对话中说"用 stop-slop 改写这段文本"
- 或者在 CLAUDE.md 中设置默认启用
