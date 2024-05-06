---
description: >-
  An overview of the numerous APIs that can be used with Chub Venus AI,
  including settings and configuration.
---

# ⚙️ API Connections

Chub Venus AI leverages several AI and natural language processing APIs to power our character interactions. By connecting to these APIs, we are able to provide more intelligent, contextual responses from characters and a smoother overall experience.

Currently, Chub Venus AI allows you to connect with the following APIs:

* OpenAI (PAID) - You can leverage the OpenAI's GPT-3 and GPT-4 models with either an API Key or the use of a reverse proxy. In the case of an API Key you'll have to pay for your usage. Sign up at [platform.openai.com](https://platform.openai.com/) and get your API key at [account/api-keys](https://platform.openai.com/account/api-keys).
* [Mars](https://venus.chub.ai/subscription) (PAID) - This is our own **uncensored** 70B language model finetuned on public datasets as well as our own proprietary data. A Mars subscription is $20 a month with unlimited messaging as well as total integration into the Chub Venus ecosystem. No managing keys and cards or setting up servers, just subscribe and it'll connect seamlessly and automatically.
* Anthropic (FREE\*) - Similar to OpenAI, Anthropic's Claude models can be leveraged with either an API Key or the use of a reverse proxy. Technically free due to being in its open beta stage but access is difficult to acquire. Request API access to Claude at [www.anthropic.com](https://www.anthropic.com/earlyaccess) and get your key at [console.anthropic.com/account/keys](https://console.anthropic.com/account/keys).
* Kobold/Ooba (FREE\*) - APIs that connect to Large Language Models hosted on local machines and shared by individuals on the internet. Free if being hosted from your machine with the only limitations being hardware requirements. Otherwise, you can rent a GPU via Vast AI or other cloud computing services.
* OpenRouter (PAID) - Third-party service that routes text generation through OpenAI, Anthropic, Google, and other models. Sign up at [openrouter.ai](https://openrouter.ai/) and get your API Key at [openrouter.ai/keys](https://openrouter.ai/keys). Usage rates will vary by model.
* PaLM 2 (FREE\*) - Google's current iteration of generative AI models, Bison. Sign up at [developers.generativeai.google](https://developers.generativeai.google/) and get your API Key at [makersuite.google.com/app/apikey](https://makersuite.google.com/app/apikey). Free as of the time of writing.
* Additional APIs to be announced.

### API Settings

<figure><img src="../.gitbook/assets/Screenshot_2023_08_20-4 (1).png" alt=""><figcaption></figcaption></figure>

All APIs support the same three settings, with additional caveats here and there:

* System Prompt - Also referred to as the Jailbreak Prompt or Pre-history instructions, the system prompt is the very first thing sent to the AI (before the chat history, hence the name). It is, generally speaking, the instructions that define the genre and style of the conversation.&#x20;
* Post History Instructions - The post history instructions are the very last thing sent to the AI (after the chat history, hence the name). While not strictly necessary, due to the AI's natural bias towards text that comes near the end this can have major effects on the AI's response. This is often used when jailbreaking or ensuring that a certain formatting is adhere to, such as getting the AI to respond in CYOA-like format.
* Impersonation Prompt - This is the prompt that is used for the Impersonate feature. The Impersonate feature is the blue Robot icon near the left-side of the input box. The Impersonate feature writes your response for you and the Impersonation feature determine what criteria it adheres to.

### API Keys vs Reverse Proxies

We highly recommend using official API keys over third-party reverse proxies for security and ethical reasons. API keys are provided directly from the API owners and give you full control and transparency over usage and costs. Reverse proxies, on the other hand, are created by unauthorized third parties who gain access to your data and IP address. They may enable logging, leak your information, and use stolen API keys which you have no control over.

While reverse proxies are advertised as "free" to use, the proxy owners are shouldering the costs and could shut down access at any time. For privacy, security and reliability, we recommend using official API integrations and managing your own keys.

### Generation Settings

The AI models we connect to support various settings that can be adjusted to modify response generation:

* Temperature: Controls the randomness/creativity of responses. Higher values mean more random, creative responses. Lower values mean more predictable, less creative responses.
* Presence Penalty: Positive values penalize new tokens based on whether they appear in the text so far, increasing the model's likelihood to talk about new topics.
* Frequency Penalty: Positive values penalize new tokens based on their existing frequency in the text so far, decreasing the model's likelihood to repeat the same line verbatim.
* Top K: Only sample from the Top K options for each subsequent token. Used to remove 'long tail' low probability responses.
* Top P: In nucleus sampling, the cumulative distribution is computed over all the options for each subsequent token in decreasing probability order and cut off once it reaches a particular probability specified by Top P. You should either alter Temperature or Top P, but not both.

These are just a few of the settings that can be adjusted to customize response generation for your needs. Default settings are set based on our own testing, but you are free to experiment with different values to achieve your desired results.

