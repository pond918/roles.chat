---
layout: post
title: 快速了解 chat-roles-repo 服务
date: 2023-06-08 21:01:00+0800
description: 首个 chat-roles-repo 服务已发布到 (沙箱)[https://chatroles.onrender.com/docs/api#/].
tags: tag-quick-start tag-services
giscus_comments: true
related_posts: true
---

## chat-roles-repo 是什么

一个chat-roles仓库。在此你可以：

- 定义自己的chat-roles
  - 你的role可以与其他roles协作，像一个团队一样，共同完成一个任务。
- 你也可以直接使用仓库中已有的roles，完成你的目标
  - 你可以直接与chat-roles对话，像chatbot一样回复你
  - 你也可以通过API调用chat-roles，像web服务一样，返回json数据

## chat-roles-repo 如何工作

chat-roles-repo中已经预定义了3个创世roles：

- do-nothing-role
- chat-role
- roles-role

以他们为例，看看chat-roles如何工作的。

### #1 do-nothing-role

该role的基础信息如下：

```json
{
  "nick": "EmptyRole", // 给他一个昵称
  "goal": "Do nothing", // 每个role有一个明确的任务目标
  "published": true, // 发布后才能使用，且发布后不能更改
}
```

该role的目标就是啥也不干。系统中用来关联到每个用户（包括你:）。所以你的角色就是，像boss一样，啥也不干 ;)

> 要创建你自己的chat-roles，请参阅 [roles API](https://chatroles.onrender.com/docs/api#/chat-roles)

### #2 chat-role

该角色必须干点事儿了，

```json
{
  "nick": "ChatBot",
  "goal": "Default role to chat with user in new conversation, with roles recommendations.",
  "skills": [
    "to chat with user purposelessly.",
    "recommend roles during chatting."
  ]
}
```

该角色的目标是与用户聊天，聊天的过程中，推荐chat-roles-repo中相关的专家给用户，帮用户解决问题。

为了达到这个目标，在每个chat-role内，我们需要定义一个role-host来主持工作执行。我们看看怎么做：

#### role-host

现在我们为chat-role定义一个host，

```json
{
    "roleId": "chat-role", // 该host关联的role
    "onEntries": [
      {
        "handle": {
          "to": "llm",
          "prompt": "<<req.prompt>>",
          "responses": [
            { "then": { "to": "@RR#list" } },
          ]
        },
      },
    ],
    "members": [
      {
        "name": "RR",
        "hostId": "roles-role-host",
        "roleId": "roles-role",
      },
    ],
}
```

简单来说，一个host包含：

- 若干入口entry，来响应别人的请求
- 若干成员members，来帮助自己完成任务

具体看这个host，有一个默认的entry，当一个请求"req.prompt"过来时，直接发给"to": "llm"，拿到llm的结果response之后，再发给"to": "@RR#list"。最后返回结果给用户。

问题来了，"llm"，"@RR#list"是啥，他们帮助host执行具体指令，我们称之为partners：

##### partners

这些都是他的合作伙伴。partners分2种：

- members: 团队成员，也就是另一个role
- tools: 工具插件，

比如你造车，需要一个扳手(tool)和一个工程师(member), 成员都是有名有姓的，所以挂在 "members" 下面。

llm就是一个chat-GPT一样的工具，将用户的问题发给llm，拿到返回结果，

> 内置的工具有很多：llm, restAPI, json, db.. 开发人员可以申请注册更多工具插件

@RR#list意思是将结果发给 RR 的 list 入口。RR 就是接下来要说的，第3个role。

> role-host相关的操作，请参阅 [host API](https://chatroles.onrender.com/docs/api#/chat-roles%20implementation)

### #3 roles-role

看看信息：

```json
{
  "nick": "RolesRole", // 顾名思义，推荐roles的role
  "goal": "To recommend suitable professional roles to help for user's question/topic/task.",
  "skills": [
    "has knowledge of all roles on roles.chat site.",
    "ability to understand user question/topic/task.",
    "ability to match best professional roles for the task to user.",
  ],
  "professionals": ["task assistant", "experts recommender"],
  "entries": [
    {}, // default null response
    {
      "name": "list",
      ... ...
    }
  ]
}
```

> 注意，当创建你自己的roles时，请准确描述"goal"，"skills"，"professionals"，从而方便RolesRole将你的roles推荐给需要的人。

该role有2个入口，默认的入口啥也不干；入口"list"负责根据接收的内容，推荐相关的roles给用户。其host如下：

```shell
{
  "roleId": "roles-role",
  "handle": {
    "to": 'llm',
    "prompt":
      'which professionals(exclude translators) are qualified to help answering the following question?(reply in most strict json list in english:[{"professional":"val", "goal":"val", "special skills":"val"}]):\n<<req.prompt>>',
    "responses": [
      { "then": { "to": "json" } },
      {
        ...
        "to": `restAPI:POST ${process.env.API_CR_VDB}/run/predict_1`
        ...
      }
    ]
}
```

简单来说，RR拿到内容后，根据提示语模版(prompt)发给llm，生成该内容相关的professionals和skills，再通过restAPI调用我们的vector database，搜索仓库里所有的chat-roles，返回最匹配的roles list。

> 该host的详细定义可参阅 [这里](https://chatroles.onrender.com/docs/api#/chat-roles%20implementation/HostsController_findOne)，点"Try it out"，id填 roles-role-host

终于定义好一个有用的角色了，迫切需要试用一下：

### role-actor

要使用一个现有的role，你要为这个role[创建一个actor](https://chatroles.onrender.com/docs/api#/chat-actors/ActorsController_create)，

然后你就可以和他[聊天](https://chatroles.onrender.com/docs/api#/chat-APIs)，或者[api调用](https://chatroles.onrender.com/docs/api#/chat-APIs)，集成到你自己的系统中去了。

> 注意，当actor需要使用llm工具时，需要调用用户自己的llm账号或token，这部分后续展开说。

当然所有这些，目前都是用api提供的，要是有个方便的客户端工具来使用这些功能就完美了，敬请期待哈~

## 总结

通过上面3个例子，我们简单介绍了如何定义一个chat-role，并让他们互相协作。

来尝试创建你自己的roles团队，释放llm强大的威力吧！

> 注意，目前的chat-role-repo是沙箱环境，仅用于测试，您创建的数据随时可能丢失，请做好备份。
> 服务还处于alpha阶段，随时开源
