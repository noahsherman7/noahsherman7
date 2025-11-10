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
### Hosts mapping on attacker
![Kali /etc/hosts mapping dvwa.local](assets/kali-hosts-dvwa.png)
*Figure — Added `10.220.38.150 dvwa.local` to `/etc/hosts` on the attacker so `http://dvwa.local` resolves to the Ubuntu DVWA server during testing.*
### SafeLine WAF install
![SafeLine WAF installer running](assets/safeline-install.png)
*Figure — Running the SafeLine WAF installer script (`manager.sh`). Installer menu prompts to install/upgrade/uninstall; run as root to install the WAF.*

![Apache config directory](assets/apache-config-dir.png)
*Figure — Apache installed; `/etc/apache2` shows `sites-available`, `sites-enabled`, and `mods-enabled` (web server ready to host DVWA).*

![SafeLine WAF dashboard](assets/safeline-dashboard.png)
*Figure — SafeLine WAF installed and accessible through the management dashboard. Ready to monitor and protect the DVWA web application.*

![DVWA reachable from Kali](assets/dvwa-login-kali.png)
*Figure — DVWA login page loaded from the Kali attacker VM via `http://10.220.38.130:8080`. Confirms connectivity before running SQL injection tests.*

![Generating SSL certificate](assets/openssl-generate-cert.png)
*Figure — Created a 4096-bit RSA private key and certificate signing request (CSR) using OpenSSL. Certificate used to enable HTTPS traffic through the SafeLine WAF.*

![Adding DVWA as a protected application in SafeLine WAF](assets/waf-add-dvwa-application.png)
*Figure — Configured DVWA as a protected application in SafeLine WAF using HTTPS on port 443. Traffic is reverse-proxied to the backend server (`http://10.220.38.130:8080`) with the custom certificate added earlier.*

![DVWA protected by SafeLine WAF](assets/waf-protection-mode.png)
*Figure — SafeLine WAF showing DVWA (`www.dvwa.local`) actively protected over HTTPS on port 443. All inbound requests and intercepts will be logged and analyzed here.*

![HTTPS certificate details](assets/https-certificate-details.png)
*Figure — TLS certificate for `www.dvwa.local` configured on SafeLine WAF. Uses a 4096-bit RSA key, issued by WIT, and valid for one year. Confirms encrypted HTTPS traffic between Kali and the DVWA server.*

