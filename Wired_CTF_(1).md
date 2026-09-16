# WIRED CTF Writeups

> Writeups based on the steps I remember using. Exact commands or intermediate values are not included since I do not remember them.


## 1. DOUBLE_VISION

### Challenge Description

> Two images, one truth.

### Initial Analysis

I was given a ZIP (compressed archive) file containing two PNG (Portable Network Graphics) images of a mountain. At first glance, both images appeared to be identical.

### Approach

Based on the challenge description, I suspected that there was a hidden difference between the two images.

After closely examining both images, I noticed some faint and unclear text in the second image. It appeared to contain the flag, but the text was difficult to read accurately.

### Solution

I initially tried to extract the flag by examining the image visually. Although I was able to identify most of the characters, some of them were unclear.

To make the hidden text more visible, I searched for an online image-editing tool and found Photopea. I used its image-adjustment tools, including changes to colour, exposure, and threshold, to enhance the faint text.

After adjusting the image, the hidden text became much clearer, allowing me to recover the complete flag.

### Key Takeaways

- Similar-looking images can contain subtle hidden information.
- Careful visual inspection can reveal details that are not immediately visible.
- Adjusting image properties such as exposure and threshold can help reveal hidden text.






## 2. Tyler_Durden

### Challenge Description

> You are not your job. You are not your bank account. And you are definitely not your file extension.

### Initial Analysis

I was given a PDF file. I was not able to open the file. The challenge description specifically mentioned a file extension, which suggested that the extension itself might be misleading.

### Approach

I suspected that the file might actually be another type of file despite its extension.

### Solution

I changed the file extension to PNG and opened the resulting file as an image which had the file


### Key Takeaways

- A file extension does not necessarily represent the actual file format.
- The challenge description can provide a direct hint toward the intended approach.
- When a file behaves unexpectedly, checking whether its extension matches its actual format can be useful.




## 3. Metronome

### Challenge Description

> A WIRED field agent recovered this QR (Quick Response) code from a compromised server. Intel suggests it contains a hidden message — but whoever planted it didn't make it easy. Someone scrambled the payload before embedding it.
>
> The attacker was careful. No metadata, no trail, no obvious hiding spot. But they slipped up in one place — they had to leave the key somewhere, and they buried it in the rhythm of the code itself. Everything in a QR has a beat. A predictable, unbroken beat. Disturb it and you break the code. But what if the disturbance was the message?
>
> Scan it. Then look closer — not at the data, but at the pattern underneath it. Something that should be perfectly regular... isn't.

### Initial Analysis

I was given a PNG (Portable Network Graphics) image containing a QR (Quick Response) code.

At first, I tried scanning the QR (Quick Response) code normally, but it did not provide a useful result. The QR (Quick Response) code also looked suspicious because the finder patterns in the corners did not look completely normal.

I then used Python to inspect the image and determine the QR grid size. The QR (Quick Response) code was 29×29 modules.

### Approach

The challenge description repeatedly referred to the **rhythm** and **beat** of the QR (Quick Response) code.

This pointed towards the QR timing pattern. A normal QR (Quick Response) code contains timing patterns consisting of a predictable alternating sequence of black and white modules.

When I inspected the timing pattern, I noticed that it had been deliberately disturbed instead of following the normal alternating pattern.

This suggested that the timing-pattern modification was being used to hide information.

### Solution

I first restored the corrupted structural parts of the QR (Quick Response) code, including the finder and timing patterns, while preserving the data modules.

After repairing the QR structure, the QR (Quick Response) code could be decoded and produced a scrambled payload.

The challenge description indicated that the key was hidden in the disturbed timing pattern.

The key was `0x69`.

I XOR (exclusive OR)ed each byte of the decoded payload with `0x69`.

For example:

`1e XOR (exclusive OR) 69 = 77 = w`

`00 XOR (exclusive OR) 69 = 69 = i`

`1b XOR (exclusive OR) 69 = 72 = r`

`0c XOR (exclusive OR) 69 = 65 = e`

`0d XOR (exclusive OR) 69 = 64 = d`


Repeating this operation for the complete payload revealed the flag:

### Key Takeaways

- QR (Quick Response) codes contain structural patterns in addition to their encoded data.
- The timing pattern is normally highly predictable.
- A corrupted QR structure can prevent normal scanners from decoding the data.
- Structural analysis can reveal information hidden outside the actual QR payload.
- XOR (exclusive OR) encryption can be identified when a repeated key produces readable plaintext.



## 4. wired_loop

### Challenge Description

> A teammate built a small authentication utility before leaving the project, and the key it checks against was never written down anywhere. The binary still runs fine — it still knows the right answer — but nobody documented how it gets there.
>
> Reverse the binary to recover the flag.

### Initial Analysis

I was given an ELF (Executable and Linkable Format) executable named `wired_loop`.

I first inspected the binary and found that it was a stripped 64-bit Linux executable. Since there were no useful function names for the program logic, I examined the disassembly to understand how the authentication check worked.

### Approach

The binary contained several functions responsible for generating values and transforming data before checking the user input.

I noticed that the program initialized two internal values and repeatedly modified them using arithmetic and XOR (exclusive OR) operations. These values were then used to determine which of several stored byte arrays would be used.

The selected byte array was further modified inside a loop before being used for the final comparison.

### Solution

I followed the execution flow through the relevant functions and reconstructed the operations performed on the internal state.

The program first generated its internal state from:

`A = 0x1337`

`B = 0x42`

It then calculated an index to select one of four stored byte arrays.

The selected array was transformed byte-by-byte using arithmetic and XOR (exclusive OR) operations based on the byte index.

After reconstructing these operations, I obtained the expected plaintext used by the authentication check.


### Key Takeaways

- Stripped binaries can still be analyzed by following their control flow and instructions.
- Important values may be generated at runtime instead of being stored directly as strings.
- XOR (exclusive OR) and arithmetic transformations are common techniques used to obscure authentication strings.
- Following the data flow from initialization to the final comparison can reveal the expected input.




## 5. ESP-ionage-2.0

### Challenge Description

> Connect to the ESP-ionage Wi-Fi (Wireless Fidelity) and visit `192.168.4.1`. Not everything is where it seems.

### Initial Analysis

I connected to the Wi-Fi (Wireless Fidelity) network named `ESP-ionage` and opened the provided IP (Internet Protocol) address:

`192.168.4.1`

The page contained some text and an image. However, the image did not load normally.

### Approach

Since the image was not loading, I opened the image in a new browser tab to investigate it separately.

While examining the page/image, I found an encoded piece of information. I decoded it, and the decoded message pointed me towards:

`ghost`

This suggested that there was another hidden page or endpoint on the ESP-ionage web server.

### Solution

I navigated to:

`192.168.4.1/ghost`

This revealed another stage of the challenge.

At this point, I continued investigating the hidden page to find the remaining information needed to obtain the flag.

### Key Takeaways

- An image that fails to load normally may still contain useful information.
- Opening resources separately can reveal information that is not immediately visible on the main page.
- Encoded text can provide clues about hidden endpoints.
- The challenge description's phrase "not everything is where it seems" was an important hint to investigate beyond the main page.



## 6. Baud Boy

### Challenge Description

> Nobody knows his real name.
>
> The feds call him a criminal. The streets call him a legend.
> When asked if he was dangerous, he just shrugged.
>
> "I'm not a very good guy. I'm not a very bad guy. Fifty-fifty I think. I don't hurt people. I don't harm people."
> "I'm just a bad boy..... on the dance floor."
>
> What the dance floor means in hacker terms — nobody knows.
> What we DO know is that Baud Boy disappeared last Tuesday, leaving behind a single Arduino in his apartment, quietly transmitting into the void. No receiver. No documentation. Just a wire, pulsing data into silence.
>
> His farewell message? His location? His dance moves encoded in binary?
> Nobody knows.
>
> His device is on your desk. It's still transmitting.
> Figure it out before he hits the dance floor again.

### Initial Analysis

I was provided with an Arduino and a logic analyzer as the hardware for the challenge.

I installed **Logic 2** and connected the Arduino and logic analyzer to the PC (Personal Computer).

The logic analyzer was connected to the Arduino using a single female-to-male jumper wire.

### Approach

I opened Logic 2 and started checking the different channels from the Arduino to determine which channel was transmitting a signal.

After checking the channels, I found that **Channel 5** was transmitting data.

I then analyzed the signal using the **Async Serial** protocol analyzer in Logic 2.

Initially, the decoded output did not give useful information.

### Solution

The challenge title **"Baud Boy"** and the description's references to transmitting data suggested that the baud rate was important.

I realized that Async Serial needs to be configured with the correct baud rate in order to correctly decode the transmitted signal.

After determining the appropriate baud rate and configuring Async Serial with it, the signal was successfully decoded.

The decoded message revealed the flag.

### Key Takeaways

- A logic analyzer can be used to inspect digital signals from embedded devices.
- Checking each channel can help identify which pin is actively transmitting data.
- Async Serial decoding requires the correct communication parameters, especially the baud rate.
- The challenge title was an important clue toward the baud rate.
- When serial decoding produces meaningless output, checking the baud rate is one of the first things to investigate.




## 7. Do_you_have_what_it_got

### Challenge Description

> You work as Asia's biggest petroleum industry's senior cybersecurity analyst. You were reviewing the industry's control network traffic captured during a brief security alert. Thousands of packets show normal communication between controllers and field devices, making the traffic appear routine.
>
> But your juniors believe a single unauthorized transmission occurred during this period; are they right? Find the compromised packet without risking data leakage.

### Initial Analysis

I was given a network traffic capture file to investigate.

I opened the file using **Wireshark** and started examining the captured traffic for anything unusual.

### Approach

Since there were thousands of packets, manually checking every packet would not be efficient.

I used Wireshark's statistics to get an overview of the communicating hosts.

I navigated to:

`Statistics → Endpoints → IPv4`

This allowed me to examine the IPv4 addresses involved in the captured traffic and identify a suspicious IP (Internet Protocol) address.

### Solution

After identifying the suspicious IP (Internet Protocol) address, I investigated the packets associated with it.

I opened the relevant packet and found an encoded piece of text in the packet data.

I decoded the text, which revealed the flag.

### Key Takeaways

- Wireshark's **Statistics → Endpoints** feature is useful for quickly identifying hosts involved in network traffic.
- When analyzing a large packet capture, statistics can help narrow down suspicious traffic instead of inspecting every packet manually.
- Encoded data hidden inside packet contents can contain useful information in CTF challenges.
- Once suspicious traffic is identified, inspecting the packet contents can reveal the next step.




## 8. DumpMeV2

### Challenge Description

> An old ESP32 device has been discovered. It boots up, welcomes you with cryptic commands, and seems to have once belonged to a system that held important configuration data... or perhaps something more valuable.
>
> You don’t have access to its source code. All you can do is interact with it over the serial interface.
>
> “It speaks when spoken to, remembers what it once wrote. Seek not the voice, but the echo in its notes.”

### Initial Analysis

I was provided with an ESP8266 (Espressif 8266) NodeMCU development board as the hardware for the challenge.

The device communicated through a serial interface and displayed a prompt when connected.

### Approach

I connected the board to my PC (Personal Computer) and used the Arduino IDE to interact with the device.

The challenge description suggested that the device had some form of persistent memory. The phrase:

> “remembers what it once wrote”

made me suspect that the important information might be stored in EEPROM (Electrically Erasable Programmable Read-Only Memory) rather than being directly displayed through the serial interface.

I also installed the required EEPROM (Electrically Erasable Programmable Read-Only Memory)-related support in the Arduino environment and interacted with the device through the Serial Monitor.

### Solution

I communicated with the device through the Serial Monitor and investigated the information stored in its persistent memory.

The investigation revealed that the flag had been stored in the device's EEPROM (Electrically Erasable Programmable Read-Only Memory).

I then read the stored EEPROM (Electrically Erasable Programmable Read-Only Memory) data and recovered the flag.

### Key Takeaways

- Embedded devices can retain information in non-volatile memory even after restarting.
- Serial interfaces are useful for interacting with embedded devices and investigating their behaviour.
- When a challenge mentions that a device “remembers” something, persistent storage such as EEPROM (Electrically Erasable Programmable Read-Only Memory) is worth investigating.
- In embedded CTFs, the intended solution may involve examining the device's memory rather than only interacting with its visible interface.



## 9. BLEached_Secrets

### Challenge Description
> Someone is transmitting over the air. Find out the flag or do something to what you found with the precursor to it to get the content of the flag.

### Initial Analysis
The challenge suggested that something was being transmitted wirelessly. I initially tried searching for nearby Bluetooth devices and Wi-Fi (Wireless Fidelity) signals using the terminal, but I wasn't able to find the required device.

### Approach
I then switched to the nRF (Radio Frequency) Connect mobile app, which is useful for discovering and inspecting Bluetooth Low Energy (BLE) devices.

Using the app, I found a Bluetooth device named:

BLEACHED_SECRETS

I investigated the device and found a precursor along with two unknown characteristics.

### Solution
The two unknown characteristics appeared to contain encoded information. I extracted the relevant data and decoded it.

The decoded information revealed the flag.

### Key Takeaways
- Bluetooth Low Energy (BLE) devices may not always be easy to discover using standard terminal tools.
- Mobile BLE-scanning tools such as nRF (Radio Frequency) Connect can provide more detailed information about nearby Bluetooth Low Energy (BLE) devices.
- BLE characteristics can contain hidden or encoded data.
- When a challenge mentions a precursor, the discovered data may require an additional decoding step before revealing the final content.



## 10. The Unkn□wn Ratio

### Challenge Description
> The original encryption device has been lost, but a 3D model of its internal gearbox remains.
>
> The machine accepts an integer as input through the red axle. That value is transformed by the gearbox and appears at the blue axle as an encrypted output.
>
> A single encrypted value has been recovered, but the transformation ratio used by the machine is unknown.
>
> Your task is to reverse engineer the gearbox, determine the exact rotational relationship between the red input axle and the blue output axle, and use it to recover the original flag.
>
> Not all ciphers are made of code.

### Initial Analysis
I was given a `.dae` file containing a 3D model of the encryption device's internal gearbox.

I opened the model using a suitable converter/viewer and inspected the gear system. The model contained a **red input axle** and a **blue output axle**, with multiple gears connecting them.

### Approach
The main challenge was to determine the gear ratio between the red input axle and the blue output axle.

I traced the gear path from the red shaft through the connected gears until reaching the blue shaft. I calculated the gear ratios along the complete path and obtained an overall ratio of:

`1337:1`

This was the key to solving the challenge.

### Solution
The recovered encrypted value was based on the transformation performed by the gearbox.

Using the calculated ratio of `1337:1`, I divided the given encrypted number by `1337` to recover the original integer value.

The resulting number could then be decoded to obtain the flag.

### Key Takeaways
- 3D models can contain important information even when there is no source code.
- Gear teeth and shaft connections can be used to determine a mechanical transformation ratio.
- The complete gear train must be traced from the input shaft to the output shaft.
- The final gear ratio was `1337:1`, which was the crucial value needed to reverse the transformation.
- Mechanical systems can be used to implement transformations just like software-based ciphers.




## 11. The Final Broadcast

### Challenge Description
> The ECU transmitted one final broadcast before going silent.
>
> Engineers recovered the CAN (Controller Area Network) log after the shutdown, but the logger failed to preserve the arbitration order of frames transmitted simultaneously. What remains is a scrambled snapshot of the vehicle's final communication.
>
> The mechanics searched through VINs, DTCs, and sensor data, convinced the answer was buried somewhere in the diagnostics. They were wrong.
>
> The Debug ECU's last message was fragmented across multiple CAN frames and encoded before transmission. Reconstruct the original bus order, recover the complete broadcast, and decode it to reveal the flag.

### Initial Analysis
I was given a CAN (Controller Area Network) log containing multiple frames. The challenge mentioned that the original arbitration order was lost, so the frames had to be reordered before the complete message could be recovered.

The log contained CAN (Controller Area Network) IDs (Identifiers) along with hexadecimal data.

### Approach
I first converted the hexadecimal data from the CAN frames into readable text.

The challenge specifically mentioned that the arbitration order was lost. In CAN communication, arbitration is determined by the CAN identifier, with the lower-priority value winning arbitration first. Therefore, I used the CAN (Controller Area Network) IDs (Identifiers) to reconstruct the original frame order.

After sorting the relevant frames by their CAN (Controller Area Network) IDs (Identifiers), the data fragments formed a continuous encoded message.

Some frames such as `VIN_OK` and `ECU_OK` were diagnostic-looking distractions rather than part of the final encoded message.

### Solution
After removing the irrelevant diagnostic messages and reconstructing the frame order using the CAN (Controller Area Network) IDs (Identifiers), I concatenated the remaining decoded data fragments.

The resulting data contained a Base64-encoded portion. Decoding it revealed the flag:

`wired{c4n_y0u_h34r_m3}`

### Key Takeaways
- CAN (Controller Area Network) IDs (Identifiers) are important for determining arbitration priority.
- When CAN frames are provided out of order, the identifiers can help reconstruct their original order.
- Diagnostic-looking messages can be distractions in CTF challenges.
- Hexadecimal CAN payloads may need to be converted to text before identifying the next encoding layer.
- The final reconstructed message contained Base64-encoded data that revealed the flag.



## 12. Th3_Ch4mp_1s_H3r3

### Challenge Description
> The Champ is Here ♪ his time is now ♪
>
> Someone left you a message, but the display is nowhere to be found.
>
> All you have is what was left behind. Can you still find what was meant to be displayed?

### Initial Analysis
I was given a Saleae Logic capture file (`.sal`) instead of the actual LCD (Liquid Crystal Display) display.

The challenge hint suggested investigating how an LCD (Liquid Crystal Display) communicates over I2C (Inter-Integrated Circuit) and, specifically, how each character is transmitted using nibbles.

### Approach
I analyzed the captured digital signals as an **I2C (Inter-Integrated Circuit) communication** stream.

The important part was to focus on the I2C (Inter-Integrated Circuit) data rather than looking for an actual LCD (Liquid Crystal Display) display. The captured communication contained the information that would have been sent to the missing display.

Following the hint, I looked at how the LCD (Liquid Crystal Display) data was transmitted in **4-bit/nibble form** and reconstructed the characters from the I2C (Inter-Integrated Circuit) communication.

### Solution
By reconstructing the LCD (Liquid Crystal Display) character data from the captured I2C (Inter-Integrated Circuit) transmission, the message that would have appeared on the missing display could be recovered.

This revealed the flag.

### Key Takeaways
- A logic analyzer capture can contain the information sent to a missing hardware component.
- I2C (Inter-Integrated Circuit) communication can be analyzed even when the actual LCD (Liquid Crystal Display) is unavailable.
- Character data on common LCD (Liquid Crystal Display) interfaces may be transmitted as separate nibbles.
- Understanding the underlying communication protocol is more useful than relying on the physical display itself.




## 13. The Sleepy Mechanic

### Challenge Description
> A mechanic accidentally deleted the DBC (CAN database) file file while documenting the EngineSpeed signal of Skoda Octavia A5 2011. Fortunately, six observations were recovered.
>
> The recovered data contains CAN messages from multiple ECUs (Electronic Control Units). Several Data Identifiers (DIDs (Data Identifiers)) were queried during the session. One of them corresponds to the EngineSpeed signal.
>
> Using these observations, determine the CAN frame corresponding to an engine speed of 2500 RPM (Revolutions Per Minute).

### Initial Analysis
I was given six snapshots of CAN traffic on ID `77E`, each paired with a known dashboard RPM (Revolutions Per Minute) value (0, 1000, 1250, 1500, 2000, 3000 RPM (Revolutions Per Minute)). Every snapshot contained five separate UDS (Unified Diagnostic Services) `ReadDataByIdentifier` responses (service `0x62`), each querying a different DID: `0x220D`, `0x2205`, `0x220C`, `0x1014`, and `0xF40C`.

Since the DBC (CAN database) file file defining the EngineSpeed signal was "deleted," the task was to figure out — purely from the raw byte patterns across the six snapshots — which of these five DIDs (Data Identifiers) actually encoded engine speed, and how.

### Approach
I treated each response as a standard single-frame ISO-TP (ISO 15765-2 Transport Protocol) payload: the first byte is the PC (Personal Computer)I/length byte, followed by `62`, then the two DID bytes, then the actual data bytes, with the rest padded.

I extracted the data value for each DID across all six snapshots and checked whether it correlated with the known RPM (Revolutions Per Minute):

| DID | Values (0 → 3000 RPM (Revolutions Per Minute)) | Verdict |
|---|---|---|
| `0x220D` | 5565, 5465, 5565, 5465, 5065, 4065 | Doesn't track RPM (Revolutions Per Minute) monotonically → decoy |
| `0x2205` | 20, 21, 20, 21, 20, 21 | Just toggles → decoy |
| `0x220C` | 65, 66, 67, 68, 68, 69 | Slow, time-based increment → decoy |
| `0x1014` | 84, 85, 86, 87, 87, 88 | Same slow-increment pattern → decoy |
| **`0xF40C`** | 0000, 1388, 186A, 1D4C, 2710, 3A98 | Scales cleanly with RPM (Revolutions Per Minute) |

Converting the `0xF40C` values from hex to decimal and dividing by the known RPM (Revolutions Per Minute) gave a **constant ratio of 5** for every single snapshot (e.g. `1388` = 5000, and 5000 / 1000 RPM (Revolutions Per Minute) = 5). That consistency across all six data points confirmed this was the real EngineSpeed DID, with the scaling formula:

```
raw_value = RPM (Revolutions Per Minute) × 5
```

### Solution
With the formula and frame structure identified, I reconstructed the response format for `0xF40C`:

```
[PC (Personal Computer)I: 05][62][F4][0C][value_hi][value_lo][AA][55]
```

To find the frame for 2500 RPM (Revolutions Per Minute):

```
raw = 2500 × 5 = 12500 = 0x30D4
```

Plugging this into the same frame structure used across every other observation (same CAN (Controller Area Network) ID (Identifier) `77E`, same padding bytes `AA 55`) gives:

```
05 62 F4 0C 30 D4 AA 55
```

Formatted per the challenge's flag convention:

**Flag:** `wired{77E#0562F40C30D4AA55}`

### Key Takeaways
- When a DBC (CAN database) file is missing, a signal's identity can still be recovered by correlating multiple known reference points (here, dashboard RPM (Revolutions Per Minute)) against candidate raw values.
- Not every DID in a UDS (Unified Diagnostic Services) session is relevant — decoys that "look like" telemetry (counters, toggling flags, slow-changing values) are common and must be ruled out with real data, not assumptions.
- A consistent linear ratio across several independent data points is strong confirmation of a signal's scaling formula, even without documentation.
- Reconstructing the exact byte-for-byte frame structure (PC (Personal Computer)I, service ID, DID, padding) from working examples is essential to producing a valid, correctly formatted CAN frame for a target value.




## 14. The Boring Attack

### Challenge Description
> A rogue embedded systems engineer — going by the alias **NULL_PTR** — has locked critical intelligence behind a custom-built Arduino security module. The device is simple by design: four buttons, one secret.
>
> The lock is a custom Arduino-based combination module with exactly **4 input buttons** labelled A, B, C, and D. The secret sequence consists of **5 button presses**. Entering the correct sequence unlocks the device and prints the flag over Serial.
>
> Entering the wrong sequence resets the device.

### Initial Analysis
I was provided with two Arduino boards. One of them was the locked security device containing the password and flag.

The challenge also provided the source code, which helped identify how the button combinations were handled.

### Approach
Since the password consisted of 5 button presses and each press could be one of four buttons — `A`, `B`, `C`, or `D` — I decided to brute-force the possible combinations.

I used the second Arduino to write code that generated and tested the different button combinations.

### Solution
I connected the second Arduino to the locked device and used it to automatically try the possible combinations made from:

`A B C D`

The combinations were entered using the available **Enter** button.

Instead of manually trying combinations, the Arduino automated the process and iterated through the possible 5-button sequences.

Eventually, the correct combination was entered, the locked Arduino unlocked, and the flag was printed through the Serial interface.

### Key Takeaways
- Source code can reveal how an embedded security mechanism processes input.
- A short PIN or button sequence can be brute-forced when the search space is small.
- An Arduino can be used to automate repetitive physical input.
- Automating the attack is much more efficient than manually testing every combination.
- Embedded CTF challenges often combine programming with physical hardware interaction.







## 15. Whispers_of_SPIrit

### Challenge Description

> They thought it was dead. But the SPI bus kept pulsing. Something small. Something repeating. The engineers called it SPIrit — a final embedded echo trapped inside the SPI lines of a prototype controller. It's quiet, but it's not gone. Every few seconds, it transmits something — fragments, noise, maybe more. We call it SPIrit — the last trace of a forgotten firmware, quietly whispering across the wire.

The challenge involved discovering and capturing hidden communication transmitted through the SPI lines of an embedded controller.

### Initial Analysis

I was provided with two Arduino Nano boards and a logic analyzer. The challenge description suggested that the device was periodically transmitting data through an SPI bus.

Since SPI (Serial Peripheral Interface) communication uses separate lines for clock and data, I identified the standard SPI pins on the Arduino Nano:

* **D13 — SCK (Serial Clock) / Clock**
* **D11 — MOSI (Master Out, Slave In)**
* **D12 — MISO (Master In, Slave Out)**
* **D10 — SS / CS**
* **GND — Common ground**

I used **Logic 2** software to capture and analyze the signals.

### Approach

I connected the logic analyzer to the SPI pins of the Arduino Nano and configured Logic 2 to observe the communication.

The connections were:

| Logic Analyzer | Arduino Nano | Signal      |
| -------------- | ------------ | ----------- |
| CH0            | D13          | SCK (Serial Clock) / Clock |
| CH1            | D11          | MOSI (Master Out, Slave In)        |
| CH2            | D12          | MISO (Master In, Slave Out)        |
| CH3            | D10          | CS / SS     |
| GND            | GND          | Ground      |

I then started a capture in Logic 2 and observed the SPI waveform. The captured data contained a repeating message that appeared to be encoded rather than plain text.

### Solution

The captured message was:

```text
9Welcome to ROT13. The flag lies within:jverq{1_nz_on7z4a}
```

The important part was:

```text
jverq{1_nz_on7z4a}
```

The message indicated that **ROT13** should be used for decoding. ROT13 replaces each alphabetic character with the character 13 positions away in the alphabet.

Applying ROT13:

```text
jverq{1_nz_on7z4a}
        ↓
wired{1_am_bo7m4n}
```

The decoded flag was:

```text
wired{1_am_bo7m4n}
```

### Key Takeaways

* SPI (Serial Peripheral Interface) communication can be analyzed by observing its clock and data lines.
* A logic analyzer can capture communication between embedded devices without modifying their firmware.
* Logic 2 can be used to visualize waveforms and decode SPI data.
* Challenge descriptions may provide clues about the communication protocol or encoding.
* ROT13 is a simple substitution cipher that can be recognized and decoded when indicated by the captured message.
* Embedded CTF challenges often combine hardware connections, signal analysis, and basic cryptography.






## 16. Whisper_AP

### Challenge Description
> Something unusual is hiding in the air.
>
> A mysterious Wi-Fi (Wireless Fidelity) network named WHISPER_AP has been repeatedly broadcasting beacon frames on the wireless spectrum. At first glance, everything appears normal but the beacons are carrying something they were never meant to reveal.
>
> Your task is to capture the wireless traffic, inspect the beacon frames carefully, and uncover the message hidden within them.

### Initial Analysis
I used `airmon-ng` to put my Wi-Fi (Wireless Fidelity) adapter into monitor mode and then opened Wireshark to capture and inspect the wireless traffic.

There were many packets in the capture, so manually checking every packet was not practical.

### Approach
The challenge hints pointed towards **Information Elements (Information Elements (IEs))** inside 802.11 beacon frames, especially the **Vendor Specific Information Element**.

While inspecting the packets, I noticed that the Vendor Specific IE contained a recurring OUI (Organizationally Unique Identifier):

`12:34:56`

This indicated that the relevant information was being transmitted through the Vendor Specific fields of the beacon frames.

I then used Wireshark filters to narrow down the relevant WLAN beacon packets. After filtering, I found that **25 packets contained fragments of the flag**.

### Solution
I opened the relevant beacon packets and extracted the flag fragments from their Vendor Specific Information Elements.

The fragments were initially out of order, so I sorted the relevant packets chronologically and collected the fragments in their transmission order.

After combining the 25 fragments, the resulting data was still encoded and did not immediately produce readable text.

I then investigated the encoding and found that the data had been encrypted using an **XOR (exclusive OR) cipher**. I brute-forced the XOR (exclusive OR) key until the resulting plaintext became readable.

The decoded plaintext revealed the flag.

### Key Takeaways
- Wi-Fi (Wireless Fidelity) beacon frames can carry data beyond the information required for connecting to a network.
- 802.11 **Information Elements** are worth investigating when analyzing beacon frames.
- Vendor Specific Information Elements (IEs) can contain hidden challenge data.
- The OUI (Organizationally Unique Identifier) `12:34:56` was the key indicator for identifying the relevant packets.
- When data is fragmented across multiple packets, packet ordering is important.
- Encoded data may require another layer of analysis, such as XOR (exclusive OR) decoding, before the flag becomes readable.





## 17. beep_boop

### Challenge Description

> A mysterious signal has been intercepted from a rogue transmission source believed to be part of an underground radio communication network. You've been given a capture file of the transmission, recorded at 500 kHz.
>
> Note: The flag should be in small letters.

The challenge provided a raw IQ (In-phase and Quadrature) capture file (`beepboop.iq`) recorded from an SDR (Software-Defined Radio) at a sample rate of 500 kHz, suggesting the flag was hidden inside some form of radio-transmitted signal.

### Initial Analysis

Since the file was a raw IQ (In-phase and Quadrature) recording rather than plain audio, I needed a tool capable of interpreting SDR (Software-Defined Radio) capture formats directly. I used:

* **Universal Radio Hacker (URH (Universal Radio Hacker))** — for loading, interpreting, and demodulating the raw IQ (In-phase and Quadrature) signal
* **File size calculation** — to determine the correct IQ (In-phase and Quadrature) sample format before loading

I first checked the raw file size against the known sample rate (500 kHz) to figure out the correct IQ (In-phase and Quadrature) sample format:

### Approach

**Step 1 — Launch URH (Universal Radio Hacker)**

```bash
QT_QPA_PLATFORM=xcb urh
```

(The `QT_QPA_PLATFORM=xcb` override was needed to work around a Wayland/Qt plugin crash on launch.)

**Step 2 — Load the capture file**

* File → Open → selected `beepboop.iq`
* When prompted for the sample format, selected **Complex64 (Float32 I/Q)**

**Step 3 — Set the sample rate**

* In the **Interpretation** tab, set **Sample rate = 500000** (500 kHz, as stated in the challenge description)

**Step 4 — Inspect the signal**

* Switched to the **Analog view**
* The waveform showed clean, sharp transitions between full amplitude and near-zero amplitude — a textbook **On/Off Keying (OOK (On-Off Keying))** pattern, with no frequency shifting or phase modulation visible

**Step 5 — Autodetect modulation parameters**

* Clicked **Autodetect parameters** in the Interpretation panel
* URH (Universal Radio Hacker) correctly identified the modulation as **ASK (Amplitude Shift Keying)/OOK (On-Off Keying)** and estimated the bit length (samples per symbol)

**Step 6 — View the demodulated bitstream**

* Switched to the **Demodulated Signal / Analysis** view
* URH (Universal Radio Hacker) displayed the extracted signal as a series of pulses with two distinct on-durations and two distinct off-durations — **100 ms** and **300 ms** — rather than uniform fixed-width bits

This timing pattern (two pulse widths, two gap widths, no fixed bit clock) is the signature of **Morse code**, not a standard fixed-rate digital protocol:

* **on = 100 ms → dot (`.`)**
* **on = 300 ms → dash (`-`)**
* **off = 100 ms → gap between symbols in a letter**
* **off = 300 ms → gap between letters**

### Solution

Reading the pulse/gap widths off URH (Universal Radio Hacker)'s signal view and translating them against the Morse timing rules above produced the following Morse string:

```text
-.-- --- ..- ..--.- -.-. .-. .- -.-. -.- . -.. ..--.- - .... . ..--.- .-. ..-. ..--.- -- --- .-. ... . ..--.- -.-. --- -.. .
```

Decoding letter-by-letter (note: `..--.-` is the standard Morse representation for an underscore `_`):

```text
Y  O  U  _  C  R  A  C  K  E  D  _  T  H  E  _  R  F  _  M  O  R  S  E  _  C  O  D  E
```

Giving the message:

```text
YOU_CRACKED_THE_RF (Radio Frequency)_MORSE_CODE
```

Since the challenge specified the flag must be in lowercase, the final flag was:

```text
wired{you_cracked_the_rf_morse_code}
```

### Key Takeaways

* Raw IQ (In-phase and Quadrature) files require knowing (or deducing) the correct sample format — file size, sample rate, and expected duration can be cross-checked to identify it before loading into URH (Universal Radio Hacker).
* URH (Universal Radio Hacker)'s autodetect feature reliably identifies simple modulation schemes like ASK (Amplitude Shift Keying)/OOK (On-Off Keying) directly from the amplitude envelope.
* A clean, binary (non-analog) amplitude envelope with only two distinct pulse widths and two distinct gap widths is a strong indicator of Morse code timing rather than a fixed-rate digital protocol.
* Standard Morse code includes prosigns/extensions (like `..--.-` for underscore) beyond the basic alphanumeric table — worth checking a full reference table, not just letters and digits.
* RF (Radio Frequency)/SDR (Software-Defined Radio) challenges often layer a simple, human-decodable encoding (Morse, ROT13, etc.) on top of the harder signal-processing step, so once the signal is correctly demodulated, the final decode is often straightforward.








