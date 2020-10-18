---
description: Learn how to use the ShellBin CLI quickly
---

# The ShellBin CLI 🐚

You can find the source code of ShellBin CLI here: [https://github.com/nextblu/shellbin](https://github.com/nextblu/shellbin)

This simple CLI program has been created in `Vlang`a simple yet powerfull programming language that we are experimenting. 

## How to install

You can simply download the client from this link: [https://github.com/nextblu/shellbin/releases/tag/v1.0.5](https://github.com/nextblu/shellbin/releases/tag/v1.0.5)

After placing the executable in a folder of your choice you can symlink the executable for faster usage with the following code:

#### Unix\*

```text
mklink shellbin.exe "C:\<YOUR_DOWNLOAD_FOLDER>\shellbin.exe"
```

**Windows**

```text
chmod +x ./shellbin
sudo ./shellbin symlink
```

## How do I use it?

{% hint style="info" %}
ShellBin is able to capture every stdout information, but be aware that sometime special char are not supported and can lead to a broken bin.
{% endhint %}

Simple! Just run something like that:

```text
$ ls -la | shellbin
```

That's it! ShellBin will capture your stdout and upload the result to the online bin service. 



