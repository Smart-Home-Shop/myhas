# myhas.io

The myhas.io app gives you secure remote access to this Home Assistant
through myhas.io. You open it from anywhere at its own address, for example
`https://sunny-otter-4821.ha.myhas.io`, and you sign in with your normal Home
Assistant account.

The agent opens one outbound, encrypted connection to myhas.io (HTTPS, port
443). It opens no port on your network and needs no change to your router.
So it also works behind carrier-grade NAT (CGNAT), a 4G/5G router or a
firewall that you don't control.

## Before you start

You need:

- **Home Assistant OS.** Only Home Assistant OS can run apps. Home
  Assistant Container and Home Assistant Core installs can't run the agent.
- **A myhas.io account.** Accounts are by invitation. Sign in at
  <https://portal.myhas.io>.
- **About five minutes.** Home Assistant restarts once, in step 3.

## 1. Add the myhas.io repository

The quickest way: sign in to the portal at <https://portal.myhas.io>. While
you have no instances, it shows **Add to my Home Assistant**. Select it.
Your Home Assistant opens with the repository filled in. Select **Add**.

To do it by hand:

1. In Home Assistant, go to **Settings → Apps** (called **Add-ons** before
   Home Assistant 2026.2).
2. Select **Install app** (bottom right). This opens the **App store**.
3. Open the menu (**⋮**, top right) and select **Repositories**.
4. Paste `https://github.com/Smart-Home-Shop/myhas` and select **Add**.
5. Close the dialog.

## 2. Install the app

1. In the **App store**, find **myhas.io** in the **myhas.io** section. If
   you don't see it, reload the page.
2. Select it, then select **Install**. This takes a minute or two.
3. Don't start it yet.

## 3. Let Home Assistant trust the agent (one time)

**The easy way:** skip this step for now. After you start the agent in
step 4, its status page shows **Fix it for me**. Select it, and the agent
does this step for you. Home Assistant restarts once. If Home Assistant
asks you to confirm the new HTTP server settings, select **Confirm**. This
needs Home Assistant 2026.11 or later.

To do it yourself instead, read on.

myhas.io tells Home Assistant the real address of each visitor. Home Assistant
needs this for its log and for its protection against repeated failed
logins. Home Assistant accepts that information only from a proxy that it
trusts. **Without this step, every remote visit shows `400: Bad Request`.**

The agent runs in the network of the apps, `172.30.32.0/23`. Home Assistant
must trust that network.

### Home Assistant 2026.11 and later

1. Go to **Settings → System → Network**, and open **HTTP server**.
2. In the **Reverse proxy** section, turn on **Trust X-Forwarded-For**.
3. In **Trusted proxies**, add `172.30.32.0/23`. If the list already has
   entries, keep them and add this one.
4. Select **Save**, then **Save and restart**. Home Assistant restarts.

These versions ignore the `http:` section of `configuration.yaml` after
they import it once. A change there has no effect.

### Older versions of Home Assistant

If there is no **HTTP server** page, edit `configuration.yaml` (for
example with the File editor or Studio Code Server app). Add this, or merge
it into the `http:` section that you already have:

```yaml
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 172.30.32.0/23
```

Then restart Home Assistant: go to **Settings → System**, select the power
button (top right), then **Restart Home Assistant**.

## 4. Start the agent

1. Go to **Settings → Apps** and select **myhas.io**.
2. On the **Info** tab, turn on:
   - **Start on boot**, so that the agent starts with Home Assistant,
   - **Watchdog**, so that Home Assistant restarts the agent if it stops,
   - **Show in sidebar**, for quick access to the status page.
3. Select **Start**.
4. Select **Open Web UI**, or **myhas.io** in the sidebar.

## 5. Claim the agent

After a few seconds, the status page says **Waiting to be claimed**. It
shows a claim code such as `ABCD-EFGH` and a **Claim this Home Assistant**
button.

1. Select **Claim this Home Assistant**. Or go to <https://portal.myhas.io/claim> and type the
   code.
2. Sign in to myhas.io if the portal asks you to.
3. Give your Home Assistant a name if you like, for example "Home", and
   select **Claim**.

A code works for 15 minutes. If it expires, the agent shows a new code by
itself. You don't need to restart anything.

Within a few seconds the status page changes to **Connected**, with an
**Open … remotely** button and the remote address of your Home Assistant. If
it says **Almost there** instead, Home Assistant doesn't trust the agent yet:
select **Fix it for me**, or do step 3 by hand.

## 6. Open your Home Assistant remotely

Open the address from the status page or from the portal, for example
`https://sunny-otter-4821.ha.myhas.io`. Sign in with your Home Assistant user
name and password, as at home.

The first visit to a new address can take a few seconds while myhas.io gets
its certificate. Later visits are immediate.

The address is random, but it isn't a secret. Home Assistant's own login
protects it, so use strong passwords and turn on multi-factor
authentication for your Home Assistant users.

## Manage your Home Assistant in the portal

In the portal at <https://portal.myhas.io> you can:

- **Turn remote access off and on.** When it's off, the address shows
  "Remote access is off" and nothing reaches your Home Assistant. The agent
  stays connected, so you can turn it on again at once.
- **Rename it.**
- **Change its address**, for example to `https://smith-home.ha.myhas.io`.
  You can also choose the address when you claim. The old address stops
  working at once, so update your bookmarks and the Home Assistant app on
  your phone. Nobody else can take the old address for 30 days.
- **Delete it** (**Delete instance**). The address stops working at once,
  and the agent shows a new claim code. To use myhas.io again, claim the new
  code.

## The status page

Open it with **myhas.io** in the sidebar, or **Open Web UI** on the page of
the app. It shows one of these states:

| State | Meaning |
| --- | --- |
| **Connecting** | The agent is connecting to myhas.io. |
| **Waiting to be claimed** | The claim code and its link. Claim it in the portal. |
| **Almost there** | The agent is connected, but Home Assistant doesn't trust it yet. Select **Fix it for me**, or do step 3 by hand. |
| **Connected** | An **Open … remotely** button, and the name and the remote address of your Home Assistant. |
| **Not connected** | The last error, and when the agent tries again. It tries again by itself, with a longer wait after each failure (at most 2 minutes). |

The version of the app is at the top. **Details** has the server, the
address of Home Assistant and the fingerprint of the device key.

Only Home Assistant can open the status page (the sidebar or **Open Web
UI**). The claim code is
on it, and anyone with the code could claim your Home Assistant.

## Options

You find these on the **Configuration** tab of the app. You don't need to
change them.

| Option | Default | Meaning |
| --- | --- | --- |
| `server_url` | `https://portal.myhas.io` | The address of the myhas.io portal. Keep the default. |
| `log_level` | `info` | How much the agent writes to its log: `debug`, `info`, `warning` or `error`. Use `debug` only to find a problem. |

## Troubleshooting

**Every remote visit shows `400: Bad Request`.**
Home Assistant doesn't trust the agent. Select **Fix it for me** on the
status page, or do step 3, and check that Home Assistant restarted. The status page shows a warning until this is fixed.

**The address shows "Home Assistant is offline".**
The agent isn't connected. Check that the app is running (**Settings →
Apps → myhas.io**), then look at the status page and the **Log** tab.

**The address shows "Home Assistant isn't answering".**
The agent is connected, but it can't reach Home Assistant. This happens
while Home Assistant restarts. Wait a minute and try again.

**The address shows "Remote access is off".**
Turn remote access on for this Home Assistant in the portal.

**The status page says "Not connected".**
It shows the reason. Check that this Home Assistant can reach the internet.
If `server_url` was changed, set it back to `https://portal.myhas.io`.

**The claim code expired.**
Wait a few seconds. The agent shows a new code by itself.

**I restored a backup, or moved Home Assistant to new hardware.**
A backup of the app keeps the device key, so a restored agent is the same
device and stays claimed. A new install of the app is a new device: delete
the old instance in the portal and claim the new code.

**Something else.**
Set `log_level` to `debug`, restart the app, and look at the **Log** tab.
The log never holds your passwords or the device key.

## Privacy and security

- The connection to myhas.io uses TLS. The agent refuses a plain `http://`
  server address.
- The agent passes on only the requests for the web interface of Home
  Assistant. myhas.io can't use it to reach any other device on your network.
- myhas.io doesn't keep your Home Assistant password: Home Assistant itself
  checks it. Your browser reaches myhas.io over HTTPS.
- The app has access to the Home Assistant API. Only the agent uses it,
  inside the app, for **Fix it for me**. myhas.io can't use it, and the
  agent never passes the token on.
- At its first start, the agent makes a device key. The key is in the data
  of the app (`/data/identity.key`) and never leaves your Home Assistant.
  The agent proves who it is with this key at each connection.
- If you think that somebody copied the device key: delete the instance in
  the portal, uninstall the app, install it again, and claim the new code.
