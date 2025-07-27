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

### 🔍 Step 1: List Netplan Files
Run this command to list all .yaml files in the Netplan directory:
```bash
ls /etc/netplan/
 ```

You'll likely see one of the following:

- `00-installer-config.yaml`

- `01-netcfg.yaml`

- `50-cloud-init.yaml`

✅ This is your current Netplan file.



### 🧾 Step 2: View the File Content
Once you identify the filename, for example:

```bash
sudo nano /etc/netplan/00-installer-config.yaml

```

Or

```bash
cat /etc/netplan/00-installer-config.yaml

```

✅ Example Output
You may see something like this:

```bash
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [192.168.50.100/24]
      gateway4: 192.168.50.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]


```
### Replace enp0s3 with your actual interface name from step 1.

Let's say:

- `Interface name: enp0s3`

- `Static IP: 192.168.50.100`

- `Gateway: 192.168.50.1`

- `DNS: 8.8.8.8 and 1.1.1.1`

### Edit the file like this:

```bash
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [192.168.50.100/24]
      gateway4: 192.168.50.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]


```


###✅ Step 4: Apply Configuration

```bash
sudo netplan apply

```

### ✅ Step 5: Confirm
```bash
ip a

```

### And test:


```bash
ping 8.8.8.8

```


## NOW ALL DONE








