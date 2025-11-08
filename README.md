# 🌊 Flood Control System - TalkBack Card Management

## Overview
This system allows you to register, update, and remove RFID cards remotely through ThingSpeak TalkBack without re-uploading code to the ESP32.

---

## 📋 System Information

- **ThingSpeak Channel ID:** 3153327
- **TalkBack ID:** 55666
- **Auto-Check Interval:** Every 15 seconds
- **Maximum Cards:** 20

---

## 🎯 Available Commands

### 1. ADD_EMPLOYEE:CARDID
Registers an RFID card as an **Employee** (Type 1)

**Format:**
```
ADD_EMPLOYEE:CARDID
```

**Example:**
```
ADD_EMPLOYEE:B3312B39
```

**What it does:**
- Grants full access (can control pump)
- Shows as GREEN in data viewer
- Access Type = 1 in ThingSpeak

---

### 2. ADD_CITIZEN:CARDID
Registers an RFID card as a **Citizen** (Type 2)

**Format:**
```
ADD_CITIZEN:CARDID
```

**Example:**
```
ADD_CITIZEN:322FADAB
```

**What it does:**
- Grants view-only access (monitoring only)
- Shows as BLUE in data viewer
- Access Type = 2 in ThingSpeak

---

### 3. REMOVE:CARDID
Removes an RFID card from the system

**Format:**
```
REMOVE:CARDID
```

**Example:**
```
REMOVE:B3312B39
```

**What it does:**
- Deletes card from memory
- Card becomes unauthorized
- Access Type = 0 when tapped

---

### 4. LIST
Displays all registered cards in Serial Monitor

**Format:**
```
LIST
```

**What it does:**
- Shows card count
- Lists all card IDs
- Shows their access types (Employee/Citizen)

**Output example:**
```
--- Registered Cards ---
  1. B3312B39 - EMPLOYEE
  2. 322FADAB - CITIZEN
  3. 13E31B2D - CITIZEN
------------------------
```

---

### 5. CLEAR
⚠️ **WARNING:** Removes ALL cards from the system

**Format:**
```
CLEAR
```

**What it does:**
- Deletes all registered cards
- Resets system to empty state
- All cards become unauthorized
- Use with caution!

---

## 📖 Step-by-Step Instructions

### How to Add a New RFID Card

**Step 1: Get the Card ID**
1. Tap the unknown card on RFID reader
2. Check ThingSpeak channel or HTML viewer
3. Look for RFID ID in Field 4 (e.g., "322FADAB")
4. Card will show as UNAUTHORIZED (red)

**Step 2: Add Command in TalkBack**
1. Go to ThingSpeak website
2. Navigate to: **Apps → TalkBack → TalkBack 55666**
3. Click **"Add a new command"**
4. **Position:** Leave blank
5. **Command string:** Type command (see examples below)
6. Click **"Save"**

**Step 3: Wait for Processing**
- ESP32 checks TalkBack every 15 seconds
- Within 15-30 seconds, command will execute
- No need to reset ESP32!

**Step 4: Verify**
- Tap the card again
- Should now show correct access type
- Check HTML viewer or ThingSpeak to confirm

---

## 💡 Usage Examples

### Example 1: Register New Employee
**Scenario:** New employee John gets card B3312B39

1. John taps his card → Shows UNAUTHORIZED
2. Check RFID ID: B3312B39
3. Go to TalkBack
4. Add command: `ADD_EMPLOYEE:B3312B39`
5. Wait 15 seconds
6. John taps card again → Shows EMPLOYEE ✓

---

### Example 2: Register Citizen for Monitoring
**Scenario:** Citizen wants to monitor flood data

1. Citizen taps card → Shows UNAUTHORIZED
2. Check RFID ID: 322FADAB
3. Go to TalkBack
4. Add command: `ADD_CITIZEN:322FADAB`
5. Wait 15 seconds
6. Citizen taps card → Shows CITIZEN ✓

---

### Example 3: Remove Access (Employee Quits)
**Scenario:** Employee leaves company, revoke access

1. Go to TalkBack
2. Add command: `REMOVE:B3312B39`
3. Wait 15 seconds
4. Card is now unauthorized ✓

---

### Example 4: Check All Registered Cards
**Scenario:** Need to see who has access

1. Go to TalkBack
2. Add command: `LIST`
3. Check Serial Monitor
4. See complete list of all cards ✓

---

### Example 5: System Reset
**Scenario:** Starting new semester, reset all access

1. Go to TalkBack
2. Add command: `CLEAR`
3. All cards removed
4. Register new cards as needed ✓

---

## ⚠️ Important Notes

### Card ID Format
- Always use UPPERCASE (e.g., B3312B39, not b3312b39)
- No spaces in Card ID
- Include all characters exactly as shown

### Rate Limits
- ThingSpeak free account: 15 second minimum between requests
- ESP32 auto-checks every 15 seconds
- Don't add commands faster than ESP32 can process

### Command Execution
- Commands execute in order (Position 1, 2, 3...)
- Each command executes once then is deleted
- Check Serial Monitor to confirm execution

### Memory Storage
- Cards stored in ESP32 permanent memory (EEPROM)
- Survives power loss and resets
- Maximum 20 cards

---

## 🔍 Troubleshooting

### Problem: Command not executing
**Solution:**
- Check TalkBack API key in code is correct
- Wait at least 30 seconds
- Check Serial Monitor for error messages
- Verify command format (no typos)

### Problem: Card ID not showing in ThingSpeak
**Solution:**
- Field 4 shows "NaN" - this is normal
- Use HTML viewer instead to see card IDs
- Card IDs are stored as text, not numbers

### Problem: Card still shows UNAUTHORIZED after adding
**Solution:**
- Wait 15-30 seconds for auto-check
- Verify command was added correctly
- Check Serial Monitor for confirmation
- Try tapping card again

### Problem: Serial Monitor shows "Unknown command"
**Solution:**
- Check command spelling
- Ensure format matches exactly (ADD_EMPLOYEE:CARDID)
- No extra spaces before or after
- Use UPPERCASE for commands

---

## 📊 Monitoring Commands

### Via Serial Monitor
- Connect ESP32 to computer
- Open Serial Monitor (115200 baud)
- Watch for TalkBack check messages
- See real-time command execution

### Via HTML Viewer
- Open the HTML file in browser
- Auto-refreshes every 15 seconds
- See all card taps with access types
- Color-coded for easy reading

### Via ThingSpeak
- Go to channel 3153327
- View Field 3 (Access Type)
- View Field 4 (RFID ID - shows as NaN)
- Check timestamps

---

## 🎓 Quick Reference Card

| Task | Command | Example |
|------|---------|---------|
| Add Employee | `ADD_EMPLOYEE:CARDID` | `ADD_EMPLOYEE:B3312B39` |
| Add Citizen | `ADD_CITIZEN:CARDID` | `ADD_CITIZEN:322FADAB` |
| Remove Card | `REMOVE:CARDID` | `REMOVE:B3312B39` |
| List All Cards | `LIST` | `LIST` |
| Clear All Cards | `CLEAR` | `CLEAR` |

---

## 📞 Support

For issues or questions:
1. Check Serial Monitor for error messages
2. Verify command format
3. Ensure ESP32 is connected to WiFi
4. Check TalkBack commands queue

---

## 🔐 Security Notes

- Keep TalkBack API key private
- Only authorized personnel should have TalkBack access
- Regularly review registered cards using `LIST` command
- Use `CLEAR` command when needed for security reset

---

**Last Updated:** November 8, 2025
**Version:** 1.0
**System:** Flood Control RFID Access Management
