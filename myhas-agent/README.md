# myhas.io

Secure remote access to your Home Assistant through
[myhas.io](https://portal.myhas.io), with no open port and no change to your
router.

The agent opens one outbound, encrypted connection to myhas.io. You claim the
agent in the portal with a short code. Then your Home Assistant has its own
address, for example `https://sunny-otter-4821.ha.myhas.io`, and Home Assistant's
own login protects it.

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

See the **Documentation** tab for the step-by-step setup, including the one
network setting that Home Assistant needs.

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
