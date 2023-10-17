# fakeopen api 文档

## 目录

- [基本信息](#%E5%9F%BA%E6%9C%AC%E4%BF%A1%E6%81%AF)
- [ChatGPT 相关](#chatgpt-%E7%9B%B8%E5%85%B3)
  - [基础信息](#%E5%9F%BA%E7%A1%80%E4%BF%A1%E6%81%AF)
  - [接口列表](#%E6%8E%A5%E5%8F%A3%E5%88%97%E8%A1%A8)
    - [1. /api/conversation](#1-apiconversation)
    - [2. /api/models](#2-apimodels)
    - [3. /api/conversations](#3-apiconversations)
    - [4. /api/conversation/\<conversation id>](#4-apiconversationconversation-id)
    - [5. /api/conversation/\<conversation id>](#5-apiconversationconversation-id)
    - [6. /api/conversations](#6-apiconversations)
- [OpenAI API 相关](#openai-api-%E7%9B%B8%E5%85%B3)
  - [基础信息](#%E5%9F%BA%E7%A1%80%E4%BF%A1%E6%81%AF-1)
  - [接口列表](#%E6%8E%A5%E5%8F%A3%E5%88%97%E8%A1%A8-1)
    - [1. /v1/chat/completions](#1-v1chatcompletions)
- [登录相关](#%E7%99%BB%E5%BD%95%E7%9B%B8%E5%85%B3)
  - [基础信息](#%E5%9F%BA%E7%A1%80%E4%BF%A1%E6%81%AF-2)
  - [接口列表](#%E6%8E%A5%E5%8F%A3%E5%88%97%E8%A1%A8-2)
    - [1. /auth/preauth](#1-authpreauth)
    - [2. /auth/login](#2-authlogin)
    - [3. /auth/refresh](#3-authrefresh)
    - [4. /auth/platform/login](#4-authplatformlogin)
    - [5. /auth/platform/refresh](#5-authplatformrefresh)
- [Share Token 相关](#share-token-%E7%9B%B8%E5%85%B3)
  - [基础信息](#%E5%9F%BA%E7%A1%80%E4%BF%A1%E6%81%AF-3)
  - [接口列表](#%E6%8E%A5%E5%8F%A3%E5%88%97%E8%A1%A8-3)
    - [1. /token/info/\<share token>](#1-tokeninfoshare-token)
    - [2. /token/register](#2-tokenregister)
- [Pool Token 相关](#pool-token-%E7%9B%B8%E5%85%B3)
  - [基础信息](#%E5%9F%BA%E7%A1%80%E4%BF%A1%E6%81%AF-4)
  - [接口列表](#%E6%8E%A5%E5%8F%A3%E5%88%97%E8%A1%A8-4)
    - [1. /pool/update](#1-poolupdate)


## 基本信息
* Base URL: `https://ai.fakeopen.com`
* 本服务也提供每日域名，格式为：`ai-<yyyymmdd>.fakeopen.com`，如：`ai-20230913.fakeopen.com` 。
* 本服务为 `Pandora` 的一部分，用于提供 `ChatGPT` / `OpenAI` 相关的接口。
* **本服务完全免费，服务器及带宽均为自掏腰包，请大户手下留情，不要滥用。我和网友们谢谢你们了！**

## `ChatGPT` 相关

### 基础信息
* 请求接口需要提供 `Authorization` 或 `X-Authorization` 头，值为 `Bearer <Token>` 。
* `Token` 可以是 `Access Token` 或 `Share Token` 。
* **请求字段：** 请自行使用浏览器开发者工具查看。
* **返回类型：** `application/json`
* **返回字段：** 请自行使用浏览器开发者工具查看。
* **报错信息：** 内容包含在 `detail` 字段中。

### 接口列表

#### 1. `/api/conversation` 
* 对应 `https://chat.openai.com/backend-api/conversation` 的用法。
* **接口描述：** 发送对话，获取回复。
* **HTTP方法：** `POST`
* **请求类型：** `application/json`
* **返回字段：** 返回 `text/event-stream` 流式数据，需要自行解析。
* **频率控制：** 根据 `IP` 地址 `3/10s` 限制，被限制时返回 `429` 错误码。

#### 2. `/api/models` 
* 对应 `https://chat.openai.com/backend-api/models` 的用法。
* **接口描述：** 列出账号可用的模型。
* **HTTP方法：** `GET`
* **频率控制：** 无。
* **特别说明：** 可根据其中是否有 `GPT-4` 模型来判断账号是否为 `ChatGPT Plus` 。

#### 3. `/api/conversations` 
* 对应 `https://chat.openai.com/backend-api/conversations` 的用法。
* **接口描述：** 以分页方式列出会话列表。
* **HTTP方法：** `GET`
* **频率控制：** 无。
* **特别说明：** 使用隔离会话 `Share Token` 时，会话列表中只会显示 `Share Token` 所属的会话。

#### 4. `/api/conversation/<conversation id>` 
* 对应 `https://chat.openai.com/backend-api/conversation/<conversation id>` 的用法。
* **接口描述：** 根据 `conversation id` 删除指定会话。
* **HTTP方法：** `PUT`
* **请求类型：** `application/json`
* **请求字段：** ```{"is_visible": false}```
* **频率控制：** 无。

#### 5. `/api/conversation/<conversation id>` 
* 对应 `https://chat.openai.com/backend-api/conversation/<conversation id>` 的用法。
* **接口描述：** 根据 `conversation id` 修改会话标题。
* **HTTP方法：** `PUT`
* **请求类型：** `application/json`
* **请求字段：** ```{"title": "New Title"}```
* **频率控制：** 无。

#### 6. `/api/conversations` 
* 对应 `https://chat.openai.com/backend-api/conversations` 的用法。
* **接口描述：** 清除所有会话。
* **HTTP方法：** `PUT`
* **请求类型：** `application/json`
* **请求字段：** ```{"is_visible": false}```
* **频率控制：** 无。
* **特别说明：** 使用隔离会话 `Share Token` 时，不可调用本接口。

#### 更多接口请自行使用浏览器开发者工具查看。可在官方查看，也可以部署 `Pandora Cloud` 后查看。

## `OpenAI API` 相关

### 基础信息
* 请求接口需要提供 `Authorization` 或 `X-Authorization` 头，值为 `Bearer <Token>` 。
* `Token` 可以是 `Access Token` 或 `sk-` 开头的官方 `API Key` ，**会扣官方额度**。
* `Token` 可以时 `Share Token` 或 `Pool Token` ，此时为模拟接口，**不会扣官方额度**。
* **官方文档：** https://platform.openai.com/docs/api-reference

### 接口列表

#### 1. `/v1/chat/completions`
* 对应 `https://api.openai.com/v1/chat/completions` 的用法。
* **官方文档：** https://platform.openai.com/docs/api-reference/chat/create
* **接口描述：** 发送对话，获取回复。可使用 `Share Token` 或 `Pool Token` 模拟免费调用。绝大多数 `OpenAI API` 客户端均支持。
* **模型映射：** 
    * `gpt-3.5-turbo` -> `text-davinci-002-render-sha`，真实长度为：`8K` 。
    * `gpt-3.5-turbo-0301` -> `text-davinci-002-render-sha`，真实长度为：`8K` 。
    * `gpt-3.5-turbo-0613` -> `text-davinci-002-render-sha`，真实长度为：`8K` 。
    * `gpt-3.5-turbo-16k` -> `text-davinci-002-render-sha`，真实长度为：`8K` 。
    * `gpt-3.5-turbo-16k-0613` -> `text-davinci-002-render-sha`，真实长度为：`8K` 。
    * `gpt-4` -> `gpt-4`，真实长度为：`4K` 。
    * `gpt-4-0314` -> `gpt-4`，真实长度为：`4K` 。
    * `gpt-4-0613` -> `gpt-4`，真实长度为：`4K` 。
    * `gpt-4-32k` -> `gpt-4-plugins`，真实长度为：`8K` 。
    * `gpt-4-32k-0314` -> `gpt-4-plugins`，真实长度为：`8K` 。
    * `gpt-4-32k-0613` -> `gpt-4-plugins`，真实长度为：`8K` 。
* **请求字段：** 同官方，但不保证支持以下字段：
    * `functions`
    * `function_call`
    * `temperature`
    * `top_p`
    * `n`
    * `stop`
    * `max_tokens`
    * `presence_penalty`
    * `frequency_penalty`
    * `logit_bias`
    * `user`
* **返回字段：** 返回 `text/event-stream` 流式数据，需要自行解析。
* **频率控制：** 根据 `IP` 地址 `3/10s` 限制，被限制时返回 `429` 错误码。
* **特别说明：** 
    * 扣官方额度时，只存在官方频率限制。
    * 官方 `ChatGPT` 存在同时只能有 `1` 个会话的限制，建议使用多个账号组合 `Pool Token` 来解决。

#### 其余官方接口均直接转发到官方，不做任何处理。
 
## 登录相关

### 基础信息
* 登录相关接口并不支持 `Google` / `Microsoft` 等第三方登录。
* 可以在 https://ai.fakeopen.com/auth 操作界面中进行同等操作。
* **请务必仅用来操作自己的账号！！！**，撞号后果自负，且会导致拉黑 `IP` 或 `ASN` 。
* **返回类型：** `application/json`
* **报错信息：** 内容包含在 `detail` 字段中。

### 接口列表

#### 1. `/auth/preauth`
* **接口描述：** 获取登录预授权，具体见：https://zhile.io/2023/05/19/how-to-get-chatgpt-access-token-via-pkce.html
* **HTTP方法：** `GET`
* **频率控制：** 无。

#### 2. `/auth/login`
* **接口描述：** 使用账号信息登录，获取供 [ChatGPT](https://chat.openai.com) 使用的 `Access Token` 等信息。
* **HTTP方法：** `POST`
* **请求类型：** `application/x-www-form-urlencoded`
* **请求字段：** 
    * `username`：`ChatGPT` 账号。
    * `password`：`ChatGPT` 密码。
    * `mfa_code`：开启二次验证，需要提供。否则不需要。
* **返回字段：** 返回 `Access Token` 和 `Refresh Token` 等信息。
* **频率控制：** 根据IP地址 `6/1m` 限制，被限制时返回 `429` 错误码。
* **特别说明：** 可直接调用，无需先调用**获取登录预授权**接口。也无需支持国家的梯子。

#### 3. `/auth/refresh`
* **接口描述：** 使用 `Refresh Token` 获取供 [ChatGPT](https://chat.openai.com) 使用的 `Access Token` 等信息。
* **HTTP方法：** `POST`
* **请求类型：** `application/x-www-form-urlencoded`
* **请求字段：** 
    * `refresh_token`：`ChatGPT` 的 `Refresh Token`。
* **返回字段：** 返回 `Access Token` 等信息。
* **频率控制：** 无。

#### 4. `/auth/platform/login`
* **接口描述：** 使用账号信息登录，获取供 [Platform](https://platform.openai.com) 使用的 `Access Token` 等信息，用做获取用度、账单等信息。
* **HTTP方法：** `POST`
* **请求类型：** `application/x-www-form-urlencoded`
* **请求字段：** 
    * `username`：`ChatGPT` 账号。
    * `password`：`ChatGPT` 密码。
    * `mfa_code`：开启二次验证，需要提供。否则不需要。
* **返回字段：** 返回 `Access Token` 和 `Refresh Token` 等信息。
* **频率控制：** 根据IP地址 `6/1m` 限制，被限制时返回 `429` 错误码。

#### 5. `/auth/platform/refresh`
* **接口描述：** 使用 `Refresh Token` 获取供 [Platform](https://platform.openai.com) 使用的 `Access Token` 等信息，用做获取用度、账单等信息。
* **HTTP方法：** `POST`
* **请求类型：** `application/x-www-form-urlencoded`
* **请求字段：** 
    * `refresh_token`：`Platform` 的 `Refresh Token`。
* **返回字段：** 返回 `Access Token` 等信息。
* **频率控制：** 无。

## Share Token 相关

### 基础信息
* 基本格式为：`fk-[0-9a-zA-Z_\-]{43}` ，长度为：`46` 。
* 使用该功能可以实现多人共享一个账号，可以进行会话隔离。
* 可以在共享账号是隐藏 `邮箱` 等账号信息，防止被撞号。
* 可以在共享账号时隐藏 `Access Token` ，官方 `Access Token` 在有效期内无法吊销，泄露损失很大。
* 可以灵活控制 `Share Token` 的有效期，过期后会自动失效。也可随时手动吊销。
* 可以限制 `Share Token` 使用的站点，防止被滥用。
* 可以在部署的 `Pandora Cloud` 上使用 `<部署地址>/auth/login_share?token=<Share Token>` 快速使用。
* 可以在 https://ai.fakeopen.com/token 操作界面中进行同等操作。
* 既可以使用在 `ChatGPT` 上，也可以使用在模拟 `OpenAI API` 的调用上。
* **返回类型：** `application/json`
* **报错信息：** 内容包含在 `detail` 字段中。

### 接口列表

#### 1. `/token/info/<share token>`
* **接口描述：** 获取 `Share Token` 的详细信息。
* **HTTP方法：** `GET`
* **返回字段：** 返回 `Share Token` 所的详细信息。
* **频率控制：** 无。
* **特别说明：**
    * `Authorization` 可选，值为 `Bearer <Access Token>` 。
    * 若提供有效 `Share Token` 注册用户的 `Access Token` ，则返回 `Share Token` 对各个模型的**当日**用度信息。
    * 此开共享 `ChatGPT Plus` 车必备统计数据功能。
 
#### 2. `/token/register`
* **接口描述：** 注册或更新 `Share Token` 。
* **HTTP方法：** `POST`
* **请求类型：** `application/x-www-form-urlencoded`
* **请求字段：** 
    * `unique_name`：一个唯一的名字，这里要注意相同 `unique_name` 和 `access_token` **始终生成相同**的 `Share Token` 。
    * `access_token`：`ChatGPT` 账号的 `Access Token` 。
    * `site_limit`：限制 `Share Token` 使用的站点，格式为：`https://xxx.yyy.com`，可留空不作限制。
    * `expires_in`：`Share Token` 的有效期，单位为：`秒`，为 `0` 时表示与 `Access Token` 同效，为 `-1` 时吊销 `Share Token` 。
    * `show_conversations`：是否进行会话隔离，`true` 或 `false` ，默认为 `false` 。
    * `show_userinfo`：是否隐藏 `邮箱` 等账号信息，`true` 或 `false` ，默认为 `false` 。
* **返回字段：** 返回 `Share Token` 等信息。
* **频率控制：** 无。

## Pool Token 相关

### 基础信息
* 基本格式为：`pk-[0-9a-zA-Z_\-]{43}` ，长度为：`46` 。
* 使用该功能可以将最多 `100` 个 `Share Token` 组合在一起。
* 使用组合的 `Pool Token` 时会自动轮转，突破 `ChatGPT` 同时只能有 `1` 个会话的限制。
* 可以在 https://ai.fakeopen.com/pool 操作界面中进行同等操作。
* 仅可以使用在模拟 `OpenAI API` 的调用上。
* `pk-this-is-a-real-free-pool-token-for-everyone` 是一个可用的共享 `Pool Token` ，容量有几千个 `Share Token` 。**感谢社区热心人士提供。**
* **返回类型：** `application/json`
* **报错信息：** 内容包含在 `detail` 字段中。

### 接口列表

#### 1. `/pool/update`
* **接口描述：** 注册或更新 `Pool Token` 。
* **HTTP方法：** `POST`
* **请求类型：** `application/x-www-form-urlencoded`
* **请求字段：** 
    * `share_tokens`：`Share Token` 列表，每行 `1` 个，最多 `100` 个。
    * `pool_token`：`Pool Token` ，可留空，留空时生成新 `Pool Token` 。不为空则更新 `Pool Token` 。
* **返回字段：** 返回 `Pool Token` 等信息。
* **频率控制：** 无。
* **特别说明：** `share_tokens` 为空，且 `pool_token` 不为空时，会吊销指定的 `Pool Token` 。
 