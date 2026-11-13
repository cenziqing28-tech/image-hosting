# image-hosting

个人图片资源仓库，用于存放博客 / 文档 / 网页中引用的图片，并通过 GitHub 原始链接（raw）直接外链。

## 目录结构

```
image-hosting/
├── README.md
├── images/
│   ├── common/          # 通用、长期复用的图片（logo、图标、背景图等）
│   └── 2026/
│       └── 11/          # 按年月分类：images/YYYY/MM/
└── assets/
    └── icons/           # 图标类资源（SVG / PNG 小图标）
```

> 说明：Git 不跟踪空目录，所以每个目录下放了一个 `.gitkeep` 占位文件。上传第一张图片后可以保留或删除它。

## 链接规则

假设仓库地址为 `https://github.com/cenziqing28-tech/image-hosting`，默认分支为 `main`。

| 用途 | URL 格式 | 示例 |
| --- | --- | --- |
| 网页浏览（带 GitHub 界面） | `https://github.com/<用户名>/<仓库>/blob/main/<文件路径>` | `https://github.com/cenziqing28-tech/image-hosting/blob/main/images/common/logo.png` |
| 原始文件直链（raw） | `https://raw.githubusercontent.com/<用户名>/<仓库>/main/<文件路径>` | `https://raw.githubusercontent.com/cenziqing28-tech/image-hosting/main/images/common/logo.png` |
| jsDelivr CDN 加速（国内推荐） | `https://cdn.jsdelivr.net/gh/<用户名>/<仓库>@main/<文件路径>` | `https://cdn.jsdelivr.net/gh/cenziqing28-tech/image-hosting@main/images/common/logo.png` |
| Markdown 插图 | `![说明](raw 直链)` | `![logo](https://raw.githubusercontent.com/cenziqing28-tech/image-hosting/main/images/common/logo.png)` |
| HTML 引用 | `<img src="raw 直链">` | `<img src="https://cdn.jsdelivr.net/gh/cenziqing28-tech/image-hosting@main/images/common/logo.png">` |

### 重要注意事项

1. **只有公开仓库**的 raw 链接才能被外部无认证访问；私有仓库的链接需要 token。
2. **区分大小写**：`Logo.PNG` 与 `logo.png` 是两个不同文件，引用时必须完全一致。
3. **路径避免空格和中文**，建议全小写 + 连字符（如 `profile-2026-11.png`），否则 URL 会被百分号编码，部分平台解析异常。
4. 文件名建议带上时间戳或语义，避免覆盖导致 CDN 缓存不更新：`banner-20261104.png`。
5. GitHub 单文件上限 100 MB，仓库建议控制在 1 GB 以内；图片建议先压缩（WebP / 压缩后的 PNG、JPG）。
6. 用 `cdn.jsdelivr.net` 时可用 `@main` 或具体 commit/tag（如 `@v1`）锁定版本；jsDelivr 缓存更新存在延迟，换图后可改用新文件名。
7. 分支不是 `main` 时，把链接里的 `main` 换成实际分支名。

## 图片规格规范

**所有上传图片统一输出：1000×1000 正方形，JPG 格式（quality 95）。**

| 源图类型 | 处理方式 |
| --- | --- |
| 横/竖版白底图（尺码表、商品图） | 白底居中补边成正方形 → 缩放到 1000×1000，不裁剪、不拉伸 |
| 已是 1:1 的图（1728²、2048² 等） | 直接缩放到 1000×1000 |
| 竖版场景图（非白底） | 裁剪为 1:1（头部贴顶时顶部起裁，避免切脸）→ 缩放到 1000×1000 |
| PNG（含透明通道） | 先合成白底，再转 JPG 并缩放到 1000×1000 |

命名规则：`images/YYYY/MM/<款号小写>-<后缀>-YYYYMMDD.jpg`，同款多图用源文件 hash 前 6 位或数字 ID 区分，不覆盖历史文件（避免破坏已引用链接）。

## 上传方式

```bash
# 1. 把图片放进对应目录
cp ~/Pictures/logo.png images/common/

# 2. 提交并推送
git add .
git commit -m "add: logo.png"
git push

# 3. 生成链接（把路径替换为实际文件路径）
echo "https://raw.githubusercontent.com/cenziqing28-tech/image-hosting/main/images/common/logo.png"
```
