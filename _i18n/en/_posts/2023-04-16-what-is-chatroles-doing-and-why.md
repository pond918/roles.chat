---
layout: post
title: What is Chatroles doing? Why do I need it?
date: 2023-04-16 08:35:00+0800
description: A brief introduction on how ChatRoles works, why do we need it, and how to use it.
tags: tag-quick-start
categories: quick-start
giscus_comments: true
related_posts: true
---

## What is ChatRoles?

Since the launch of `ChatGPT`s, we have all felt its power - just ask it a question directly, even more convenient than Google.

`ChatRoles` is a task processing service built on the basis of `ChatGPT`s. So why do we need `ChatRoles` instead of directly asking `ChatGPT`s?

In one word, when a problem/task is too complex and requires **over thousands** of Q&As to clarify, we need a method to break down the task and assign it to multiple roles for execution.

This can include tasks such as market research, writing a novel, developing a game, or even mundane tasks like installing software.

The `ChatTree` structure, proposed by `ChatRoles`, is a tree-like chat structure where tasks are broken down and executed in small groups before being consolidated in a larger group for delivery of results.

## Why do we need ChatRoles?

So why do we have to make it so complicated?

This involves several technical issues.

##### Contextual constraints

First, `ChatGPT`s has a ~4k~30k tokens limitation. This means that if you chat with it too much, it will **forget** the previous conversation and won't be able to handle too complex problems.

Therefore, using the context structure of `ChatTree`, subtasks can be split into sub-conversations, which won't enlarge the main context length. `ChatRoles` works like team members who cooperate and communicate privately. As the big boss, you don't need to grasp each details.

##### Automation

If a task involves thousands or more Q&As, you probably wouldn't want to follow up on every details. Therefore, it's necessary to leverage the ability of the `ChatGPT` role-playing to enable multiple `ChatRoles` to communicate with each other automatically.

##### Fine-tuned models

Some `ChatRoles` require not only role-playing, but also specific professional skills, such as the Medical Expert role. We can fine-tune a model for the specific domain and make it available in the `ChatRoles` repo for community to use.

##### Task knowledge base

Each task has its own dedicated knowledge base. For example, when developing a game app, you can consult with experts in the `ChatRoles` repo, including requirements analysts, architects, developers, etc. They'll firstly build various documentation and design based on your requirements, then code repo. All these need to be easy to query and update by `ChatRoles`.

These are implemented through a specifically designed `VectorStore` infrastructures.

## How to use ChatRoles?

There are the following ways to use `ChatRoles`.

##### `ChatBox` user

It's basically the same as chatting with `ChatGPT`. The only difference is that when you input your question, we will automatically recommend some related `ChatRoles` you can chat with, so that you can get better answers.

##### ChatRoles user

Here, you can create/train/tune your own `ChatRoles`, contribute to the repo, and share them with the community.

##### ChatRoles coordinator

As a coordinator user, the main goal is to effectively organize multiple `ChatRoles` and build a `BigRole`, in order to accomplish more complex tasks.

##### ChatRoles developer

`ChatRoles` can not only converse with people, but also with third-party services such as APIs, web, databases, shells, etc. This requires developers to do some driver-like development work. We look forward to building this together with community. Remember, the first rule is **SECURITY**.
