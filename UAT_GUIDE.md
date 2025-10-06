# ExtHelper v2 Bot - UAT Testing Guide

## SETUP

### Step 1: Connect with the Bot
1. Open Telegram
2. Search for `@exthelper_v2_bot` (or the actual bot username)
3. Start a conversation with the bot
4. Send `/start` or `/help` to initiate the conversation

### Step 2: Create Testing Group
1. Create a new Telegram group
2. Name it: `exthelper_v2 - Testing - [Your Name]`
   - Example: `exthelper_v2 - Testing - Daniela`
   - This naming convention helps with debugging
3. Add the bot `@exthelper_v2_bot` to the group
   - No need to set as admin

### Step 3: Configure Tier Groups (Optional - for T1/T2/T3 testing)
**Location:** Google Sheets > `MESSAGE_TIERS` tab

To test tier broadcasts (`/sendt1`, `/sendt2`, `/sendt3`):
1. Open the database link (use `/db` command)
2. Go to `MESSAGE_TIERS` sheet tab
3. Add your test group tag in the appropriate column:
   - Column A `Tier 1` - for T1 groups
   - Column B `Tier 2` - for T2 groups  
   - Column C `Tier 3` - for T3 groups
4. Example: Add `testgroup` in `Tier 1` column

**For `/sendmisc` testing:**
- Column D `Misc` - group tag
- Column E `Message` - pre-configured message to send
- Example: `testgroup` | `This is a misc message`

---

## TEST

### 1️⃣ PRIVATE CHAT TESTS (Direct message with bot)

| Command Type | Command | Expected Result | ✅/❌ |
|-------------|---------|-----------------|-----|
| **Help & Info** |
| Start | `/start` | Welcome message with command list |  |
| Help | `/help` | Full command guide |  |
| Database | `/db` | Link to Google Sheets database |  |
| **Broadcast Messages** |
| Send All (valid) | `/sendall <Test message>` | Sent to all active groups + confirmation |  |
| Send All (single word) | `/sendall hello` | Sent to all active groups + confirmation |  |
| Send All (invalid) | `/sendall` | Error: missing message |  |
| Send Group (valid) | `/sendgroup <yourgroup> <message>` | Sent to specific group + confirmation |  |
| Send Group (not found) | `/sendgroup <fake> <msg>` | Error: group not registered |  |
| Send Group (invalid) | `/sendgroup` | Error: missing parameters |  |
| Send Tier 1 | `/sendt1 <T1 test>` | Sent to T1 groups + summary *(requires Step 3)* |  |
| Send Tier 2 | `/sendt2 <T2 test>` | Sent to T2 groups + summary *(requires Step 3)* |  |
| Send Tier 3 | `/sendt3 <T3 test>` | Sent to T3 groups + summary *(requires Step 3)* |  |
| Send Misc | `/sendmisc` | Sends pre-configured messages *(requires Step 3)* |  |
| **List Groups** |
| List All | `/list` | Shows all groups (active + inactive) |  |
| List Active | `/listactive` | Shows only active groups |  |
| List Inactive | `/listinactive` | Shows only inactive groups |  |
| **Invalid** |
| Unknown command | `/unknown` | Error with available commands |  |

### 2️⃣ GROUP CHAT TESTS (Inside your testing group)

| Command Type | Command | Expected Result | ✅/❌ |
|-------------|---------|-----------------|-----|
| **Register Group** |
| Add (single word) | `/addgroup testgroup` | Group registered successfully |  |
| Add (multi-word) | `/addgroup <Test Group>` | Group registered successfully |  |
| Add (already active) | `/addgroup testgroup` | Info: already registered |  |
| Add (invalid) | `/addgroup` | Error: missing group name |  |
| Add (invalid spaces) | `/addgroup multi word` | Error: use angle brackets |  |
| **Control Group** |
| Status (after add) | `/status` | Shows active status + details |  |
| Stop | `/stop` | Group deactivated |  |
| Status (after stop) | `/status` | Shows inactive + reactivation info |  |
| Reactivate | `/addgroup <newname>` | Group reactivated with new tag |  |
| Help | `/help` | Shows group commands only |  |
| **Invalid** |
| Unknown | `/broadcast` | Error: invalid command |  |

### 3️⃣ VALIDATION TESTS (Either chat)

| Test Case | Command | Expected Result | ✅/❌ |
|-----------|---------|-----------------|-----|
| Empty brackets | `/sendall <  >` | Error: empty message |  |
| Unclosed bracket | `/addgroup <test` | Error: invalid format |  |
| Extra spaces | `/sendall     <msg>` | Works normally |  |
| Special chars | `/addgroup <Test@#$>` | Accepts special characters |  |

---

## NOTES

- ✅ = Test Passed (Green checkmark)
- ❌ = Test Failed (Red X)
- All timestamps should be in GMT+7
- Invalid commands should provide helpful error messages
- Multi-word parameters require angle brackets `<like this>`
- Single words don't need brackets