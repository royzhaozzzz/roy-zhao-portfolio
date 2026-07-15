# Roy Zhao 摄影作品集 — 完整项目文档

> 最后更新：2026年6月23日

---

## 1. 项目概览

| 项目 | 信息 |
|------|------|
| 网站地址 | https://royzhao.cn |
| GitHub 仓库 | https://github.com/royzhaozzzz/roy-zhao-portfolio |
| 托管 | GitHub Pages |
| 数据库 | Firebase Firestore（仅留言） |
| 技术栈 | 单文件 HTML + Tailwind CSS CDN + 原生 JS |

---

## 2. 文件结构

```
项目根目录/
├── index.html           # ⭐ 部署版本（线上访问的就是这个）
├── web se.html          # ✏️ 主力编辑版（所有修改在这里做）
├── web copy.html        # 📦 备份存档（.gitignore 不推送）
├── admin.html           # 🎛️ 可视化编辑后台
├── web.md               # 本文件
├── CHANGELOG.md         # 更新日志
├── WORKFLOW.md          # 旧版工作流程
├── .gitignore
└── images/
    ├── favicon.png
    ├── URC_2374.jpg     # Hero 封面
    ├── ROY_8128_01.jpg  # 关于我头像
    └── ...              # 15 张底片册照片
```

---

## 3. 网站板块结构

从上到下共 6 个板块：

### 3.1 导航栏
- 固定在顶部，黑色毛玻璃背景
- 品牌 logo `zᶻ`（点击登录）
- 4 个跳转按钮：底片册 | 目的地 | 留影 | 关于
- 中/英语言切换
- 平滑缓动滚动（900ms cubic ease-in-out）

### 3.2 Hero 首页
- 左边：标签 + 标题 + 引言
- 右边：宝丽来相框包裹的封面大图
- 底部标注：地点 + 相机参数
- 所有文字支持中英双语
- 数据来源：`i18n['hero.*']` 对象，admin 可覆盖

### 3.3 底片册（Gallery）
- 响应式 CSS Grid（1/2/3 列）
- 每页 6 张，分页导航
- 分类筛选：全部 / 数码 / 胶片
- 卡片：宝丽来风格，hover 放大 + 浏览按钮
- 点击卡片 → 遮罩灯箱（大图 + EXIF + 拍摄手记）
- 翻页动画：淡出 → 替换 → 卡片依次飘入（0.4s）
- 数据来源：`photos[]` 数组（15 张），admin 可覆盖

### 3.4 目的地（Destinations）
- 横向方波时间轴，卡片交替上下排列
- SVG 直角连线 + 圆点标记
- 3 个目的地：甘南 (2022) → 甘孜 (2025) → 新疆 (2026)
- 点击卡片 → 18% 灰遮罩（`#7c7c7c`）
- 遮罩内：左侧 6:7 照片拼图 + 右侧游记内容（HTML）
- 数据来源：`destinations[]` 数组，admin 可覆盖

### 3.5 留影簿（Guestbook）
- 访客留言板，Firebase Firestore 实时存储
- 留言卡片：可选暖白/焦糖/蓝晒三种相纸色
- 登录/注册系统
- 站长模式（ROYZHAO 密码登录）
- 置顶、删除留言
- 每 5 秒自动拉取新留言
- 翻页动画（同底片册）

### 3.6 关于我（About）
- 左边：双层旋转宝丽来肖像
- 右边：自我介绍 + 设备/底片/地区/学校信息
- 联系方式：Email、微信
- 社交链接：抖音、小红书、QQ音乐、微博

### 3.7 页脚
- 版权声明 + 备用域名链接

---

## 4. 后台管理系统（admin.html）

### 4.1 6 个 Tab

| Tab | 功能 |
|-----|------|
| Hero 首页 | 编辑封面图路径、标签、标题、引语、地点标注、器材标注 |
| 底片册 | 照片 CRUD：增删改排序拖拽，编辑 EXIF/描述 |
| 关于我 | 编辑自我介绍、设备、底片、地区、学校、Email、微信、社交链接 |
| 留影簿 | 编辑板块标题和标签 |
| 留言管理 | Firestore 留言查看/删除 |
| 目的地 | 目的地 CRUD：名称、日期、封面图、筛选词、游记 HTML |

### 4.2 数据流

```
admin.html（可视化编辑）
    │
    ▼ localStorage（roy_zhao_admin_data）
    │
    ▼
web se.html（admin bridge IIFE 读取）
    │
    ├─ Hero → i18n 对象 + DOM
    ├─ Photos → photos[] 数组
    ├─ About → i18n + DOM
    ├─ Guestbook → i18n
    └─ Destinations → destinations[] 数组
```

### 4.3 保存机制

- **自动保存**：修改后 800ms 自动写入 localStorage
- **手动保存**：Cmd+S 立即强制保存
- **导出 JSON**：下载完整数据备份
- **重置**：恢复默认值

### 4.4 admin → 源码同步

admin 改完只在浏览器 localStorage 里，源代码不会自动更新。需要：

1. admin 里改满意 → 刷新 web se.html 确认效果
2. 把改动告诉我，或导出的 JSON 给我
3. 我写入 web se.html 源码
4. git push 上线

---

## 5. 技术架构

### 5.1 前端
- **单文件架构**：一个 HTML 文件包含所有 CSS + JS
- **Tailwind CSS**：CDN 运行时加载，无需构建
- **FontAwesome 6.4**：图标
- **Google Fonts**：Noto Serif SC 思源宋体
- **自定义 CSS**：暗房胶片风暗角效果、翻页动画、节日特效

### 5.2 后端 / 存储
- **GitHub Pages**：静态托管
- **Firebase Firestore**（v9 compat）：仅存储留言
  - `guestbook` 集合：留言
  - `guestbook/{id}/replies` 子集合：回复
  - `users` 集合：注册用户
- **localStorage**：网站内容数据（Hero、Photos、About、Destinations）

### 5.3 i18n 多语言
- `i18n` 对象包含所有中英翻译
- `data-i18n` 属性标记需要翻译的元素
- `t(key)` 函数根据 `currentLang` 返回对应语言
- 语言切换：`toggleLang()` → `applyLang()` → 重渲染画廊和目的地

---

## 6. 关键功能

### 6.1 生日特效
- 7月27日：全屏模糊遮罩 + SVG 蛋糕 + 瀑布 emoji
- 配置：`HOLIDAY_CONFIG` 数组
- 预览：注释掉日期判断即可每天显示

### 6.2 底片册分页
- 每页 6 张，`GALLERY_PAGE_SIZE = 6`
- 15 张 → 3 页（6 + 6 + 3）
- 分页 UI 复用留言册样式

### 6.3 翻页动画
- 底片册 + 留影簿：淡出 0.2s → 替换 → 卡片依次飘入 0.35s

### 6.4 平滑滚动
- 自定义 900ms cubic ease-in-out
- 偏移 70px 避开导航栏

---

## 7. 工作流程

### 方式 A：直接编辑源码

```
1. 编辑 web se.html
2. 浏览器预览
3. 确认无误后：
   cp "web se.html" index.html
   git add index.html
   git commit -m "..."
   git push
   cp "web se.html" "web copy.html"
```

### 方式 B：admin 可视化编辑

```
1. 打开 admin.html → 编辑内容
2. 刷新 web se.html 确认效果
3. 导出 JSON / 告诉我改动
4. 我写入源码 → git push
```

### 一键 Push 命令

```bash
cd "/Users/mac/项目/web" && cp "web se.html" index.html && git add index.html && git commit -m "更新" && git push && cp "web se.html" "web copy.html"
```

---

## 8. 常用搜索标记

在 `web se.html` 中搜索这些关键字快速定位：

| 关键字 | 位置 |
|--------|------|
| `HOLIDAY_CONFIG` | 生日特效配置 |
| `const photos = [` | 底片册照片数据 |
| `const destinations = [` | 目的地数据 |
| `GALLERY_PAGE_SIZE` | 分页大小 |
| `currentLocationFilter` | 地点筛选 |
| `data-admin` | admin 桥接标记 |
| `// ========== Admin Data Bridge` | admin 数据同步逻辑 |
| `buildConnectorSVG` | 方波时间轴连线 |
| `openDestination` | 目的地遮罩打开 |
| `Zrh20060727` | 管理员密码 |

---

## 9. 图片规格

| 用途 | 尺寸 | 格式 | 最长边 |
|------|------|------|--------|
| Hero 大图 | 16:10 比例 | JPG | 1600px |
| 底片册照片 | 不限 | JPG | 1600px |
| 头像 | 正方形 | JPG | 1600px |
| 网页图标 | 64×64 | PNG | — |

---

## 10. 域名

| 域名 | 状态 |
|------|------|
| royzhao.cn | ✅ 已启用 |
| royzhaozzzz.github.io/roy-zhao-portfolio | ✅ 备用 |
| royzhao.top | ⏳ 待配置 |

GitHub Pages IP（A 记录）：
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```
