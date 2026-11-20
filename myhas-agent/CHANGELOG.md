# Changelog

## 0.4.0

- **Fix it for me:** while Home Assistant doesn't trust the agent yet, the
  status page has a button that sets it up for you. Home Assistant restarts
  once; if it asks you to confirm the new HTTP server settings, select
  **Confirm**.
- The app now asks for access to the Home Assistant API for this, so Home
  Assistant shows a lower security rating for it. Only the agent uses this
  access, inside the app: myhas.io can't use it.

## 0.3.0

- When you change the address of your Home Assistant in the portal, the
  agent connects again at once and shows the new address.

## 0.2.0

- The app is now called **myhas.io** in Home Assistant and in the sidebar.
- A friendlier status page: one clear button for each step (claim, open
  remotely, open the network settings), and the version at the top.
- The page says **Almost there** until Home Assistant trusts the agent, and
  the warning goes away by itself within a minute after you fix it.

## 0.1.1

- The images of the agent are now `ghcr.io/smart-home-shop/myhas-agent-*`.
  The agent itself does not change.

## 0.1.0

- The first version of the agent: one outbound connection to myhas.io,
  pairing with a claim code, remote access to the Home Assistant web
  interface (HTTP and WebSocket), and a status page in the sidebar.
