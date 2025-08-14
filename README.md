Here is the **fastest, fully-open-source way** to get a Holochain-powered VPN (really a peer-to-peer, end-to-end encrypted “dark-mesh” tunnel) running **in under 2 minutes**, without buying a HoloPort hardware device.

What you’ll get  
- A tiny overlay network (2 or more laptops/servers) that looks and behaves like a VPN, but is actually a Holochain hApp.  
- All traffic between the two machines is end-to-end encrypted and routed directly P2P—no central server, no blockchain, no fees.  
- Every component is Apache-2/MIT licensed.

This uses the **Holochain hApp “dark-mesh”** example maintained by the community (MIT license).  
It is the closest, drop-in equivalent of “OpenVPN for Holochain”.

---

### 0. Prerequisites (30-second install)

**Linux / macOS / WSL2 on Windows**  
If you already have **nix** installed, skip to step 2.

```bash
# 10-second Nix installer (one-liner)
sh <(curl -L https://nixos.org/nix/install) --daemon
```

Re-open your shell or run `source ~/.nix-profile/etc/profile.d/nix.sh`.

---

### 1. Clone the ready-made hApp (2 seconds)

```bash
git clone https://github.com/holochain-open-dev/dark-mesh
cd dark-mesh
```

---

### 2. Start the hApp (60 seconds)

```bash
nix develop   # downloads Holochain binaries, 30–40 MB
hc sandbox generate ./workdir/happ/dark-mesh.happ --run=8888
```

You’ll see:

```
Conductor ready.
Web-happ available at http://localhost:8888
```

Repeat the same two commands on a **second machine** (or a second terminal on the same laptop for testing).

---

### 3. Pair the two nodes (30 seconds)

1. Open the UI printed above (`http://localhost:8888`) on **both** machines.  
2. Click **“Copy my public key”** on Node A.  
3. Paste that key into Node B’s **“Add peer”** box and hit Enter.  
4. Do the reverse (B → A).

That’s it—the Holochain network is now formed.

---

### 4. Use it like a VPN (no extra tools)

- Each node exposes a **TUN interface** (`utun12345` on macOS, `tun0` on Linux).  
- Any packet sent to `10.42.0.0/24` on one node pops out of the other node exactly as if you were on the same LAN.

Quick test:

```bash
# on Node A
ping 10.42.0.2        # Node B answers

# on Node B
curl 10.42.0.1:8080   # reaches Node A’s local web server
```

---

### 5. Add more peers

Just give the next friend the same public key exchange steps; the mesh automatically re-configures.

---

### 6. Stop / uninstall (one command)

```bash
# kill the sandbox
Ctrl-C
# remove the sandbox data (optional)
rm -rf .hc
```

---

That’s all—your own **zero-server, open-source VPN**
