
今年以来，`LLM`s（智能语言大模型）比如`ChatGPT`等的威力炸翻全场。当您向它们发出提示语`prompt`指令后，它们会化身各个领域的专家，回应您的提问，解决您的问题。

如果有多个不同领域的专家一起配合，帮您解决问题呢？

例如，当您需要安装软件时，程序将为您自动匹配3个AI `ChatRoles`专家：

- WebSearcher：擅长选择合适的搜索关键词和正确的网页结果
- Downloader：知悉如何下载文件
- ShellExecutor：编写/执行脚本命令，使其正常工作

值得注意的是，这些专家之间可能会同时展开几个小窗对话：例如，下载的网址被损坏，脚本执行时遇到错误，等等。遇到这种情况，`ChatRoles`专家将会进行更多的网络搜索，以找到正确的合适的答案。

我们想要实现的就是：建立一个高度专业化的AI `ChatRoles` `repo`专家库：

- 基于提示语的专家角色
- 模型微调（fine-tuned）的专家角色
- 任务协调的专家角色
- 执行外部动作的专家角色，如网络浏览器、API调用程序、脚本执行程序等（包括安全问题）。
- 大师（BigRole）的角色：整合若干专家成员功能的大师级角色
- ……

您可以直接在聊天框旁创建、验证、改进或报告你自己的ChatRole，，最重要的是您可以很方便的与专家库中任意一个`ChatRoles`专家互动，借鉴他们的意见！

在所有这些场景背后，我们利用vectorStore/LLMs规则，为您的对话匹配最合适的`ChatRoles`专家成员。

此外，让`ChatRoles`的专家们进行小窗对话的优势在于控制聊天上下文的长度，您不必了解每个细节，专家们会轻松解决您的问题！

我们首先要做的是，定义`ChatRoles`专家库的规范和协作规则！

所以，请持续保持关注。