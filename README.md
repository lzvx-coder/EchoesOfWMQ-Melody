# wmq 的个人专辑

这是一个纯静态个人音乐专辑页面。打开 `index.html` 后，可以点击曲目播放同一文件夹中的 MP3；GitHub Pages 地址为：

<https://lzvx-coder.github.io/EchoesOfWMQ-Melody/>

## 当前曲目

1. `trust me.mp3`
2. `怎样.mp3`
3. `难过233秒.mp3`
4. `恋爱困难少女.mp3`
5. `世界正中.mp3`
6. `这世界也是我的家.mp3`
7. `巴拉莱卡.mp3`
8. `完美先生和差不多小姐.mp3`
9. `偶尔.mp3`
10. `失落沙洲.mp3`
11. `春夜坠落.mp3`
12. `病变.mp3`
13. `第57次取消发送.mp3`
14. `花园.mp3`
15. `过江寒.mp3`
16. `阴天.mp3`
17. `飞鸟和蝉.mp3`
18. `惧高症.mp3`
19. `你的名字.mp3`
20. `唯一.mp3`
21. `夜奔.mp3`
22. `青花瓷.mp3`

## 更新歌曲的代码操作

### 1. 放入或删除音频

把新的 `.mp3` 文件放到 `index.html` 所在目录。如果某首歌不再需要，也应同时删除它在 `index.html` 中对应的曲目代码，避免页面出现无法播放的按钮。

建议文件名保持简洁，并保留 `.mp3` 扩展名。文件名、大小写和空格必须与代码中的 `data-src` 完全一致。

### 2. 修改 `index.html`

在 `<ul class="tracks">` 和 `</ul>` 之间，为每首新歌增加一个 `<li>`。例如添加第 23 首《示例歌曲》：

```html
<li>
  <button type="button"
          data-src="示例歌曲.mp3"
          data-title="示例歌曲"
          data-note=""
          aria-pressed="false">
    <span class="track-number">23</span>
    <span class="track-title">示例歌曲</span>
    <span class="track-state" aria-hidden="true">▶</span>
  </button>
</li>
```

字段说明：

- `data-src`：实际 MP3 文件名，必须包含 `.mp3`。
- `data-title`：播放器状态栏显示的歌曲名称。
- `data-note`：点击歌曲后显示的短句，不需要时可以留空。
- `track-number`：两位曲目序号，例如 `01`、`09`、`23`。
- `track-title`：歌曲列表中显示的名称。

添加或删除歌曲后，应重新检查编号是否连续，并同步修改本 README 的“当前曲目”。页面现有 JavaScript 会自动识别所有带 `data-src` 的按钮，因此不需要再修改播放器脚本。

### 3. 本地检查

在 PowerShell 中进入项目目录：

```powershell
cd F:\music\wmq\album\EchoesOfWMQ-Melody
```

查看本地改动：

```powershell
git status --short
git diff -- index.html README.md
```

可直接双击 `index.html` 进行试听，也可以启动本地静态服务器：

```powershell
python -m http.server 8000
```

然后在浏览器访问 <http://localhost:8000/>。试听结束后按 `Ctrl+C` 停止服务器。

### 4. 提交到 Git

先暂存页面、说明文件以及所有新增或删除的 MP3：

```powershell
git add index.html README.md
git add -A -- "*.mp3"
git status --short
```

确认列表正确后提交：

```powershell
git commit -m "更新专辑歌曲"
```

推送到主分支：

```powershell
git push origin main
```

### 5. 更新 GitHub Pages

本仓库的 Pages 使用 `gh-pages` 分支。把刚提交的 `main` 同步到该分支：

```powershell
git push origin main:gh-pages
```

推送成功后等待 GitHub Pages 部署，通常需要 1～5 分钟。可在链接后添加查询参数绕过浏览器或微信缓存：

```text
https://lzvx-coder.github.io/EchoesOfWMQ-Melody/?v=本次版本号
```

例如：

```text
https://lzvx-coder.github.io/EchoesOfWMQ-Melody/?v=20261006
```

二维码指向的是固定 Pages 地址，因此正常更新歌曲后不需要重新生成二维码。

## Git 代理问题

如果推送时报错 `Failed to connect to 127.0.0.1`，说明 Git 配置的本地代理没有启动或端口不正确。查看当前代理：

```powershell
git config --global --get http.proxy
git config --global --get https.proxy
```

代理软件已启动时，把下面的端口改为软件实际使用的 HTTP/Mixed 端口：

```powershell
git config --global http.proxy http://127.0.0.1:7897
git config --global https.proxy http://127.0.0.1:7897
```

如果网络可以直连 GitHub，可以删除 Git 的全局代理：

```powershell
git config --global --unset http.proxy
git config --global --unset https.proxy
```

只让单次推送绕过 Git 代理、不修改永久配置：

```powershell
git -c http.proxy= -c https.proxy= push origin main
git -c http.proxy= -c https.proxy= push origin main:gh-pages
```
