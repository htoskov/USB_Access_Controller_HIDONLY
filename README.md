# USB Access Control (Windows)

A lightweight, open-source USB input monitoring and authorization tool for Windows designed to reduce the risk of opportunistic or low skill HID attacks (e.g., BadUSB / Rubber Ducky devices).

This project is not a full endpoint protection solution or a kernel-level security control. It is a user-space safeguard intended to raise the bar against **low skill unsophisticated HID injection attacks** by introducing a human approval step before new input devices are trusted.

---

## Security posture and threat model

This tool is designed with a **defensive, convenience-oriented threat model**, not a high-assurance or adversarial one.

It is effective against:
- Casual or automated HID attack tools
- “Plug-and-run” malicious keyboards/mice
- Drive-by HID payloads that rely on immediate execution

It is **not designed to stop**:
- Memory storage devices (USB Flash Drives)
- A determined attacker with local access
- Malware already running on the system
- Someone willing to invest time in bypassing or disabling protections
- Kernel-level or driver-based USB attacks

If you need strong, enterprise-grade USB control, you should look into:
- Microsoft Endpoint Manager (Intune) USB policies  
- Windows Defender Application Control  
- Third-party endpoint security platforms  

---

## What the program does

When running, the tray monitor:

- Starts automatically on user logon  
- Continuously monitors USB device insertions  
- Detects newly connected HID-class devices (keyboards, mice, and similar input devices)  
- Prompts the user with a GUI dialog when an unknown device appears  
- Allows the user to “whitelist” trusted devices  
- Stores the whitelist persistently and reuses it across sessions  

If a device is **not** whitelisted, the user will be prompted again the next time it is inserted.

---

## Password handling (important)

⚠️ **Current implementation uses a hardcoded password in the GUI.**

This is intentionally simple and should **not** be considered secure authentication. The current approach is only meant to prevent accidental or impulsive whitelisting.

A more appropriate design for real use would be to:

- Hash the password using a strong algorithm (e.g., bcrypt, scrypt, or Argon2)  
- Store only the password hash (never the plaintext password) in a protected location  
- Compare the entered password against the stored hash  
- Potentially integrate with Windows authentication instead of using a custom password  

Because of this, the current implementation should be treated as a **proof of concept / security aid**, not a hardened access control system.

---

## Limitations

- This is a **user-space** monitor, not a kernel driver.  
- It does **not** control or block USB storage devices.  
- It relies on Python and Windows APIs, which can be tampered with by malware.  
- A knowledgeable user or attacker can disable or bypass it with sufficient effort.  
- The current password mechanism is rudimentary and should be improved before any real-world deployment.

---

## Requirements

- Windows 10 or Windows 11  
- Python 3.10+  
- Required packages:

```bash
pip install pywin32 wmi
