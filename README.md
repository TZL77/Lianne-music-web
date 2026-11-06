# Lianne Music Web（iPhone / 手机端）

Lianne Music 的网页版 + 音乐 API，一套代码同时部署到 Vercel：
页面是 PWA（可添加到 iPhone 主屏幕），API 是 serverless 函数，**电脑关机也能听歌**。

## 目录结构

```
api/[[...path]].js   音乐 API（Vercel Serverless，包装 netease-cloud-music-api）
public/              手机端页面（构建好的静态文件，含粉色主题）
local-server.js      本地预览用的小服务器（不参与线上部署）
vercel.json          路由规则：静态文件优先，其余请求交给 API
```

## 部署步骤（约 5 分钟）

### 1. 推到 GitHub

先在 GitHub 网页新建一个空仓库（不要勾选 README / .gitignore），名字随意，例如 `lianne-music-web`。

然后在本机这个目录执行（把 `<你的用户名>` 换成 GitHub 用户名）：

```bash
cd D:/soft/music/lianne-web
git remote add origin https://github.com/<你的用户名>/lianne-music-web.git
git branch -M main
git push -u origin main
```

弹出登录框时 **不能用账号密码**，需要 Personal Access Token：
GitHub → 右上角头像 → Settings → Developer settings → Personal access tokens →
Tokens (classic) → Generate new token → 勾选 `repo` → 生成后复制，粘贴到密码框。

### 2. 导入 Vercel

1. 打开 https://vercel.com ，用 **GitHub 账号登录**（Continue with GitHub）
2. Add New… → Project → 找到 `lianne-music-web` → Import
3. 不用改任何设置，直接 Deploy（构建配置已写在 `vercel.json` 里）
4. 约 1 分钟后得到地址，如 `https://lianne-music-web.vercel.app`

### 3. 装到 iPhone 桌面上

Safari 打开上面的地址 → 底部「分享」按钮 → **添加到主屏幕** → 添加。
之后从桌面图标打开就是全屏、无地址栏的样式；锁屏和控制中心都有播放控制。

## 本地预览（可选）

```bash
cd D:/soft/music/lianne-web
npm install
node local-server.js
```

浏览器打开 http://localhost:30489 。手机和电脑同一 WiFi 时，用手机访问
`http://<电脑局域网IP>:30489` 也能直接试。

## 已知限制

- **VIP / 版权歌曲需要登录**：页面上用「扫码登录」登录网易云账号后即可正常播放；
  未登录时只有免费歌曲能出声。
- Vercel 免费节点在海外：少数歌曲会显示无版权/无法播放，属于地区限制，不是 bug。
- 首次访问或长时间不用后，第一个请求会慢几秒（serverless 冷启动）。
- 下载到手机受 iOS 限制，只能存进「文件」App，不能直接进系统音乐库。

## 以后怎么更新

改完页面文件（`public/`）推一次 GitHub，Vercel 会自动重新部署；
也可以在 Vercel 面板点 Redeploy。
