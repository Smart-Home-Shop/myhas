# myhas for Home Assistant

This is the Home Assistant app repository of [myhas](https://portal.myhas.io):
secure remote access to Home Assistant, with no open port and no change to
your router.

[![Add the repository to my Home Assistant](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2FSmart-Home-Shop%2Fmyhas)

## Apps

### [myhas agent](myhas-agent)

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

Connects this Home Assistant to myhas. See the
[documentation](myhas-agent/DOCS.md) for the setup.

You need Home Assistant OS, because only it can run apps, and a myhas
account (by invitation).

## Add this repository by hand

1. In Home Assistant, go to **Settings → Apps** and select **Install app**.
2. In the **App store**, open the menu (**⋮**, top right) and select
   **Repositories**.
3. Add `https://github.com/Smart-Home-Shop/myhas`.

## For maintainers

This repository holds only the app manifest and its docs. The agent images
(`ghcr.io/smart-home-shop/myhas-agent-{arch}`) come from a separate, private
repository. To release a new version of the app:

1. Publish the agent images with the tag `X.Y.Z` (a tag `agent-vX.Y.Z` in
   the source repository).
2. Here, set `version: "X.Y.Z"` in `myhas-agent/config.yaml` and add a
   `## X.Y.Z` entry at the top of `myhas-agent/CHANGELOG.md`, in one pull
   request. CI checks that both agree and that the images exist.

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
