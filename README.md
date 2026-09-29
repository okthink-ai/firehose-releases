# Firehose releases

Install Firehose on macOS or Linux with one command:

```sh
curl -fsSL https://github.com/okthink-ai/firehose-releases/releases/latest/download/install.sh | sh
```

The installer picks the package for your machine, verifies its checksum, installs it into `~/.firehose`, starts Firehose, and opens the dashboard, where you activate it with your Firehose account. Firehose needs an active subscription; see https://agents.okthink.ai.

New to Firehose? **[Read the getting-started guide](GETTING-STARTED.md)** for step-by-step instructions on a Mac, a Linux server, and your phone.

This repository only hosts release packages. Remove Firehose with `firehose uninstall`.
