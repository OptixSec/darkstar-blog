+++ 
draft = false
date = 2024-05-31T14:30:02+02:00
title = "No Cloud Needed: How to Install and Run Large Language Models with Ollama"
description = "A guide about how to install Ollama and run Large Language Models locally."
slug = ""
authors = ["Darkstar"]
tags = ["Ollama"]
categories = ["Guides", "AI"]
externalLink = ""
series = []
+++

Are you ready to unlock the power of running LLMs locally on your Linux machine? In this post, I'll walk you through the installation process, from a one-liner command that takes care of everything to a manual install.

## Installing Ollama with One-Liner Command

For those who prefer simplicity, installing Ollama is as easy as running this command: `curl -fsSL https://ollama.com/install.sh | sh`. This one-liner will install Ollama and its dependencies, saving you the trouble of manually downloading and configuring everything.

## Manual Install Options

### Downloading the binary

If you want more control over the installation process, you can opt for a manual install. This involves downloading the Ollama binary and adding it to your system's PATH. Follow these steps:

- Download the Ollama binary to a directory inside your PATH:

```bash
sudo curl -L https://ollama.com/download/ollama-linux-amd64 -o /usr/bin/ollama
```

- Change permissions to allow the execution of the binary.

```bash
sudo chmod +x /usr/bin/ollama
```

### Starting Ollama as a systemd Service

To simplify management and logging, you can start Ollama as a systemd service. Follow these steps:

- Create a new file in the `/etc/systemd/system` directory called `ollama.service`:

```bash
sudo nano /etc/systemd/system/ollama.service
```

- Add the following content to the file: 

```systemd
 [Unit] 
 Description=Ollama Service 
 
 [Service] 
 Type=simple 
 ExecStart=/usr/bin/ollama 
 StartLimitBurst=5 
 
 [Install] 
 WantedBy=multi-user.target`
```

- Reload the systemd daemon: `sudo systemctl daemon-reload`
- Enable and start the Ollama service: `sudo systemctl enable ollama` and `sudo systemctl start ollama`

## Using Ollama

To download a model, use the following command:

`ollama pull <model>`

> **Note:** You can check the available [models](https://ollama.com/library) on the Ollama website.

To run a downloaded model, use this command:

`ollama run <model>`

To view the list of downloaded models, enter:

`ollama list`

To see which models are currently running, use:

`ollama ps`

Finally, to remove a downloaded model, execute:

`ollama rm <model>`

## Updating Ollama

To update Ollama, you can run the installation script again: `curl -fsSL https://ollama.com/install.sh | sh`. Alternatively, you can download the latest Ollama binary directly and replace the existing one.

## Viewing Logs

To view logs of Ollama running as a startup service, use the `journalctl` command: `sudo journalctl -u ollama`

## Uninstalling Ollama

To uninstall Ollama, you'll need to remove the service, binary, and user/group. Follow these steps:

- Remove the Ollama service:

```bash
sudo systemctl stop ollama
sudo systemctl disable ollama
sudo rm /etc/systemd/system/ollama.service
```

- Remove the Ollama binary:

```bash
sudo rm $(which ollama)
```

- Remove the Ollama user and group:

```bash
sudo rm -r /usr/share/ollama
sudo userdel ollama
sudo groupdel ollama
```

That's it! With these steps, you should be able to get started with Ollama on your Linux machine. Happy hacking!
