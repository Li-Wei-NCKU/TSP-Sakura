# 櫻花南科之櫻施工進度紀錄（多期 3DGS 檢視網頁）

## 資料夾結構

```
index.html          網頁本體（一般不用改）
config.json         標題、期別清單、起始視角（新增期別時改這裡）
lib/                PlayCanvas 引擎（不用改）
epochs/             各期的 .sog 檔
.nojekyll           讓 GitHub Pages 直接提供檔案（不要刪）
```

## 在本機預覽

在這個資料夾開 Anaconda PowerShell Prompt，執行：

```
cd F:\Sakura\sakura-3dgs-viewer
python -m http.server 8000
```

再用 Chrome 打開 `http://localhost:8000`。不能直接雙擊 `index.html`。

## 第一次放上 GitHub Pages

1. 把 `113-11-23.sog`、`115-11-21.sog` 複製到 `epochs/` 資料夾。
2. 在 GitHub 建立新的 repository（例如 `sakura-3dgs`），設成 **Public**。
   免費帳號的 GitHub Pages 只能用在 Public repository，代表網址和檔案任何人都能看到。
3. 上傳整個資料夾的內容（不是資料夾本身），讓 `index.html` 位在 repository 的最上層：
   - 網頁上傳（Add file → Upload files）單檔上限 25MB，一期 SOG 剛好在邊緣，超過就會失敗。
   - 建議改用 **GitHub Desktop** 或 `git push`，單檔上限是 100MB。
   - **不要用 Git LFS**。GitHub Pages 不會提供 LFS 檔案，網頁會載入失敗。
4. 到 repository 的 **Settings → Pages**：Source 選 **Deploy from a branch**，Branch 選 `main`、資料夾選 `/ (root)`，按 Save。
5. 等 1–2 分鐘，網址會是 `https://你的帳號.github.io/sakura-3dgs/`。

## 新增一期

1. 在 SuperSplat 匯出該期的 SOG，檔名用期別，例如 `116-02-15.sog`，放進 `epochs/`。
2. 在 `config.json` 的 `epochs` 清單最後加一行（注意前一行結尾要有逗號）：

```json
    { "label": "116/02/15", "file": "epochs/116-02-15.sog" }
```

3. 上傳 `config.json` 和新的 SOG，GitHub Pages 約 1 分鐘後更新。

清單順序就是時間軸順序。網頁預設打開最後一期（最新）。

## 調整起始視角

1. 在網址後面加 `?edit`，例如 `https://你的帳號.github.io/sakura-3dgs/?edit`。
2. 右上角會出現「目前視角」，把畫面轉到想要的角度，按「複製」。
3. 把複製的內容貼到 `config.json`，取代 `"camera"` 後面的 `{ ... }`。

## 左右對照

按下方「左右對照」（或按 `C`），畫面中間會出現拉桿：

- 左右各自用下拉選單選期別，拖曳中間的圓形把手調整分界。
- 兩邊的視角永遠同步，旋轉、縮放時會一起動。
- 再按一次「結束對照」回到單一期別模式。

## 衛星底圖

- 來源是 Esri World Imagery Wayback（歷年的 Esri 衛星影像版本），每一期會自動選用**發布日期最接近拍攝日期**的版本，右下角會顯示用了哪一版。
- 「發布日期」是 Esri 發布該版影像的日期，不一定等於衛星實際拍攝的日期。某些地點的影像可能好幾版都沒更新。
- 想指定某一期用哪個版本，可以在該期加上 `"basemapRelease": 版本編號`（編號可在 Esri Wayback 網站查到）。
- 讀不到 Wayback 的版本清單時，會自動改用目前最新的 Esri 衛星影像。
- **使用條款**：Esri 影像需要標示來源（網頁右下角已自動加上）。如果要用於商業宣傳，請先確認 Esri 的使用條款。

## config.json 其他設定

| 欄位 | 說明 |
|---|---|
| `title` / `subtitle` | 左上角標題與操作提示 |
| `geo` | 共用原點的經緯度（由 `gs_origin.json` 換算），底圖靠它對位，不要改 |
| `groundY` | 衛星底圖的高度，目前設在模型地面下方 0.3 m 左右。底圖穿出模型地面就調低一點 |
| `basemap.source` | `wayback`（依日期挑版本）、`esri`（固定用最新影像）、`none`（不要底圖） |
| `basemap.radius` | 底圖涵蓋範圍，以模型中心為準的半徑（公尺），預設 600 |
| `basemap.zoom` | 圖磚層級，預設 18（約 0.6 m 解析度） |
| `basemap.brightness` | 衛星底圖亮度，0–1，預設 0.7。調暗一點能讓模型更突出 |
| `modelBounds` | 地面黃色框線的範圍（就是 SuperSplat 裁切框的大小），`color` 是顏色 RGB、`width` 是線寬（公尺）。不想要框線就把整段刪掉 |
| `background` | 背景色 RGB，0–1 |
| `modelRotation` | 模型旋轉，目前 `[0, 0, 180]` 是正的，不要改 |
| `startEpoch` | 起始期別，從 0 開始算；不填就是最後一期 |
| `cacheRadius` | 前後各預載幾期，預設 1。手機記憶體不足可維持 1 |
| `playSeconds` | 自動播放每期停留秒數，預設 2.5 |

## 操作方式

- 電腦：左鍵拖曳旋轉、右鍵拖曳平移、滾輪縮放；`←` `→` 切換期別、空白鍵播放、`C` 左右對照、`B` 開關衛星底圖、`R` 重設視角。
- 手機：單指旋轉、雙指縮放與平移。
