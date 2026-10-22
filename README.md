# Morgan-Alpha Bot

A starter personal-account WhatsApp bot with QR linking, basic commands, and simple auto-replies.

## Important safety and reliability notes

- `whatsapp-web.js` is an **unofficial** WhatsApp Web integration. It can stop working when WhatsApp changes its website and may lead to account restrictions. Use at your own risk.
- For a business-critical or policy-compliant service, prefer the official WhatsApp Business Platform. The official platform has different capabilities and does not act as a general personal-account WhatsApp Web clone.
- Never share your QR code, `.wwebjs_auth` folder, browser session, or `.env` secrets. Anyone who gets the session can potentially access your linked WhatsApp.
- This starter bot deliberately ignores groups and status broadcasts. It does not claim to read deleted or expired statuses/messages.
- This project is a starting point, not a guarantee of 24/7 availability. The host must keep the service running and preserve its persistent disk.

## Commands

Commands use the prefix `..` by default:
- `..menu`
- `..ping`
- `..owner`

Unknown commands are ignored. Ordinary incoming private messages get a basic automatic reply when `AUTO_REPLY=true`.

## Run locally

Requires Node.js 20 or newer.

1. Extract the project folder.
2. Open a terminal in the folder.
3. Install dependencies:
   ```bash
   npm install
   ```
4. Copy `.env.example` to `.env`.
5. Edit `OWNER_NUMBER` to your number including country code, digits only, e.g. `2547XXXXXXXX`.
6. Start:
   ```bash
   npm start
   ```
7. The first run prints a QR code in the terminal. On your phone, open WhatsApp → **Linked devices** → **Link a device**, then scan it.
8. Keep the process running. The session is saved locally in `.wwebjs_auth`.

## Online deployment using Render

This repository includes `Dockerfile` and `render.yaml` as a deployment starting point.

1. Create a private GitHub repository and upload these project files. Do not upload `.env`, `node_modules`, or `.wwebjs_auth`.
2. In Render, create a new Blueprint from that repository, or create a Docker web service.
3. Ensure a persistent disk is mounted at `/app/.wwebjs_auth`. The provided Blueprint requests a 1 GB disk. Check the host's current plan and disk pricing before deploying.
4. Add environment variables:
   - `BOT_NAME` = `Morgan-Alpha Bot`
   - `COMMAND_PREFIX` = `..`
   - `AUTO_REPLY` = `true`
   - `OWNER_NUMBER` = your number in international digits-only format
5. Deploy and open the **private service logs**. Scan the QR shown there using WhatsApp → Linked devices → Link a device. Never share those logs or a QR screenshot.
6. After it connects, send your number `..ping` from another account or ask a trusted contact to test. Check `/health` on the service URL.
7. Confirm that the host does not sleep and that the persistent disk is retained after redeploys. A free web-service plan may sleep or may not support the needed persistent storage, so 24/7 operation may require a paid plan.

## Updating the code

Edit `index.js`, commit the changes, and redeploy. Back up session data securely before making infrastructure changes. Do not copy the session folder to a public repository.

## Troubleshooting

- **No QR:** inspect logs; ensure Chromium dependencies are installed and the service can start.
- **QR expires:** restart or wait for a fresh QR in private logs.
- **Disconnected after deploy:** verify the persistent disk mount and ensure only one running instance uses the same session.
- **Replies do not arrive:** check `/health`, the private logs, `AUTO_REPLY=true`, and that the incoming message is a private chat rather than a group/status.
- **Authentication fails:** remove the broken session only if you understand this will require relinking, then redeploy and scan a fresh QR.
