# Git: Setup for Git for Windows

This repo is created to curate the key points to initialise a Git setup on Windows (Git for Windows).

Contents:

1. [GPG signing using Gpg4win](#gpg-signing-using-gpg4win)
1. [SSH authentication using Git for Windows](#ssh-authentication-using-git-for-windows)


## GPG signing using Gpg4win

**UPDATE**: This information about setting up GPG signing on Git for Windows may be outdated. Feedback based on recent testing and setup is welcome.

This setup was deemed necessary based on [the discussion on Stack Overflow](https://stackoverflow.com/questions/46992596/configure-gpg-for-git-on-windows). If your Git client is unable to locate a suitable GPG key to sign your commits, even though the key is in your GPG keyring, this setup should resolve the issue.

We assume the following:

1. The user uses Git for Windows for `git` operations.
1. The user uses `gpg` key to sign commits.
1. The user uses [Gpg4win](https://www.gpg4win.org/) for `gpg` operations.


### Git config: Unset `gpg.format`

Possible cause: The `global` Git configuration for `gpg.format` might have been set to something else.

To fix it, run:

```bash
git config --global --unset gpg.format
```

[Source: GitHub docs](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key#telling-git-about-your-gpg-key)


### Git config: Set `gpg.program`

Source: https://jamesmckay.net/2016/02/signing-git-commits-with-gpg-on-windows/

Possible cause: Git for Windows is using the `gpg.exe` client bundled with it, instead of Gpg4win's `gpg.exe`.

To fix it, we set the Git client to use the `gpg.exe` client included with Gpg4win at `global` scope:

```bash
git config --global gpg.program "C:\Program Files (x86)\GnuPG\bin\gpg.exe"
```

or

```bash
git config --global gpg.program "C:\Program Files\GnuPG\bin\gpg.exe"
```

### Default signing behaviour

To configure Git so that its default behaviour (`global` config) is to sign commits and tags:

```bash
git config --global user.name <name>
git config --global user.email <email>
git config --global user.signingkey <key ID>

git config --global commit.gpgsign true
git config --global tag.gpgSign true
```

The config above can also be applied at repo level:

```bash
git config user.name <name>
git config user.email <email>
git config user.signingkey <key ID>

git config commit.gpgsign true
git config tag.gpgSign true
```


## SSH authentication using Git for Windows

This section is only relevant if you are storing your SSH key passphrase using Window's OpenSSH Authentication Agent. To ensure Git for Windows uses the agent where you've stored your credentials and not the one bundled with Git for Windows, force Git to use the system's SSH binary:

```bash
git config --global core.sshCommand "C:/Windows/System32/OpenSSH/ssh.exe"
```

[Source: GitHub Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=windows#troubleshooting-ssh-agent-conflicts-in-windows)

<!--
This is a mirrored post:
1. [Git-WindowsSetup repo](https://github.com/loneguardian/Git-WindowsSetup#ssh-authentication-using-git-for-windows)
1. [SSH-GithubSetup repo](https://github.com/loneguardian/SSH-GithubSetup#ssh-authentication-using-git-for-windows)
-->