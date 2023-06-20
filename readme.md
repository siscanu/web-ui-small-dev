# smol developer

***Human-centric & Coherent Whole Program Synthesis*** aka your own personal junior developer

> [Build the thing that builds the thing!](https://twitter.com/swyx/status/1657578738345979905) a `smol dev` for every dev in every situation

this is a prototype of a "junior developer" agent (aka `smol dev`) that scaffolds an entire codebase out for you once you give it a product spec, but does not end the world or overpromise AGI. instead of making and maintaining specific, rigid, one-shot starters, like `create-react-app`, or `create-nextjs-app`, this is basically [`create-anything-app`](https://news.ycombinator.com/item?id=35942352) where you develop your scaffolding prompt in a tight loop with your smol dev.

AI that is helpful, harmless, and honest is complemented by a codebase that is simple, safe, and smol - <200 lines of Python and Prompts, so this is easy to understand and customize.

<p align="center">
  <img height=200 src="https://pbs.twimg.com/media/FwEzVCcaMAE7t4h?format=jpg&name=large" />
</p>

*engineering with prompts, rather than prompt engineering* 

The demo example in `prompt.md` shows the potential of AI-enabled, but still firmly human developer centric, workflow:

- Human writes a basic prompt for the app they want to build
- `main.py` generates code
- Human runs/reads the code
- Human can:
  - simply add to the prompt as they discover underspecified parts of the prompt
  - manually runs the code and identifies errors
  - *paste the error into the prompt* just like they would file a GitHub issue
  - for extra help, they can use `debugger.py` which reads the whole codebase to make specific code change suggestions

Loop until happiness is attained. Notice that AI is only used as long as it is adding value - once it gets in your way, just take over the codebase from your smol junior developer with no fuss and no hurt feelings. (*we could also have smol-dev take over an existing codebase and bootstrap its own prompt... but that's a Future Direction*)

*Not no code, not low code, but some third thing.* 

Perhaps a higher order evolution of programming where you still need to be technical, but no longer have to implement every detail at least to scaffold things out.

## examples/prompt gallery

- [6 minute video demo](https://youtu.be/UCo7YeTy-aE) - (sorry for sped up audio, we were optimizing for twitter, bad call)
  - this was the original smol developer demo - going from prompt to full chrome extension that requests and stores and apikey, generates a popup window, reads and transmits page content, and usefully summarizes any website with Anthropic Claude, switching models up to the 100k one based on length of input
  - the prompt is located in [prompt.md](https://github.com/smol-ai/developer/blob/main/prompt.md) and it outputs [/exampleChromeExtension](https://github.com/smol-ai/developer/tree/main/examples/exampleChromeExtension)
- `smol-plugin` - prompt to ChatGPT plugin ([tweet](https://twitter.com/ultrasoundchad/status/1659366507409985536?s=20), [fork](https://github.com/gmchad/smol-plugin))

  <img src="https://github.com/smol-ai/developer/assets/6764957/6ffaac3b-5d90-460a-a590-c8a8c004bd36" height=200 />

- [Prompt to Pokemon App](https://twitter.com/RobertCaracaus/status/1659312419485761536?s=20)
  
  <img src="https://github.com/smol-ai/developer/assets/6764957/15fa189a-3f52-4618-ac8e-2a77b6500264" height=200 />
  
- [Political Campaign CRM Program example](https://github.com/smol-ai/developer/pull/22/files)
- [Lessons from Creating a VSCode Extension with GPT-4](https://bit.kevinslin.com/p/leveraging-gpt-4-to-automate-the) (also on [HN](https://news.ycombinator.com/item?id=36071342))
- [7 min Video: Smol AI Developer - Build ENTIRE Codebases With A Single Prompt](https://www.youtube.com/watch?v=DzRoYc2UGKI) produces a full working OpenAI CLI python app from a prompt

  <img src="https://github.com/smol-ai/developer/assets/6764957/e80058f1-ea9c-42dd-87ff-004b61f08f2e" height=200 />
  
- [12 min Video: SMOL AI - Develop Large Scale Apps with AGI in one click](https://www.youtube.com/watch?v=zsxyqz6SYp8) scaffolds a surprisingly complex React/Node/MongoDB full stack app in 40 minutes and $9

  <img src="https://github.com/smol-ai/developer/assets/6764957/c51f9f8c-021d-446a-b44d-7a6f48e64550" height=200 />

I'm actively seeking more examples, please PR yours! 

sorry for the lack of examples, I know that is frustrating but I wasnt ready for so many of you lol

## major forks/alternatives

please send in alternative implementations, and deploy strategies on alternative stacks!

- **JS/TS**: https://github.com/PicoCreator/smol-dev-js A pure JS variant of smol-dev, allowing even smoler incremental changes via prompting (if you dun want to do the whole spec2code thing), allowing you to plug it into any project live (for better or worse)
- **C#/Dotnet**: https://github.com/colhountech/smol-ai-dotnet in C#!
- **Golang**: https://github.com/tmc/smol-dev-go in Go
- https://github.com/gmchad/smol-plugin automatically generate @openai plugins by specifying your API in markdown in smol-developer style
- your fork here!

## arch diagram

naturally generated with gpt4, like [we did for babyagi](https://twitter.com/swyx/status/1648724820316786688)
![image](https://github.com/smol-ai/developer/assets/6764957/f8fc68f4-77f6-43ee-852f-a35fb195430a)


### innovations and insights

> Please subscribe to https://latent.space/ for a fuller writeup and insights and reflections

- **Markdown is all you need** - Markdown is the perfect way to prompt for whole program synthesis because it is easy to mix english and code (whether `variable_names` or entire \`\`\` code fenced code samples)
  - turns out you can specify prompts in code in prompts and gpt4 obeys that to the letter
- **Copy and paste programming**
  - teaching the program to understand how to code around a new API (Anthropic's API is after GPT3's knowledge cutoff) by just pasting in the `curl` input and output
  - pasting error messages into the prompt and vaguely telling the program how you'd like it handled. it kind of feels like "logbook driven programming".
- **Debugging by `cat`ing** the whole codebase with your error message and getting specific fix suggestions - particularly delightful!
- **Tricks for whole program coherence** - our chosen example usecase, Chrome extensions, have a lot of indirect dependencies across files. Any hallucination of cross dependencies causes the whole program to error. 
  - We solved this by adding an intermediate step asking GPT to think through `shared_dependencies.md`, and then insisting on using that in generating each file. This basically means GPT is able to talk to itself...
  - ... but it's not perfect, yet. `shared_dependencies.md` is sometimes not comperehensive in understanding what are hard dependencies between files. So we just solved it by specifying a specific `name` in the prompt. felt dirty at first but it works, and really it's just clear unambiguous communication at the end of the day. 
  - see `prompt.md` for SOTA smol-dev prompting
- **Low activation energy for unfamiliar APIs**
  - we have never really learned css animations, but now can just say we want a "juicy css animated red and white candy stripe loading indicator" and it does the thing. 
  - ditto for Chrome Extension Manifest v3 - the docs are an abject mess, but fortunately we don't have to read them now to just get a basic thing done
  - the Anthropic docs (bad bad) were missing guidance on what return signature they have. so just curl it and dump it in the prompt lol.
- **Modal is all you need** - we chose Modal to solve 4 things:
  - solve python dependency hell in dev and prod
  - parallelizable code generation
  - simple upgrade path from local dev to cloud hosted endpoints (in future)
  - fault tolerant openai api calls with retries/backoff, and attached storage (for future use)

> Please subscribe to https://latent.space/ for a fuller writeup and insights and reflections

### caveats

We were working on a Chrome Extension, which requires images to be generated, so we added some usecase specific code in there to skip destroying/regenerating them, that we haven't decided how to generalize.

We dont have access to GPT4-32k, but if we did, we'd explore dumping entire API/SDK documentation into context.

The feedback loop is very slow right now (`time` says about 2-4 mins to generate a program with GPT4, even with parallelization due to Modal (occasionally spiking higher)), but it's a safe bet that it will go down over time (see also "future directions" below).

## install

it's basically: 

- `git clone https://github.com/smol-ai/developer`.
- copy over `.example.env` to `.env` filling in your API keys.

There are no python dependencies to wrangle thanks to using Modal as a [self-provisioning runtime](https://www.google.com/search?q=self+provisioning+runtime).

Unfortunately this project also uses 3 other things:

- Modal.com - [sign up](https://modal.com/signup), then `pip install modal-client && modal token new`
  - You can run this project w/o Modal following these instructions:
  - `pip install -r requirements.txt`
  - `export OPENAI_API_KEY=sk-xxxxxx` (your openai api key here)
  - `python main_no_modal.py YOUR_PROMPT_HERE`
- GPT-4 api (private beta) - this project now defaults to using `gpt-3.5-turbo` but it obviously wont be as good. we are working on a hosted version so you can try this out on our keys.
- (for the demo project only) anthropic claude 100k context api (private beta) - not important unless you're exactly trying to repro my demo

you'll have to adapt this code on a fork if you want to use it on other infra. please open issues/PRs and i'll happily highlight your fork here.

### trying the example chrome extension from the demo video

the `/examples/exampleChromeExtension` folder contains `a Chrome Manifest V3 extension that reads the current page, and offers a popup UI that has the page title+content and a textarea for a prompt (with a default value we specify). When the user hits submit, it sends the page title+content to the Anthropic Claude API along with the up to date prompt to summarize it. The user can modify that prompt and re-send the prompt+content to get another summary view of the content.`

- go to Manage Extensions in Chrome
- load unpacked
- find the relevant folder in your file system and load it
- go to any content heavy site
- click the cute bird
- see it work and rejoice

this entire extension was generated by the prompt in `prompt.md` (except for the images), and was built up over time by adding more words to the prompt in an iterative process.

## usage: smol dev

basic usage (by default it runs with `gpt-3.5-turbo`, but we strongly encourage running with `gpt-4` if you have access)

```bash
# inline prompt
modal run main.py --prompt "a Chrome extension that, when clicked, opens a small window with a page where you can enter a prompt for reading the currently open page and generating some response from openai" --model=gpt-4
```

after a while of adding to your prompt, you can extract your prompt to a file, as long as your "prompt" ends in a .md extension we'll go look for that file

```bash
# prompt in markdown file
modal run main.py --prompt prompt.md --model=gpt-4
```

each time you run this, the generated directory is deleted (except for images) and all files are rewritten from scratch. 

In the `shared_dependencies.md` file is a helper file that ensures coherence between files. This is in the process of being expanded into an official `--plan` functionality (see https://github.com/smol-ai/developer/issues/12)

### smol dev in single file mode

if you make a tweak to the prompt and only want it to affect one file, and keep the rest of the files, specify the file param:

```bash
modal run main.py --prompt prompt.md  --file popup.js
```

### smol dev without modal.com

By default, `main.py` uses Modal, beacuse it provides a nice upgrade path to a hosted experience (coming soon, so you can try it out without needing GPT4 key access).

However if you want to just run it on your own machine, you can run smol dev w/o Modal following these instructions:

```bash
pip install -r requirements.txt
export OPENAI_API_KEY=sk-xxxxxx # your openai api key here)

python main_no_modal.py YOUR_PROMPT_HERE
```

If no command line argument is given, **and** the file `prompt.md` exists, the main function will automatically use the `prompt.md` file. All other command line arguments are left as default. *this is handy for those using the "run" function on a `venv` setup in PyCharm for Windows, where no opportunity is given to enter command line arguments. Thanks [@danmenzies](https://github.com/smol-ai/developer/pull/55)* 

## usage: smol debugger

*this is a beta feature, very very MVP, just a proof of concept really*

take the entire contents of the generated directory in context, feed in an error, get a response. this basically takes advantage of longer (32k-100k) context so we basically dont have to do any embedding of the source.

```bash
modal run debugger.py --prompt "Uncaught (in promise) TypeError: Cannot destructure property 'pageTitle' of '(intermediate value)' as it is undefined.    at init (popup.js:59:11)"

# gpt4
modal run debugger.py --prompt "your_error msg_here" --model=gpt-4
```

## usage: smol pm

*this is even worse than beta, its kind of a "let's see what happens" experiment*

take the entire contents of the generated directory in context, and get a prompt back that could synthesize the whole program. basically `smol dev`, in reverse.

```bash
modal run code2prompt.py # ~0.5 second with gpt 3.5

# use gpt4
modal run code2prompt.py --model=gpt-4 # 2 mins, MUCH better results
```

We have done indicative runs of both, stored in `examples/code2prompt/code2prompt-gpt3.md` vs `examples/code2prompt/code2prompt-gpt4.md`. Note how incredibly better gpt4 is at prompt engineering its future self.

Naturally, we had to try `code2prompt2code`...

```bash
# add prompt... this needed a few iterations to get right
modal run code2prompt.py --prompt "make sure all the id's of the DOM elements, and the data structure of the page content (stored with {pageTitle, pageContent }) , referenced/shared by the js files match up exactly. take note to only use Chrome Manifest V3 apis. rename the extension to code2prompt2code" --model=gpt-4 # takes 4 mins. produces semi working chrome extension copy based purely on the model-generated description of a different codebase

# must go deeper
modal run main.py --prompt code2prompt-gpt4.md --directory code2prompt2code
```

We leave the social and technical impacts of multilayer generative deep-frying of codebases as an exercise to the reader.

## Development using a Dev Container

> this is a [new addition](https://github.com/smol-ai/developer/pull/30)! Please try it out and send in fixes if there are any issues.

We have configured a development container for this project, which provides an isolated and consistent development environment. This approach is ideal for developers using Visual Studio Code's Remote - Containers extension or GitHub's Codespaces.

If you have [VS Code](https://code.visualstudio.com/download) and [Docker](https://www.swyx.io/running-docker-without-docker-desktop) installed on your machine, you can make use of the devcontainer to create an isolated environment with all dependencies automatically installed and configured. This is a great way to ensure a consistent development experience across different machines.

Here are the steps to use the devcontainer:

1. Open this project in VS Code.
2. When prompted to "Reopen in Container", choose "Reopen in Container". This will start the process of building the devcontainer defined by the `Dockerfile` and `.devcontainer.json` in the `.devcontainer` directory.
3. Wait for the build to finish. The first time will be a bit longer as it downloads and builds everything. Future loads will be much faster.
4. Once the build is finished, the VS Code window will reload and you are now working inside the devcontainer.


<details>
<summary> Benefits of a Dev Container </summary>

1. **Consistent Environment**: Every developer works within the same development setup, eliminating "it works on my machine" issues and easing the onboarding of new contributors.

2. **Sandboxing**: Your development environment is isolated from your local machine, allowing you to work on multiple projects with differing dependencies without conflict.

3. **Version Control for Environments**: Just as you version control your source code, you can do the same with your development environment. If a dependency update introduces issues, it's easy to revert to a previous state.

4. **Easier CI/CD Integration**: If your CI/CD pipeline utilizes Docker, your testing environment will be identical to your local development environment, ensuring consistency across development, testing, and production setups.

5. **Portability**: This setup can be utilized on any computer with Docker and the appropriate IDE installed. Simply clone the repository and start the container.
</details>


## future directions

things to try/would accept open issue discussions and PRs:

- **specify .md files for each generated file**, with further prompts that could finetune the output in each of them
  - so basically like `popup.html.md` and `content_script.js.md` and so on
- **bootstrap the `prompt.md`** for existing codebases - write a script to read in a codebase and write a descriptive, bullet pointed prompt that generates it
  - done by `smol pm`, but its not very good yet - would love for some focused polish/effort until we have quine smol developer that can generate itself lmao
- **ability to install its own dependencies**
  - this leaks into depending on the execution environment, which we all know is the path to dependency madness. how to avoid? dockerize? nix? [web container](https://twitter.com/litbid/status/1658154530385670150)?
  - Modal has an interesting possibility: generate functions that speak modal which also solves the dependency thing https://twitter.com/akshat_b/status/1658146096902811657
- **self-heal** by running the code itself and use errors as information for reprompting 
  - however its a bit hard to get errors from the chrome extension environment so we did not try this
- **using anthropic as the coding layer**
  - you can run `modal run anthropic.py --prompt prompt.md --outputdir=anthropic` to try it
  - but it doesnt work because anthropic doesnt follow instructions to generate file code very well.
- **make agents that autonomously run this code in a loop/watch the prompt file** and regenerate code each time, on a new git branch
  - the code could be generated on 5 simultaneous git branches and checking their output would just involve switching git branches






<div align="center">
<img src="./docs/images/icon.svg" alt="icon"/>

<h1 align="center">ChatGPT Next Web</h1>

English / [简体中文](./README_CN.md)

One-Click to get well-designed cross-platform ChatGPT web UI.

一键免费部署你的跨平台私人 ChatGPT 应用。

[![Web][Web-image]][web-url]
[![Windows][Windows-image]][download-url]
[![MacOS][MacOS-image]][download-url]
[![Linux][Linux-image]][download-url]

[Web App](https://chatgpt.nextweb.fun/) / [Desktop App](https://github.com/Yidadaa/ChatGPT-Next-Web/releases) / [Issues](https://github.com/Yidadaa/ChatGPT-Next-Web/issues) / [Buy Me a Coffee](https://www.buymeacoffee.com/yidadaa)

[网页版](https://chatgpt.nextweb.fun/) / [客户端](https://github.com/Yidadaa/ChatGPT-Next-Web/releases) / [反馈](https://github.com/Yidadaa/ChatGPT-Next-Web/issues) / [QQ 群](https://github.com/Yidadaa/ChatGPT-Next-Web/discussions/1724) / [打赏开发者](https://user-images.githubusercontent.com/16968934/227772541-5bcd52d8-61b7-488c-a203-0330d8006e2b.jpg)

[web-url]: https://chatgpt.nextweb.fun
   
[download-url]: https://github.com/Yidadaa/ChatGPT-Next-Web/releases

[Web-image]: https://img.shields.io/badge/Web-PWA-orange?logo=microsoftedge

[Windows-image]: https://img.shields.io/badge/-Windows-blue?logo=windows

[MacOS-image]: https://img.shields.io/badge/-MacOS-black?logo=apple

[Linux-image]: https://img.shields.io/badge/-Linux-333?logo=ubuntu

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2FYidadaa%2FChatGPT-Next-Web&env=OPENAI_API_KEY&env=CODE&project-name=chatgpt-next-web&repository-name=ChatGPT-Next-Web)

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/Yidadaa/ChatGPT-Next-Web)

![cover](./docs/images/cover.png)

</div>

## Features

- **Deploy for free with one-click** on Vercel in under 1 minute
- Compact client (~5MB) on Linux/Windows/MacOS, [download it now](https://github.com/Yidadaa/ChatGPT-Next-Web/releases)
- Fully compatible with self-deployed llms, recommended for use with [RWKV-Runner](https://github.com/josStorer/RWKV-Runner) or [LocalAI](https://github.com/go-skynet/LocalAI)
- Privacy first, all data stored locally in the browser
- Markdown support: LaTex, mermaid, code highlight, etc.
- Responsive design, dark mode and PWA
- Fast first screen loading speed (~100kb), support streaming response
- New in v2: create, share and debug your chat tools with prompt templates (mask)
- Awesome prompts powered by [awesome-chatgpt-prompts-zh](https://github.com/PlexPt/awesome-chatgpt-prompts-zh) and [awesome-chatgpt-prompts](https://github.com/f/awesome-chatgpt-prompts)
- Automatically compresses chat history to support long conversations while also saving your tokens
- I18n: English, 简体中文, 繁体中文, 日本語, Français, Español, Italiano, Türkçe, Deutsch, Tiếng Việt, Русский, Čeština, 한국어

## Roadmap

- [x] System Prompt: pin a user defined prompt as system prompt [#138](https://github.com/Yidadaa/ChatGPT-Next-Web/issues/138)
- [x] User Prompt: user can edit and save custom prompts to prompt list
- [x] Prompt Template: create a new chat with pre-defined in-context prompts [#993](https://github.com/Yidadaa/ChatGPT-Next-Web/issues/993)
- [x] Share as image, share to ShareGPT [#1741](https://github.com/Yidadaa/ChatGPT-Next-Web/pull/1741)
- [x] Desktop App with tauri
- [x] Self-host Model: Fully compatible with [RWKV-Runner](https://github.com/josStorer/RWKV-Runner), as well as server deployment of [LocalAI](https://github.com/go-skynet/LocalAI): llama/gpt4all/rwkv/vicuna/koala/gpt4all-j/cerebras/falcon/dolly etc.
- [ ] Plugins: support network search, calculator, any other apis etc. [#165](https://github.com/Yidadaa/ChatGPT-Next-Web/issues/165)

## What's New

- 🚀 v2.0 is released, now you can create prompt templates, turn your ideas into reality! Read this: [ChatGPT Prompt Engineering Tips: Zero, One and Few Shot Prompting](https://www.allabtai.com/prompt-engineering-tips-zero-one-and-few-shot-prompting/).
- 🚀 v2.7 let's share conversations as image, or share to ShareGPT!
- 🚀 v2.8 now we have a client that runs across all platforms!

## 主要功能

- 在 1 分钟内使用 Vercel **免费一键部署**
- 提供体积极小（~5MB）的跨平台客户端（Linux/Windows/MacOS）, [下载地址](https://github.com/Yidadaa/ChatGPT-Next-Web/releases)
- 完整的 Markdown 支持：LaTex 公式、Mermaid 流程图、代码高亮等等
- 精心设计的 UI，响应式设计，支持深色模式，支持 PWA
- 极快的首屏加载速度（~100kb），支持流式响应
- 隐私安全，所有数据保存在用户浏览器本地
- 预制角色功能（面具），方便地创建、分享和调试你的个性化对话
- 海量的内置 prompt 列表，来自[中文](https://github.com/PlexPt/awesome-chatgpt-prompts-zh)和[英文](https://github.com/f/awesome-chatgpt-prompts)
- 自动压缩上下文聊天记录，在节省 Token 的同时支持超长对话
- 多国语言支持：English, 简体中文, 繁体中文, 日本語, Español, Italiano, Türkçe, Deutsch, Tiếng Việt, Русский, Čeština
- 拥有自己的域名？好上加好，绑定后即可在任何地方**无障碍**快速访问

## 开发计划

- [x] 为每个对话设置系统 Prompt [#138](https://github.com/Yidadaa/ChatGPT-Next-Web/issues/138)
- [x] 允许用户自行编辑内置 Prompt 列表
- [x] 预制角色：使用预制角色快速定制新对话 [#993](https://github.com/Yidadaa/ChatGPT-Next-Web/issues/993)
- [x] 分享为图片，分享到 ShareGPT 链接 [#1741](https://github.com/Yidadaa/ChatGPT-Next-Web/pull/1741)
- [x] 使用 tauri 打包桌面应用
- [x] 支持自部署的大语言模型：开箱即用 [RWKV-Runner](https://github.com/josStorer/RWKV-Runner) ，服务端部署 [LocalAI 项目](https://github.com/go-skynet/LocalAI) llama / gpt4all / rwkv / vicuna / koala / gpt4all-j / cerebras / falcon / dolly 等等
- [ ] 插件机制，支持联网搜索、计算器、调用其他平台 api [#165](https://github.com/Yidadaa/ChatGPT-Next-Web/issues/165)

## 最新动态

- 🚀 v2.0 已经发布，现在你可以使用面具功能快速创建预制对话了！ 了解更多： [ChatGPT 提示词高阶技能：零次、一次和少样本提示](https://github.com/Yidadaa/ChatGPT-Next-Web/issues/138)。
- 💡 想要更方便地随时随地使用本项目？可以试下这款桌面插件：https://github.com/mushan0x0/AI0x0.com
- 🚀 v2.7 现在可以将会话分享为图片了，也可以分享到 ShareGPT 的在线链接。
- 🚀 v2.8 发布了横跨 Linux/Windows/MacOS 的体积极小的客户端。

## Get Started

> [简体中文 > 如何开始使用](./README_CN.md#开始使用)

1. Get [OpenAI API Key](https://platform.openai.com/account/api-keys);
2. Click
   [![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2FYidadaa%2FChatGPT-Next-Web&env=OPENAI_API_KEY&env=CODE&project-name=chatgpt-next-web&repository-name=ChatGPT-Next-Web), remember that `CODE` is your page password;
3. Enjoy :)

## FAQ

[简体中文 > 常见问题](./docs/faq-cn.md)

[English > FAQ](./docs/faq-en.md)

## Keep Updated

> [简体中文 > 如何保持代码更新](./README_CN.md#保持更新)

If you have deployed your own project with just one click following the steps above, you may encounter the issue of "Updates Available" constantly showing up. This is because Vercel will create a new project for you by default instead of forking this project, resulting in the inability to detect updates correctly.

We recommend that you follow the steps below to re-deploy:

- Delete the original repository;
- Use the fork button in the upper right corner of the page to fork this project;
- Choose and deploy in Vercel again, [please see the detailed tutorial](./docs/vercel-cn.md).

### Enable Automatic Updates

> If you encounter a failure of Upstream Sync execution, please manually sync fork once.

After forking the project, due to the limitations imposed by GitHub, you need to manually enable Workflows and Upstream Sync Action on the Actions page of the forked project. Once enabled, automatic updates will be scheduled every hour:

![Automatic Updates](./docs/images/enable-actions.jpg)

![Enable Automatic Updates](./docs/images/enable-actions-sync.jpg)

### Manually Updating Code

If you want to update instantly, you can check out the [GitHub documentation](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork) to learn how to synchronize a forked project with upstream code.

You can star or watch this project or follow author to get release notifictions in time.

## Access Password

> [简体中文 > 如何增加访问密码](./README_CN.md#配置页面访问密码)

This project provides limited access control. Please add an environment variable named `CODE` on the vercel environment variables page. The value should be passwords separated by comma like this:

```
code1,code2,code3
```

After adding or modifying this environment variable, please redeploy the project for the changes to take effect.

## Environment Variables

> [简体中文 > 如何配置 api key、访问密码、接口代理](./README_CN.md#环境变量)

### `OPENAI_API_KEY` (required)

Your openai api key.

### `CODE` (optional)

Access passsword, separated by comma.

### `BASE_URL` (optional)

> Default: `https://api.openai.com`

> Examples: `http://your-openai-proxy.com`

Override openai api request base url.

### `OPENAI_ORG_ID` (optional)

Specify OpenAI organization ID.

### `HIDE_USER_API_KEY` (optional)

> Default: Empty

If you do not want users to input their own API key, set this value to 1.

### `DISABLE_GPT4` (optional)

> Default: Empty

If you do not want users to use GPT-4, set this value to 1.

## Development

> [简体中文 > 如何进行二次开发](./README_CN.md#开发)

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/Yidadaa/ChatGPT-Next-Web)

Before starting development, you must create a new `.env.local` file at project root, and place your api key into it:

```
OPENAI_API_KEY=<your api key here>

# if you are not able to access openai service, use this BASE_URL
BASE_URL=https://chatgpt1.nextweb.fun/api/proxy
```

### Local Development

```shell
# 1. install nodejs and yarn first
# 2. config local env vars in `.env.local`
# 3. run
yarn install
yarn dev
```

## Deployment

> [简体中文 > 如何部署到私人服务器](./README_CN.md#部署)

### Docker (Recommended)

```shell
docker pull yidadaa/chatgpt-next-web

docker run -d -p 3000:3000 \
   -e OPENAI_API_KEY="sk-xxxx" \
   -e CODE="your-password" \
   yidadaa/chatgpt-next-web
```

You can start service behind a proxy:

```shell
docker run -d -p 3000:3000 \
   -e OPENAI_API_KEY="sk-xxxx" \
   -e CODE="your-password" \
   -e PROXY_URL="http://localhost:7890" \
   yidadaa/chatgpt-next-web
```

### Shell

```shell
bash <(curl -s https://raw.githubusercontent.com/Yidadaa/ChatGPT-Next-Web/main/scripts/setup.sh)
```

## Screenshots

![Settings](./docs/images/settings.png)

![More](./docs/images/more.png)

## Donation

[Buy Me a Coffee](https://www.buymeacoffee.com/yidadaa)

## Special Thanks

### Sponsor

> 仅列出捐赠金额 >= 100RMB 的用户。

[@mushan0x0](https://github.com/mushan0x0)
[@ClarenceDan](https://github.com/ClarenceDan)
[@zhangjia](https://github.com/zhangjia)
[@hoochanlon](https://github.com/hoochanlon)
[@relativequantum](https://github.com/relativequantum)
[@desenmeng](https://github.com/desenmeng)
[@webees](https://github.com/webees)
[@chazzhou](https://github.com/chazzhou)
[@hauy](https://github.com/hauy)
[@Corwin006](https://github.com/Corwin006)
[@yankunsong](https://github.com/yankunsong)
[@ypwhs](https://github.com/ypwhs)
[@fxxxchao](https://github.com/fxxxchao)
[@hotic](https://github.com/hotic)
[@WingCH](https://github.com/WingCH)
[@jtung4](https://github.com/jtung4)
[@micozhu](https://github.com/micozhu)
[@jhansion](https://github.com/jhansion)
[@Sha1rholder](https://github.com/Sha1rholder)
[@AnsonHyq](https://github.com/AnsonHyq)
[@synwith](https://github.com/synwith)

### Contributor

[Contributors](https://github.com/Yidadaa/ChatGPT-Next-Web/graphs/contributors)

## LICENSE

[Anti 996 License](https://github.com/kattgu7/Anti-996-License/blob/master/LICENSE_CN_EN)
