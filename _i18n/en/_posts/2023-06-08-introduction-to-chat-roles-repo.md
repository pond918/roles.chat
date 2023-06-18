---
layout: post
title: A gentle introduction to the chat-roles-repo service
date: 2023-06-08 21:01:00+0800
description: The first online chat-roles-repo service goes on (sandbox)[https://repo-sandbox.roles.chat/docs/api#/] now.
tags: tag-quick-start tag-services
giscus_comments: true
related_posts: true
---

## Quick Overview of chat-roles-repo Service

A repository of chat-roles. Here you can:

- Define your own chat-roles.
  - Your roles can collaborate with other roles, like a team, to accomplish a task.
- You can also use existing roles in the repository to achieve your goals.
- You can directly converse with chat-roles and they will respond like a chatbot.
  - You can also call chat-roles through the API, similar to web services, to return json data

## How chat-roles-repo works

Three genesis roles have been predefined in the chat roles-repo：

- do-nothing-role
- chat-role
- roles-role

Take them as an example to see how chat-roles work.

### #1 do-nothing-role

The basic information of this role is as follows:

```json
{
  "nick": "EmptyRole", // Give him a nickname
  "goal": "Do nothing", // Each role has a clear task goal
  "published": true, // Can only be used after publication, and cannot be changed after publication
}
```

The goal of this role is to do nothing. Used in the system to associate with each user （including you:）.So your character is, like a boss, doing nothing ;)

> To create your own chat-roles, please refer to [roles API](https://repo-sandbox.roles.chat/docs/api#/chat-roles)

### #2 chat-role

The character must do something now，

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

The goal of this role is to chat with users, and during the chat process, recommend relevant experts in chat-roles repo to users to help them solve problems.

To achieve this goal, within each chat role, we need to define a role host to host work execution. Let's see how to do it:

#### role-host

Now we define a host for the chat role,

```json
{
    "roleId": "chat-role", // The role associated with this host
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

Simply put, a host contains:

- Several entry points to respond to requests from others
- Several members to help oneself complete tasks

Specifically, for this host, there is a default entry. When a request for 'req. prompt' arrives, it is directly sent to 'to': 'llm'. After receiving the response from llm, it is then sent to 'to': '@ RR # list'. Finally, the result is returned to the user.

The problem is, what are 'llm' and '@ RR # list'? They help the host execute specific instructions, which we call partners：

##### partners

These are all his partners. There are two types of partners:

- members: Team members, that is, another role
- tools: Tool plugin，

For example, when you build a car, you need a wrench (tool) and an engineer (member), and the members are all named, so they are hung under "members".

Llm is a chat-GPT like tool that sends user questions to llm and returns the results，

> There are many built-in tools：llm, restAPI, json, db.. Developers can apply to register more tool plugins

@RR # list means to send the results to the list entry of RR. RR is the third role to be discussed next.

> For role-host related operations, please refer to [host API](https://repo-sandbox.roles.chat/docs/api#/chat-roles%20implementation)

### #3 roles-role

Check the information：

```json
{
  "nick": "RolesRole", //As the name suggests, recommend roles for roles
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

> Note that when creating your own roles, please accurately describe "goal," "skills," and "professionals," so that RolesRole can recommend your roles to those in need.

This role has 2 entrances, and the default entrance does nothing; The entry "list" is responsible for recommending relevant roles to users based on the received content. The host is as follows：

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

Simply put, after RR receives the content, it sends it to llm based on the prompt template, generates relevant professionals and skills for the content, and then calls our vector database through the rest API to search for all chat-roles in the warehouse and return the most matching role list.

> The detailed definition of this host can be found in [Here](https://repo-sandbox.roles.chat/docs/api#/chat-roles%20implementation/HostsController_findOne)，Click on"Try it out"，fill in the id as roles-role-host

I have finally defined a useful role and urgently need to try it out：

### role-actor

To use an existing role, you need to[create an actor](https://repo-sandbox.roles.chat/docs/api#/chat-actors/ActorsController_create)，

Then you can [chat] with him(https://repo-sandbox.roles.chat/docs/api#/chat-APIs)，or[API calls](https://repo-sandbox.roles.chat/docs/api#/chat-APIs)，Integrated into your own system.

> Note that when an actor needs to use the llm tool, they need to call the user's own llm account or token, which will be further elaborated in this section.
Of course, all of these are currently provided using APIs. If there were a convenient client tool to use these functions, it would be perfect. Stay tuned~

## Summarize

Through the above three examples, we briefly introduced how to define a chat role and have them collaborate with each other.
Try creating your own roles team and unleash the powerful power of llm!

> Note that the current chat role repo is a sandbox environment and is only used for testing. The data you create may be lost at any time, please make a backup.
> The service is still in the alpha phase and is open source at any time
