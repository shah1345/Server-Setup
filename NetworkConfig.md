## 🧾 Step 1: Find Your Network Interface Name

To find the name of your Ethernet interface on Ubuntu Server, follow these steps:

### 🔧 1. Open your terminal and run:

```bash
ip a
 ```
## 🔍 2. Look for lines that resemble the following:
```bash
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> ...
    inet 192.168.1.101/24 brd 192.168.1.255 scope global dynamic enp0s3

 ```

In this case, the network interface name is `enp0s3`.

You might see other names like:

- `eth0`
- `ens33`
- `enp2s0`

These are valid too. Just **note down the name of the interface that has an `inet` IP address**, as that's your Ethernet interface.
### ⚠️ Avoid using loopback (lo) or virtual interfaces (docker0, etc.).

### 🔍 Step 2: List Netplan Files
Run this command to list all .yaml files in the Netplan directory:
```bash
ls /etc/netplan/
 ```

You'll likely see one of the following:

- `00-installer-config.yaml`

- `01-netcfg.yaml`

- `50-cloud-init.yaml`

✅ This is your current Netplan file.
