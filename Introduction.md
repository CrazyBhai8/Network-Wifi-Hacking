### 💡 **Example Summary Table (like in `airodump-ng`):**

| BSSID             | PWR | Beacons | #Data | CH | MB | ENC  | CIPHER | AUTH | ESSID      |
| ----------------- | --- | ------- | ----- | -- | -- | ---- | ------ | ---- | ---------- |
| 00:14:6C:7E:40:80 | -42 | 543     | 234   | 6  | 54 | WPA2 | CCMP   | PSK  | MyHomeWiFi |

---
### 🛰️ **1. BSSID**

* **Meaning:** Basic Service Set Identifier
* **What it is:** It’s the **MAC address** (unique hardware address) of the **Wi-Fi access point** (the router).
* **Think of it like:** A unique ID tag for each router. Even if two routers have the same Wi-Fi name (SSID), their **BSSID will always be different**.
* **Example:**

  ```
  BSSID: 00:14:6C:7E:40:80
  ```

  → This means your device is seeing a router with that MAC address.

---

### ⚡ **2. PWR**

* **Meaning:** Signal Power (strength)
* **What it is:** Shows **how strong the Wi-Fi signal** is from your position. Usually measured in **dBm** (decibels per milliwatt).
* **Lower number = weaker signal**, because it’s a negative scale.
* **Example:**

  ```
  PWR: -30  → Very strong signal (close to the router)
  PWR: -80  → Weak signal (far away)
  ```

---

### 📡 **3. Beacons**

* **Meaning:** Periodic broadcast messages from an Access Point (AP)
* **What it is:** Wi-Fi routers send these small packets to **announce their presence** to nearby devices.
* **Think of it like:** “Hey, I’m here! This is my name, channel, and capabilities!”
* **Example:** If you see “Beacons: 543” → that means the router has broadcasted 543 beacon frames since you started scanning.

---

### 💾 **4. #Data**

* **Meaning:** Number of data packets captured from that Wi-Fi network.
* **What it is:** These are **actual communication packets** between clients (like phones/laptops) and the router.
* **The higher the number → more traffic captured**.
* **Example:**

  ```
  #Data: 234
  ```

  → You’ve captured 234 data packets so far.

---

### 🔢 **5. CH**

* **Meaning:** Channel
* **What it is:** The **frequency band** the Wi-Fi is using.
* Common channels: 1–13 (for 2.4GHz), and 36–165 (for 5GHz).
* **Example:**

  ```
  CH: 6 → The router is using channel 6
  ```

---

### ⚙️ **6. MB**

* **Meaning:** Maximum Speed (Megabits per second)
* **What it is:** The **maximum theoretical data rate** supported by that Wi-Fi.
* **Example:**

  ```
  MB: 54
  ```

  → This network supports speeds up to 54 Mbps (typical for 802.11g Wi-Fi).

---

### 🔒 **7. ENC**

* **Meaning:** Encryption type
* **What it is:** Shows **what encryption** the Wi-Fi network uses.
* Common types:

  * `WEP` (very old and weak 🔓)
  * `WPA` (better)
  * `WPA2` (strong)
  * `WPA3` (latest & most secure)
* **Example:**

  ```
  ENC: WPA2
  ```

---

### 🧩 **8. CIPHER**

* **Meaning:** The specific **encryption algorithm** used inside the ENC type.
* Common examples:

  * `CCMP` (used by WPA2 – strong)
  * `TKIP` (older, weaker)
* **Example:**

  ```
  CIPHER: CCMP
  ```

---

### 🔐 **9. AUTH**

* **Meaning:** Authentication method
* **What it is:** How devices verify themselves when connecting to Wi-Fi.
* Common types:

  * `PSK` (Pre-Shared Key — normal home Wi-Fi password)
  * `MGT` (Enterprise authentication — usually requires username/password)
* **Example:**

  ```
  AUTH: PSK
  ```

---

### 📶 **10. ESSID**

* **Meaning:** Extended Service Set Identifier
* **What it is:** The **name of the Wi-Fi network** — what you see when you open your phone’s Wi-Fi list.
* **Example:**

  ```
  ESSID: MyHomeWiFi
  ```

---

## client row

```
BSSID: 00:14:6C:7E:40:80
STATION: 88:53:2C:AA:11:7F
Rate: 54
Lost: 3
Frames: 342
Probe: MyHomeWiFi
```

Interpretation: Client `88:53:2C:AA:11:7F` is talking to AP `00:14:6C:7E:40:80` at ~54 Mbps, we saw 342 frames from it, missed 3 packets, and it probed for `MyHomeWiFi`.

---

### 🔹 BSSID

* **What it is:** The MAC address of the Access Point (the router) the client is talking to.
* **Why it matters:** Shows which AP a client is associated with (useful when multiple APs exist).
* **Example:** `BSSID: 00:14:6C:7E:40:80`

---

### 🔹 STATION

* **What it is:** The MAC address of the **client device** (phone, laptop, IoT device).
* **Why it matters:** Identifies which device is communicating with the AP.
* **Example:** `STATION: 88:53:2C:AA:11:7F`

---

### 🔹 Rate

* **What it is:** The **current link data rate** between the client and AP (in Mbps). Often shown as a single number like `6.0` or `54M`.
* **Why it matters:** Tells you how fast data can be sent/received at that moment (depends on signal quality, channel, and Wi-Fi standards).
* **Example:** `Rate: 54` → approx 54 Mbps (typical for 802.11g/legacy connection)

---

### 🔹 Lost

* **What it is:** A count (or indicator) of **packets that were not seen/acknowledged** from that client during the capture session.
* **Why it matters:** High “Lost” usually means poor connection quality, interference, or packet capture gaps.
* **Note:** Different tools or versions may compute this slightly differently — generally it signals missing/unreliable traffic.
* **Example:** `Lost: 12` → the capture missed or observed 12 packets marked as lost

---

### 🔹 Frames

* **What it is:** The **number of data frames (packets)** captured that are **from** that STATION (or sometimes to/from, depending on tool settings).
* **Why it matters:** Shows how active the device is — a lot of frames means heavy traffic.
* **Example:** `Frames: 342` → you’ve captured 342 frames involving that client

---

### 🔹 Probe

* **What it is:** **Probe requests** (SSIDs) the client is actively searching for — i.e., names of networks the device has asked about.
* **Why it matters:** Reveals which networks a device is looking for (useful for troubleshooting, or privacy considerations — devices sometimes "leak" remembered SSIDs).
* **Example:** `Probe: MyHomeWiFi, CoffeeShopNet` → the device sent probe requests for those network names

---

