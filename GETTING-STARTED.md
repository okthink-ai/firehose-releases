# Getting started with Firehose

Firehose runs on your own Mac or Linux machine and gives you a dashboard for
your coding agents (Claude Code, Codex, and others). This guide takes you from
nothing to using Firehose on your computer and your phone.

**You will need**

- A Mac (Apple silicon or Intel) or a Linux machine (x64 or arm64, a normal
  distribution such as Ubuntu or Debian; Alpine is not supported).
- A Firehose subscription, which you can start during activation.
- Optional, to use Firehose from your phone or another computer: a free
  [Tailscale](https://tailscale.com) account.

Pick your machine: [Mac](#install-on-a-mac) · [Linux server](#install-on-a-linux-server) · then [use it from your phone](#use-firehose-from-your-phone).

---

## Install on a Mac

### 1. (Optional) Install Tailscale

Only needed if you want to open Firehose from your phone or another computer.
Download Tailscale from <https://tailscale.com/download/mac>, open it, and sign
in. You'll see its icon in the menu bar.

### 2. Install Firehose

Open **Terminal** (press ⌘-Space, type *Terminal*, press Return) and paste:

```sh
curl -fsSL https://github.com/okthink-ai/firehose-releases/releases/latest/download/install.sh | sh
```

The installer asks:

1. **Do you accept the end-user license agreement?** Type `y` and press Return.
2. **Also open Firehose from your other devices on your tailnet?** (only if
   Tailscale is running) Type `y` to use Firehose from your phone, or press
   Return to skip. You can turn it on later in Settings.

When it finishes, your browser opens on the **Activate Firehose** screen, and
Firehose starts by itself every time you log in.

### 3. Activate

See [Activate Firehose](#activate-firehose) below.

---

## Install on a Linux server

These steps suit a VPS or any machine you reach over SSH. Run them on the
server.

### 1. Connect and install the basics

```sh
ssh you@your-server
```

On Ubuntu or Debian:

```sh
sudo apt update && sudo apt install -y curl git tmux
```

Firehose launches your agents, so install at least one, for example Claude Code:

```sh
curl -fsSL https://claude.ai/install.sh | bash
```

### 2. Keep Firehose running after you log out

```sh
sudo loginctl enable-linger $USER
```

This lets Firehose start when the server boots and keep running when you close
your SSH session.

### 3. (Optional) Put the server on Tailscale

Only needed to open Firehose from your phone or laptop without an SSH tunnel.

```sh
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
sudo tailscale set --operator=$USER
```

`tailscale up` prints a link: open it and sign in with your Tailscale account.
The last line lets Firehose publish itself on your tailnet without `sudo`.

### 4. Install Firehose

```sh
curl -fsSL https://github.com/okthink-ai/firehose-releases/releases/latest/download/install.sh | sh
```

1. **Do you accept the end-user license agreement?** Type `y` and press Enter.
2. **Also open Firehose from your other devices on your tailnet?** Type `y`.

A server has no browser, so the installer prints the addresses to open
instead. With Tailscale it looks like
`https://your-server.your-tailnet.ts.net:4801`; write it down.

Log out and back in (or open a new terminal) so the `firehose` command is on
your PATH.

**No Tailscale?** Open Firehose from your laptop through an SSH tunnel:

```sh
ssh -L 4801:localhost:4801 you@your-server
```

then browse to <http://localhost:4801>.

### 5. Activate

Open the address from step 4 and follow [Activate Firehose](#activate-firehose).

---

## Activate Firehose

The first time you open Firehose it shows **Activate Firehose**.

1. Click **Activate**, then open the link it shows.
2. Enter your email and click **Email me a sign-in link**. Open the email (check
   your spam folder) **on the same device** and click the link.
3. Click **Approve this code**.
   - If you don't have a subscription yet, choose a plan and pay with Stripe;
     you come straight back and the code is approved.
4. Go back to Firehose. It unlocks within a few seconds.

Signed in with the wrong email? Click **Use a different email** under
"Verified as".

Each activation code works once and expires after an hour. If you see "code is
unknown or already used", click **Activate** in Firehose again for a new one.

---

## Use Firehose from your phone

1. On the machine running Firehose, turn on **Settings → Open from your other
   devices** (or run `firehose tailnet on`), if you didn't during install. It
   shows the address, for example `https://your-mac.your-tailnet.ts.net:4801`.
2. Install **Tailscale** on your phone from the App Store or Play Store and sign
   in with the **same** Tailscale account. Make sure it says *Connected*.
3. Open that address in your phone's browser.

Only you can get in: anyone else on your tailnet is turned away, because
Firehose gives access to a terminal on your machine. The first time, Tailscale
may ask you to turn on **HTTPS** and **Serve** for your tailnet; follow the link
it prints and click *Enable*. You only do this once.

---

## Manage your subscription

In Firehose, open **Settings → License → Manage subscription**, or go to
<https://agents.okthink.ai/account>. Sign in with the same email to change your
card, see invoices, switch plans, or cancel.

To move Firehose to another computer, click **Deactivate** in Settings → License
on the old one, then activate on the new one.

---

## Everyday commands

| Command | What it does |
|---|---|
| `firehose status` | Quick status in the terminal |
| `firehose update` | Install the latest version |
| `firehose tailnet on` / `off` / `status` | Open Firehose from your other devices, or stop |
| `firehose service uninstall` | Stop starting Firehose at login |
| `firehose uninstall` | Remove Firehose (add `--keep-data` to keep your data) |

---

## Troubleshooting

**`firehose: command not found`** — Open a new terminal window, or use the full
path: `~/.local/bin/firehose`.

**The installer says Firehose did not answer** — Give it a minute and open
<http://localhost:4801>. On Linux, make sure you ran
`sudo loginctl enable-linger $USER`, then run `firehose service install`.

**Port 4801 is already in use** — Install on another port:
`curl -fsSL https://github.com/okthink-ai/firehose-releases/releases/latest/download/install.sh | sh -s -- --port 4830`

**My phone can't open the tailnet address** — Check the Tailscale app says
*Connected* with the same account, then run `firehose tailnet status` on the
computer.

**"This Firehose only answers its owner"** — You're signed in to Tailscale with
a different account than the one that owns the computer.

**The sign-in email doesn't arrive** — Check spam, and mark it *Not spam* so
future ones arrive normally.

**Start over** — `firehose uninstall`, then run the install command again.
