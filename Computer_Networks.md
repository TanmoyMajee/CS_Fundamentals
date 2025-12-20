

# OSI Model – Layer-wise Notes (Interview Ready)
>OSI stands for Open Systems Interconnection.
It is a theoretical 7-layer framework that explains how data travels in a network.
Used mainly for understanding, designing, and troubleshooting networks.

---

## 1️⃣ Application Layer (Layer 7)
- Closest to the user  
- Provides interface to applications like **browser, email, file transfer**  
- **Protocols:** HTTP, HTTPS, FTP, SMTP, DNS  
- Handles network services directly used by end-users  

**Interview Tip:**  
> This layer deals with user-level network operations.

---

## 2️⃣ Presentation Layer (Layer 6)
### Main responsibilities:
- **Translation** (converts data into machine readable  formats, e.g., ASCII ↔ UTF-8)  
- **Encryption & Decryption** (SSL/TLS works here)  
- **Compression & Decompression**  

**Interview Tip:**  
> It ensures the data is in a readable and secure format for communication.

---

## 3️⃣ Session Layer (Layer 5)
### Responsible for:
- Creating, managing, and terminating **sessions**
-  token management
- Keeping track of communication between sender & receiver  
- **Synchronization** (checkpoints to resume communication)  

**Simple Example:**  
- A **video call session** between two users

---

## 4️⃣ Transport Layer (Layer 4)
*(Most important layer for interviews)*

### Responsibilities:
- **Reliable data delivery**  
- **Segmentation** (break data into segments)  each segment contains a **sequence number** for proper reassembly)
- **Reassembly** (receiver merges segments)  
- **Error detection & recovery**  
- **Flow control**  

### Protocols:
- **TCP** → reliable, connection-oriented  
- **UDP** → fast, connectionless  

**Interview Tip:**  
> Ensures end-to-end communication.

---

## 5️⃣ Network Layer (Layer 3)
Handles **packet delivery across networks**.

### Major functions:
- **Routing:** finds the best path  
- **Logical addressing:** assigns IP addresses to the packets

**Devices:** Routers  
**Protocol:** IP  

**Interview Tip:**  
> Responsible for routing packets using IP addresses.

---

## 6️⃣ Data Link Layer (Layer 2)
Responsible for **node-to-node delivery**.

### Key functions:
- **MAC addressing**  
- **Framing**  
- **Error detection (CRC)**  
- **Collision handling (CSMA/CD)**  

**Devices:** Switches, Bridges  

**Interview Tip:**  
> Adds MAC address and converts packets into frames.

---

## 7️⃣ Physical Layer (Layer 1)
Handles **raw bit transmission** over physical media.

### Includes:
- Copper/Fiber Cables  
- Hubs  
- Electrical signals / Light pulses  

**Interview Tip:**  
> Deals with hardware and transmission of bits.


# OSI vs TCP/IP – Short Interview Table

| Feature | OSI Model | TCP/IP Model |
|--------|-----------|---------------|
| Purpose | Theoretical model for understanding & troubleshooting networks | Practical model used for real Internet communication |
| Number of Layers | **7 layers** | **4 layers** |
| Protocol Dependency | Protocol-independent | Protocol-specific (TCP, UDP, IP, HTTP, etc.) |
| Usage | Used for teaching & analysis | Used in real-world networking |
| Layer Separation | Layers are strictly defined | Some layers are combined |

# Layer Mapping (OSI → TCP/IP)

| OSI Layer(s) | TCP/IP Layer |
|--------------|--------------|
| **Application + Presentation + Session** | **Application Layer** |
| **Transport** | **Transport Layer** |
| **Network** | **Internet Layer** |
| **Data Link + Physical** | **Network Access Layer** |

# tcp vs udp 
# Ip -> v4 ,v6
# subnet
# public vs private IP
# MAC
# Diff device
# types - LAN , MAN , WAN  , PAN
# NAT / DHCP 
# vpn / dns

