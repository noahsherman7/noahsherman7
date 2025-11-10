# DVWA + SafeLine WAF Lab
![VM network adapter set to Bridged](assets/vm-network-setup.png)
*Figure — VM network adapter set to Bridged so it receives its own IP on the network.*
## VM IP Addresses
![Kali attacker IP](assets/kali-ip.png)
*Figure — Kali attacker VM assigned IP `10.220.38.74` on the bridged network (verified with `ip addr`).*
![Ubuntu server IP](assets/ubuntu-ip.png)
*Figure — Ubuntu DVWA server assigned IP `10.220.33.146` on the bridged network (verified with `ip addr`).*
![DVWA setup - files and permissions](assets/dvwa-setup-files.png)
*Figure — Cloned DVWA into `/var/www/html`, set ownership and permissions, and copied `config.inc.php` to prepare DB credentials.*
![DVWA MySQL setup](assets/dvwa-mysql-setup.png)
*Figure — Created `dvwa` database and `dvwa_user` with granted privileges (sensitive passwords redacted).*
