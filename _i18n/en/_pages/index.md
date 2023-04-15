
`LLM`s like `ChatGPT`s are powerful. They leverage `prompt` instructions to act as a specific role to respond you.

What if several different roles chat together to do your job...

For example, when you require to install a software, there may be 3 AI `chatRoles` involved:

- WebSearcher: be good at choosing search keywords and picking proper webpage results
- Downloader: knows how to download files
- ShellExecutor: writes/executes shell commands, to make it works

Note that, there may be several small conversations between these roles. For example, download url may be broken, shell execution may encounter exceptions. In these situations, roles may require more web searches to get it right.

This is what we want to achieve: to build a huge specialized AI `chatRoles` `repo`:

- `prompt`s based roles
- `fine-tuned` specilized model roles
- task `coordinator` roles
- action roles, like web surfer, API invoker, shell executor, etc. (with security concerns).
- big roles: several roles bounded together based on routines
- ...

You may create/verify/refine/report, (and most important)chat with each one of the `chatRoles` in the repo **on the fly**, right beside your `chatbox`!

Behind all these scenarios, we utilize the `vectorStore`/`LLM`s, to match the exact `chatRoles` to your conversation.

More over, letting `chatRoles` to have small conversations, has the advantage to control the context length, as the big boss, you needn't know about each details, and things are settled!

First things first, we are defining the specifications for `chatRoles` and `coordination`s!

So, stay tuned.
