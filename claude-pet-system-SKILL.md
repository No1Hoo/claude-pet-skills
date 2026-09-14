---
name: pet-system
description: Claude Code桌面宠物系统 - 可配置动画、语录、气泡通知
---

# Pet System - 桌面宠物系统

Claude Code 桌面宠物（Momo）的完整使用文档。

## 项目位置

- **主目录**: `D:\claude-workspace\claude-pet-runtime`
- **备份**: `D:\claude-workspace\claude-pet-runtime.backup`
- **脚本**: `D:\claude-workspace\claude-pet-runtime\scripts`

## 启动命令

```bash
cd D:/claude-workspace/claude-pet-runtime && npm start
```

## 功能清单

### 动画状态

| 状态 | 触发 | 动画 |
|------|------|------|
| idle | 无活动 | 呼吸/眨眼 |
| running | 工具执行中 | 奔跑 |
| running-left | 左拖拽 | 向左跑 |
| running-right | 右拖拽 | 向右跑 |
| waiting | 等待用户 | 等待 |
| failed | 执行失败 | 沮丧 |
| review | 待查看 | 审视 |
| waving | 双击/点击 | 挥手 |
| jumping | 点击 | 跳跃 |

### 交互

- **点击**: 随机语录 + 随机动作
- **拖拽**: 根据方向切换跑动动画
- **双击**: 挥手动画
- **右键**: 显示菜单（关闭宠物/退出）

### 气泡通知

- 位置: 宠物正上方（绝对定位）
- 自动跟随窗口移动
- 支持中文消息

### 发送通知命令

```bash
# Bash
pet-notify "消息内容" [状态]

# 示例
pet-notify "正在写代码" running
pet-notify "搞定了！" review
```

### 开机自启

- 快捷方式: `C:\Users\26288\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\ClaudePet.lnk`
- 启动脚本: `D:\claude-workspace\claude-pet-runtime\scripts\startup.bat`

## 文件结构

```
claude-pet-runtime/
├── src/
│   ├── main.js         # Electron主进程
│   ├── preload.js      # IPC桥接
│   ├── renderer.js     # 动画引擎
│   ├── index.html      # 窗口HTML
│   ├── style.css       # 样式
│   └── state.js        # 状态管理
├── config/
│   └── animation.json  # 动画配置
├── pets/
│   └── momo/           # 当前宠物(Momo)
│       ├── pet.json
│       └── spritesheet.webp
└── scripts/
    ├── pet-notify.sh   # 发送通知
    ├── notify.ps1       # PowerShell版
    └── startup.bat     # 开机自启
```

## 状态文件

- 路径: `C:\Users\26288\.claude\claude-pet\state.json`
- 日志: `C:\Users\26288\.claude\claude-pet\logs\hook.log`

## 修改语录

编辑 `src/renderer.js` 中的 `RANDOM_QUOTES` 数组。

## 修改动画

编辑 `config/animation.json` 中的状态配置。

## 常见问题

- **气泡漂移**: 确保 `style.css` 使用 `position: absolute`
- **右键菜单不显示**: 检查 `renderer.js` 中 `contextmenu` 事件监听
- **开机不自启**: 检查 `startup.bat` 路径是否正确

## 相关链接

- **换宠技能**: [pet-switch](pet-switch/SKILL.md) - 从 Petdex 下载并切换宠物
- **Petdex 宠物库**: https://petdex.crafter.run
