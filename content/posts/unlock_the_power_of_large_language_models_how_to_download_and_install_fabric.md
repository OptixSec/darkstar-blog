+++ 
draft = false
date = 2024-06-02T15:43:58+02:00
title = "Unlock the Power of Large Language Models: How to Download and Install Fabric"
description = "A small guide about using the Fabric framework to augment your interaction with large language models."
slug = ""
authors = ["Darkstar"]
tags = ["Ollama", "Fabric"]
categories = ["Guides", "AI"]
externalLink = ""
series = []
+++

I've recently come across a really cool tool for interacting with AI models called [Fabric](https://github.com/danielmiessler/fabric). Fabric is an open-source framework created by [Daniel Miessler](https://github.com/danielmiessler/) that can perform specific tasks using a large set of prompts. Today, I'm going to show you how I installed it on my system and use it together with [Ollama](https://ollama.com/) to run AI on my local machine and show you some examples of Fabric in action.

## Installing Fabric

Before installing Fabric, ensure you have at least **Ollama**, **Python 3.10** or newer and **pipx** installed on your system.

### Cloning the repo

Decide where on your system you want to store the Fabric project. I decided to put it in `~/repos`. Now navigate to that directory:

```bash
cd ~/repos/
```

Clone the Fabric repository to your current directory:

```bash
git clone https://github.com/danielmiessler/fabric.git
```

When done, navigate to the main Fabric directory:

```bash
cd fabric/
```

### Install Fabric

Continue by installing Fabric on your computer by typing:

```bash
pipx install .
```

Run the setup script:

```bash
fabric --setup
```

Follow the instructions to add any API keys if desired. I suggest adding an API key for YouTube, as we'll be using it in one of the examples later. However, this is optional; you can proceed without adding any API keys and run the AI model locally. Now restart your shell to reload your configurations and test if Fabric is working by typing:

```bash
fabric --help
```

Before continuing, I've created an alias inside my shell's configuration file called `clip`. This alias outputs the content of the clipboard to the terminal, allowing us to pipe that content into Fabric for running various patterns against it. We can also use tools like `cat` or `echo` to pipe the content of a file or string into Fabric. Having the ability to use `clip` to pipe the content of your clipboard is very handy though! If you want to set up the alias yourself, you can add the following line to your shell's configuration file:

```bash
alias clip="xclip -selection clipboard -o"
```

> **Note**: You need to have `xclip` installed on your system for this alias to work.

After reloading your config, we're now ready to start exploring Fabric.

## Using Fabric

Let's set the default model to `llama3:latest` so we don't have to specify which model we want to use everytime we use Fabric:

```bash
fabric --changeDefaultModel llama3:latest
```

> **Note**: You can obviously change this to any other model you want to use instead.

Fabric uses patterns to complete various tasks, to get a list of available patterns, type:

```bash
fabric --list
```

Let's use Fabric to extract wisdom from the transcript of a YouTube video. I'll use [this](https://www.youtube.com/watch?v=JgsGH5IOCFE) YouTube video, which is Daniel's video about creating custom patterns for use with Fabric:

```bash
yt --transcript 'https://www.youtube.com/watch?v=JgsGH5IOCFE' | fabric -sp extract_wisdom
```

![Example of how Fabric is used to extract wisdom from a YouTube video transcript.](/darkstar-blog/images/fabric_guide/extract_wisdom.png)

Pretty awesome, right? The ```-s``` flag tells Fabric to stream the output. In other words, display the output of the command we gave it to the terminal. The ```-p``` flag tells Fabric which pattern we want to use against the input we gave it. Let's take it a step further and let Fabric write an essay based on the wisdom we extracted from the YouTube video by copying the output of the previous command and:

```bash
clip | fabric -sp write_essay
```

![Example of Fabric writing an essay about the wisdom we extracted from a YouTube video transcript](/darkstar-blog/images/fabric_guide/write_essay.png)

You can also pipe multiple patterns together, which is called a stitch in Fabric's terms:

```bash
yt --transcript 'https://youtu.be/XNQhDl4a9Ko' | fabric -p extract_wisdom_agents | fabric -sp write_tweet
```

> **Note**: `write_tweet` is a custom pattern I created myself. It still needs some tweaking.

![Example of stitching various Fabric patterns together](/darkstar-blog/images/fabric_guide/stitching.png)

## Conclusion

These are the basics of setting up Fabric and using it to augment you in your interactions with AI. There are many more patterns you can explore, but I'll leave that to you! I might do another guide in the future on how to create your own patterns and use them with Fabric. And don't forget to check out the Fabric documentation to discover more about how you can utilize Fabric. I hope you learned something, happy hacking!
