# 我要下歌 MusicFree 插件 (xiage.yiwuku.com)

将「我要下歌」(https://xiage.yiwuku.com) 音乐站适配为 MusicFree 插件，支持搜索、在线播放、歌词。

> 本插件**不实现**歌单/排行榜（`getTopLists`/`getTopListDetail`），MusicFree 中不会显示对应入口，仅提供搜索与播放能力。

## 功能

| 方法 | 说明 | 状态 |
|------|------|------|
| `search` | 关键词搜索，返回歌曲（标题/歌手/时长） | ✅ |
| `getMediaSource` | 获取 kuwo CDN 直链（HTTP→HTTPS 升级） | ✅ |
| `getLyric` | 歌词（取详情页 meta description，纯文本） | ✅ |

## 逆向来源（全部浏览器实测，无盲猜/网络搜索）

- 搜索：`GET /search.php?q=<关键词>` → 解析 `.sound-item`（无分页，单页结果）
- 音源：歌曲详情页 `/s/<id>` 内联 `songs.php?pos=<内部索引>`，该接口返回含 `src` 的播放列表 JSON
  - ⚠️ `pos` 为站点内部索引，无法从歌曲 ID 推导，故 `getMediaSource` 必须先抓详情页取 `pos`
- 歌词：详情页 `<meta name="description">` 文本（无逐行时间戳）

## 已知限制

- 不提供歌单/排行榜（仅搜索 + 播放 + 歌词）
- 搜索为单页结果（`isEnd` 恒为 `true`）
- 部分歌曲仅提供网盘（迅雷）下载、无在线播放源，此类在 `getMediaSource` 抛友好错误
- 歌词为纯文本，无逐行时间轴
- 播放直链来自 kuwo CDN，可能带时效，`cacheControl` 设为 `no-store`

## 安装

MusicFree → 插件管理 → 从 URL 安装 → 填入 `xiage.js` 的 raw 链接（若已发布），或导入本地 `xiage.js` 文件。

## 测试

```bash
NODE_PATH=<buguyy的node_modules路径> node test.cjs
```

依赖：仅 `axios`（MusicFree 沙箱内置）。
