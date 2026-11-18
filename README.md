# unlock-music
音乐解锁：移除已购音乐的加密保护。

## 支持的格式

- QQ 音乐：qmc0/qmc2/qmc3/qmcflac/qmcogg、mflac/mgg 系列、tkm、bkcm 系列等
- 网易云音乐：ncm
- **酷狗音乐：kgm / kgma / vpr**（本次新增，纯 JS 实现，无需 WASM 与 70MB 公钥表）
- 酷我音乐：kwm
- 其他：tm0/tm2/tm3/tm6、mg3d、xm、x2m/x3m 等

> 注：酷狗 `.kgg`（v5 新格式）的密钥存于本机 KGMusicV3.db，纯前端无法读取，需使用 CLI 本地工具处理。

原仓库为[git.unlock-music.dev/um/web](https://git.unlock-music.dev/um/web)。本仓库基于官方源码 v1.10.3 构建，并做了如下改造以支持酷狗解密：
- 新增 `src/decrypt/kugou_crypto_pure.ts`：移植自官方 `KgmWasm/kgm.hpp` 的纯 JS KGM/VPR 解密算法
- 移除已从 npm 下架的 `@jixun/kugou-crypto`、`@unlock-music/joox-crypto` 依赖，改用本地实现/降级桩
- 为缺失的 WASM bundle 提供加载桩，运行时自动回退到纯 JS 解码器
- 构建产物已包含 kgm/kgma/vpr 扩展名分发与解密逻辑（BUILD SUCCESS 验证）

改造后的完整源码位于 `../unlock-music-src`，备份的原始站点位于 `../unlock-music.backup`。

直接部署可使用Fork本仓库后使用GitHub Pages。

或者使用 `Docker` 部署：

使用以下命令构建和运行容器： 

```
docker build -t unlock-music .
docker run -d -p 9200:9200 unlock-music
```

或者使用 `docker-compose` 部署：

```
docker-compose up -d
```
