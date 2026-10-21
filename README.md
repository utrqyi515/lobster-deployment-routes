# 腾讯云部署龙虾：一键镜像与 Docker 两条路线怎么选，2核4G 起步把模型和聊天通道一次配好

搜"腾讯云部署龙虾"的人，多半已经知道 OpenClaw 是什么，卡在"装在哪、怎么装、花多少钱"。答案先给：腾讯云轻量应用服务器 Lighthouse 提供 OpenClaw 应用镜像，新购实例时选它，30 秒左右创建完成，OpenClaw 和依赖环境都预装好了，剩下就是进控制台配模型和聊天通道。名下已有轻量服务器且旧数据不要了，重装系统选 OpenClaw 模板更省钱；服务器上还跑着别的应用，就走 Docker 自己装。

> **选购速览**
> - 大多数人：入门型 2核4G + OpenClaw 应用镜像，国内地域，接 QQ、企微、飞书都够用
> - 只尝鲜：2核2G 是官方最低配置，适合单通道简单体验
> - 有闲置服务器：重装系统选 OpenClaw 模板，重装前先备份
>
> 👉 [去腾讯云 OpenClaw 活动页挑一台配置](https://bit.ly/yunhuodong)

"龙虾"是 OpenClaw 的中文俗称。这个项目原名 Clawdbot/Moltbot，2026 年 1 月在中外技术社区走红，代码 100% 由 AI 完成。它装在服务器上、接上聊天软件，就能帮你执行命令、读写文件、上网搜索。也正因为权限给得足，官方社区建议不要装在主力电脑上。云服务器与本地隔离，还能 7×24 小时在线，是更稳妥的位置。

## 腾讯云部署龙虾有哪几种方式？

四条路：新购实例选 OpenClaw 镜像、给现有实例重装系统、用轻量云 AI 助手安装、Docker 自建。前两条是控制台点选，适合大多数人；后两条分别对应"想保留现有数据"和"服务器上跑着别的应用"两种情况。

| 方式 | 适合谁 | 要做的事 | 注意点 |
| --- | --- | --- | --- |
| 新购实例选 OpenClaw 镜像 | 第一次部署，想最快跑通 | 选应用镜像、地域、套餐，下单 | 30 秒左右创建完成，开箱即用 |
| 重装现有轻量实例 | 有闲置服务器，旧数据可清 | 控制台"重装系统"，应用模板选 OpenClaw | 会清空数据，先选"备份后重装" |
| 轻量云 AI 助手安装 | 想保留服务器现有应用和数据 | 按 AI 助手教程操作 | 官方有单独教程 |
| Docker 自建 | 熟悉命令行，服务器上跑着别的服务 | 写 Dockerfile、构建镜像、挂载配置运行 | 环境、升级、备份都要自己管 |

下面重点讲新购、重装和 Docker 这三条，第三种按官方教程走即可。

## 用 Lighthouse 一键镜像部署，全程不用敲命令

这条路径一共四步：买一台带 OpenClaw 镜像的服务器、进 Agent 控制台、配模型、配通道。全程在网页上点。

### 新购实例

1. 打开腾讯云 OpenClaw 活动页或 Lighthouse 购买页
2. 应用创建方式选"使用应用镜像"，应用列表里选 OpenClaw（龙虾）
3. 选地域。接 QQ、企业微信一般选国内；接 Discord、WhatsApp 一般选海外；调国内模型也建议国内地域，延迟低
4. 选套餐。官方推荐 2核4G 或更高，最低支持 2核2G
5. 下单，等 30 秒左右创建完成，点"查看服务器"进控制台

### 重装现有实例

控制台找到实例 → "更多" → "重装系统" → 应用模板里选"AI智能体 > OpenClaw（龙虾）" → 登录凭证选"重装后设置" → 备份选项选"备份后重装" → 确定，等重装完成。重装会清空服务器内全部数据，这一步别省备份，之后还能用快照回退。

### 配模型和通道

进 Lighthouse 控制台，左侧"Agent 控制台" → "设置" → "Agent 管理"，页面顶部先选中要配置的 Agent。模型、通道、技能都只对当前选中的 Agent 生效。

到这里服务器就算部署完了。之后能让它干什么，取决于你接什么模型、开什么通道。有人用它把网站项目直接部署上线，3 轮对话、1 块钱左右的 DeepSeek token 就搞定，也有人把它当 24 小时在线的聊天助理。

## 部署龙虾到底要花多少钱？

两笔钱：服务器和模型。服务器是固定支出，官方对 Lighthouse 的描述是"几十块钱即可立即开始"；模型按用量或套餐计费，用多用少差别很大。很多人比价时只盯着服务器价格，忽略了模型这一头。

服务器这边，Lighthouse 按套餐售卖，价格和地域、购买时长、当期活动都有关系，新用户活动价和续费价也可能不同，下单页显示的才是实时价格。官方给 OpenClaw 场景的建议是入门型 2核4G 起步。

| 套餐类型 | 官方给出的配置参考 | 适合谁 | 价格 | 选购 |
| --- | --- | --- | --- | --- |
| 入门型 | 最低 2核2G，推荐 2核4G | 个人用户，官方称为高性价比首选 | 下单页实时价 | [去活动页选入门型](https://bit.ly/yunhuodong) |
| 锐驰型 | 页面可选档位 | 需要更高配置时考虑 | 下单页实时价 | [去活动页看锐驰型](https://bit.ly/yunhuodong) |
| 通用型 | 页面可选档位 | 需要更高配置时考虑 | 下单页实时价 | [去活动页看通用型](https://bit.ly/yunhuodong) |

模型这边，接入方式有三种：Coding Plan 套餐、模型 API、自定义模型。腾讯云的 Token Plan 把混元、智谱 GLM、Kimi、MiniMax 等模型打包在一起，支持"自动"匹配；MiniMax、Kimi 也各有自己的 Coding Plan。按量付费的话，轻度聊天花不了多少，但让它长时间跑自动化任务，token 会一点点累积，最好在控制台里盯一眼用量。

👉 [到活动页看当前套餐和实时价格](https://bit.ly/yunhuodong)

## 模型怎么配？遇到 401 先查这三处

在 Agent 管理页找到模型卡片，选服务商、完成授权或填 Key、设为默认，三步。面板里能直接选 DeepSeek、月之暗面 Kimi、智谱 GLM、MiniMax、豆包、通义千问、文心一言，也能自定义 OpenAI GPT、Gemini、Claude。

三种常见的配置路径：

- **MiniMax Token Plan**：先买套餐，回面板选"MiniMax Token Plan（国内）"和模型版本，点 OAuth 授权，跳转官网完成授权后回来点"我已完成授权"
- **Kimi Code Plan**：在 Kimi Code 控制台拿到 API Key，面板选"Kimi Coding Plan"和对应模型，填入 Key，点"添加为默认"
- **腾讯云 Token Plan**：面板里选腾讯云 Token Plan，模型默认"自动"（算法匹配），点"一键添加为默认"；没买过会弹购买页，买完回来点"已购买，继续配置"

配置时如果报"HTTP 401: InvalidAuthentication"或类似的 401，重点查三处：API Key 本身填错了；把 Coding Plan 的 Key 和普通模型 API Key 混用了；服务商的国内版和国际版选错了。

## 聊天通道怎么接？先想清楚用哪个 App

在同一个 Agent 管理页，通道卡片里选微信、企业微信、QQ、钉钉或飞书，按提示授权或手动填参数，添加成功后重启 Gateway 生效。通道和地域要对上：接 QQ、企业微信通常用国内地域，接 Discord、WhatsApp 通常用海外地域。

官方对每类通道都出了接入教程，微信、QQ、企业微信、飞书、钉钉、Discord 都能找到对应文档。配置本身不复杂，麻烦的是各平台自己的机器人申请流程，这部分按教程一步步来即可。

如果你只打算在 Telegram 或 Discord 上用，手上又有 AWS 的资源，AWS Lightsail 也推出了 OpenClaw 方案，EC2 上自建也有人跑通（社区案例用 t4g.large、Ubuntu 24.04）。国内模型和微信、QQ、企微、飞书这类通道，腾讯云的面板和地域更直接。两个平台本质都是一台云服务器的事，区别在生态和你平时的使用习惯。

## 已有服务器不想重装？走 Docker

Docker 适合服务器上还跑着其他应用、不方便重装的人。代价是要自己写配置、管环境。社区里有现成的中文汉化版 openclaw-cn，可以直接拿来构建镜像。如果你还没买服务器，先 👉 [去活动页选一台配置](https://bit.ly/yunhuodong)，再回来折腾 Docker。

Dockerfile 大概长这样：

dockerfile
FROM node:22-bookworm

RUN node --version && \
    npm install -g openclaw-cn@2026.2.5

WORKDIR /root/clawd

RUN mkdir -p /root/.openclaw
ENV OPENCLAW_CONFIG=/root/.openclaw
EXPOSE 18789

CMD ["openclaw-cn", "gateway", "--port", "18789"]


构建：

bash
docker build -t openclaw-cn:2026.2.5 -f Dockerfile .


然后在 `~/.openclaw/openclaw.json` 里写模型和网关配置。以阿里云百炼兼容接口接 GLM-4.7 为例，核心是这几项：

json
{
  "models": {
    "mode": "merge",
    "providers": {
      "bailian": {
        "baseUrl": "https://dashscope.aliyuncs.com/compatible-mode/v1",
        "apiKey": "换成你自己的API Key",
        "api": "openai-completions",
        "models": [
          {
            "id": "glm-4.7",
            "name": "glm-4.7",
            "contextWindow": 262144,
            "maxTokens": 65536
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": { "primary": "bailian/glm-4.7" },
      "workspace": "/root/clawd"
    }
  },
  "gateway": {
    "bind": "lan",
    "port": 18789,
    "auth": { "mode": "token", "token": "换成你自己的token" }
  }
}


启动容器，把配置目录挂载进去，实现配置和容器分离：

bash
docker run -d -p 18789:18789 -v ~/.openclaw:/root/.openclaw openclaw-cn:2026.2.5


进容器用 `docker exec -it <容器名> bash`，然后 `openclaw-cn tui` 就能开始对话。WebUI 访问需要 SSH 隧道，配置起来麻烦一些；TUI 在终端里直接聊，对先跑通流程的人来说够用。

示例里的 API Key、gateway token 都要换掉，尤其是服务器有公网 IP 的时候，别拿 `abc123` 这种示例值直接上线。

## 常见问题

**"龙虾"和 OpenClaw 是什么关系？**
龙虾是 OpenClaw 的中文俗称，项目原名 Clawdbot/Moltbot，是一个开源 AI 助手框架，可以部署在自有电脑或云服务器上，通过聊天软件交互。

**2核2G 能跑吗？**
能。官方说最低支持 2核2G，适合简单体验；推荐配置是 2核4G 或更高。通道开得多、任务跑得重，就往上升配置。

**部署完一定要买模型套餐吗？**
要接模型才能干活。面板支持 Coding Plan、模型 API、自定义模型三种接入方式，按套餐或按量付费都行。

**重装系统会丢数据吗？**
会。重装会清空服务器内全部数据，务必在备份选项里选"备份后重装"，之后可以用快照回退。

**部署完在哪里跟它聊天？**
在你接入的聊天软件里，比如 QQ、企业微信、飞书；Docker 部署可以先用终端里的 TUI 对话。

**Windows 能部署吗？**
OpenClaw 目前对 Windows 支持较弱，不建议基于 Windows 尝鲜。云服务器选 Linux 系统更稳。

按你的情况选：

- **第一次部署，想最快跑通**：新购一台轻量服务器，应用镜像选 OpenClaw，入门型 2核4G，国内地域，下单后进 Agent 控制台配模型和通道
- **有闲置服务器、数据可清**：重装系统选 OpenClaw 模板，先备份
- **服务器跑着别的应用**：Docker 或轻量云 AI 助手
- **只接 Telegram/Discord 且已有 AWS 资源**：可以自己对比 AWS Lightsail 的 OpenClaw 方案后再定

下单前确认两件事：地域和你要接的聊天软件匹配；模型费用单独预算，别只算服务器。

👉 [去腾讯云 OpenClaw 活动页看当前套餐](https://bit.ly/yunhuodong)
