# ExtHelper v2 Bot - Documentation

## Table of Contents
1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [Core Concepts](#core-concepts)
4. [Commands Guide](#commands-guide)
5. [Google Sheets Database](#google-sheets-database)
6. [Troubleshooting](#troubleshooting)

---

## Overview

ExtHelper v2 is a Telegram bot for broadcasting messages across multiple Telegram groups. Authorized users can send text and images to registered groups.

- **Bot Username:** `@exthelper_v2_bot`
- **Database:** Google Sheets
- **Timezone:** GMT+7
- **Platform:** n8n workflow automation

---

## Quick Start

### Step 1: Connect
1. Open Telegram
2. Search `@exthelper_v2_bot`
3. Send `/start` or `/help`

### Step 2: Register a Group
1. Create or open your Telegram group
2. Add `@exthelper_v2_bot` to the group
3. In the group, send:
   ```
   /addgroup <YourGroupName>
   ```

### Step 3: Start Broadcasting
1. Go to private chat with bot
2. Send your first broadcast:
   ```
   /sendall <Your message here>
   ```

---

## Core Concepts

### 1. Active vs Inactive Groups

**Active Groups:**
- ✅ Receive all broadcasts
- ✅ Appear in `/listactive`
- ✅ Set when using `/addgroup`

**Inactive Groups:**
- ❌ Do NOT receive any broadcasts
- ❌ Set when using `/stop`
- ℹ️ Can be reactivated with `/addgroup`

**Important:** Broadcasting only sends to ACTIVE groups.

---

### 2. Command Syntax Rules

#### Bracket Usage

**Single Word = No Brackets**
```
✅ /addgroup sales
✅ /sendall hello
```

**Multiple Words = Use Angle Brackets `< >`**
```
✅ /addgroup <Sales Team>
✅ /sendall <Your long message here>
✅ /sendgroup <tag> <message>
```

#### Common Errors
```
❌ /addgroup Sales Team        → Use: /addgroup <Sales Team>
❌ /sendall                    → Missing message
❌ /addgroup <  >               → Empty parameter
❌ /addgroup <test              → Unclosed bracket
```

---

### 3. Chat Context

**Private Chat Commands** (send to bot directly):
- All broadcasting commands (`/sendall`, `/sendgroup`, `/sendt1/t2/t3`)
- All image broadcasting (`/sendpict1/2/3`)
- Listing commands (`/list`, `/listactive`, `/listinactive`)
- Database access (`/db`)

**Group Chat Commands** (send in group):
- Registration (`/addgroup`)
- Deactivation (`/stop`)
- Status check (`/status`)

**Both:**
- Help (`/start`, `/help`)

---

### 4. Group Tiers

Groups can be organized into 3 tiers for targeted broadcasting:

- **Tier 1 (T1):** Use `/sendt1 <message>`
- **Tier 2 (T2):** Use `/sendt2 <message>`
- **Tier 3 (T3):** Use `/sendt3 <message>`

Configuration: Google Sheets → `MESSAGE_TIERS` tab

---

### 5. Authorization

**All commands require authorization.**

- Managed in Google Sheets
- Only authorized users can execute commands
- Unauthorized users receive error message

---

## Commands Guide

### General Commands

#### `/start` or `/help`
Shows full command guide.

**Where:** Any (private or group)

**Output:**
```
ExtHelper v2 Commands

Private Chat:
• /sendall <message>
• /sendgroup <tag> <message>
• /sendt1/t2/t3 <message>
• /sendmisc
• /sendpict1/2/3
• /list, /listactive, /listinactive
• /db

Group Chat:
• /addgroup <n>
• /stop
• /status
```

---

#### `/db`
Provides Google Sheets database link for configuration.

**Where:** Private chat

**Output:**
```
📊 Database: https://docs.google.com/spreadsheets/d/...
```

---

### Group Management (Group Chat)

#### `/addgroup <name>`
Register the group for broadcasts.

**Where:** Group chat

**Examples:**
```
/addgroup sales
/addgroup <Sales Team APAC>
/addgroup <VIP Customer Group>
```

**Success Output:**
```
✅ Group Registered
Tag: sales
Status: Active
Registered: 2025-10-27 14:30:00 GMT+7
```

**Rules:**
- Single word: No brackets needed
- Multiple words: Use `< >` brackets
- Tag must be unique among active groups
- Reactivates previously inactive groups

---

#### `/stop`
Deactivate group from receiving broadcasts.

**Where:** Group chat

**Output:**
```
⛔ Group Deactivated
This group will no longer receive broadcasts.
Reactivate with: /addgroup <new_tag>
```

**Notes:**
- Group data is preserved
- Can be reactivated anytime

---

#### `/status`
Check current group registration status.

**Where:** Group chat

**Output (Active):**
```
✅ Status: Active
Tag: sales
Tier: T1
Registered: 2025-10-15 14:30:00
```

**Output (Inactive):**
```
⛔ Status: Inactive
Reactivate with: /addgroup <tag>
```

---

### Text Broadcasting (Private Chat)

#### `/sendall <message>`
Broadcast to all active groups.

**Where:** Private chat

**Examples:**
```
/sendall hello
/sendall <System maintenance tonight 10 PM - 2 AM>
```

**Output:**
```
📤 Broadcast Complete
✅ Sent: 15 groups
❌ Failed: 0 groups
```

---

#### `/sendgroup <tag> <message>`
Send to one specific group.

**Where:** Private chat

**Examples:**
```
/sendgroup <sales> <Q4 targets updated>
/sendgroup team hello
```

**Success Output:**
```
✅ Sent to: Sales Team APAC
```

**Error Output:**
```
❌ Group 'unknown' not registered
❌ Group 'oldteam' is inactive
```

---

#### `/sendt1 <message>`, `/sendt2 <message>`, `/sendt3 <message>`
Broadcast to specific tier groups.

**Where:** Private chat

**Examples:**
```
/sendt1 <Premium announcement>
/sendt2 <Standard update>
/sendt3 <Free tier message>
```

**Output:**
```
📤 Tier 1 Broadcast
✅ Sent: 5 groups
❌ Failed: 0 groups
```

**Setup Required:**
Configure tiers in Google Sheets → `MESSAGE_TIERS` tab

---

#### `/sendmisc`
Send pre-configured messages to specific groups.

**Where:** Private chat

**Usage:** `/sendmisc`

**Setup Required:**
Google Sheets → `MESSAGE_TIERS` tab
- Column G: Group tags
- Column H: Messages

**Example Config:**
| Misc (G) | Message (H) |
|----------|-------------|
| dailynews | Daily update message |
| reports | Weekly report ready |

**Output:**
```
📤 Misc Messages Sent
✅ dailynews: Delivered
✅ reports: Delivered
```

---

### Image Broadcasting (Private Chat)

**All three commands work the same way: Send photo with command in the caption.**

#### `/sendpict1`, `/sendpict2`, `/sendpict3`

**Where:** Private chat

**Target Groups:**
- `/sendpict1` → Groups configured in database reference column 1
- `/sendpict2` → Groups configured in database reference column 2
- `/sendpict3` → Groups configured in database reference column 3

**How to Use (Same for All):**
1. Send photo to bot
2. Include command in the caption
3. Add your message after the command (use brackets for multi-word)

**Examples:**
```
Photo with caption: /sendpict1 <Product launch today!>
Photo with caption: /sendpict2 <New promotion available now!>
Photo with caption: /sendpict3 <Event announcement>
```

**Bracket Rules:**
- Single word: `/sendpict1 hello`
- Multi-word: `/sendpict1 <Your message here>`

**Output:**
```
📤 Image Broadcast Complete
✅ Sent: 12 groups
```

**Notes:**
- All three use the same method (photo with command in caption)
- Only sends to active groups in their configured list
- Caption text after command becomes the broadcast message
- Follow bracket rules: multi-word captions need `< >`

---

### Group Listing (Private Chat)

#### `/list`
Show all registered groups.

**Where:** Private chat

**Output:**
```
📋 All Groups

Active (12):
1. sales - Sales Team (T1)
2. marketing - Marketing Dept (T2)

Inactive (3):
1. oldteam - Old Team

Total: 15 (12 active, 3 inactive)
```

---

#### `/listactive`
Show active groups only.

**Where:** Private chat

**Output:**
```
✅ Active Groups (12)
• sales - Sales Team (T1)
• marketing - Marketing Dept (T2)
```

---

#### `/listinactive`
Show inactive groups only.

**Where:** Private chat

**Output:**
```
⛔ Inactive Groups (3)
• oldteam - Deactivated 2025-10-20
```

---

### Keyword Management (Automatic)

**What It Does:**
Automatically detects and logs specific keywords from group messages.

**No Command Needed** - Runs automatically in background.

#### How It Works

1. Configure keywords in Google Sheets → `KEYWORD_LIST`
2. Bot monitors all group messages
3. When keyword detected → logs to `KEYWORD_MANAGEMENT` sheet

#### Detection Rules

**Standalone Matching:**
- Keyword must be complete word/phrase
- Not part of another word

**Examples:**

Keyword: `promotion`
```
✅ "Check our promotion today"     → Detected
✅ "Promotion available now"       → Detected
❌ "promotional offer"             → Not detected
```

Keyword: `优惠` (Chinese)
```
✅ "今天有优惠吗？"                  → Detected
✅ "请给我优惠"                     → Detected
```

#### What Gets Logged

| Field | Example |
|-------|---------|
| Timestamp | 2025-10-27 15:30:00 GMT+7 |
| Chat ID | -1001234567890 |
| Group Name | Sales Team |
| Keyword | promotion |
| User ID | 5005604119 |
| User Name | John |
| Message | When is the next promotion? |

#### Configuration

1. Access database: `/db`
2. Go to `KEYWORD_LIST` sheet
3. Add keywords (one per row)

**Example:**
| Keywords |
|----------|
| promotion |
| discount |
| urgent |
| 优惠 |
| 折扣 |

**Features:**
- Works for English and Chinese
- Case insensitive
- Supports multi-word keywords
- Updates take effect immediately

---

## Google Sheets Database

### Database Structure

Access via `/db` command.

#### Sheet 1: REGISTERED_GROUP
**Purpose:** All group registrations

**Columns:**
- `TIMESTAMP_GMT7` - Registration time
- `USER_ID` - Registering user
- `USER_FIRST_NAME` - User's first name
- `USER_LAST_NAME` - User's last name
- `USER_NAME` - Telegram username
- `CHAT_ID` - Group chat ID
- `CHAT_TITLE` - Group name
- `GROUP_TAG` - Unique group tag
- `STATUS` - Active/Inactive

---

#### Sheet 2: MESSAGE_TIERS
**Purpose:** Tier and Misc configuration

**Layout:**
| A (Tier 1) | B | C (Tier 2) | D | E (Tier 3) | F | G (Misc) | H (Message) |
|------------|---|------------|---|------------|---|----------|-------------|
| vipgroup | | standardgroup | | freegroup | | dailynews | Daily update |
| premiumteam | | regularteam | | trial | | reports | Weekly report |

**Usage:**
- Columns A, C, E: Group tags for each tier
- Column G: Misc group tags
- Column H: Pre-configured messages for Misc groups

---

#### Sheet 3: Authorized Users
**Purpose:** Access control

**Columns:**
- `user_id` - Telegram user ID
- `first_name` - User's first name
- `last_name` - User's last name
- `username` - Telegram username
- `authorized` - TRUE/FALSE
- `notes` - Additional info

---

#### Sheet 4: KEYWORD_LIST
**Purpose:** Keywords to monitor

**Columns:**
- `Keywords` - One keyword per row

**Example:**
| Keywords |
|----------|
| promotion |
| discount |
| urgent |

---

#### Sheet 5: KEYWORD_MANAGEMENT
**Purpose:** Keyword detection logs

**Columns:**
- `TIMESTAMP_GMT7` - When detected
- `CHAT_ID` - Group ID
- `GROUP_TITLE` - Group name
- `KEYWORD` - Matched keyword
- `USER_ID` - User who sent message
- `USER_NAME` - User's name
- `MESSAGE` - Full message text

---

## Troubleshooting

### Bot Not Responding

**Check:**
- Workflow is active
- You are authorized (check Google Sheets)
- Bot is in the group (for group commands)

---

### Broadcast Failed

**Check:**
- Group is active: `/listactive`
- Bot not removed from group
- Group tag is correct

---

### Command Errors

**Common Issues:**

```
❌ Multi-word without brackets
Fix: Use <angle brackets>

❌ Empty parameter
Fix: Provide content

❌ Unclosed bracket
Fix: Close with >
```

**Verify syntax:** `/help`

---

### Group Won't Register

**Check:**
- Bot added to group first
- No duplicate tag (active groups)
- Proper bracket format

---

### Image Won't Send

**Check:**
- Photo is attached
- Command included in caption
- Multi-word captions use brackets: `/sendpict1 <your message>`
- Single word no brackets: `/sendpict1 hello`
- Correct command spelling

---

## Feature Summary

### Phase 1 Features
✅ Text broadcasting (all, group, tiers)  
✅ Group management (register, deactivate, status)  
✅ Group listing (all, active, inactive)  
✅ Pre-configured messages  
✅ Help and database access

### Phase 2 Features
✅ Image broadcasting (3 configurations)  
✅ Authorization gate checking  
✅ Bracket validation system  
✅ Keyword management (auto-detection)  
✅ Bot profile customization

---

## Command Quick Reference

| Command | Where | What It Does |
|---------|-------|--------------|
| **Information** | | |
| `/start`, `/help` | Any | Show guide |
| `/db` | Private | Database link |
| **Group Setup** | | |
| `/addgroup <n>` | Group | Register group |
| `/stop` | Group | Deactivate group |
| `/status` | Group | Check status |
| **Text Broadcast** | | |
| `/sendall <msg>` | Private | Send to all |
| `/sendgroup <tag> <msg>` | Private | Send to one |
| `/sendt1/t2/t3 <msg>` | Private | Send to tier |
| `/sendmisc` | Private | Pre-configured |
| **Image Broadcast** | | |
| `/sendpict1` | Private | Send to config 1 groups |
| `/sendpict2` | Private | Send to config 2 groups |
| `/sendpict3` | Private | Send to config 3 groups |
| **Group Listing** | | |
| `/list` | Private | All groups |
| `/listactive` | Private | Active only |
| `/listinactive` | Private | Inactive only |

---

**Version:** 2.0  
**Last Updated:** October 27, 2025  
**Bot:** @exthelper_v2_bot