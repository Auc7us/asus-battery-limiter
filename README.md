# asus-battery-limiter
## TLP Setup for Custom Battery Charge Thresholds on Ubuntu

### Steps to Configure and Automate TLP

1. Install TLP and required tools:
    ```bash
    sudo apt update
    sudo apt install tlp tlp-rdw
    ```

2. Modify TLP configuration for battery thresholds:
    ```bash
    sudo nano /etc/tlp.conf
    ```
    Set battery charge thresholds (for `BAT1`):
    ```ini
    START_CHARGE_THRESH_BAT1=30
    STOP_CHARGE_THRESH_BAT1=50
    ```

3. Create a systemd service to reload TLP on boot:
    ```bash
    sudo nano /etc/systemd/system/reload-tlp.service
    ```
    Add this content:
    ```ini
    [Unit]
    Description=Reload and restart TLP after boot
    After=network.target

    [Service]
    Type=oneshot
    ExecStart=/bin/bash -c 'systemctl daemon-reload && systemctl restart tlp.service'

    [Install]
    WantedBy=multi-user.target
    ```

4. Enable and start the service:
    ```bash
    sudo systemctl daemon-reload
    sudo systemctl enable reload-tlp.service
    sudo systemctl start reload-tlp.service
    ```

5. After reboot, verify if the thresholds are applied:
    ```bash
    sudo tlp-stat -b
    ```
