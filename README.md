# HomeDesktopPlugins

HomeDesktop 的**在线插件市场仓库**：应用内「插件管理 → 市场 → 在线市场」从这里拉取索引并下载安装插件。

## 目录结构

```
market/
  index.json           # 市场索引（应用读取此文件）
  *.zip                # 插件安装包（zip 根目录必须含 manifest.json）
README.md
```

## 添加新插件

1. 把插件打成 zip（根目录含 `manifest.json`），放进 `market/`
2. 在 `market/index.json` 的 `plugins` 数组里加一项：

```json
{
  "id": "com.example.myplugin",        // 与 manifest.json 的 id 一致
  "name": "我的插件",
  "version": "1.0.0",
  "pluginType": "widget",              // icon | widget
  "emoji": "🧩",
  "file": "myplugin-1.0.0.zip",        // 必须与放入的文件名一致
  "size": 12345,                       // 字节数（显示用）
  "description": "一句话介绍"
}
```

3. 提交推送 `main` 分支 → 应用内点「🔄 刷新列表」即可看到

> 注意：`base` 字段是下载基础 URL（`https://cdn.jsdelivr.net/gh/Eggplant4363/HomeDesktopPlugins@76c97816ee81c34df0f81da2a8c2d43affd41888/market/`），新增插件不需要改它。

## 插件开发规范

见主项目 [HomeDesktop](https://github.com/Eggplant4363/HomeDesktop) 的 `docs/PLUGIN_API.md`。

## 更新插件后如何让应用看到

jsDelivr 的 `@main` 分支解析有 12 小时缓存，新增插件后需要**改用最新 commit SHA**：
1. 推送插件包 + 更新 index.json 后，获取最新 commit：`git rev-parse HEAD`
2. 把本 README 与 `market/index.json` 里的 `base` 换成新 SHA
3. 同时更新 HomeDesktop 应用 `src/components/MarketList.svelte` 的 `DEFAULT_INDEX_URL` 为新 SHA URL