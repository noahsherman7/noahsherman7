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

### WAF Configuration & Security Testing

After deploying DVWA behind the SafeLine WAF, I enabled rate-limiting and bot-prevention rules to detect high-frequency attacks and automated traffic. These settings allowed the WAF to intercept requests and require verification when suspicious behavior was detected.

#### Rate-Limit Rule Configuration
![Rate Limit Rule](assets/cc-protection-rate-limit-settings.png)
*Enabled High-Frequency Access Restrictions. I configured the WAF to trigger human-machine verification if more than **5 requests** occur within **3 seconds** from a single IP.*

#### CC Protection Dashboard
![CC Protection Dashboard](assets/cc-protection-dashboard.png)
*Overview of active CC protection controls for www.dvwa.local — including request frequency limits, attack restrictions, and error-based blocking.*

#### Frequency-Limit Blocks
![Frequency Limit Logs](assets/frequency-limit-blocks.png)
*The WAF successfully intercepted traffic bursts to DVWA. Repeated fast visits triggered human-machine verification for **3 minutes**, proving the rule worked.*

#### Human-Machine Verification Logs
![Human Machine Verification](assets/human-machine-verification.png)
*Shows individual IP addresses being flagged and challenged, confirming that the application is protected against automated scanning or bot-style traffic.*

### Results & Takeaways
- DVWA was successfully deployed and reachable from an attacker machine over HTTP and HTTPS.
- SafeLine WAF was installed as a reverse proxy, encrypted with a 4096-bit RSA TLS certificate.
- Automated rate-limit and high-frequency access rules blocked rapid requests from the attacker.
- Human-machine verification was triggered, proving that the WAF actively protected the application.
