# Orange Glitter Scratch-Off Game for Slack

A Slack bot that lets users play a scratch-off game with orange glitter theme using Socket Mode.

## Setup Instructions

### 1. Create a Slack App

1. Go to [api.slack.com/apps](https://api.slack.com/apps)
2. Click "Create New App" → "From scratch"
3. Enter app name (e.g., "Orange Glitter Game") and select your workspace
4. Click "Create App"

### 2. Enable Socket Mode

1. In your app settings, go to "Socket Mode" in the sidebar
2. Toggle "Enable Socket Mode" to ON
3. Under "App-Level Tokens", click "Generate Token and Scopes"
4. Name it "socket-token" and add the `connections:write` scope
5. Click "Generate" and copy the token (starts with `xapp-`)

### 3. Add OAuth Scopes

1. Go to "OAuth & Permissions" in the sidebar
2. Under "Scopes" → "Bot Token Scopes", add:
   - `commands`
   - `chat:write`
   - `chat:write.public`

### 4. Create Slash Command

1. Go to "Slash Commands" in the sidebar
2. Click "Create New Command"
3. Fill in:
   - Command: `/glitter`
   - Request URL: (leave blank for Socket Mode)
   - Short description: "Merch drop"
4. Click "Save"

### 5. Install App to Workspace

1. Go to "Install App" in the sidebar
2. Click "Install to Workspace"
3. Review permissions and click "Allow"
4. Copy the "Bot User OAuth Token" (starts with `xoxb-`)

### 6. Get Signing Secret

1. Go to "Basic Information" in the sidebar
2. Under "App Credentials", copy the "Signing Secret"

### 7. Configure Environment

1. Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```

2. Fill in your `.env` file with the tokens from above:
   ```
   SLACK_BOT_TOKEN=xoxb-your-bot-token-here
   SLACK_SIGNING_SECRET=your-signing-secret-here
   SLACK_APP_TOKEN=xapp-your-app-token-here
   PORT=3000
   STORE_URL=https://your-swag-store.com
   HERO_GIF_URL=https://media.giphy.com/media/orange-glitter-gif/giphy.gif
   ONE_WIN_PER_USER=true
   ```

### 8. Install Dependencies and Run

```bash
npm install
npm start
```

For development with auto-reload:
```bash
npm run dev
```

### 9. Test the Game

1. In Slack, type `/glitter` in any channel where the bot is present
2. Click on tiles to scratch them off
3. Find the prize (🎁) to win!

## Game Rules

- Each game has a 3×3 grid of tiles covered with 🟧✨
- Only one tile contains the prize (🎁)
- Other tiles reveal misses (🧡)
- Winners get a celebratory modal with a discount code
- By default, each user can only win once (configurable via `ONE_WIN_PER_USER`)

## Optional: Shopify Integration

To generate real discount codes instead of stubbed ones, add these to your `.env`:

```
SHOPIFY_STORE=your-store.myshopify.com
SHOPIFY_ADMIN_TOKEN=shpat_your-admin-token
SHOPIFY_API_VERSION=2023-10
DISCOUNT_PREFIX=GLITTER
DISCOUNT_PERCENT=20
```

### Getting Shopify Admin Token

1. In your Shopify admin, go to Apps → "Manage private apps" (or "App and sales channel settings")
2. Create a private app with these permissions:
   - Discounts: Read and write
3. Copy the Admin API access token

## Architecture Notes

- Game state is stored in memory (Map) for simplicity
- For production, replace with Redis or database (see comments in `app.js`)
- Uses Slack Socket Mode to avoid needing public URLs
- One game instance per user, resets on new `/glitter` command