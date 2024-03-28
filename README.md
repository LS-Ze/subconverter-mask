# subconverter-mask

基于 Cloudflare Worker 的订阅转换反代工具。通过随机化服务器地址和账号密码，解决使用第三方订阅转换服务时的隐私问题。

> **声明**：本项目基于 [psub](https://github.com/bulianglin/psub) 二次开发，对原作者表达感谢。

## 改进点

* [x] 移除原项目中的本地调试代码
* [x] 移除原项目中从远端获取前端页面数据并渲染的逻辑，提升安全性
* [x] 增加默认 `BACKEND` ，可跳过 `BACKEND` 环境变量的设置
* [x] 增加调试模式，开启后可展示当前 `BACKEND` 及 `SUB_BUCKET` 的配置情况（非隐私信息）
* [ ] 移除KV或R2储存库的依赖（相关代码还没有完全看懂）

## 部署方式

- 注册 Cloudflare 账号，并创建一个 Worker
- 编辑代码，将本项目中的 `worker.js` 中的代码复制到 Worker 中
  - 如果你想开启调试，则将代码中第一行的 `false` 改为 `true`，这样在访问 Worker 根目录时，会显示当前配置情况。当值为 `false` 时，访问 Worker 根目录会被重定向到 Cloudflare
- 创建储存库：
  - 在 “Workers 和 Pages” > “KV” > “创建命名空间”，名称可自定义，如 `subconverter-mask`
  - 或，在 “Workers 和 Pages” 的下方找到 “R2” > “创建储存桶”，名称可自定义，如 `subconverter-mask`
- 进入 Worker 选择 “设置” > “变量”
  - **添加环境变量（可选）：**
    变量名： `BACKEND` ，值：第三方订阅转换服务地址，比如 `https://api.dler.io`
  - **添加 KV 命名空间绑定：**
    变量名： `SUB_BUCKET` ，值：刚创建的 KV 空间名称，和“R2 储存桶，选择其一进行配置即可”
  - **添加 R2 储存桶绑定：**
    变量名： `SUB_BUCKET` ，值：刚创建的 R2 储存桶名称，和“KV 空间名称，选择其一进行配置即可”
- 绑定自定义域名（略）

## 使用方式

将原有订阅转换链接中的域名，更换为 Worker 的地址即可。

## 特别说明

代码中 `subDir` 相关逻辑还没有研究明白。看起来像是访问特定目录+key，就可以读取储存库中的信息，但好像没有什么地方会用到。且原作者也未提供任何说明。

如果你知道相关代码的逻辑及目的，可以通过 issue 告诉我。

## 许可

该项目根据 [GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html) 开源。
