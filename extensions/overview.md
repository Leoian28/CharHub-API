# 🦅 Overview

A Chub Extension is a software component written by other people that can be used within a chat. They are meant to add functionality like expression packs (showing a character's emotional state with a set of images), UIs for mini-games, special prompt handling, or even interacting with third-party APIs.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2024-04-29 at 14-44-27 A private chat with The Maze.png" alt=""><figcaption><p>An extension for playing a maze.</p></figcaption></figure>

#### Is that secure? In other frontends there are warnings about third-party code.

Generally, yes. Extensions are hosted from a separate domain and run in something called an inline frame ("iframe") that is sandboxed, meaning that it doesn't have any access to the cookies and local storage on Chub itself, i.e. it cannot access any of your keys, codes, or passwords. It's also prevented from certain hostile behaviors like opening browser alerts, and each extension is run from a different sub-domain so that each extension cannot access another extension's cookies or storage, either.&#x20;

Additionally, any extension marked with the 'Verified' symbol has been checked by the site developers for any hostile behavior.

<figure><img src="../.gitbook/assets/image (6).png" alt="" width="258"><figcaption><p>The 'Verified' marker for an extension.</p></figcaption></figure>

Still, no amount of sandboxing and isolation can prevent social engineering. _**Never provide any extension with your API keys or passwords to any site or service, including Chub itself.**_

## _**Never provide any extension with your API keys or passwords to any site or service, including Chub itself.**_
