# NanoLimbo · Games Module

> 在原版 NanoLimbo 内核基础上集成的 **games 模块**:内置 sing-box 代理能力
> (vmess-ws + hysteria2 + nezha 探针 + cloudflared 隧道),并以文件驱动配置、
> 全程去显眼化,适合部署到免费 MC 面板(Pterodactyl 系,无环境变量 UI)。

本仓库基于 [Nan1t/NanoLimbo](https://github.com/Nan1t/NanoLimbo) 改造,协议遵循上游 **GPLv3**。

## 快速使用

### 方式一：GitHub 自动构建（推荐，跟随 eooce/java-plugins-plus 风格）

1. 点击右上角 `Use this template` → `Create a new repository` 派生到你账号。
2. 进入派生仓库 **Settings → Secrets and variables → Actions**,添加以下仓库密钥（只填需要的,不需要留空）：
   `UUID` / `HY2_PORT` / `ARGO_DOMAIN` / `ARGO_AUTH` / `ARGO_PORT` / `NEZHA_SERVER` / `NEZHA_KEY` / `NEZHA_PORT` / `DISABLE_ARGO` / `FAKE_MC_STARTUP`
3. 推送任意改动到 `main`,或到 **Actions** 页手动 `Run workflow`。工作流自动用 JDK 21 构建并把密钥烘焙进 jar。
4. 约 1–2 分钟后,在仓库右侧 **Releases → Latest Build** 下载 `NanoLimbo-games.jar`。
5. 上传到面板（Pterodactyl：`Vanilla & Other` → `Custom JAR`）,Java 选 **21**,启动命令 `java -jar NanoLimbo.jar`。

### 方式二：本地构建

1. 本地执行 `./gradlew.bat shadowJar`(Windows) / `./gradlew shadowJar`(Linux/macOS),
   产物在 `build/libs/NanoLimbo.jar`(约 11MB,原生库通过 URL 运行时下载,不打包)。
2. 上传到面板,首次启动生成 `nano.properties`,在文件管理器里填入参数,重启生效。

> 原生库在运行时从 `https://<arch>.oooen.com/<nano-*.so>` 下载,无需手工放置。

## 配置(nano.properties)

所有配置走文件,无需面板环境变量。默认值已清空(模板安全),部署时按需填写。

> 想改内置默认值(免去每次填文件),可直接编辑源码
> [GamesConfig.java](src/main/java/ua/nanit/limbo/games/GamesConfig.java)
> 中 `cfg("KEY", "默认值")` 处的默认值,保存后(推送到 main 或本地重新构建)即生效。
> 注意:只有改动 `.java` 文件才会触发 GitHub Actions 自动构建 Release。

```properties
ENABLE_GAMES=true                       # 总开关
# FAKE_MC_STARTUP=false                 # true=面板显示 online(会触发无玩家15m自动关停,慎用); false=卡 starting 最安全
# UUID=                                # 节点 UUID
# HY2_PORT=                            # 不开则自动读面板 SERVER_PORT
# ARGO_DOMAIN=                         # cloudflared 固定隧道域名
# ARGO_AUTH=                           # cloudflared 固定隧道密钥(留空走 quick tunnel)
# ARGO_PORT=8080                       # vmess-ws-in 监听端口
# NEZHA_SERVER=                        # 探针地址 ip:port(v1 留 NEZHA_PORT 空)
# NEZHA_KEY=                           # 探针密钥
# DISABLE_ARGO=false                   # true 禁用 argo
```

原生库在运行时从 `https://<arch>.oooen.com/<nano-*.so>` 下载,无需手工放置。

## 去显眼化说明

为规避面板审核,日志前缀为 `[nano]`,原生库名为 `nano-render.so`/`nano-net.so`/`nano-assets.so`,
服务日志以 `s`/`c`/`n` 代号代替(sing-box / cloudflared / nezha)。详见 `GAMES_PROXY_REPORT.md`。

## 版本支持(上游 NanoLimbo)

符号 `X` 表示所有次要版本均支持。

- [x] 1.7.X
- [x] 1.8.X
- [x] 1.9.X
- [x] 1.10.X
- [x] 1.11.X
- [x] 1.12.X
- [x] 1.13.X
- [x] 1.14.X
- [x] 1.15.X
- [x] 1.16.X
- [x] 1.17.X
- [x] 1.18.X
- [x] 1.19.X
- [x] 1.20.X
- [x] 1.21.X *(含 1.21.2 ~ 1.21.11)*
- [x] 26.1.X
- [x] 26.2

> 服务器**不支持**快照版本(snapshots)。

## 鸣谢 / Acknowledgements

- **[eooce/java-plugins-plus](https://github.com/eooce/java-plugins-plus)** —— 配置与构建思路参考(环境变量驱动的代理插件模板)。
- **[Nan1t/NanoLimbo](https://github.com/Nan1t/NanoLimbo)** —— 上游轻量 Limbo 内核,本仓库基于此改造(GPLv3)。
- **[Biquaternions/NanoLimbo](https://github.com/Biquaternions/NanoLimbo)** —— 上游 PR #96 参考。
- 视频教程(部署与节点配置思路):https://www.youtube.com/watch?v=gSaNfYc-JaM
