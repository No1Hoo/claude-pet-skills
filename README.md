# Claude Pet Skills

Claude Code 桌面宠物系统的 Skills 集合，包括宠物使用和换宠物流程。

## Skills

### [claude-pet-system](claude-pet-system-SKILL.md)

桌面宠物系统（Momo）的完整使用文档。

功能：
- 9种动画状态（idle/running/waiting/failed/review/waving/jumping等）
- 点击随机语录+动作
- 拖拽方向识别
- 气泡通知
- 右键菜单
- 开机自启

### [claude-pet-switch](claude-pet-switch-SKILL.md)

从 Petdex 下载并切换 Claude Pet 桌面宠物的完整 Windows 流程。

功能：
- 列出可用宠物
- 下载宠物精灵图
- 切换到新宠物
- 恢复原宠物

## 快速开始

### 启动宠物

```bash
cd D:/claude-workspace/claude-pet-runtime && npm start
```

### 发送通知

```bash
pet-notify "消息" [状态]
```

### 切换宠物

下载并切换到新宠物（例如 ikun）。

## 相关项目

- [claude-pet-runtime](https://github.com/browser-use/claude-pet-runtime) - 桌面宠物系统核心
- [Petdex](https://petdex.crafter.run) - 宠物精灵图库
