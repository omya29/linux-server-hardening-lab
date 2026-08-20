\# Day 1 — Commands Reference



Commands used on Day 1: system setup, inspection, and DNS troubleshooting.



\## System Update



| Command | Purpose |

|---|---|

| `sudo apt update` | Refreshes the list of available packages — doesn't install anything, just checks what's out there |

| `sudo apt upgrade -y` | Installs newer versions of already-installed packages; `-y` auto-confirms |



\*\*Why it matters:\*\* outdated packages are a common attack vector. Updating first is standard practice before administering a server.



\## OS Inspection



| Command | Purpose |

|---|---|

| `cat /etc/os-release` | Shows Ubuntu version and codename |

| `uname -a` | Shows kernel version, architecture, hostname |



\## Resource Inspection



| Command | Purpose |

|---|---|

| `df -h` | Disk usage, human-readable |

| `free -h` | RAM and swap usage |

| `lscpu` | CPU info — cores, architecture, model |



\*\*Why it matters:\*\* establishes a baseline of "normal" so abnormal behavior (high load, full disk, crashing service) is easier to spot later.



\## Networking



| Command | Purpose |

|---|---|

| `ip addr` | Shows network interfaces and assigned IP addresses |

| `hostname -I` | Quick shortcut to show IP address(es) |



\## Filesystem Navigation



| Command | Purpose |

|---|---|

| `pwd` | Prints current directory |

| `ls` | Lists files/folders in current directory |

| `ls -la` | Long format + hidden files |

| `cd` | Change directory (alone = go home) |

| `mkdir <name>` | Create a folder |

| `touch <name>` | Create an empty file |

| `cp <src> <dest>` | Copy a file |

| `mv <src> <dest>` | Move or rename a file |

| `rm <name>` | Delete a file (permanent) |

| `ls /` | List the root of the filesystem |



Key directories:



| Directory | Purpose |

|---|---|

| `/etc` | Configuration files |

| `/home` | User directories |

| `/var` | Logs and application data |

| `/tmp` | Temporary files |

| `/usr` | Programs and libraries |

| `/bin` | Essential commands |



\## Project Setup



| Command | Purpose |

|---|---|

| `mkdir \~/linux-security-lab` | Create the working project folder |

| `cd \~/linux-security-lab` | Move into it |

| `mkdir users permissions ssh apache firewall screenshots` | Create subfolders for each lab area |



\## Logs (noted, not analyzed until Day 7)



| Command | Purpose |

|---|---|

| `ls /var/log` | List log files — "logs are evidence" |



\## Networking Troubleshooting (VMware NAT / DNS issue)



| Command | Purpose |

|---|---|

| `ip link` | Shows interface status (up/down, carrier detected) |

| `ping -c 4 <ip>` | Tests connectivity; `-c 4` sends 4 test packets |

| `cat /etc/resolv.conf` | Shows configured DNS servers |

| `resolvectl status` | Shows systemd-resolved's actual DNS config per interface |

| `nslookup <domain> <server>` | Directly queries a specific DNS server |

| `sudo resolvectl dns ens33 8.8.8.8 8.8.4.4` | Sets DNS servers for ens33 via systemd-resolved |

| `sudo resolvectl domain ens33 "\~."` | Routes all domain lookups through that DNS server |



\*\*Root cause:\*\* VMware NAT Service was not running on the Windows host, and VMware's DNS proxy was non-responsive even after NAT/DHCP were restored. Fixed by setting DNS directly via `resolvectl` (appropriate since this VM uses `systemd-networkd`/`systemd-resolved`, not NetworkManager).

