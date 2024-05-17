# 🦅 Overview

A Chub Stage is a software component written by other people that can be used within a chat. They are meant to add functionality like expression packs (showing a character's emotional state with a set of images), UIs for mini-games, special prompt handling, or even interacting with third-party APIs.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2024-04-29 at 14-44-27 A private chat with The Maze.png" alt=""><figcaption><p>A stage for playing a maze.</p></figcaption></figure>

#### Is that secure? In other frontends there are warnings about third-party code.

Generally, yes. Stages are hosted from a separate domain and run in something called an inline frame ("iframe") that is sandboxed, meaning that it doesn't have any access to the cookies and local storage on Chub itself, i.e. it cannot access any of your keys, codes, or passwords. It's also prevented from certain hostile behaviors like opening browser alerts, and each stage is run from a different sub-domain so that each stage cannot access another stage's cookies or storage, either.&#x20;

Additionally, any stage marked with the 'Verified' symbol has been checked by the site developers for any hostile behavior.

<figure><img src="../.gitbook/assets/image (6).png" alt="" width="258"><figcaption><p>The 'Verified' marker for a stage.</p></figcaption></figure>

Still, no amount of sandboxing and isolation can prevent social engineering. _**Never provide any stage with your API keys or passwords to any site or service, including Chub itself.**_

