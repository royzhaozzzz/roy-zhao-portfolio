# Roy Zhao 摄影作品集 — 维护工作流程

> 最后更新：2026年6月8日

---

## 1. 项目概览

| 项目 | 信息 |
|------|------|
| 网站地址 | https://royzhao.cn |
| 备用域名 | https://royzhao.top（待配置） |
| GitHub 仓库 | https://github.com/royzhaozzzz/roy-zhao-portfolio |
| 数据库 | Firebase Firestore（免费） |
| 托管 | GitHub Pages |
| 站长密码 | 可在代码中修改 |

---

## 2. 文件结构

```
项目根目录/
├── index.html          # ⭐ 部署版本（线上访问的就是这个）
├── web copy.html       # 🧪 本地测试版（修改先在这里预览）
├── web.html            # 📦 存档备份（不动）
├── admin.html          # 🎛️ 可视化编辑后台
├── CHANGELOG.md        # 更新日志
├── WORKFLOW.md         # 本文件
├── .gitignore
└── images/
    ├── favicon.png     # 网页图标
    ├── URC_2374.jpg    # Hero 大图
    ├── ROY_8128_01.jpg # 关于我头像
    └── ...             # 底片册照片（12张）
```

> 🔴 **重要**：`index.html` 是线上版本，`web copy.html` 是本地测试版。修改时先在 `web copy.html` 试，确认无误再复制到 `index.html`。

---

## 3. 日常内容更新（推荐方式）

### 方式 A：可视化后台编辑

1. 浏览器打开 `admin.html`
2. 切换到对应标签编辑内容
3. 数据自动保存到浏览器 localStorage
4. 打开 `web copy.html` 预览效果

### 方式 B：直接改代码

1. 用 VS Code 打开 `web copy.html`
2. 找到你要改的部分直接修改
3. 浏览器打开 `web copy.html` 预览

---

## 4. 部署流程（更新线上网站）

### 第 1 步：确认修改

在 `web copy.html` 里确认改好了，本地浏览器看过了。

### 第 2 步：复制到部署文件

**Windows（PowerShell）：**
```powershell
copy "h:\PROJECT\web\web copy.html" "h:\PROJECT\web\index.html"
```

**Mac（终端）：**
```bash
cp "~/Desktop/web/web copy.html" "~/Desktop/web/index.html"
```

### 第 3 步：推送上线

**Windows：**
```powershell
cd h:\PROJECT\web
git add index.html
git commit -m "描述你的改动"
git push
```

**Mac：**
```bash
cd ~/Desktop/web
git add index.html
git commit -m "描述你的改动"
git push
```

### 第 4 步：等待生效

推送后约 30 秒，刷新 https://royzhao.cn 即可看到更新。

---

## 5. 从 Mac 开始维护（首次设置）

```bash
# 1. 克隆仓库
cd ~/Desktop
git clone https://github.com/royzhaozzzz/roy-zhao-portfolio.git

# 2. 进入目录
cd roy-zhao-portfolio

# 3. 复制测试文件（如果仓库里没有 web copy.html）
cp index.html "web copy.html"

# 4. 开始编辑
open web copy.html
```

### 设置 Git 身份

```bash
git config --global user.name "royzhaozzzz"
git config --global user.email "你的邮箱"
```

### 多电脑同步

```bash
# 编辑前：拉取最新代码
git pull

# 编辑后：推送
git add index.html
git commit -m "更新"
git push
```

> ⚠️ 两台电脑间切换前，务必先 `git pull` 拉取最新代码，否则会冲突。

---

## 6. 图片管理

### 添加新图片

1. 把图片放到 `images/` 文件夹
2. 压缩图片（避免页面太慢）：
```powershell
# Windows（需要 Python + Pillow）
python -c "from PIL import Image; img=Image.open('images/新图片.jpg'); ratio=1600/max(img.size); img=img.resize((int(img.size[0]*ratio),int(img.size[1]*ratio))); img.save('images/新图片.jpg','JPEG',quality=82)"
```
3. 在 `web copy.html` 的 `photos` 数组中添加条目

### 图片规格建议

| 用途 | 尺寸 | 格式 |
|------|------|------|
| Hero 大图 | 1600×1066 | JPG |
| 底片册 | 最长边 1600px | JPG |
| 头像 | 1600×1600 | JPG |
| 网站图标 | 64×64 | PNG |

---

## 7. 留言管理

### 查看留言

- **线上**：进入 ROYZHAO 管理模式（点击留言区 "ROYZHAO" → 输入密码）
- **后台**：`admin.html` → "留言管理" 标签 → 可查看和删除

### 删除留言

- **站长模式**：点进留言卡片 → 删除按钮（可删任何留言）
- **登录用户**：只能删自己发的留言
- **后台**：`admin.html` 留言管理 → 逐条删除或清空

---

## 8. 数据库

### Firebase Console

- 地址：https://console.firebase.google.com/project/my-site-6ad7d/firestore/data
- 可在此查看所有留言、用户数据
- 集合说明：
  - `guestbook` → 留言
  - `guestbook/{id}/replies` → 回复
  - `users` → 注册用户

### Firestore 安全规则

去 https://console.firebase.google.com/project/my-site-6ad7d/firestore/rules

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} { allow read, write: if true; }
    match /guestbook/{docId} {
      allow read, create, update, delete: if true;
      match /replies/{replyId} {
        allow read, create, delete: if true;
      }
    }
  }
}
```

---

## 9. 管理员功能速查

| 功能 | 如何操作 |
|------|---------|
| 编辑文字/照片 | `admin.html` 可视化编辑 |
| 查看私密留言 | 网页留言区 → ROYZHAO → 输入密码 |
| 置顶留言 | ROYZHAO 模式 → 点卡片 → 📌 置顶 |
| 删除留言 | 后台 `admin.html` → 留言管理 |
| 重置某人密码 | 登录弹窗 → 忘记密码 → 输入昵称+新密码 |
| 换管理员密码 | 搜索代码里的 `Zrh20060727` 改为新密码 |

---

## 10. 域名相关

### 当前域名

| 域名 | 状态 |
|------|------|
| royzhao.cn | ✅ 已启用 |
| royzhao.top | ⏳ 待配置跳转 |
| royzhaozzzz.github.io/roy-zhao-portfolio | ✅ 备用 |

### 添加新域名

1. 在域名 DNS 添加 4 条 A 记录指向 GitHub Pages IP
2. GitHub → Settings → Pages → Custom domain 填入域名
3. 等待证书生成 → 勾选 Enforce HTTPS

### 域名 DNS 设置

GitHub Pages IP（A 记录）：
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

---

## 11. 常见问题

**Q: 改完内容线上没变化？**  
A: 检查是否把 `web copy.html` 复制到了 `index.html`，然后 `git push`。

**Q: 图片不显示？**  
A: 检查图片文件名大小写，本地图片路径是 `images/xxx.jpg`。

**Q: 留言功能国内不能用？**  
A: Firebase 是 Google 服务，国内需 VPN。暂未找到免费替代。

**Q: 网站显示"不安全"？**  
A: GitHub → Settings → Pages → Enforce HTTPS 勾上。

**Q: push 失败说 rejected？**  
A: 先 `git pull --rebase origin master` 再 `git push`。

**Q: 怎样改管理员密码？**  
A: 在 `web copy.html` 和 `index.html` 里搜索 `Zrh20060727`，替换为新密码。

---

## 12. 快捷命令汇总

```powershell
# Windows
copy "h:\PROJECT\web\web copy.html" "h:\PROJECT\web\index.html"
cd h:\PROJECT\web && git add index.html && git commit -m "更新" && git push
```

```bash
# Mac
cp ~/Desktop/web/web\ copy.html ~/Desktop/web/index.html
cd ~/Desktop/web && git add index.html && git commit -m "更新" && git push
```
