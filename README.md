# AI Workspace — Unnotarized macOS Beta

AI Workspace runs AI models privately on your Mac and helps you chat, work on projects and create images, video and audio. It comes
with its tools and no AI models. You choose models inside the app and it sets them up for you.

> **This is a community beta.** It is **not** signed with an Apple Developer ID and **not** notarized by Apple. macOS may ask you to
> approve it, and you should read [what that means](#if-macos-blocks-the-app) first. Source code is not published here. This repository
> holds only the installer, release files and notices.

## Install

You need an Apple Silicon Mac (M1 or later) with macOS 14.0 or later, and about 1 GB of free disk for the app, plus
room for any models you choose. You do not need Homebrew, Git, Python, Node or Xcode.

Open Terminal, paste this, and press Return:

```sh
f=$(mktemp) && curl -fsSL https://github.com/JCOBYTE/ai-workspace-beta/releases/latest/download/install.sh -o "$f" && sh "$f"
```

If someone you trust sent you a longer command with a hash in it, use theirs instead. It refuses a script that isn't the one they checked,
which protects you even if this page has been changed. This version's is:

```sh
f=$(mktemp) && curl -fsSL https://github.com/JCOBYTE/ai-workspace-beta/releases/download/v2.0.0-beta.1/install.sh -o "$f" && if [ "$(shasum -a 256 "$f" | cut -d" " -f1)" = "166d5b8ba12894830d13a95e66143c556a652da223afeab37a4e115f8836a93a" ]; then sh "$f"; else echo "This is not the installer you were given. Nothing was run." >&2; fi
```

Want to read the script first? Download it, read it, then run that same file with `sh`, because the command above fetches it again. It
is short. It checks your Mac, downloads one file over https, checks that file's SHA-256, checks it is exactly the app built for this
release, and puts the app in `/Applications` without using `sudo`. It never changes Gatekeeper, System Integrity Protection or a file's quarantine flag, and it never touches your
chats, models or projects.

Prefer to download by hand? Use the `.dmg` on the [release page](https://github.com/JCOBYTE/ai-workspace-beta/releases/latest). It has a read-me inside.

## If macOS blocks the app

A build that Apple has not notarized is held by Gatekeeper the first time it opens if it came through a browser. macOS is working as
designed, and you are the one who decides. It will say **"AI Workspace" Not Opened** and that Apple could not verify it is free of
malware. That is true: Apple has not checked this beta.

1. Click **Done**. Move to Trash is the highlighted button, so take care.
2. Open **System Settings > Privacy & Security** and scroll down to **Security**.
3. You should see "AI Workspace was blocked to protect your Mac". Check that you got this from `https://github.com/JCOBYTE/ai-workspace-beta`, and only
   then click **Open Anyway**.
4. macOS asks once more. Click **Open Anyway** again, not Move to Trash.
5. Enter an administrator's name and password. AI Workspace opens, and opens normally after that.

The install command usually skips all of this, because a file fetched from Terminal does not get the flag that triggers Gatekeeper's
check. That does not mean Apple checked it. Nothing has.

Tested on macOS 26.6 (Tahoe), in a clean virtual machine. What you see depends on your macOS version, and some versions may not offer Open Anyway.

**Never** turn off Gatekeeper or System Integrity Protection for this app, and do not follow any guide that tells you to.

**If macOS says the app is damaged, or that it will damage your computer, stop.** That is not the same as "could not verify". Do not
override it. Delete the app and open an issue here.

## Check what you downloaded

Each release lists a SHA-256 for every file in `SHA256SUMS`, and the installer checks the app's file for you.

A hash from this page proves your download is intact. It cannot prove who published it, because the hash and the file come from the
same place. Two things go further:

- This repository has GitHub's immutable releases turned on: a published release's files and tag can't be changed, and GitHub signs a
  record of them. With GitHub's `gh` tool you can check one: `gh release verify-asset <tag> <file> -R JCOBYTE/ai-workspace-beta`. That protects a release at
  its own tag. The short command follows `latest`, so a newer release changes what it installs.
- The longer command, sent to you by someone you trust, does not depend on this page at all.

## Upgrade, go back, remove

| To | Do this |
|---|---|
| Upgrade | Quit AI Workspace and run the install command again |
| Go back to the version before | Save the script, then `sh install.sh --rollback` |
| Remove the app | `sh install.sh --uninstall` moves it to the Trash |
| See what is installed | `sh install.sh --status` |

Your chats, settings and models stay in `~/Library/Application Support/AI Workspace`, and your projects in `~/AI Workspace`. Removing or
replacing the app leaves them alone, so installing again brings everything back. A version you go back to may refuse data that a newer
version already updated, and it will say so.

## Limits of this beta

- Not signed with an Apple Developer ID and not notarized by Apple.
- Apple Silicon only.
- Creating images and video needs macOS 26 or later for now.
- AI models are not included. You choose and download them in the app.
- Updates are by running the install command again. The app does not update itself in this beta.
- Hermes, an optional agent, needs a model offering at least 64,000 tokens. Small models such as Qwen3 1.7B stop at 40,960, so they can't run it. The one model tested needs about 13.4 GiB, on 24 GB of memory.

## Terms and notices

[Beta terms](BETA_TERMS.md). [Third-party notices](THIRD_PARTY_NOTICES.txt).
AI models are not part of AI Workspace. Each model you download has its own license, and you will see it before you download.
