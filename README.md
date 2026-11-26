# 小盒子应用市场（OpenMini Market）

这是「小盒子」(OpenMini) 的官方自用应用市场仓库。小盒子 APK 启动后默认打开这里的 Release，列出里面的 `.zip` 应用包，点击即可下载并安装到盒子内。

## 给小盒子用户

1. 先安装小盒子宿主 APK（见 [joeyjiaojg/openmini](https://github.com/joeyjiaojg/openmini) 的 Release）。
2. 打开小盒子 → 首页「应用市场」自动加载本仓库最新 Release 里的应用。
3. 点应用即可安装；同 id 再装一次是升级，存档不丢。

国内网络下载 Release 文件请走代理前缀：

```
https://gh-proxy.org/https://github.com/joeyjiaojg/openmini-market/releases/download/<tag>/<file>
```

例如：

```
https://gh-proxy.org/https://github.com/joeyjiaojg/openmini-market/releases/download/v1.0.0/wordbuddy-v1.0.0.zip
```

## 在售应用

| 应用 | id | 最新版本 | 说明 |
|---|---|---|---|
| 血战到底 | `mahjong` | 3.1.1 | 四川血战到底：108张牌、缺一门、不能吃、多人连续胡；横屏大牌桌、碰杠胡按钮、庄家与出牌标识 |
| 楚汉象棋 | `xiangqi` | 1.9.0 | 人机对战，入门~棋圣五档，可配置步数上限 |
| WordBuddy 背单词 | `wordbuddy` | 1.0.0 | 分组词库 7105 词、SM-2 复习、双语故事 |
| 五子棋 | `gomoku` | 1.0.0 | 横屏大棋盘人机对战，三档 AI、悔棋、落子提示、胜负统计 |
| 数独 | `sudoku` | 1.0.0 | 横屏九宫格，四档难度、唯一解、笔记模式、错误提示与计时 |
| 24点 | `point24` | 1.0.0 | 四张牌算 24，保证有解、表达式校验、计时挑战、求解提示 |
| 每日资讯 | `news` | 1.2.4 | 竖屏五栏目资讯；左右滑动切换栏目、标题最长 3 行、窄屏刷新按钮不溢出；内置阅读面板；多镜像切换与离线缓存 |
| 节拍器 | `metronome` | 1.0.0 | 竖屏精准节拍器：Web Audio 采样级计时、拍号/细分/三种音色、敲拍测速、节拍灯光与摆锤 |

## 不在此仓库分发的应用

`music`（音乐盒）这类需要加载第三方音源解析接口的应用，**不在公开仓库分发**：它的接口配置与榜单快照不随包发布，只放在私有市场（`openmini-market-private`）的加密仓里，需要在「小盒子 → 私有市场」用账号密码解锁后才能安装与使用。公开仓库的历史与 Release 里也不再保留任何 music 包。

完整机器可读清单见 [catalog.json](catalog.json)。应用包源码见各应用仓库；构建产物同时放在 `apps/` 与 Release assets。

## 发布新版本（维护者）

1. 用 `box/tools/make-pkg.py` 打出 `<id>-v<版本>.zip`，校验 `manifest.json` 的 `id`/`version`/`versionCode`。
2. 把新 zip 放进 `apps/`，**删掉 `apps/` 里同 id 的旧版本包**（例如上架 3.1.0 就删掉 v1.2.0、v3.0.1），
   更新 `catalog.json` 和本 README 的表格。
3. 提交推送。
4. 发布 Release 时**只上传每个 id 的最新一个包，不要 `apps/*.zip` 全量传**：
   - 新 tag：`gh release create v<tag> apps/<id1>-v<ver>.zip apps/<id2>-v<ver>.zip ... --generate-notes`
   - 已有 tag：`gh release upload <tag> apps/<file>`，并用
     `gh release delete-asset <tag> <旧的zip名> -y` 删掉同 id 的旧资产。
   - 若某旧资产已在 Release 里，务必先删，否则市场会把它当成同一个应用的多个条目重复显示。
5. 小盒子会拉取**最新** Release 的 assets 列表，把每个 `.zip` 当成一个应用（按文件名 `<id>-v<版本>.zip` 解析），
   所以 assets 里每个 id 只能有一个包，否则会出现多个同名应用。无需改 APK。

## 包格式

见 [openmini 包格式规范](https://github.com/joeyjiaojg/openmini/blob/main/docs/PACKAGE-FORMAT.md)：zip 根目录含 `manifest.json` + 入口 HTML，单包解压后 ≤ 40MB。
