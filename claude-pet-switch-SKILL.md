---
name: pet-switch
description: 从 Petdex 下载并切换 Claude Pet 桌面宠物的完整流程（Windows版）
---

# Pet Switch - 宠物切换技能

从 Petdex 下载并切换 Claude Pet 桌面宠物。

## 项目位置

- **宠物系统**: `D:\claude-workspace\claude-pet-runtime`
- **下载缓存**: `D:\claude-workspace\temp\<宠物名>\`
- **当前宠物目录**: `D:\claude-workspace\claude-pet-runtime\pets\momo\`
- **Momo 备份**: `D:\claude-workspace\claude-pet-runtime\pets\momo_backup\`

## 快速命令

### 列出可用宠物
```bash
curl -sL "https://petdex.crafter.run/api/manifest" -o temp/petdex_manifest.json
```
然后在 JSON 文件中搜索你想要的宠物名。

### 下载并切换宠物（以 ikun 为例）
```bash
# 1. 获取 ikun 的 URL
# 已在 manifest 中确认:
# spritesheet: https://yu2vz9gndp.ufs.sh/f/hC7hRnEK7mnLawD1UsdusAQm6wIjzlpLVfCaBo2KNnHi3Ph7
# pet.json: https://yu2vz9gndp.ufs.sh/f/hC7hRnEK7mnLV5M3I2dKoT8Z5U9EBqRQHrauvYf2k6b7yXmJ

# 2. 创建目录并下载
mkdir -p D:/claude-workspace/temp/ikun
curl -sL "https://yu2vz9gndp.ufs.sh/f/hC7hRnEK7mnLawD1UsdusAQm6wIjzlpLVfCaBo2KNnHi3Ph7" -o D:/claude-workspace/temp/ikun/spritesheet.webp
curl -sL "https://yu2vz9gndp.ufs.sh/f/hC7hRnEK7mnLV5M3I2dKoT8Z5U9EBqRQHrauvYf2k6b7yXmJ" -o D:/claude-workspace/temp/ikun/pet.json

# 3. 备份当前宠物
mkdir -p D:/claude-workspace/claude-pet-runtime/pets/momo_backup
cp D:/claude-workspace/claude-pet-runtime/pets/momo/* D:/claude-workspace/claude-pet-runtime/pets/momo_backup/

# 4. 复制新宠物
cp D:/claude-workspace/temp/ikun/* D:/claude-workspace/claude-pet-runtime/pets/momo/

# 5. 重启宠物
powershell.exe -Command "Stop-Process -Name electron -Force"
powershell.exe -Command "Start-Process 'C:\Program Files\nodejs\npm.cmd' -ArgumentList 'start' -WorkingDirectory 'D:\claude-workspace\claude-pet-runtime'"
```

### 恢复 Momo
```bash
cp D:/claude-workspace/claude-pet-runtime/pets/momo_backup/* D:/claude-workspace/claude-pet-runtime/pets/momo/
powershell.exe -Command "Stop-Process -Name electron -Force; Start-Process 'C:\Program Files\nodejs\npm.cmd' -ArgumentList 'start' -WorkingDirectory 'D:\claude-workspace\claude-pet-runtime'"
```

## 完整换宠物流程（通用）

### 1. 获取宠物列表和 URL

```bash
curl -sL "https://petdex.crafter.run/api/manifest" -o temp/petdex_manifest.json
```

在 JSON 中找到目标宠物的 `slug`，提取 `spritesheetUrl` 和 `petJsonUrl`。

### 2. 下载文件

```bash
mkdir -p D:/claude-workspace/temp/<宠物名>
curl -sL "<spritesheetUrl>" -o D:/claude-workspace/temp/<宠物名>/spritesheet.webp
curl -sL "<petJsonUrl>" -o D:/claude-workspace/temp/<宠物名>/pet.json
```

### 3. 验证下载

```bash
python3 -c "from PIL import Image; img = Image.open('D:/claude-workspace/temp/<宠物名>/spritesheet.webp'); print('Size:', img.size)"
# 应该输出 (1536, 1872)
```

### 4. 备份并切换

```bash
mkdir -p D:/claude-workspace/claude-pet-runtime/pets/momo_backup
cp D:/claude-workspace/claude-pet-runtime/pets/momo/* D:/claude-workspace/claude-pet-runtime/pets/momo_backup/
cp D:/claude-workspace/temp/<宠物名>/* D:/claude-workspace/claude-pet-runtime/pets/momo/
```

### 5. 重启宠物

```bash
powershell.exe -Command "Stop-Process -Name electron -Force"
powershell.exe -Command "Start-Process 'C:\Program Files\nodejs\npm.cmd' -ArgumentList 'start' -WorkingDirectory 'D:\claude-workspace\claude-pet-runtime'"
```

## 常用宠物 URL（快速使用）

| 宠物 | spritesheetUrl | petJsonUrl |
|------|----------------|------------|
| ikun | https://yu2vz9gndp.ufs.sh/f/hC7hRnEK7mnLawD1UsdusAQm6wIjzlpLVfCaBo2KNnHi3Ph7 | https://yu2vz9gndp.ufs.sh/f/hC7hRnEK7mnLV5M3I2dKoT8Z5U9EBqRQHrauvYf2k6b7yXmJ |

其他宠物请先从 manifest 获取最新 URL。

## 移动宠物窗口到屏幕中央

```powershell
# 保存为 move_pet.ps1 运行
Add-Type -TypeDefinition @"
using System;
using System.Runtime.InteropServices;
public class Win32 {
    [DllImport("user32.dll")]
    public static extern bool SetWindowPos(IntPtr hWnd, IntPtr hWndInsertAfter, int X, int Y, int cx, int cy, uint uFlags);
}
"@

$HWND_TOPMOST = [IntPtr](-1)
$pet = Get-Process electron | Where-Object { $_.MainWindowTitle -eq "Claude Pet" }
if ($pet) {
    [Win32]::SetWindowPos($pet.MainWindowHandle, $HWND_TOPMOST, 672, 328, 192, 208, 0x0040)
}
```

## 故障排除

### 下载失败
检查网络连接，或使用 `all_proxy=""` 避免代理干扰。

### 精灵图加载失败
- 验证文件: `python3 -c "from PIL import Image; print(Image.open('path').size)"`
- 应输出 `(1536, 1872)`

### 宠物不显示
- 重启宠物系统
- 检查 `pets/momo/` 目录下有两个文件: `spritesheet.webp` 和 `pet.json`

### 需要恢复原宠物
```bash
cp D:/claude-workspace/claude-pet-runtime/pets/momo_backup/* D:/claude-workspace/claude-pet-runtime/pets/momo/
```

## 触发词

- "换宠物"
- "切换宠物"
- "换个宠物"
- "下载宠物"
