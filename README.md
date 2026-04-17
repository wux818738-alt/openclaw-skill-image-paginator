# 🖼️ Image Paginator（律师证据长截图转PDF）

> OpenClaw Skill — 将长截图/长图智能分页为规范 PDF，支持网格切片、页码标注、微信证据排版

---

## ⚖️ License | 许可证

本技能源码采用 **GNU GPL-3.0** 许可证。完整文本见 [LICENSE](LICENSE) 文件。

> ⚠️ **ClawHub 显示说明**：ClawHub 平台目前仅支持 MIT-0 展示，该页面显示 MIT-0 为平台限制，不影响本项目的实际许可证。
> 下载安装本技能即表示同意遵守 [GPL-3.0](LICENSE) 的条款。

---
---

## 封面效果

![封面](https://raw.githubusercontent.com/wux818738-alt/openclaw-skill-image-paginator/main/cover.png)

---

## 它能做什么？

| 场景 | 输入 | 输出 |
|------|------|------|
| 🏛️ **律师 / 法院** | 微信长截图、聊天记录、合同附件 | A4 规范 PDF，带页码，可直接打印提交 |
| 👨💻 **程序员** | 代码截图、API 文档截图 | 2×2 网格 PDF，便于 Code Review |
| 📋 **办公** | 邮件截图、长表格截图 | 带间距 PDF，翻页不串行 |
| 📱 **社交** | 小红书、微博长图 | 手机竖图 → 规范 PDF 保存 |

---

## 效果预览

### 📊 适用场景总览
![场景总览](https://raw.githubusercontent.com/wux818738-alt/openclaw-skill-image-paginator/main/diagram_scenarios.png)

### 🏛️ 律师场景：输入 → 输出
![律师场景对比](https://raw.githubusercontent.com/wux818738-alt/openclaw-skill-image-paginator/main/diagram_lawyer_beforeafter.png)

---

## 安装

### 方法一：一键安装（推荐）

```bash
curl -fsSL https://raw.githubusercontent.com/wux818738-alt/openclaw-skill-image-paginator/main/install.sh | bash
```

### 方法二：手动安装

```bash
# 1. 克隆仓库
git clone https://github.com/wux818738-alt/openclaw-skill-image-paginator.git
cd openclaw-skill-image-paginator

# 2. 安装 Python 依赖
pip3 install fpdf2 Pillow

# 3. 运行
python scripts/slice_n_pdf.py <图片路径> -d <输出目录> -o <文件名.pdf> --clean
```

---

## 快速上手

### 单张长图

```bash
python scripts/slice_n_pdf.py ~/Desktop/聊天记录.jpg \
  -d ~/Desktop -o 聊天证据.pdf --clean
```

### 多张图片（自动拼接）

```bash
python scripts/slice_n_pdf.py \
  ~/Desktop/截图1.jpg \
  ~/Desktop/截图2.jpg \
  ~/Desktop/截图3.jpg \
  -d ~/Desktop -o 完整证据.pdf --clean
```

### 自定义网格（1 列 3 行）

```bash
python scripts/slice_n_pdf.py ~/Desktop/长图.jpg \
  -d ~/Desktop --cols 1 --rows 3 --clean
```

---

## 参数说明

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `--tile` | 2000 | 每格切片高度（像素） |
| `--bleed` | 200 | 上下切片重叠高度（像素） |
| `--cols` | 2 | 每页网格列数（1-10） |
| `--rows` | 2 | 每页网格行数（1-10） |
| `--gutter` | 40 | 格子间距（像素） |
| `--edge` | 25 | 页面边距（像素） |
| `--no-numbers` | — | 关闭页码 |
| `--clean` | — | 生成后清理临时文件（推荐） |

> ⚠️ 自动校验：tile > bleed、cols/rows 在 1-10 之间、文件存在性

---

## OpenClaw 用户

本 skill 已上架 OpenClaw SkillHub，在 OpenClaw 对话中直接说「安装 image-paginator」即可自动安装。

---

## 技术栈

- **Python 3** + PIL/Pillow（图像处理）
- **fpdf2**（PDF 生成，无需外部依赖）
- 纯本地运行，不上传任何数据

---

## License

GPL-3.0

---

## 更新日志

### 2026-04-17 (v2.1)
- 🏷️ 技能改名：Image Paginator → Image Paginator（律师证据长截图转PDF）
- 📝 SKILL.md description 新增律师证据场景标识
- ✨ Key Features 新增参数校验功能说明
- 📌 版本号升级 2.0 → 2.1

### 2026-04-15 (v2.0)
- ✨ 添加参数校验功能：检查 tile/bleed/cols/rows 等参数合法性
- 🐛 修复 install.sh 中的变量名错误：`$REPO_URL` → `$ZIP_URL`
- 💡 优化错误提示：参数错误时显示清晰的错误信息
