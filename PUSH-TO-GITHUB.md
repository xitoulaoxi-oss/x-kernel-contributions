# 把它推到 GitHub（在你自己的机器上执行）

> 本机（sandbox）**连不上 github.com**：DNS 能解析（20.205.243.166），但 443 超时。
> 所以这一步必须在能访问 GitHub 的机器/网络里做（挂代理或换网络）。

## 0. 准备

目标账号：**xitoulaoxi-oss**（<https://github.com/xitoulaoxi-oss>）

## 1. 建仓库（网页最快）

<https://github.com/new>
- Repository name: `x-kernel-contributions`
- 可见性：Public / Private 自选
- **不要**勾选 "Initialize this repository with a README"（否则首次 push 冲突）

## 2. 导入并推送

把本目录（`x-kernel-contributions/`）拷到那台机器，然后：

```powershell
cd <拷过去的路径>\x-kernel-contributions
git remote add github https://github.com/xitoulaoxi-oss/x-kernel-contributions.git
git branch -M main
git push -u github main
```

如果本目录已经被打成 `git bundle`（`x-kernel-contributions.bundle`），也可以从 bundle 起步：

```powershell
git clone x-kernel-contributions.bundle x-kernel-contributions
cd x-kernel-contributions
git remote set-url origin https://github.com/xitoulaoxi-oss/x-kernel-contributions.git
git push -u origin main
```

## 3. 如果你更想镜像整个 fork（可选，重）

```powershell
git clone --mirror https://gitee.com/xyy12589865/x-kernel.git x-kernel.git
cd x-kernel.git
git remote set-url --push origin https://github.com/xitoulaoxi-oss/x-kernel.git
git push --mirror
```

⚠️ fork 是上游全量拷贝（155 个分支、完整历史，数 GB），只在确实需要"完整镜像"时用。

## 4. 认证

- 网页建好仓库后，push 时用 **PAT**（Personal Access Token，`repo` 权限）当密码；
- 或先 `gh auth login`（若装了 GitHub CLI），再用 `gh repo create`。
