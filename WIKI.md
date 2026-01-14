# WardrobePanel Plugin - Complete Wiki

<p align="center">
  <img src="https://img.shields.io/badge/Minecraft-1.21.4-brightgreen" alt="Minecraft Version">
  <img src="https://img.shields.io/badge/Java-21+-orange" alt="Java Version">
  <img src="https://img.shields.io/badge/Version-1.3.1-blue" alt="Plugin Version">
  <img src="https://img.shields.io/badge/License-MIT-yellow" alt="License">
</p>

A powerful Minecraft plugin that provides a web-based character creator, allowing players to customize their skins with clothing, accessories, hair styles, and more through an intuitive browser interface.

---

## 📋 Table of Contents

1. [Overview](#-overview)
2. [Features](#-features)
3. [Requirements](#-requirements)
4. [Installation](#-installation)
5. [Configuration](#-configuration)
6. [Commands](#-commands)
7. [Permissions](#-permissions)
8. [Permission-Based Item Filtering](#-permission-based-item-filtering)
9. [Owned Items System](#-owned-items-system)
10. [Character Profiles](#-character-profiles)
11. [MineSkin API Integration](#-mineskin-api-integration)
12. [Web Server Setup](#-web-server-setup)
13. [Adding Custom Overlays](#-adding-custom-overlays)
14. [Panorama Background](#-panorama-background)
15. [File Structure](#-file-structure)
16. [Troubleshooting](#-troubleshooting)
17. [API Reference](#-api-reference)
18. [Credits](#-credits)

---

## 🎮 Overview

WardrobePanel is a comprehensive character customization system for Minecraft servers. Players receive a unique, secure link to a web-based wardrobe editor where they can customize their character's appearance. The system supports multiple outfit layers including base skins, hair styles, eye colors, clothing, accessories, and more.

### How It Works

1. Player runs `/webchar` or `/webchar editor`
2. Plugin generates a secure, time-limited session token
3. Player receives a clickable link to the web panel
4. Player customizes their character in the browser
5. Player clicks "Save" or "Apply"
6. Skin is generated, sent to MineSkin API for signing, and applied to the player

---

## ✨ Features

### Core Features
- **Web-Based Character Creator**: Intuitive browser interface with 3D preview
- **Real-Time Skin Customization**: Layer multiple overlays (clothing, hair, accessories)
- **Color Customization**: Adjust colors for hair, eyes, skin tones, and clothing
- **3D Skin Preview**: Powered by skinview3d for realistic rendering
- **Built-in Web Server**: No external hosting required

### Player Features
- **Save & Load Outfits**: Persistent outfit storage per player
- **Multiple Character Profiles**: Create and manage multiple character looks
- **Secure Sessions**: Time-limited, token-based authentication

### Admin Features
- **Permission System**: Fine-grained control over which items players can access
- **Owned Items System**: Grant/revoke specific overlays to players
- **Starter Items**: Define items available to all players by default
- **Auto Overlay Scanning**: Automatically detect and catalog new overlay files

### Customization Categories
- **Base Skins**: Steve/Alex body types, male/female variants
- **Hair Styles**: Various hairstyles with color customization
- **Eyes**: Eye styles and colors
- **Shirts**: Upper body clothing
- **Jackets**: Outerwear layers
- **Pants**: Lower body clothing
- **Shoes**: Footwear options
- **Accessories**: Additional decoration items
- **Beards**: Facial hair for male characters
- **Markings**: Face paint, scars, tattoos, etc.
- **Skin Colours**: Skin tone palette options

---

## 📦 Requirements

- **Java**: 21 or higher
- **Server**: Paper 1.21.4+ (recommended) or Spigot compatible
- **MineSkin API Key**: Required for skin application (free at [mineskin.org](https://mineskin.org/apikey))

---

## 🔧 Installation

### Step-by-Step Installation

1. Download the latest `wardrobepanel-x.x.x.jar`
2. Place the JAR in your server's `plugins/` folder
3. Start/restart your server
4. The plugin will generate default configuration and asset files
5. Configure `plugins/WardrobePanel/config.yml` (see [Configuration](#-configuration))
6. Get a MineSkin API key from [mineskin.org/apikey](https://mineskin.org/apikey) and add it to the config
7. Restart the server or run `/webchar reload`


### Network/Port Configuration

For players to access the web panel, ensure:

1. **Port is Open**: Open port 50424 (or your configured port) on your firewall
2. **Port Forwarding**: If behind NAT, set up port forwarding
3. **Public URL**: Update `web.public-url` in config to your server's public IP/domain

---

## ⚙️ Configuration

The main configuration file is located at `plugins/WardrobePanel/config.yml`.

### Web Server Settings

```yaml
web:
  # Enable the built-in web server
  enabled: true
  # Port for the web server (make sure this port is open)
  port: 50424
  # Host to bind to (0.0.0.0 = all interfaces)
  host: "0.0.0.0"
  # Public URL for the wardrobe panel (used in links sent to players)
  public-url: "http://play.myserver.com:8080"
```

### Character Profiles

```yaml
profiles:
  # Enable multiple character profiles per player
  enabled: true
  # Maximum profiles per player
  max-per-player: 5
```

### Feature Toggles

Control which customization features are available:

```yaml
features:
  skin-change: true    # Allow changing base skin
  skin-colour: true    # Allow changing skin palette
  eye-colour: true     # Allow changing eye color
  eye-style: true      # Allow changing eye style
  hair-colour: true    # Allow changing hair color
  hair-style: true     # Allow changing hair style
  beard-style: true    # Allow changing beard style
```

### Session Settings

```yaml
session:
  expiry-minutes: 1440  # 24 hours
  max-per-player: 1
```

### Cooldown Settings

```yaml
cooldown:
  enabled: true
  seconds: 30  # Time between link requests
```

### MineSkin API

```yaml
mineskin:
  api-key: "your-api-key"
  variant: "auto"  # "steve", "slim", or "auto"
```

### Skin Application

```yaml
skin-apply:
  auto-apply: true       # Apply skin when player saves
  apply-on-join: true    # Apply saved skin on login
  apply-message: "&aYour custom skin has been applied!"
  apply-failed: "&cFailed to apply skin. Please try again later."
```

### Storage

```yaml
storage:
  data-folder: "playerdata"
  web-folder: "web"
```

### Messages

All messages support color codes with `&`:

```yaml
messages:
  prefix: "&8[&6WebChar&8] "
  link-message: "&aClick here to open your character editor: &n%link%"
  link-hover: "&7Click to open the character creator"
  session-created: "&aYour character editor link has been created!"
  session-expired: "&cYour session has expired. Use /webchar editor for a new link."
  no-permission: "&cYou don't have permission to do that."
  reload-success: "&aConfiguration reloaded! Overlays rescanned."
  server-not-running: "&cThe web server is not running. Contact an administrator."
  cooldown-message: "&cPlease wait %time% before requesting a new link."
```

---

## 💬 Commands

### Player Commands

| Command | Description |
|---------|-------------|
| `/webchar` | Open the default character editor |
| `/webchar editor [profile]` | Edit a specific character profile |
| `/webchar create <name>` | Create a new character profile |
| `/webchar delete <name>` | Delete a character profile |
| `/webchar list` | List your character profiles |
| `/webchar profiles` | Alias for list |
| `/webchar help` | Show command help |

**Aliases**: `/wc`, `/character`

### Admin Commands

| Command | Description |
|---------|-------------|
| `/webchar reload` | Reload configuration & rescan overlays |
| `/webchar status` | Show plugin status (web server, sessions, etc.) |
| `/webchar link <player>` | Generate a wardrobe link for another player |
| `/webchar sessions` | List all active sessions |

### Ownership Commands (Admin)

| Command | Description |
|---------|-------------|
| `/webchar give <player> all` | Give all overlays to a player |
| `/webchar give <player> category <cat>` | Give all overlays in a category |
| `/webchar give <player> overlay <cat> <id>` | Give a specific overlay |
| `/webchar revoke <player> all` | Revoke all owned overlays from a player |
| `/webchar revoke <player> category <cat>` | Revoke a category from a player |
| `/webchar revoke <player> overlay <cat> <id>` | Revoke a specific overlay |

---

## 🔐 Permissions

### Basic Permissions

| Permission | Default | Description |
|------------|---------|-------------|
| `webchar.use` | `true` | Use the character editor |
| `webchar.admin` | `op` | Access admin commands |
| `webchar.reload` | `op` | Reload configuration |
| `webchar.link.others` | `op` | Generate links for other players |

### Item Permissions

| Permission | Description |
|------------|-------------|
| `wardrobepanel.bypass` | Bypass all permission checks (see all items) |
| `wardrobepanel.*` | Access to all skins and overlays |
| `wardrobepanel.skin.*` | Access to all base skins |
| `wardrobepanel.skin.<skin-id>` | Access to a specific base skin |
| `wardrobepanel.overlay.*` | Access to all overlays |
| `wardrobepanel.overlay.<category>.*` | Access to all overlays in a category |
| `wardrobepanel.overlay.<category>.<overlay-id>` | Access to a specific overlay |

### Permission Examples

```yaml
# Give access to all hair styles
- wardrobepanel.overlay.hairs.*

# Give access to a specific shirt
- wardrobepanel.overlay.shirts.steve-male-casual_tee

# Give access to all jackets
- wardrobepanel.overlay.jackets.*

# Give access to a specific base skin
- wardrobepanel.skin.base-steve-male

# Give full bypass (see everything)
- wardrobepanel.bypass
```

---

## 🎫 Permission-Based Item Filtering

The permission system allows fine-grained control over which items players can see and use.

### Configuration

```yaml
permissions:
  # Enable permission-based filtering
  enabled: true
  
  # Default behavior for items without specific permissions
  # true = players can use items by default
  # false = players need explicit permission
  default-allowed: true
  
  # Permission to bypass all checks
  bypass-permission: "wardrobepanel.bypass"
```

### How It Works

1. When `permissions.enabled: true`, players only see items they have permission for
2. When `default-allowed: true`, players see all items unless explicitly denied
3. When `default-allowed: false`, players see nothing unless explicitly granted
4. Players with `wardrobepanel.bypass` see all items regardless

### Permission Format

| Type | Format | Example |
|------|--------|---------|
| Base Skin | `wardrobepanel.skin.<skin-id>` | `wardrobepanel.skin.base-steve-male` |
| Overlay | `wardrobepanel.overlay.<category>.<overlay-id>` | `wardrobepanel.overlay.hairs.male-90s_Side_Part` |
| Category | `wardrobepanel.overlay.<category>.*` | `wardrobepanel.overlay.hairs.*` |
| All Overlays | `wardrobepanel.overlay.*` | - |
| All Skins | `wardrobepanel.skin.*` | - |
| Everything | `wardrobepanel.*` | - |

---

## 🎁 Owned Items System

The owned items system allows you to grant specific overlays to players independently of permissions. This is useful for shop systems, rewards, or progression mechanics.

### Configuration

```yaml
owned-items:
  # Enable the owned items system
  enabled: false
  
  # Categories always available to everyone
  starter-categories:
    - "eyes"
  
  # Specific starter overlays (format: category/overlayId)
  starter-overlays: []
  
  # Combine with permission system
  combine-with-permissions: true
```

### Behavior

- **When `enabled: false`**: Only the permission system is used
- **When `enabled: true`**: Players see owned items + starter items
- **When `combine-with-permissions: true`**: Players see owned + starter + permission-allowed items
- **When `combine-with-permissions: false`**: Only owned + starter items are shown (permissions ignored)

### Granting/Revoking Items

Use the admin commands to manage player-owned items:

```bash
# Give all overlays to a player
/webchar give PlayerName all

# Give all items in the hairs category
/webchar give PlayerName category hairs

# Give a specific overlay
/webchar give PlayerName overlay shirts casual_tee

# Revoke all items
/webchar revoke PlayerName all

# Revoke a category
/webchar revoke PlayerName category hairs

# Revoke a specific overlay
/webchar revoke PlayerName overlay shirts casual_tee
```

### Storage

Owned items are stored in `plugins/WardrobePanel/playerdata/<uuid>_owned.json`.

---

## 👥 Character Profiles

Players can create multiple character profiles, each with their own outfit configuration.

### Configuration

```yaml
profiles:
  enabled: true
  max-per-player: 5
```

### Usage

```bash
# Create a new profile
/webchar create MyKnight

# Edit a specific profile
/webchar editor MyKnight

# List all profiles
/webchar list

# Delete a profile
/webchar delete MyKnight
```

### Storage

Profiles are stored in `plugins/WardrobePanel/profiles/<uuid>.json`.

---

## 🎨 MineSkin API Integration

WardrobePanel uses the MineSkin API to generate signed skin textures that can be applied to players.

### Getting an API Key

1. Visit [https://mineskin.org/apikey](https://mineskin.org/apikey)
2. Log in with your Minecraft account
3. Generate an API key
4. Add the key to your config:

```yaml
mineskin:
  api-key: "your-api-key-here"
  variant: "auto"
```

### Skin Variants

- `"steve"`: Classic wide arms (4px)
- `"slim"` or `"alex"`: Slim arms (3px)
- `"auto"`: Automatically detect from base skin selection

### Rate Limits

MineSkin has rate limits on their free tier. If you have a high-traffic server, consider:
- Getting a premium API key
- Implementing caching for frequently used skins
- Adding longer cooldowns between skin changes

---

## 🌐 Web Server Setup

### Built-in Web Server

The plugin includes a built-in web server (Javalin). No external hosting is required.

```yaml
web:
  enabled: true
  port: 50424
  host: "0.0.0.0"
  public-url: "http://your-domain-or-ip:50424"
```

### Reverse Proxy Setup (Recommended for Production)

For production use, run behind a reverse proxy (nginx, Apache, Cloudflare) with HTTPS.

#### Nginx Example

```nginx
server {
    listen 443 ssl http2;
    server_name wardrobe.yourserver.com;
    
    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;
    
    location / {
        proxy_pass http://localhost:50424;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Update your config to use the HTTPS URL:

```yaml
web:
  public-url: "https://wardrobe.yourserver.com"
```

---

## 🖼️ Adding Custom Overlays

### Folder Structure

Overlays are stored in `plugins/WardrobePanel/overlays/` with category subfolders:

```
overlays/
├── hairs/
│   ├── male/
│   │   └── cool_hairstyle.png
│   ├── female/
│   │   └── long_hair.png
│   └── any/
│       └── generic_style.png
├── shirts/
│   ├── steve-male/
│   ├── alex-male/
│   ├── female/
│   └── any/
├── pants/
├── shoes/
├── accessories/
├── beards/
├── markings/
└── eyes/
```

### Naming Convention

Overlay filenames become the overlay ID. The subfolder determines the gender/model compatibility:

- `male/` - Only for male characters
- `female/` - Only for female characters
- `steve-male/` - For Steve model male characters
- `alex-male/` - For Alex model male characters
- `any/` - Available to all character types

### File Requirements

- **Format**: PNG with transparency
- **Size**: 64x64 pixels (standard Minecraft skin format)
- **Layers**: Use alpha channel for transparent areas

### Auto-Scanning

When you add new overlay files:

1. Place the PNG in the appropriate category/model folder
2. Run `/webchar reload`
3. The manifest will be automatically regenerated

### Example: Adding a New Shirt

1. Create your shirt overlay as a 64x64 PNG
2. Save it to `plugins/WardrobePanel/overlays/shirts/steve-male/my_cool_shirt.png`
3. Run `/webchar reload`
4. The overlay is now available in the wardrobe panel

---

## 🏞️ Panorama Background

The wardrobe panel supports interactive panorama backgrounds that move with the 3D viewer camera.

### Minecraft-style Cubemap (Recommended)

Place 6 panorama images in `plugins/WardrobePanel/web/assets/bg/`:

| File | Direction |
|------|-----------|
| `panorama_0.png` | Right face (+X) |
| `panorama_1.png` | Left face (-X) |
| `panorama_2.png` | Top face (+Y) |
| `panorama_3.png` | Bottom face (-Y) |
| `panorama_4.png` | Front face (+Z) |
| `panorama_5.png` | Back face (-Z) |

Images should be square (512x512 or 1024x1024).

### Single Equirectangular Image

Alternatively, place a single panorama at `assets/bg/panorama.png` (2:1 aspect ratio, e.g., 2048x1024).

### Getting Panorama Images

- Extract from Minecraft: `assets/minecraft/textures/gui/title/background/`
- Use shader mods to capture custom screenshots
- Find pre-made Minecraft panoramas online

### Fallback Order

1. Minecraft cubemap (`panorama_0.png` to `panorama_5.png`)
2. Single equirectangular (`panorama.png`)
3. Static background (`bg.png`)

---

## 📁 File Structure

```
plugins/WardrobePanel/
├── config.yml              # Main configuration
├── overlays/               # Custom overlay images
│   ├── hairs/
│   ├── shirts/
│   ├── pants/
│   └── ...
├── skins/                  # Base skin images
├── profiles/               # Character profile data
│   └── <uuid>.json
├── playerdata/             # Player outfit and ownership data
│   ├── <uuid>.json         # Outfit configuration
│   ├── <uuid>_skin.png     # Exported skin image
│   └── <uuid>_owned.json   # Owned items
└── web/                    # Website files
    ├── index.html
    ├── main.js
    ├── style.css
    └── assets/
        ├── manifest.json   # Auto-generated overlay catalog
        ├── catalog.json
        ├── overlays/       # Copied overlay images
        ├── skins/          # Copied skin images
        ├── bg/             # Background/panorama images
        └── icons/
```

---

## 🔧 Troubleshooting

### "Web server is not running"

- Check if the port is already in use by another application
- Check server logs for startup errors
- Try a different port in config.yml
- Ensure Java has permission to bind to the port

### "Website files not found"

- Ensure the `web/` folder exists in the plugin directory
- Check that `index.html` exists in the web folder
- Run `/webchar reload` to regenerate assets

### Players can't access the panel

- **Firewall**: Ensure the port is open (inbound TCP)
- **Port Forwarding**: Configure your router if behind NAT
- **Public URL**: Verify `web.public-url` is correct
- **Test Locally**: Try `http://localhost:50424` first

### Skin not applying

- **MineSkin API Key**: Ensure it's valid and not expired
- **Rate Limits**: Wait if you've hit API limits
- **Server Logs**: Check for API error messages
- **Paper Server**: Skin application works best on Paper

### Session expired too quickly

- Increase `session.expiry-minutes` in config
- Check that server time is correct
- Ensure only one session per player (`max-per-player: 1`)

### Overlays not appearing

- Check file format (must be PNG)
- Verify image size (64x64 pixels)
- Ensure correct folder structure
- Run `/webchar reload` after adding files
- Check permissions if using permission system

### Debug Mode

Enable debug logging for more detailed output:

```yaml
debug: true
```

---

## 📡 API Reference

### REST API Endpoints

The web server exposes a REST API for plugin integration.

#### Create Session
```http
POST /api/plugin/session
Headers: X-API-Key: your-api-key
Body: { "playerUuid": "uuid", "playerName": "name" }
Response: { "success": true, "token": "...", "url": "...", "expiresAt": 123456789 }
```

#### Invalidate Session
```http
DELETE /api/plugin/session
Headers: X-API-Key: your-api-key
Body: { "token": "session-token" }
Response: { "success": true }
```

#### Get Player Outfit
```http
GET /api/plugin/outfit?playerUuid=uuid
Headers: X-API-Key: your-api-key
Response: { "success": true, "outfit": { ... } }
```

#### Validate Session (Client)
```http
GET /api/session/validate?token=session-token
Response: { "success": true, "playerName": "...", "playerUuid": "...", "outfit": { ... } }
```

#### Save Outfit (Client)
```http
POST /api/outfit/save
Body: { "token": "session-token", "outfit": { ... } }
Response: { "success": true }
```

#### Export Skin (Client)
```http
POST /api/skin/export
Body: { "token": "session-token", "skinData": "data:image/png;base64,..." }
Response: { "success": true, "skinPath": "/api/skin/uuid" }
```

#### Get Exported Skin
```http
GET /api/skin/:playerUuid
Response: PNG image
```

---

## 📜 Credits

- **Developed by**: Anonventions (Axmon, amon_m)
- **3D Skin Preview**: [skinview3d](https://github.com/bs-community/skinview3d)
- **Skin Signing**: [MineSkin API](https://mineskin.org)
- **License**: MIT License

---

## 🔗 Links

- **GitHub**: [https://github.com/Anonventions](https://github.com/Anonventions)
- **MineSkin**: [https://mineskin.org](https://mineskin.org)
- **Issue Tracker**: Report bugs and feature requests on GitHub

---

*Last updated: January 14, 2026 | Version 1.3.1*

