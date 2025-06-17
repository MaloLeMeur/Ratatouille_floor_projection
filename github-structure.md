# GitHub Repository Structure

Here's the complete file structure for your GitHub repository:

## 📁 Repository Files

### 1. `.gitignore`
```gitignore
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
env/
venv/
ENV/
.venv
*.egg-info/

# Video files (too large for git)
*.mp4
*.avi
*.mov
*.mkv

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Logs
*.log

# Config
config.local.json
```

### 2. `LICENSE` (MIT License)
```
MIT License

Copyright (c) 2024 [Your Name]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

### 3. `requirements.txt` (root - for both)
```
# Server requirements (Raspberry Pi)
RPi.GPIO==0.7.1; platform_machine=='armv7l' or platform_machine=='aarch64'

# Client requirements
pygame==2.5.2
python-vlc==3.0.18122
```

### 4. `server/requirements.txt`
```
RPi.GPIO==0.7.1
```

### 5. `client/requirements.txt`
```
pygame==2.5.2
python-vlc==3.0.18122
```

### 6. `docs/server-setup.md`
```markdown
# Raspberry Pi Server Setup Guide

## Hardware Setup

### Materials Needed
- Raspberry Pi (any model)
- PIR sensor (HC-SR501 or similar)
- Jumper wires (3x female-to-female)
- Power supply for Raspberry Pi

### Wiring Diagram
```
PIR Sensor    →    Raspberry Pi
---------          -------------
VCC (Red)     →    Pin 2 (5V)
GND (Black)   →    Pin 6 (Ground)
OUT (Yellow)  →    Pin 18 (GPIO 24)
```

## Software Setup

### 1. Update Raspberry Pi OS
```bash
sudo apt update
sudo apt upgrade -y
```

### 2. Install Python and Git
```bash
sudo apt install python3-pip git -y
```

### 3. Clone the repository
```bash
git clone https://github.com/yourusername/pir-motion-video-player.git
cd pir-motion-video-player/server
```

### 4. Install dependencies
```bash
pip3 install -r requirements.txt
```

### 5. Configure as WiFi Hotspot
See [Hotspot Setup Guide](hotspot-setup.md)

### 6. Run the server
```bash
sudo python3 raspberry_pi_wifi_server.py
```

### Auto-start on boot (optional)
```bash
# Edit rc.local
sudo nano /etc/rc.local

# Add before 'exit 0':
cd /home/pi/pir-motion-video-player/server && sudo python3 raspberry_pi_wifi_server.py &
```
```

### 7. `docs/client-setup.md`
```markdown
# Video Player Client Setup Guide

## Prerequisites

1. **Install VLC Media Player**
   - Windows: Download from [videolan.org](https://www.videolan.org/vlc/)
   - macOS: `brew install vlc`
   - Linux: `sudo apt install vlc`

2. **Install Python 3.7+**
   - Download from [python.org](https://www.python.org/)

## Installation Steps

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/pir-motion-video-player.git
cd pir-motion-video-player/client
```

### 2. Create virtual environment (recommended)
```bash
python3 -m venv venv

# Activate:
# Windows: venv\Scripts\activate
# macOS/Linux: source venv/bin/activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Add your video file
Place your video file in the client directory and rename it to match the configuration (default: `projection version04.mp4`)

### 5. Connect to Raspberry Pi hotspot
1. Open WiFi settings
2. Connect to "RaspberryPi-PIR" network
3. Enter password (default: pir12345)

### 6. Run the video player
```bash
python video_player_pir_wifi.py
```

## Configuration

Edit `video_player_pir_wifi.py` to customize:
- Video resolution
- Fade duration
- Motion timeout
- Video file path
```

### 8. `docs/hotspot-setup.md`
```markdown
# Raspberry Pi Hotspot Configuration

## Quick Setup Script

```bash
#!/bin/bash
# Save as setup-hotspot.sh

# Install required packages
sudo apt install hostapd dnsmasq -y

# Stop services
sudo systemctl stop hostapd
sudo systemctl stop dnsmasq

# Configure static IP
sudo cat >> /etc/dhcpcd.conf << EOF
interface wlan0
static ip_address=192.168.4.1/24
nohook wpa_supplicant
EOF

# Configure DHCP server
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.bak
sudo cat > /etc/dnsmasq.conf << EOF
interface=wlan0
dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h
EOF

# Configure access point
sudo cat > /etc/hostapd/hostapd.conf << EOF
interface=wlan0
driver=nl80211
ssid=RaspberryPi-PIR
hw_mode=g
channel=7
wmm_enabled=0
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=pir12345
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
EOF

# Point to config file
sudo sed -i 's|#DAEMON_CONF=""|DAEMON_CONF="/etc/hostapd/hostapd.conf"|' /etc/default/hostapd

# Enable services
sudo systemctl unmask hostapd
sudo systemctl enable hostapd
sudo systemctl enable dnsmasq

# Start services
sudo systemctl start hostapd
sudo systemctl start dnsmasq

echo "Hotspot configured! Reboot to apply changes."
```

## Manual Configuration

### 1. Install packages
```bash
sudo apt update
sudo apt install hostapd dnsmasq -y
```

### 2. Configure network interface
Edit `/etc/dhcpcd.conf`:
```
interface wlan0
static ip_address=192.168.4.1/24
nohook wpa_supplicant
```

### 3. Configure DHCP
Edit `/etc/dnsmasq.conf`:
```
interface=wlan0
dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h
```

### 4. Configure access point
Create `/etc/hostapd/hostapd.conf`:
```
interface=wlan0
ssid=RaspberryPi-PIR
wpa_passphrase=pir12345
wpa=2
```

### 5. Enable and start services
```bash
sudo systemctl enable hostapd dnsmasq
sudo systemctl start hostapd dnsmasq
```

## Troubleshooting

- Check status: `sudo systemctl status hostapd`
- View logs: `sudo journalctl -u hostapd`
- Test config: `sudo hostapd -d /etc/hostapd/hostapd.conf`
```

### 9. `docs/troubleshooting.md`
```markdown
# Troubleshooting Guide

## Common Issues

### Server Issues

#### "GPIO already in use"
```bash
# Reset GPIO
python3 -c "import RPi.GPIO as GPIO; GPIO.setmode(GPIO.BCM); GPIO.cleanup()"
```

#### PIR sensor not detecting motion
- Check wiring connections
- Adjust PIR sensor sensitivity potentiometer
- Wait 30-60 seconds for PIR to calibrate
- Test with simple GPIO script

#### Hotspot not visible
```bash
# Check service status
sudo systemctl status hostapd

# Restart services
sudo systemctl restart hostapd dnsmasq

# Check for errors
sudo journalctl -xe
```

### Client Issues

#### "No PIR server found"
1. Verify connected to correct WiFi network
2. Check server is running: `ping 192.168.4.1`
3. Try manual IP entry: `192.168.4.1`
4. Check firewall settings

#### Video not playing
- Install VLC: `vlc --version`
- Test video directly in VLC
- Check file path and permissions
- Try different video format

#### Fade effect not working
- Update VLC to latest version
- Check VLC video filters are enabled
- Try running with administrator privileges

### Network Issues

#### Connection drops frequently
- Check power supply (use 2.5A+ for Pi)
- Reduce WiFi interference
- Limit number of clients
- Check signal strength

#### Slow response time
- Adjust `SEND_INTERVAL` in server
- Check CPU usage on Pi: `top`
- Reduce video resolution
- Close other applications

## Debug Mode

### Enable verbose logging

Server:
```python
# Add to server script
import logging
logging.basicConfig(level=logging.DEBUG)
```

Client:
```python
# Already included, check console output
```

## Performance Optimization

### Raspberry Pi
- Use Raspberry Pi 3 or newer
- Use Class 10 SD card
- Enable GPU acceleration
- Overclock (with cooling)

### Client
- Close unnecessary programs
- Update graphics drivers
- Use wired connection if possible
- Reduce video resolution

## Getting Help

1. Check console output for errors
2. Review [User Manual](USER_MANUAL.md)
3. Search existing [Issues](https://github.com/yourusername/pir-motion-video-player/issues)
4. Create new issue with:
   - Error messages
   - Hardware details
   - Steps to reproduce
```

### 10. `examples/config-examples.py`
```python
"""
Configuration examples for PIR Motion Video Player
"""

# Basic configuration
BASIC_CONFIG = {
    'res': (1920, 1080),      # Full HD
    'no_input_secs': 7,       # Default timeout
    'fade_ms': 500,           # Half second fade
    'vid_path': 'video.mp4',
    'port': 5555,
}

# Gallery/Museum setup (longer timeout, slower fades)
GALLERY_CONFIG = {
    'res': (3840, 2160),      # 4K display
    'no_input_secs': 15,      # Longer timeout
    'fade_ms': 2000,          # Slow 2 second fade
    'vid_path': 'artwork.mp4',
    'port': 5555,
}

# Interactive kiosk (quick response)
KIOSK_CONFIG = {
    'res': (1280, 720),       # 720p for performance
    'no_input_secs': 3,       # Quick timeout
    'fade_ms': 200,           # Fast fade
    'motion_debounce_ms': 100, # Quick response
    'vid_path': 'demo.mp4',
    'port': 5555,
}

# Multiple sensor setup (future feature)
MULTI_SENSOR_CONFIG = {
    'sensors': [
        {'pin': 24, 'zone': 'left'},
        {'pin': 25, 'zone': 'right'},
        {'pin': 23, 'zone': 'center'},
    ],
    'res': (1920, 1080),
    'no_input_secs': 5,
    'fade_ms': 500,
}

# Network configurations
NETWORK_CONFIGS = {
    'local_hotspot': {
        'raspberry_pi_ip': '192.168.4.1',
        'port': 5555,
    },
    'home_network': {
        'raspberry_pi_ip': '192.168.1.100',
        'port': 5555,
    },
    'auto_discover': {
        'raspberry_pi_ip': None,  # Auto-detect
        'port': 5555,
    }
}
```

### 11. `setup.py` (optional, for pip installation)
```python
from setuptools import setup, find_packages

with open("README.md", "r", encoding="utf-8") as fh:
    long_description = fh.read()

setup(
    name="pir-motion-video-player",
    version="1.0.0",
    author="Your Name",
    author_email="your.email@example.com",
    description="WiFi-based motion detection system for video playback control",
    long_description=long_description,
    long_description_content_type="text/markdown",
    url="https://github.com/yourusername/pir-motion-video-player",
    packages=find_packages(),
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
        "Development Status :: 4 - Beta",
        "Intended Audience :: Developers",
        "Topic :: Multimedia :: Video :: Display",
    ],
    python_requires=">=3.7",
    install_requires=[
        "pygame>=2.5.2",
        "python-vlc>=3.0.18122",
    ],
    extras_require={
        "rpi": ["RPi.GPIO>=0.7.1"],
    },
)
```

## 📝 How to Create the GitHub Repository

1. **Create a new repository on GitHub**
   - Go to https://github.com/new
   - Name: `pir-motion-video-player`
   - Description: "WiFi-based motion detection system for video playback control"
   - Public repository
   - Add README: No (we'll push our own)
   - License: MIT

2. **Clone and set up locally**
   ```bash
   mkdir pir-motion-video-player
   cd pir-motion-video-player
   git init
   ```

3. **Create the file structure**
   ```bash
   mkdir -p server client docs examples
   # Copy all files to their respective locations
   ```

4. **Initial commit and push**
   ```bash
   git add .
   git commit -m "Initial commit: PIR motion-activated video player"
   git branch -M main
   git remote add origin https://github.com/yourusername/pir-motion-video-player.git
   git push -u origin main
   ```

5. **Add topics on GitHub**
   - raspberry-pi
   - pir-sensor
   - video-player
   - motion-detection
   - pygame
   - vlc
   - iot
   - interactive-art

6. **Create releases**
   ```bash
   git tag -a v1.0.0 -m "First stable release"
   git push origin v1.0.0
   ```