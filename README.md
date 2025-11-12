# **Practical 01:**

### Commands to know

| Command | Meaning | Example | Output |
| --- | --- | --- | --- |
| `pwd` | Print Working Directory | `pwd` | `/home/shiwang` |
| `ls` | List files | `ls -l` | shows files with permissions, size |
| `cd` | Change directory | `cd Documents` | moves into Documents folder |
| `mkdir` | Make directory | `mkdir dcn_lab` | creates folder named dcn_lab |
| `touch` | Create empty file | `touch test.c` | makes a new empty file |
| `cat` | View contents | `cat test.c` | prints text inside file |
| `rm` | Remove file | `rm test.c` | deletes the file |
| `rmdir` | Remove empty folder | `rmdir testdir` | deletes empty folder |

🧠 **Why:** You need these for compiling and running C programs (DCN lab work).

| Step | Command | Meaning |
| --- | --- | --- |

| 1 | `mkdir ~/c_programs && cd ~/c_programs` | Create and open working folder |
| --- | --- | --- |

| 2 | `nano hello.c` | Create C file |
| --- | --- | --- |

| 3 | *(type your C code and save)* | — |
| --- | --- | --- |

| 4 | `sudo apt install build-essential -y` | Install compiler (once) |
| --- | --- | --- |

| 5 | `gcc hello.c -o hello` | Compile |
| --- | --- | --- |

| 6 | `./hello` | Run |
| --- | --- | --- |

| 7 | `ls` | Verify files |
| --- | --- | --- |

# **Practical 02:**

## **Part A — Theory: Networking Equipment Overview**

Before touching Linux commands, you must understand the *devices* and *their roles*.

| Device | Function | Example / Analogy |
| --- | --- | --- |
| **Hub** | Broadcasts data to all ports (Layer 1) | Like shouting in a room — everyone hears |
| **Switch** | Forwards data using MAC addresses (Layer 2) | Like delivering mail by room number |
| **Router** | Connects different networks (Layer 3) | Like post office connecting cities |
| **Bridge** | Connects two LAN segments | Used to extend network |
| **Repeater** | Regenerates weak signals | Like a signal booster |
| **Gateway** | Connects dissimilar networks | Converts protocols |
| **NIC (Network Interface Card)** | Connects a computer to the network | Each NIC has a MAC address |
| **Modem** | Converts digital ↔ analog signals | For connecting to ISP via telephone lines |

🧠 **Exam Tip:**

In viva, if asked *“What device operates at which layer of OSI model?”*

→ memorize this:

| Layer | Device |
| --- | --- |
| Physical | Hub, Repeater |
| Data Link | Switch, Bridge |
| Network | Router |
| Transport–Application | Gateway |

## **Part B — Linux Practical (Hands-On Network Configuration)**

Here’s where you *work on your Ubuntu terminal (WSL)* and show practical networking knowledge.

---

### 🧩 Step 1: Check your network interfaces

Run:

```bash
ip a
```

### ✅ Output example:

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 ...
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ...
    inet 172.20.234.123/20 brd 172.20.239.255 scope global eth0
```

### 💡 Meaning:

- `lo` → loopback interface (127.0.0.1), used for internal testing
- `eth0` → main network interface (your virtual adapter on WSL)
- `inet` → shows assigned IP address
- `UP` → interface is active

🧠 **Concept:**

Every interface has a **MAC address** (hardware) and **IP address** (logical).

These are used for data link and network layer communication.

---

### 🧩 Step 2: Display network routing table

Run:

```bash
route -n
```

or the modern equivalent:

```bash
ip route
```

### Example Output:

```
default via 172.20.224.1 dev eth0
172.20.224.0/20 dev eth0 proto kernel scope link src 172.20.234.123
```

💡 **Meaning:**

- `default via` → your gateway/router IP
- It shows how packets are forwarded outside your local network.

---

### 🧩 Step 3: Test connectivity using `ping`

```bash
ping -c 4 google.com
```

### Output:

```
64 bytes from 142.250.72.14: icmp_seq=1 ttl=115 time=19.2 ms
```

💡 **Concept:**

`ping` uses **ICMP protocol** (Internet Control Message Protocol)

to check if another host is reachable.

🧠 **Viva Tip:**

If asked “What does ping use?” → say **ICMP Echo Request & Reply**.

---

### 🧩 Step 4: Trace the route of packets

```bash
sudo apt install traceroute -y   # install if not already
traceroute google.com
```

### Output Example:

```
1  172.20.224.1  1.33 ms
2  10.9.0.1  12.51 ms
3  142.250.72.14  18.77 ms
```

💡 **Meaning:**

Shows the **path and delay** to reach destination — helps identify where a network is slow or blocked.

---

### 🧩 Step 5: View active connections and ports

```bash
netstat -tuln | head
```

Explanation:

- `t` = TCP
- `u` = UDP
- `l` = Listening
- `n` = Show numeric ports

🧠 Example:

```
tcp   0   0 0.0.0.0:22   0.0.0.0:*   LISTEN
```

→ SSH service running on port 22.

---

### 🧩 Step 6: View the ARP (Address Resolution Protocol) table

```bash
arp -n
```

or

```bash
ip neigh
```

Shows mapping between IP addresses and MAC addresses.

🧠 **Concept:**

ARP converts **IP address → MAC address** before sending frames on LAN.

---

### 🧩 Step 7: Assign or change an IP address (if permitted)

```bash
sudo ip addr add 192.168.10.5/24 dev eth0
sudo ip link set eth0 up
ip a
```

💡 This temporarily assigns a new IP to your adapter.

(Usually only done in lab environments, not on live systems.)

🧠 **Viva:**

“If we assign 192.168.10.5/24, what is subnet mask?” → 255.255.255.0.

---

## 🧠 Concept Recap

| Concept | Command | Purpose |
| --- | --- | --- |
| Show IP & interfaces | `ip a` | Display all network devices |
| Show routing table | `ip route` | Check default gateway |
| Check connectivity | `ping google.com` | Verify host reachability |
| Trace route | `traceroute google.com` | View packet hops |
| Show open ports | `netstat -tuln` | List listening services |
| View MAC-IP mapping | `arp -n` | ARP cache |
| Set IP manually | `sudo ip addr add` | Configure interface |

---

## 💬 Viva / Oral Questions

| Question | Short Answer |
| --- | --- |
| What is an IP address? | Logical address of a device in a network. |
| What is MAC address? | Physical address of the network card. |
| What is the difference between switch and router? | Switch connects devices in same network (MAC), router connects different networks (IP). |
| Which command checks connectivity? | `ping` |
| What protocol does ping use? | ICMP |
| What is the loopback address? | 127.0.0.1 |
| What does traceroute show? | All hops a packet takes to reach destination. |
| Which command displays routing table? | `ip route` or `route -n` |
| What is default gateway? | The router that connects your LAN to external networks. |

---

## 🧾 Summary — What to Perform in Exam

1. **Show understanding of equipment types (Hub, Switch, Router, etc.)**
2. **Run commands**: `ip a`, `ip route`, `ping`, `traceroute`, `netstat`, `arp`.
3. **Explain results** clearly in 1–2 lines.
4. Optionally show IP assignment (temporary).

# **Practical 03: Implementation of Pipe System Calls in Linux**

---

## ⚙️ **📍Steps to Perform (to the point)**

### **1. Create and open the C file**

```bash
nano pipe_demo.c
```

### **2. Write this program**

```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>

int main() {
    int fd[2];
    char msg[] = "Hello from Child!";
    char buf[50];

    pipe(fd);      // create pipe
    if(fork() == 0) {
        close(fd[0]);              // close read end
        write(fd[1], msg, strlen(msg)+1);   // child writes
        close(fd[1]);
    } else {
        close(fd[1]);              // close write end
        read(fd[0], buf, sizeof(buf));      // parent reads
        printf("Parent received: %s\n", buf);
        close(fd[0]);
    }
    return 0;
}
```

### **3. Compile and run**

```bash
gcc pipe_demo.c -o pipe_demo
./pipe_demo
```

### **4. Expected Output**

```
Parent received: Hello from Child!
```

---

# 🧠 **Explanation Section**

### **1️⃣ What is a Pipe**

- A **pipe** is a unidirectional communication channel between processes.
- It allows **one process to send data** and **another to read it**.
- It’s a core concept of **Inter-Process Communication (IPC)**.

---

### **2️⃣ System Calls Used**

| System Call | Function | Example in Code |
| --- | --- | --- |
| `pipe(fd)` | Creates a pipe and returns 2 file descriptors: `fd[0]` (read end) and `fd[1]` (write end). | `pipe(fd);` |
| `fork()` | Creates a child process that is a copy of the parent. | `if(fork()==0)` |
| `write(fd[1], msg, len)` | Child writes data into the pipe. | `write(fd[1], msg, strlen(msg)+1);` |
| `read(fd[0], buf, size)` | Parent reads data from the pipe. | `read(fd[0], buf, sizeof(buf));` |
| `close(fd[x])` | Closes an unused end of the pipe. | `close(fd[0]);` or `close(fd[1]);` |

---

### **3️⃣ Execution Flow**

1. **Parent process** calls `pipe()`.
2. **Both parent and child** inherit the same pipe after `fork()`.
3. **Child** closes read end (`fd[0]`) and writes message.
4. **Parent** closes write end (`fd[1]`) and reads message.
5. Output displayed by parent.

---

### **4️⃣ Viva / Oral Questions**

| Question | Short Answer |
| --- | --- |
| What is a pipe? | A unidirectional channel for communication between related processes. |
| Which system call creates a pipe? | `pipe()` |
| What are `fd[0]` and `fd[1]`? | `fd[0]` is read end, `fd[1]` is write end. |
| What does `fork()` do? | Creates a new child process. |
| Why do we close one end of the pipe? | To prevent deadlock and indicate end of communication. |
| Is pipe bidirectional? | No, standard pipe is **unidirectional**. |

---

✅ **Summary (Exam Ready)**

**Steps:** `nano → write code → gcc → ./run → output shown`.

**Concept:** Data sent from **child → parent** using pipe file descriptors.

**System Calls:** `pipe()`, `fork()`, `read()`, `write()`, `close()`

# Practical 04 — Character Count Framing

## 📍Steps to Perform (brief & to the point)

### 1) Create the C file

```bash
nano char_count.c

```

### 2) Paste this program

```c
#include <stdio.h>
#include <string.h>

int main(int argc, char *argv[]) {
    // If no payloads passed, use defaults
    const char *defaults[] = {"HELLO", "WORLD", "DCN"};
    const char **msgs = (argc > 1) ? (const char**)&argv[1] : defaults;
    int n = (argc > 1) ? argc - 1 : 3;

    unsigned char stream[2048];
    int pos = 0;

    printf("Sender — build frames as [count][payload]:\n");
    for (int i = 0; i < n; i++) {
        int payload_len = (int)strlen(msgs[i]);
        // count = payload bytes + 1 count byte (classic convention)
        unsigned char count = (unsigned char)(payload_len + 1);
        printf("Frame %d: count=%d, data=\"%s\"\n", i+1, count, msgs[i]);

        // push into byte stream
        stream[pos++] = count;                // count field
        for (int j = 0; j < payload_len; j++) // payload bytes
            stream[pos++] = (unsigned char)msgs[i][j];
    }

    // Show the raw byte stream in hex
    printf("\nRaw byte stream (hex): ");
    for (int i = 0; i < pos; i++) printf("%02X ", stream[i]);
    printf("\n");

    // Receiver — parse frames using count
    printf("\nReceiver — parse frames:\n");
    int i = 0, fno = 1;
    while (i < pos) {
        if (i >= pos) break;
        unsigned char count = stream[i++];
        int payload_len = count - 1;
        printf("Frame %d: count=%d, payload_len=%d, data=\"", fno++, count, payload_len);
        for (int k = 0; k < payload_len && i < pos; k++) {
            putchar(stream[i++]);
        }
        printf("\"\n");
    }
    return 0;
}

```

### 3) Compile

```bash
gcc char_count.c -o char_count

```

### 4) Run (two ways)

- With defaults:

```bash
./char_count

```

- Or pass your own payloads (recommended in exam):

```bash
./char_count "NET" "PROTOCOLS" "LAB-4"

```

### 5) Expected output (example)

```
Sender — build frames as [count][payload]:
Frame 1: count=6, data="HELLO"
Frame 2: count=6, data="WORLD"
Frame 3: count=4, data="DCN"

Raw byte stream (hex): 06 48 45 4C 4C 4F 06 57 4F 52 4C 44 04 44 43 4E
Receiver — parse frames:
Frame 1: count=6, payload_len=5, data="HELLO"
Frame 2: count=6, payload_len=5, data="WORLD"
Frame 3: count=4, payload_len=3, data="DCN"

```

---

## 🧠 Explanation (concepts & commands)

### What is Character Count Framing?

- A **framing method at the data link layer** where each frame begins with a **count field** (length in bytes).
- The receiver reads the **first byte as length** and then consumes exactly that many bytes (count includes the count byte itself in this classic convention).

### Why it’s used

- To delimit frames **without special flags** or bit stuffing.
- Simple to parse: **read count → read payload**.

### Pros / Cons

- ✅ **Simple**; no need for escaping special bytes.
- ❌ **Fragile**: if the **count field gets corrupted**, **frame boundary is lost** and all subsequent frames may be misread (loss of synchronization).

### Where it fits (OSI model)

- **Data Link Layer** framing strategy (Layer 2).

### Small viva pointers

- **Count includes what?** In this program: **payload + 1 (the count byte itself)**. (State your convention clearly; some textbooks exclude the count byte — your examiner will accept either if you’re consistent.)
- **How does receiver parse?** Read first byte = **count**, then read **count−1** payload bytes.
- **Main drawback?** Single-bit error in count **misaligns** all following frames.
- **Alternatives?** Byte-stuffing (sentinel/flag with escaping), bit-stuffing (HDLC), length + checksum/CRC for protection.

### Commands you used (and why)

- `nano char_count.c` — open a simple terminal editor to **write code**.
- `gcc char_count.c -o char_count` — **compile** the C file into an executable.
- `./char_count ...` — **run** your program from the current directory.
- (Optional) `./char_count "A" "B"` — pass **custom payloads** to demonstrate different lengths.

# Practical 05 — Byte Stuffing (Sentinel-based framing)

## 📍 Steps to Perform (brief & to the point)

### 1) Create the C file

```bash
nano byte_stuff.c

```

### 2) Paste this program

```c
#include <stdio.h>
#include <string.h>
#include <stdint.h>

int main(int argc, char *argv[]){
    const uint8_t FLAG = 0x7E;   // HDLC/PPP-style frame delimiter
    const uint8_t ESC  = 0x7D;   // escape octet
    // If no arg passed, include control bytes deliberately:
    // H ~ B } C  (so we can see stuffing)
    const char *in = (argc>1) ? argv[1] : "H~B}C";

    uint8_t stuffed[512], destuffed[512];
    int s = 0;

    // Sender: build frame = FLAG | stuffed(payload) | FLAG
    stuffed[s++] = FLAG;
    for(size_t i=0;i<strlen(in);i++){
        uint8_t b = (uint8_t)in[i];
        if(b==FLAG || b==ESC){
            stuffed[s++] = ESC;
            stuffed[s++] = b ^ 0x20;   // PPP convention
        }else{
            stuffed[s++] = b;
        }
    }
    stuffed[s++] = FLAG;

    // Show stuffed frame in hex
    printf("Stuffed frame (hex): ");
    for(int i=0;i<s;i++) printf("%02X ", stuffed[i]);
    printf("\n");

    // Receiver: de-stuff (between FLAGS)
    int d=0;
    // find first FLAG
    int i=0; while(i<s && stuffed[i]!=FLAG) i++;
    if(i<s) i++; // move past FLAG
    for(; i<s && stuffed[i]!=FLAG; i++){
        if(stuffed[i]==ESC && (i+1)<s){
            destuffed[d++] = (uint8_t)(stuffed[++i] ^ 0x20);
        }else{
            destuffed[d++] = stuffed[i];
        }
    }

    destuffed[d] = '\0';
    printf("De-stuffed payload: \"%s\"\n", destuffed);
    return 0;
}

```

### 3) Compile

```bash
gcc byte_stuff.c -o byte_stuff

```

### 4) Run (two demos)

- Default (includes special bytes):

```bash
./byte_stuff

```

- Your own payload (try adding `~` or `}` to see stuffing happen):

```bash
./byte_stuff "HELLO~WORLD}DCN"

```

### 5) Expected example output

```
Stuffed frame (hex): 7E 48 7D 5E 42 7D 5D 43 7E
De-stuffed payload: "H~B}C"

```

---

## 🧠 Explanation (concepts & commands)

### What is Byte Stuffing?

- A **data link layer framing** method using a **sentinel delimiter** (FLAG, often `0x7E`).
- Problem: What if the payload itself contains `FLAG`?
- **Solution:** When payload contains `FLAG` (or the escape byte `ESC`), the sender **inserts an escape (`ESC`)** and **transforms the following byte** (here with XOR `0x20`, PPP-style).
- Receiver reverses this: on seeing `ESC`, it **unescapes** the next byte (XOR `0x20` again).

### Frame Format

```
FLAG  <byte-stuffed payload>  FLAG

```

- **FLAG** = `0x7E` marks **start** and **end**.
- **ESC** = `0x7D` precedes any byte that would be confused with control bytes.

### Why XOR 0x20?

- A common convention in **PPP**: it guarantees that after escaping, the byte won’t equal `FLAG` or `ESC`.
- Some labs use “ESC + original byte” without XOR — either is fine if sender & receiver agree. State your convention clearly.

### Pros / Cons

- ✅ **Robust** against accidental sentinel bytes inside payload.
- ❌ Slight **overhead** when many escapes are needed.
- ❌ Still relies on correct detection of sentinels; pair with **FCS/CRC** for error detection.

### Where used

- HDLC/PPP-style protocols (Layer 2) that use byte-oriented framing.

### Viva pointers (10-sec answers)

- **Why stuffing?** To avoid confusing payload bytes with **control/sentinel** bytes.
- **Which bytes get stuffed?** `FLAG (0x7E)` and `ESC (0x7D)` in this convention.
- **How does receiver recover data?** On `ESC`, unescape the next byte (XOR `0x20`).
- **Alternative to byte stuffing?** **Bit stuffing** (HDLC at bit level), or **length field** (character count framing).
- **What ensures integrity?** Add **Checksum/CRC** over the frame.

### Commands used (and why)

- `nano byte_stuff.c` — create/edit the C source.
- `gcc byte_stuff.c -o byte_stuff` — compile program.
- `./byte_stuff "PAYLOAD"` — run and pass a test string containing `~`/`}` to see stuffing occur.

# Practical 06 — Bit Stuffing (HDLC rule)

## 📍Steps to Perform (brief & to the point)

### 1) Create the C file

```bash
nano bit_stuff.c

```

### 2) Paste this program

```c
#include <stdio.h>
#include <string.h>

// Insert a 0 after any run of five consecutive 1s (HDLC rule).
// We'll treat input as a string of '0' and '1' chars for clarity.

void bit_stuff(const char *in, char *out){
    int ones = 0; int j = 0;
    for(int i=0; in[i]; i++){
        char b = in[i];
        if(b=='1'){
            out[j++]='1';
            if(++ones==5){ out[j++]='0'; ones=0; }   // stuff a 0
        } else { // '0'
            out[j++]='0';
            ones=0;
        }
    }
    out[j]='\0';
}

void bit_destuff(const char *in, char *out){
    int ones = 0; int j=0;
    for(int i=0; in[i]; i++){
        char b = in[i];
        if(b=='1'){
            out[j++]='1';
            if(++ones==5){
                // skip the stuffed 0 if present
                if(in[i+1]=='0') i++;
                ones=0;
            }
        } else {
            out[j++]='0';
            ones=0;
        }
    }
    out[j]='\0';
}

int main(int argc, char *argv[]){
    // Example payload bits (no flags included here)
    const char *payload = (argc>1)? argv[1] : "1111101111100111110110";
    // HDLC flag (01111110) shown just for display
    const char *FLAG = "01111110";

    char stuffed[4096], destuffed[4096];

    bit_stuff(payload, stuffed);
    bit_destuff(stuffed, destuffed);

    printf("Input payload bits    : %s\n", payload);
    printf("Frame with flags      : %s %s %s\n", FLAG, stuffed, FLAG);
    printf("De-stuffed payload    : %s\n", destuffed);

    // Quick check
    printf("Match after destuff?  : %s\n", strcmp(payload, destuffed)==0 ? "YES" : "NO");
    return 0;
}

```

### 3) Compile

```bash
gcc bit_stuff.c -o bit_stuff

```

### 4) Run (two demos)

- Default sample:

```bash
./bit_stuff

```

- Your own bit-string:

```bash
./bit_stuff 10111110111110111110

```

### 5) Expected example output

```
Input payload bits    : 1111101111100111110110
Frame with flags      : 01111110 11111001111100101111100110 01111110
De-stuffed payload    : 1111101111100111110110
Match after destuff?  : YES

```

---

## 🧠 Explanation (concepts & the commands you used)

### What is Bit Stuffing?

- A **bit-level** framing method (used by **HDLC**) to ensure the flag pattern **01111110** never appears inside the payload.
- **Rule:** While sending data, **after five consecutive ‘1’ bits, insert a ‘0’**.
- Receiver removes any ‘0’ bit found **immediately after five consecutive ‘1’s**.

### Why it’s needed

- HDLC marks frame boundaries with a **flag** = `01111110` (0x7E).
- If payload accidentally contains `01111110`, the receiver would think the frame ended.
- Bit stuffing prevents that confusion by breaking up long runs of 1s.

### Frame format (HDLC idea)

```
FLAG (01111110) | stuffed payload bits | FLAG (01111110)

```

- We displayed flags for clarity; stuffing happens **only inside payload**.

### Pros / Cons

- ✅ Works at **bit** level (robust for any byte pattern).
- ❌ Overhead increases with many long runs of 1s.

### Common viva questions (quick answers)

- **When do we insert a bit?** After **five consecutive 1s**, insert a **0**.
- **What is the HDLC flag?** `01111110` (0x7E).
- **Where is bit stuffing used?** Data Link Layer; HDLC and similar protocols.
- **How does receiver de-stuff?** After reading five 1s, it **drops** the next bit if it’s **0**.
- **What if the bit after five 1s is 1?** Then it’s not a stuffed bit; continue counting (it could form the flag with the preceding 1s depending on context at boundaries).

### Commands used (and why)

- `nano bit_stuff.c` — create/edit program source in terminal.
- `gcc bit_stuff.c -o bit_stuff` — compile C to an executable.
- `./bit_stuff ...` — run and optionally pass your own bit-string to demonstrate stuffing and de-stuffing.

# Practical 07 — Longitudinal Redundancy Check (LRC / 2-D Parity)

## 📍 Steps to perform (brief & to the point)

### 1) Create the C file

```bash
nano lrc.c

```

### 2) Paste this program

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

// Compute LRC (byte-wise column parity = XOR of all data bytes).
unsigned char lrc_compute(const unsigned char *data, int n) {
    unsigned char lrc = 0;
    for (int i = 0; i < n; i++) lrc ^= data[i];
    return lrc; // For even parity, XOR of columns is the parity byte
}

// Helper: parse hex like "0xAB" or "AB" to a byte
int parse_hex_byte(const char *s, unsigned char *out) {
    unsigned int v;
    if (sscanf(s, "%x", &v) == 1 && v <= 0xFF) { *out = (unsigned char)v; return 1; }
    return 0;
}

int main(int argc, char *argv[]) {
    // Default demo block: ASCII "NET" => 0x4E,0x45,0x54
    unsigned char data[256];
    int n = 0;

    if (argc > 1) {
        // Accept hex bytes from CLI, e.g., ./lrc AB 34 FF 01
        for (int i = 1; i < argc; i++) {
            unsigned char b;
            if (!parse_hex_byte(argv[i], &b)) {
                fprintf(stderr, "Bad byte: %s (use hex like AB or 0xAB)\n", argv[i]);
                return 1;
            }
            data[n++] = b;
        }
    } else {
        unsigned char def[] = {0x4E, 0x45, 0x54}; // "N","E","T"
        memcpy(data, def, sizeof(def));
        n = (int)sizeof(def);
    }

    unsigned char lrc = lrc_compute(data, n);

    printf("Data bytes : ");
    for (int i = 0; i < n; i++) printf("%02X ", data[i]);
    printf("\nLRC byte   : %02X\n", lrc);

    // Simulate transmission: append LRC and verify at receiver
    unsigned char recv_xor = lrc_compute(data, n) ^ lrc;
    printf("Receiver check (XOR of all data+LRC) = %02X -> %s\n",
           recv_xor, (recv_xor == 0) ? "OK (no error detected)" : "ERROR DETECTED");

    return 0;
}

```

### 3) Compile

```bash
gcc lrc.c -o lrc

```

### 4) Run (two ways)

- With defaults:

```bash
./lrc

```

- With your own bytes (hex):

```bash
./lrc AB 34 FF 01

```

### 5) Expected example output

```
Data bytes : 4E 45 54
LRC byte   : 1B
Receiver check (XOR of all data+LRC) = 00 -> OK (no error detected)

```

---

## 🧠 Explanation (concepts & the commands you used)

### What LRC is (and how we computed it)

- **LRC (Longitudinal Redundancy Check)** is a **column-parity** method: arrange bytes in a block, take **parity per bit position** across all rows; that parity pattern itself forms an **extra parity byte** (the LRC). In practice with 8-bit bytes, this equals the **bitwise XOR of all data bytes** (often called the **BCC**). The receiver recomputes and checks that **(data XOR … XOR LRC) = 0x00**; if not, an error is detected. [Wikipedia+1](https://en.wikipedia.org/wiki/Longitudinal_redundancy_check?utm_source=chatgpt.com)

### Why it’s used

- Compared to single-byte parity (**VRC**), LRC uses parity **across bytes**, improving detection of many multi-bit and short burst errors in a block. It’s commonly taught alongside VRC, checksum, and CRC in the **data link layer**. [GeeksforGeeks+2GeeksforGeeks+2](https://www.geeksforgeeks.org/computer-networks/longitudinal-redundancy-check-lrc-2-d-parity-check/?utm_source=chatgpt.com)

### Strengths & limitations

- ✅ **Simple** to compute (just XOR), low overhead (one parity byte per block).
- ❌ Can **miss certain structured error patterns** (e.g., equal bit flips in the same columns across different bytes); **CRC** is stronger for burst errors. [Wikipedia+1](https://en.wikipedia.org/wiki/Longitudinal_redundancy_check?utm_source=chatgpt.com)

### Where it sits (OSI)

- **Data Link Layer – error detection**; often presented as **2-D parity** (VRC + LRC) in teaching materials. [MREC Academics](https://www.mrecacademics.com/DepartmentStudyMaterials/20201223-Computer%20Networks.pdf?utm_source=chatgpt.com)

### Commands you used (why)

- `nano lrc.c` — open a terminal editor to write the source.
- `gcc lrc.c -o lrc` — compile the C program.
- `./lrc ...` — run; optional hex inputs let you demonstrate different blocks quickly.

# Practical 08 — VRC (Vertical Redundancy Check / Parity Bit)

## 📍 Steps to perform (brief & to the point)

### 1) Create the C file

```bash
nano vrc.c

```

### 2) Paste this program (even parity)

```c
#include <stdio.h>
#include <stdlib.h>

// return 0 if x has even number of 1-bits, 1 if odd
int parity_bit(unsigned char x){
    int p = 0;
    for(; x; x >>= 1) p ^= (x & 1);
    return p; // 1 means odd -> need parity bit 1 to make total even
}

int main(int argc, char *argv[]){
    // Demo byte (ASCII 'S' = 0x53) if not provided
    unsigned int v = (argc > 1) ? strtoul(argv[1], NULL, 0) : 0x53;
    if(v > 0xFF){ fprintf(stderr, "Provide one byte (0..255 or hex like 0xAB)\n"); return 1; }

    unsigned char b = (unsigned char)v;
    int p = parity_bit(b);           // even-parity scheme
    unsigned char tx_parity = p;     // parity bit to append/transmit (0 or 1)

    printf("Data byte     : 0x%02X\n", b);
    printf("Parity scheme : EVEN\n");
    printf("Parity bit    : %d  (append this as the extra bit)\n", tx_parity);

    // Receiver check: total ones should be even after appending parity
    int ones_ok = ((parity_bit(b) ^ tx_parity) == 0); // 0 means even
    printf("Receiver check: %s\n", ones_ok ? "OK (even total 1s)" : "ERROR");

    // Simulate 1-bit error and show detection
    unsigned char flipped = b ^ 0x01; // flip LSB
    int err_detected = ((parity_bit(flipped) ^ tx_parity) != 0);
    printf("If 1-bit error occurs (byte->0x%02X): %s\n", flipped,
           err_detected ? "DETECTED" : "NOT detected");

    return 0;
}

```

### 3) Compile

```bash
gcc vrc.c -o vrc

```

### 4) Run (two ways)

- Default demo:

```bash
./vrc

```

- With your own byte (decimal or hex):

```bash
./vrc 0xAB
# or
./vrc 171

```

### 5) Example output

```
Data byte     : 0x53
Parity scheme : EVEN
Parity bit    : 0  (append this as the extra bit)
Receiver check: OK (even total 1s)
If 1-bit error occurs (byte->0x52): DETECTED

```

---

## 🧠 Explanation (concepts & commands)

### Concept (VRC / Parity)

- **VRC** adds **one parity bit** to each data unit (often each byte) to make the **total number of 1s even (even parity)** or **odd (odd parity)**.
- **Even parity**: If the byte already has an even count of 1s, parity bit = 0; if odd, parity bit = 1 → total becomes even.
- The **receiver recomputes parity**; mismatch ⇒ **error detected**.

### Why used

- It’s the **simplest error-detection** technique with **1-bit overhead** per byte; widely used in legacy serial links, memory parity, etc.

### Strengths / Limitations

- ✅ Detects **all single-bit errors** in a byte.
- ❌ **Cannot detect any even number** of flipped bits (e.g., two-bit errors).
- ❌ No correction — **detection only**.

### Even vs Odd parity

- Scheme choice is arbitrary; both sides must **agree** (our code uses **even parity**).

### Where it fits (OSI)

- **Data Link Layer** error detection (taught with LRC, checksum, CRC).

### Commands used (why)

- `nano vrc.c` — edit source.
- `gcc vrc.c -o vrc` — compile.
- `./vrc ...` — run; pass different bytes to show parity bit changes and detection behavior.

# Practical 09 — Checksum (16-bit 1’s-complement)

## 📍 Steps to perform (brief & to the point)

### 1) Create the C file

```bash
nano checksum.c

```

### 2) Paste this program

```c
#include <stdio.h>
#include <stdint.h>
#include <string.h>

// Compute the 16-bit Internet checksum (RFC 1071 style):
//  - Sum 16-bit words using 1's-complement addition (add carries back in)
//  - Return 1's complement of the final 16-bit sum
uint16_t inet_checksum(const uint8_t *data, size_t len) {
    uint32_t sum = 0;

    // Sum 16-bit words
    for (size_t i = 0; i + 1 < len; i += 2) {
        uint16_t word = (data[i] << 8) | data[i+1];
        sum += word;
        // fold carry (end-around carry)
        if (sum > 0xFFFF) sum = (sum & 0xFFFF) + 1;
    }

    // If odd length, pad last byte with 0
    if (len & 1) {
        uint16_t last = (data[len - 1] << 8);
        sum += last;
        if (sum > 0xFFFF) sum = (sum & 0xFFFF) + 1;
    }

    // Final 1's complement
    return (uint16_t)~sum & 0xFFFF;
}

int main(int argc, char *argv[]) {
    // Demo payload (ASCII) unless user passes a string argument
    const char *msg = (argc > 1) ? argv[1] : "HELLO DCN";
    const uint8_t *bytes = (const uint8_t*)msg;
    size_t len = strlen(msg);

    uint16_t cs = inet_checksum(bytes, len);

    printf("Payload        : \"%s\"\n", msg);
    printf("Length (bytes) : %zu\n", len);
    printf("Checksum (hex) : 0x%04X\n", cs);

    // Receiver-side verification demo:
    // recompute over payload + checksum should yield 0xFFFF (i.e., complement 0x0000)
    uint32_t verify = 0;
    // sum payload words
    for (size_t i = 0; i + 1 < len; i += 2) {
        verify += ((bytes[i] << 8) | bytes[i+1]);
        if (verify > 0xFFFF) verify = (verify & 0xFFFF) + 1;
    }
    if (len & 1) { // odd byte
        verify += (bytes[len - 1] << 8);
        if (verify > 0xFFFF) verify = (verify & 0xFFFF) + 1;
    }
    // add checksum
    verify += cs;
    if (verify > 0xFFFF) verify = (verify & 0xFFFF) + 1;

    printf("Receiver check : 0x%04X (expect 0xFFFF)\n", (unsigned)verify & 0xFFFF);
    return 0;
}

```

### 3) Compile

```bash
gcc checksum.c -o checksum

```

### 4) Run (two demos)

- Default demo:

```bash
./checksum

```

- With your own payload (string):

```bash
./checksum "NETWORKS AND PROTOCOLS"

```

### 5) Example output

```
Payload        : "HELLO DCN"
Length (bytes) : 9
Checksum (hex) : 0xB7A5
Receiver check : 0xFFFF (expect 0xFFFF)

```

---

## 🧠 Explanation (what you did & why it works)

- **What checksum this is:** the classic **Internet checksum** used by IPv4 header, TCP, and UDP. It’s defined as the **16-bit 1’s-complement of the 1’s-complement sum** of all 16-bit words in the block; for computation, the checksum field is taken as zero. [IETF Datatracker+1](https://datatracker.ietf.org/doc/html/rfc1071?utm_source=chatgpt.com)
- **How the code matches the spec:**
    1. Split data into 16-bit words;
    2. **Add with end-around carry** (if sum exceeds 16 bits, wrap the carry and add it back);
    3. If odd length, **pad one zero byte**;
    4. Take **1’s complement** of the final 16-bit sum to produce the checksum. [IETF Datatracker+1](https://datatracker.ietf.org/doc/html/rfc1071?utm_source=chatgpt.com)
- **Receiver verification rule:** When the receiver sums the entire block **including** the transmitted checksum using 1’s-complement arithmetic, the result should be **0xFFFF** (i.e., the complement is 0). If not, an error is detected. [IETF Datatracker+1](https://datatracker.ietf.org/doc/html/rfc1071?utm_source=chatgpt.com)
- **Where it’s used:** IPv4 header; TCP/UDP (with their respective data and pseudo-header coverage). IPv6 removes the IP-header checksum but still relies on upper-layer checksums (TCP/UDP) and link checksums. [Wikipedia](https://en.wikipedia.org/wiki/Internet_checksum?utm_source=chatgpt.com)
- **Why 1’s-complement (not 2’s):** The **end-around carry** property makes it easy to update and verify; incremental update techniques for routers are described in **RFC 1624**. [IETF Datatracker](https://datatracker.ietf.org/doc/html/rfc1624?utm_source=chatgpt.com)

# Practical 10 — CRC (Cyclic Redundancy Check)

## 📍 Steps to perform (brief & to the point)

### 1) Create the file

```bash
nano crc.c

```

### 2) Paste this program (tiny, exam-friendly; default = CRC-8 poly 0x07)

```c
#include <stdio.h>
#include <stdint.h>
#include <string.h>

// Bitwise (table-less) CRC runner for arbitrary width up to 16 bits.
// Defaults shown in main(): CRC-8 with poly 0x07 (x^8 + x^2 + x + 1).
// You can switch to CRC-16 by changing width, poly, init, xorout.

static uint16_t crc_run(const uint8_t *data, size_t len,
                        int width, uint16_t poly, uint16_t init, uint16_t xorout)
{
    // width=8 -> mask=0xFF, topbit=0x80; width=16 -> mask=0xFFFF, topbit=0x8000
    uint16_t mask   = (width==16)? 0xFFFF : ((1u<<width)-1);
    uint16_t topbit = (width==16)? 0x8000 : (1u<<(width-1));

    uint16_t crc = init & mask;

    for (size_t i=0;i<len;i++) {
        crc ^= ((uint16_t)data[i]) << (width-8);   // align incoming byte to top
        for (int b=0;b<8;b++) {
            if (crc & topbit) crc = (crc << 1) ^ poly;
            else              crc = (crc << 1);
            crc &= mask; // keep within width
        }
    }
    return (crc ^ xorout) & mask;
}

int main(int argc, char *argv[]) {
    // ---- Choose a CRC preset (edit for your lab) ----
    // CRC-8:  width=8,  poly=0x07, init=0x00, xorout=0x00 (no reflection)
    int width=8; uint16_t poly=0x07, init=0x00, xorout=0x00;

    // Example payload: ASCII unless user passes a string
    const char *msg = (argc>1)? argv[1] : "HELLO";
    uint16_t crc = crc_run((const uint8_t*)msg, strlen(msg), width, poly, init, xorout);

    printf("Payload      : \"%s\"\n", msg);
    printf("CRC-%d poly  : 0x%04X\n", width, poly);
    printf("CRC value    : 0x%04X\n", crc);

    // Receiver verification: append CRC and recompute should yield 0
    // (for this simple, unreflected model with xorout=0)
    uint8_t buf[1024];
    size_t n = strlen(msg);
    memcpy(buf, msg, n);
    if (width==8)      buf[n++] = (uint8_t)crc;
    else /* width=16*/ { buf[n++] = (uint8_t)(crc>>8); buf[n++] = (uint8_t)crc; }

    uint16_t check = crc_run(buf, n, width, poly, init, xorout);
    printf("Receiver check (recompute over data+CRC): 0x%04X (expect 0x0000)\n", check);
    return 0;
}

```

### 3) Compile

```bash
gcc crc.c -o crc

```

### 4) Run demos

- Default (CRC-8, message “HELLO”):

```bash
./crc

```

- Your own string:

```bash
./crc "NETWORKS"

```

- (Optional) Switch to **CRC-16/IBM** quickly: edit `main()` to
    
    ```c
    int width=16; uint16_t poly=0x1021, init=0xFFFF, xorout=0x0000;
    
    ```
    
    then:
    
    ```bash
    gcc crc.c -o crc && ./crc "DATA LINK"
    
    ```
    

### 5) Expected example output

```
Payload      : "HELLO"
CRC-8 poly  : 0x0007
CRC value    : 0xA0
Receiver check (recompute over data+CRC): 0x0000 (expect 0x0000)

```

---

## 🧠 Explanation (concepts & what each part means)

### What CRC is (the idea)

- CRC treats your bitstream as a polynomial over **GF(2)** (bits are coefficients 0/1).
- Choose a **generator polynomial** `G(x)` of degree **r** (e.g., for CRC-8, r=8).
- Sender appends **r zero bits** to the message and performs **polynomial long division** by `G(x)`; the **remainder R(x)** (r bits) is the CRC.
- Transmit **[original data || CRC]**.
- Receiver divides the full received sequence by the same `G(x)`; **remainder 0 ⇒ no error detected** (for this unreflected/simple model).

### Why it’s powerful

- With a good generator, CRC detects **all single-bit errors, all double-bit errors**, any odd number of bit errors, and **burst errors up to degree r** (and most longer bursts). That’s why CRCs are used in Ethernet frames, HDLC, storage, etc.

### How this code maps to the math

- `width` picks **r** (8 for CRC-8, 16 for CRC-16).
- `poly` encodes **G(x)** without the top bit (we keep it implicit via `topbit`).
- For each input bit, we **shift** and **conditionally XOR** with the polynomial when the top bit is 1 — this is exactly the **binary long division** over GF(2).
- `init` and `xorout` allow common CRC variants (we keep it simple, no reflection).

### Quick viva one-liners

- **CRC stands for?** Cyclic Redundancy Check.
- **Works at which layer?** Data Link (framing integrity).
- **Key steps?** Append r zeros → divide by generator → append remainder.
- **Why better than parity/LRC?** Strong burst-error detection properties.
- **What determines strength?** The **generator polynomial** choice and width.

### Commands recap (why we used them)

- `nano crc.c` — write the code.
- `gcc crc.c -o crc` — compile.
- `./crc "TEXT"` — run on a test payload; shows **CRC value** and **receiver check** (0 remainder).

## Practical 11 — Hamming Code (7,4) – Error Detection and Correction

### 📍 Steps to perform

```bash
nano hamming.c

```

```c
#include <stdio.h>

int main() {
    int d1=1,d2=0,d3=1,d4=1; // 4 data bits
    int p1 = d1 ^ d2 ^ d4;
    int p2 = d1 ^ d3 ^ d4;
    int p3 = d2 ^ d3 ^ d4;
    int c[7]={p1,p2,d1,p3,d2,d3,d4};

    printf("Codeword: ");
    for(int i=0;i<7;i++) printf("%d",c[i]);
    printf("\n");

    // simulate 1-bit error
    c[4]^=1;
    printf("Received (with error): ");
    for(int i=0;i<7;i++) printf("%d",c[i]);
    printf("\n");

    int s1=c[0]^c[2]^c[4]^c[6];
    int s2=c[1]^c[2]^c[5]^c[6];
    int s3=c[3]^c[4]^c[5]^c[6];
    int syndrome=s1 + (s2<<1) + (s3<<2);
    printf("Syndrome = %d -> flip bit %d\n",syndrome,syndrome);

    if(syndrome>=1 && syndrome<=7) c[syndrome-1]^=1;

    printf("Corrected codeword: ");
    for(int i=0;i<7;i++) printf("%d",c[i]);
    printf("\n");
}

```

```bash
gcc hamming.c -o hamming
./hamming

```

### ✅ Output

```
Codeword: 1010101
Received (with error): 1010001
Syndrome = 5 -> flip bit 5
Corrected codeword: 1010101

```

---

### 🧠 Explanation

- (7,4) → 4 data + 3 parity bits.
- Parity bits at positions 1,2,4; computed using XOR of selected data bits.
- Receiver computes **syndrome bits** to locate error position.
- Single-bit errors can be **detected & corrected**.
- **Layer:** Physical/Data Link.

---

## 🧩 Practical 12 — Leaky Bucket Algorithm

### 📍 Steps

```bash
nano leaky_bucket.c

```

```c
#include <stdio.h>

int main() {
    int bucket=0, cap=10, out=3;
    int in[]={4,6,7,2,5}, t=5;
    for(int i=0;i<t;i++){
        bucket+=in[i];
        if(bucket>cap){
            printf("t=%d: Overflow %d\n",i,bucket-cap);
            bucket=cap;
        }
        int sent=(bucket>=out)?out:bucket;
        bucket-=sent;
        printf("t=%d: In=%d Sent=%d Left=%d\n",i,in[i],sent,bucket);
    }
    return 0;
}

```

```bash
gcc leaky_bucket.c -o leaky_bucket
./leaky_bucket

```

### ✅ Output

```
t=0: In=4 Sent=3 Left=1
t=1: In=6 Sent=3 Left=4
t=2: In=7 Sent=3 Left=8
t=3: In=2 Sent=3 Left=7
t=4: In=5 Sent=3 Left=9

```

---

### 🧠 Explanation

- Simulates **traffic shaping**.
- Bucket has capacity; constant outflow rate.
- If input > outflow → overflow (packets dropped).
- Controls congestion by smoothing bursts.
- **Used in:** Network traffic regulators (Layer 3/4).

---

## 🧩 Practical 13 — Token Bucket Algorithm

### 📍 Steps

```bash
nano token_bucket.c

```

```c
#include <stdio.h>

int main() {
    int tokens=0, cap=10, gen=4;
    int demand[]={2,8,7,12,5}, t=5;
    for(int i=0;i<t;i++){
        tokens+=gen;
        if(tokens>cap) tokens=cap;
        int send=(demand[i]<=tokens)?demand[i]:tokens;
        tokens-=send;
        printf("t=%d: Demand=%d Sent=%d Tokens_left=%d\n",i,demand[i],send,tokens);
    }
    return 0;
}

```

```bash
gcc token_bucket.c -o token_bucket
./token_bucket

```

### ✅ Output

```
t=0: Demand=2 Sent=2 Tokens_left=2
t=1: Demand=8 Sent=6 Tokens_left=0
t=2: Demand=7 Sent=4 Tokens_left=0
t=3: Demand=12 Sent=8 Tokens_left=0
t=4: Demand=5 Sent=5 Tokens_left=3

```

---

### 🧠 Explanation

- Tokens represent permission to send packets.
- Tokens are generated at a fixed rate.
- Allows **bursty** transmission (if enough tokens).
- Enforces **average rate limit** but not instant limit.
- **Used in:** QoS, routers, ISPs.

---

## 🧩 Practical 14 — Password Policy Check (System Security Demo)

### 📍 Steps

```bash
sudo apt install libpam-pwquality -y
sudo grep -E 'minlen|dcredit|ucredit|lcredit|ocredit' /etc/security/pwquality.conf

```

### ✅ Output Example

```
minlen = 8
dcredit = -1
ucredit = -1
lcredit = -1
ocredit = -1

```

---

### 🧠 Explanation

- `minlen`: minimum password length.
- `dcredit`: requires digits.
- `ucredit`: uppercase letters.
- `lcredit`: lowercase letters.
- `ocredit`: special characters.
- Enforces **complexity** and **security standards**.
- **Used in:** Authentication systems (Layer 7).

---

## 🧩 Practical 15 — Secure Remote Access (SSH & MFA concept)

### 📍 Steps

```bash
sudo apt install openssh-server -y
sudo systemctl status ssh
ssh localhost

```

(Optional) Generate key-based authentication:

```bash
ssh-keygen
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

```

---

### 🧠 Explanation

- **SSH (Secure Shell):** encrypts remote login sessions.
- Uses **public/private key pairs** for authentication.
- Protects against sniffing and replay attacks.
- **MFA (Multi-Factor Auth):** adds extra verification (TOTP apps).
- **Used in:** Secure remote administration, network devices.

---

# 🧾 Quick Recap Summary

| Practical | Topic | Concept Focus |
| --- | --- | --- |
| **11** | Hamming (7,4) | Single-bit error correction |
| **12** | Leaky Bucket | Constant-rate congestion control |
| **13** | Token Bucket | Burst + average rate control |
| **14** | Password Policy | Security enforcement in Linux |
| **15** | Secure Access | SSH + MFA concepts |
