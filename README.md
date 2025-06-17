# PIR Motion-Activated Video Player

A WiFi-based motion detection system that controls video playback. When motion is detected by a PIR sensor connected to a Raspberry Pi, the video fades to black. When no motion is detected for 7 seconds, the video fades back in.

## 📁 Files

- `raspberry_pi_wifi_server.py` - Server code for Raspberry Pi with PIR sensor
- `video_player_pir_wifi.py` - Client code for computer with video display
- `USER_MANUAL.md` - Complete setup and usage instructions

## 🚀 Quick Start

### On Raspberry Pi:
1. Connect PIR sensor to GPIO 24
2. Configure Raspberry Pi as WiFi hotspot
3. Run: `python3 raspberry_pi_wifi_server.py`

### On Client Computer:
1. Install VLC media player
2. Connect to Raspberry Pi hotspot
3. Run: `python3 video_player_pir_wifi.py`

## 📖 Documentation

See [USER_MANUAL.md](USER_MANUAL.md) for detailed setup instructions, configuration options, and troubleshooting.

## 📋 Requirements

### Raspberry Pi (Server):
- PIR motion sensor
- Python 3 with RPi.GPIO

### Client Computer:
- VLC media player
- Python 3 with pygame and python-vlc
- Video file: `projection version04.mp4`

## ⚙️ Basic Configuration

The system uses Raspberry Pi's hotspot (typically 192.168.4.1) on port 5555.

To change settings, edit the configuration section in each Python file.

