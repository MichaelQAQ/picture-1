# 手机展厅相册

这是一个可以部署到公网 HTTPS 链接的静态网页相册。别人打开你发出去的链接后，可以在自己的手机上选择相册照片、拍照导入、使用实时相机、浏览展厅、下载单张照片或批量 ZIP。

重要说明：当前版本不需要服务器，照片会保存在每个访问者自己的浏览器 IndexedDB 中。也就是说，A 上传的照片不会自动出现在 B 的手机里。如果你想做“所有人上传到同一个公共云相册”，下一步需要加后端接口和云存储。

## 项目文件

- `index.html`：网页主体，包含界面、相册/相机 API、下载和本地保存逻辑。
- `site.webmanifest`：手机添加到主屏幕时使用。
- `sw.js`：缓存网页壳，提升再次打开速度。
- `netlify.toml`：Netlify 静态站点配置。
- `vercel.json`：Vercel 静态站点配置。
- `.github/workflows/pages.yml`：GitHub Pages 自动发布配置。

## 本地预览

```bash
python3 -m http.server 8765 --bind 127.0.0.1
```

然后打开：

```text
http://127.0.0.1:8765/index.html
```

实时相机 API 需要 HTTPS 或 localhost。部署到 Netlify、Vercel、GitHub Pages 后会自动使用 HTTPS。

## 发布成别人能访问的链接

### 方案 1：Netlify

1. 登录 Netlify。
2. 新建站点并连接这个 Git 仓库，发布目录填 `.`。
3. 或者安装 Netlify CLI 后在当前目录运行：

```bash
netlify deploy --prod --dir .
```

部署成功后，Netlify 会给你一个 `https://...netlify.app` 链接。

### 方案 2：Vercel

1. 登录 Vercel。
2. 导入这个 Git 仓库。
3. 保持默认静态站点设置即可。
4. 或者安装 Vercel CLI 后在当前目录运行：

```bash
vercel --prod
```

部署成功后，Vercel 会输出一个 `https://...vercel.app` 链接。

### 方案 3：GitHub Pages

1. 把这个目录推送到 GitHub 仓库。
2. 在仓库 Settings -> Pages 里把 Source 设置为 GitHub Actions。
3. 推送到 `main` 或 `master` 后，`.github/workflows/pages.yml` 会自动发布。
4. 发布完成后，仓库的 Actions 页面会显示正式链接。

## 隐私和共享逻辑

- 用户上传/拍摄的照片只保存在自己的浏览器本地。
- “分享链接”分享的是网页地址，不会把本机照片同步给别人。
- 如果要做多人共享相册，需要再增加登录、上传 API、对象存储和权限控制。
