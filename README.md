# x-kernel 贡献归档（gitee openkylin/x-kernel）

作者：**xitoulaoxi-oss**（Gitee 账号 `xyy12589865`，<https://gitee.com/xyy12589865/projects>）

本仓库是其向 [openkylin/x-kernel](https://gitee.com/openkylin/x-kernel) 提交的全部 Pull Request 归档，
**无论是否已并入上游**，每个 PR 都保留完整补丁（可直接 `git apply`）。

生成方式：逐 PR 从 Gitee 取 `https://gitee.com/openkylin/x-kernel/pulls/<编号>.diff`，原样保存；
并入状态与合并提交来自 `git log --author=xitoulaoxi-oss`。

## 总览

| PR | 标题 | 状态 | 源分支 | 文件数 | 上游合并提交 | 补丁 |
|---|---|---|---|---|---|---|
| [!1035](https://gitee.com/openkylin/x-kernel/pulls/1035) | aarch64: allow EL0 reads of CTR_EL0 (clear SCTLR_EL1.UCT/UCI) | open | `pr-a-aarch64-sctlr-uct-uci` | 1 | — | [patch](patches/!1035-aarch64-sctlr-uct-uci.patch) |
| [!1039](https://gitee.com/openkylin/x-kernel/pulls/1039) | io/drmdevice: legacy ADDFB, MODE_CURSOR and SET_CLIENT_CAP semantics | merged | `pr-b-drm-legacy-modeset` | 6 | `1614cdf2 (2026-10-18)` | [patch](patches/!1039-drm-legacy-modeset.patch) |
| [!1049](https://gitee.com/openkylin/x-kernel/pulls/1049) | net/knet: accept TCP_KEEPIDLE, TCP_KEEPINTVL and TCP_KEEPCNT | open | `pr-c-tcp-keepalive` | 4 | — | [patch](patches/!1049-knet-tcp-keepalive.patch) |
| [!1050](https://gitee.com/openkylin/x-kernel/pulls/1050) | io/drmdevice: implement MODE_SETPROPERTY and MODE_OBJ_SETPROPERTY | merged | `pr-g-drm-set-property` | 5 | `8fd7a457 (2026-10-18)` | [patch](patches/!1050-drm-set-property.patch) |
| [!1058](https://gitee.com/openkylin/x-kernel/pulls/1058) | fs/boot: mount tmpfs with the modes Linux uses (1777 for /dev/shm and /tmp) | closed (not merged) | `pr-d-tmpfs-modes` | 1 | — | [patch](patches/!1058-boot-tmpfs-modes.patch) |
| [!1059](https://gitee.com/openkylin/x-kernel/pulls/1059) | io/drmdevice: implement GEM_CLOSE and MODE_CLOSEFB | merged | `pr-f-gem-close-closefb` | 5 | `731fe7cc (2026-10-19)` | [patch](patches/!1059-drm-gem-close-closefb.patch) |

## 说明

- **!1039 / !1050 / !1059 已并入上游 main**（squash 合并，提交作者保留为 `xitoulaoxi-oss`）。
- **!1035 / !1049 仍在评审中**（未合并）。
- **!1058 已关闭**：复核确认其在当前 main 上是空操作（tmpfs 根 inode 在基线 `d3a6ec9` 就已按 `0o1777` 构造），故未合入。
- 补丁文件是 PR 相对其 base 的完整 diff，应用示例：
  ```sh
  git clone https://gitee.com/openkylin/x-kernel.git
  cd x-kernel && git checkout <PR 的 base 提交>
  git apply /path/to/patches/!1050-drm-set-property.patch
  ```
- 每个 PR 的完整讨论、CI 记录与复审意见见对应 Gitee 链接。

## 各 PR 一句话说明

- **!1035** — aarch64 启动时清除 `SCTLR_EL1.UCT/UCI`，让 EL0 读 `CTR_EL0` 与缓存维护指令不再被陷入（否则 libgcc 缓存同步路径在用户态触发 SIGILL）。
- **!1039** — 补齐 legacy 模式设置路径：`ADDFB`(0xAE)、`MODE_CURSOR`/`CURSOR2`、`SET_CLIENT_CAP` 语义、`CREATE_DUMB` 接受 24bpp、`DIRTYFB` 未知 fb 返回 `ENOENT`，并附用户态 ABI 回归包。
- **!1049** — `net/knet` 接受 `TCP_KEEPIDLE`/`TCP_KEEPINTVL`/`TCP_KEEPCNT`，其中 `KEEPIDLE` 真正接到 keep-alive 定时器。
- **!1050** — 新增 `MODE_SETPROPERTY`(0xAB) 与 `MODE_OBJ_SETPROPERTY`(0xBA)，按 Linux errno 语义实现，带 `abi-tests/drm-set-property` 回归包与提交串行化（`commit_lock`）。
- **!1058** — `fs/boot` 在挂载 tmpfs 后设置 `/dev/shm`、`/tmp` 为 `1777`（结论：现代 main 上等价空操作，PR 关闭）。
- **!1059** — 新增 `GEM_CLOSE`(0x09) 与 `MODE_CLOSEFB`(0xD0)，引入 dumb buffer 的引用式退役生命周期（park/retire、扫描标记、`commit_lock` 串行化），带 `abi-tests/drm-gem-close` 回归包。
