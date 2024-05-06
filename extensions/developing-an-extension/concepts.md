---
description: >-
  A high-level view of an extension project and how they fit in the context of a
  chat.
---

# 💭 Concepts

## Project Structure

An extension is, at its core, a React website, just a very small one.&#x20;

<pre><code>public/
    chub_meta.yaml  # Information like the extension's name.
src/
<strong>    ChubExtension.tsx  # The core extension implementation.
</strong>                       # This is the main file to edit.
    TestRunner.tsx     # A runner for testing.
    assets/
        test-init.json    # Testing data for the Test Runner.
    index.scss         # CSS styling
    main.tsx           # The app's entry point.
    App.tsx
package.json        # Dependency management
.github/            # A utility file that creates the extension
    workflows/      # project in Chub, and builds and deploys it
        deploy.yml  # on push.
.eslintrc.cjs       # Linter configuration file
index.html          # The root HTML
tsconfig.json       # TypeScript configuration file
tsconfig.node.json  # TypeScript configuration file
vite.config.ts      # Vite configuration file
yarn.lock           # Freezes dependencies
</code></pre>

Communication between the chat UI itself and the extension site is handled by the extensions library -- to develop you only need to be concerned with implementing the Extension interface.&#x20;

## Where an Extension Exists in a Chat

If an extension is given a position of 'NONE' in the chub-meta.yaml, it doesn't display at all, instead running in the background. Otherwise, on a desktop or wide screen, it displays to the right of a chat, with the majority of the height of the window minus some padding.

<figure><img src="../../.gitbook/assets/image (1).png" alt="" width="375"><figcaption><p>The extension's place highlighted with the browser's inspector.</p></figcaption></figure>

On mobile devices or narrow windows, the extension is given a smaller space between the chat header and messages, with the messages fading out in the background.

<figure><img src="../../.gitbook/assets/image (2).png" alt="" width="124"><figcaption><p>Narrow-screen/mobile view.</p></figcaption></figure>

If there are multiple visible extensions, they will share the space, with equal-height rows on wide screens and equal-width columns on narrow screens.

If your extension outputs system messages, they will display at the end of whatever message they were given in response to. System messages are visible to the human only, and not sent to the LLM.

<figure><img src="../../.gitbook/assets/Screenshot 2024-04-29 at 14-44-27 A private chat with The Maze.png" alt=""><figcaption><p>An extension that outputs system messages. The 'Available Directions' and lists that appear at the end of The Maze's messages are in fact from an extension attached to it. System messages are stored separately from language model responses and user messages, so that the language model doesn't get confused and try to generate stats blocks itself. Here, the extension can correctly show the available directions from a maze it has generated, the sort of geometric logic that a language model might have trouble with.</p></figcaption></figure>

## Extension Lifecycle and Communication

### Top-Down Communication Points

There are four places where an extension is called: initialization, before a message is sent to an LLM, after a response is received from an LLM, and when the user swipes or jumps from one place in the chat tree to another. The interface template file ([https://github.com/CharHubAI/extension-template/blob/main/src/ChubExtension.tsx](https://github.com/CharHubAI/extension-template/blob/main/src/ChubExtension.tsx)) is heavily commented with explanations of each case.

#### Initialization

This corresponds to the 'constructor' and 'load' functions in the extension interface. When a chat is started, an initialization function is called in your extension with information about the chat and its participants. The extension has the opportunity to return some initialization-specific state information -- for a more detailed explanation of state types, see [state.md](state.md "mention").

#### Before a Prompt

This corresponds to the 'beforePrompt' function in the extension interface. When a user initiates a call to an LLM, the extension is first sent information on the prompt being sent, and has the chance to modify the user's message, save the extension's updated internal state, append something to the prompt, and attach a system message to the user's message.

#### After a Response

This corresponds to the 'afterResponse' function in the extension interface. When a response is fully received from the LLM, the extension has an opportunity to modify the response message, save the extension's updated internal state, and attach a system message to the bot's message.

#### On a Swipe or Jump

This corresponds to the 'setState' function in the extension interface. On a swipe or jump to a message that has already been seen previously, the extension is sent what its message-level state for that message was.  For example, in an extension that shows a character's expression pack, the message-level state has each character's current emotion. Note how this is only one of the three types of state -- see [state.md](state.md "mention") for more information.

#### Rendering

The 'render' function may be called at any time, and returns a ReactElement component. It's what your extension will look like. Try to avoid doing significant work inside this function.

### Bottom-Up Communication Points (Experimental/Unstable)

Additionally, there are functions an extension may call at any time. They are contained in the 'generator' member of your extension class, i.e. it can be called with `this.generator.someFunction()`. Its interface is here: [https://github.com/CharHubAI/chub-extensions-ts/blob/main/src/types/generation/service.ts](https://github.com/CharHubAI/chub-extensions-ts/blob/main/src/types/generation/service.ts). Note: of these, only {'makeImage', 'imageToImage', 'removeBackground', 'inpaintImage'} are implemented, and none of them are stable. For this reason, `await`ing on any of them within any of the top-down communication points is not wise at this time. Best results are always with the 1:1 aspect ratio. Since these are ad-hoc generations meant to come back in a few seconds, there is a quality tradeoff. A proper imagegen and \*gen UI with tradeoffs balanced towards higher quality is under development.

