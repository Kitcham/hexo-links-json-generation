# hexo-links-json-generation / Hexo友链Json生成

## What is this? 这是甚么？

This is a json generator for hexo links page, which transforms YML to Json.

这是一个供 HEXO 使用的 Json 生成器，用于友链页面。其将 YML 格式文件转换为 Json 格式文件。

Attention please! This page is only a suggestion, it should be subject to my post [Generate links json via github action](https://blog.uiharu.top/archives/generate-links-json-via-github-action.html) in the blog.

请注意，本页面文档仅供参考之用，最新文档以我博客的[没人比我更懂方便——透过 Github Action 优雅生成友链数据](https://blog.uiharu.top/archives/generate-links-json-via-github-action.html)为准。

## 如何食用

### 申请 Token

若果你是**第一次**使用 Github Action，你需要参照本步骤先行申请 `Token`，用於启动 Action 及 Commit Log。若果你已拥有 `Token`，可跳过本步骤。

1. 鼠标置於右上角头像处，进入个人设置 `Settings`

2. 於 `Settings` 页面进入 `Developer settings` --> `Personal access tokens`，並新增一个 `Token`

3. 请将新增的 `Token` 命名为 `GITHUB_TOKEN`，同时勾选 `repo` ,`admin:repo_hook` ,`workflow` 等选项

**请务必确保**新增的 `Token` 命名为 `GITHUB_TOKEN`。

### Secrets 配置及运行

考虑到各人的 Hexo 並非均部署在具有 FTP 服务的 VPS 中，下列步骤将区分有无 FTP 两种情况，以供参考。

#### 有 FTP 服务

请先 Fork 项目仓库 [Kitcham/hexo-links-json-generation](https://github.com/Kitcham/hexo-links-json-generation) 到自己的 Github Repo 中。然后於 Repo 的 ``Settings`` --> `Secrets` 中配置好以下 `Secrets` ，请务必注意名称的一致。

| `Secrets` 名称 | 变量值描述 |
| :----: | :----: |
| ``USER_EMAIL`` | 你的 Email 地址（用於 Commit 信息的提交） |
| ``USER_NAME`` | 你的 Github 用户名（用於 Commit 信息的提交） |
| ``FTP_ACCOUNT`` | 你的服务器 FTP 用户名 |
| ``FTP_ADDRESS`` | 你的服务器 FTP 地址（不指定端口时，默认为 21 端口） |
| ``FTP_PASSWORD`` | 你的服务器 FTP 密码 |

**请务必确保**设置的 `Secrets` 名称与上表一致。

具体的 Workflow 情况请转到 Repo 的 `Action` 界面瞭解，你亦可於此处查看 Workflow 的工作情况。待 Workflow 成功运行后，你可於 Repo 的 `/links/links.json` 中查看生成结果。此时，你即可以於你的 VPS 中发现 `links.json` 文件，此即友链 `YML` 信息文件的转换结果。

**注意：** 该情况下具有两个 Workflow 的 `YML` 配置文件。其将先执行 `build.yml`，待其完成后，`ftp.yml` 随之执行。

本项目的 FTP 服务采用 [GitHub-Action/FTP-Deploy](https://github.com/marketplace/actions/ftp-deploy)，更多高级配置可参照其项目文档。

#### 无 FTP 服务

请先 Fork 项目仓库 [Kitcham/hexo-links-json-generation](https://github.com/Kitcham/hexo-links-json-generation) 到自己的 Github Repo 中。然后於 Repo 的 ``Settings`` --> `Secrets` 中配置好以下 `Secrets` ，请务必注意名称的一致。

| `Secrets` 名称 | 变量值描述 |
| :----: | :----: |
| ``USER_EMAIL`` | 你的 Email 地址（用於 Commit 信息的提交） |
| ``USER_NAME`` | 你的 Github 用户名（用於 Commit 信息的提交） |

**请务必确保**设置的 `Secrets` 名称与上表一致。

具体的 Workflow 情况请转到 Repo 的 `Action` 界面瞭解，你亦可於此处查看 Workflow 的工作情况。待 Workflow 成功运行后，你可於 Repo 的 `/links/links.json` 中查看生成结果。此时，你即可以於你的 VPS 中发现 `links.json` 文件，此即友链 `YML` 信息文件的转换结果。

**注意：** 该情况下仅有一个 Workflow 的 `YML` 配置文件，`build.yml` 将会执行。

如你需要在本情况下访问生成的 `links.json`，你**可能**需要（三者其一）：
- 在 Repo 中进入 `/links/links.json`，点击文件预览界面的 `RAW` 选项，获取其於 Github 上的 URL 以便引用访问（不推荐）
- 在 Repo 的 `Settings` 中开启 `Github Pages` 服务，然后即可透过 `http(s)://YOUR_PAGE_DOMAIN/links/links.json` 访问生成的 `links.json`（推荐）
- 你会的其他黑科技 / 黑魔法

### 友链信息的更新

你惟须依照以下格式进行填写相关信息，每当你进行一次 `PUSH` 或 `Pull requests` 操作后，即可触发 Github Action 完成转换。

- ``src\links.yml``

```yaml
"站点名称": # 请置于双引号之中
  url: https://example.com # 网站的 URL，请注意冒号后的空格
  img: https://example.com/example.png # 网站 Logo 的 URL，请注意冒号后的空格
  text: "Hello, World" # 网站的 Slogan，请置于双引号之中，请注意冒号后的空格
```

如生成过程出现问题，请查看 Action 日志瞭解详情以排查问题。如问题仍未解决，欢迎提交 Issue 交流。

### 前端怎么用好它呢

本项目仅用於 `YML` 到 `JSON` 的转换工作，並非前端友链页面的直接实现。

若果希望查看效果，可到本站的友链页面 [Kitcham的友链页面](https://blog.uiharu.top/links) 预览效果。

## 结语

该方案只是经我个人思考后，认为普适性较高的方案。如果你有更优雅的方案，欢迎交流~