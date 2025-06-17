# PIR Motion-Activated Video Player

A WiFi-based motion detection system that controls video playback. When motion is detected by a PIR sensor connected to a Raspberry Pi, the video fades to black. When no motion is detected for 7 seconds, the video fades back in.

![Python](https://img.shields.io/badge/python-3.7+-blue.svg)
![Platform](https://img.shields.io/badge/platform-Raspberry%20Pi%20%7C%20Windows%20%7C%20macOS%20%7C%20Linux-lightgrey.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

## 🎥 Demo

The system creates an interactive video installation that responds to viewer presence:
- Motion detected → Video fades to black (0.5s)
- No motion for 7s → Video fades back in (1s)
- Automatic video loop
- Manual trigger with spacebar (for testing)

## 🚀 Features

- **Wireless PIR sensor** via WiFi connection
- **Smooth video fading** using VLC contrast adjustment
- **Auto-discovery** of PIR server on network
- **Standalone operation** using Raspberry Pi hotspot
- **Real-time motion detection** with debouncing
- **Fullscreen video playback**
- **Cross-platform support** (Windows, macOS, Linux)

## 📋 Requirements

### Hardware
- Raspberry Pi (any model with GPIO)
- PIR motion sensor
- Computer for video playback
- WiFi adapter (on client computer)

### Software
- Python 3.7+
- VLC media player
- Required Python libraries (see installation)

## 🛠️ Installation

### Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/pir-motion-video-player.git
   cd pir-motion-video-player
   ```

2. **Set up the Raspberry Pi** (see [Server Setup Guide](docs/server-setup.md))
   ```bash
   cd server
   pip3 install -r requirements.txt
   sudo python3 raspberry_pi_wifi_server.py
   ```

3. **Set up the client computer** (see [Client Setup Guide](docs/client-setup.md))
   ```bash
   cd client
   pip3 install -r requirements.txt
   python3 video_player_pir_wifi.py
   ```

## 📖 Documentation

- [User Manual](docs/USER_MANUAL.md) - Complete setup and usage guide
- [Server Setup](docs/server-setup.md) - Raspberry Pi configuration
- [Client Setup](docs/client-setup.md) - Video player installation
- [Hotspot Configuration](docs/hotspot-setup.md) - WiFi access point setup
- [Troubleshooting](docs/troubleshooting.md) - Common issues and solutions

## 🎮 Usage

1. Start the PIR server on Raspberry Pi:
   ```bash
   python3 raspberry_pi_wifi_server.py
   ```

2. Connect your computer to the Raspberry Pi hotspot

3. Run the video player:
   ```bash
   python3 video_player_pir_wifi.py
   ```

4. Controls:
   - **SPACE** - Manual trigger (testing)
   - **ESC** - Exit application

## ⚙️ Configuration

Edit settings in the Python files:

```python
# Server settings
PIR_PIN = 24          # GPIO pin for PIR sensor
PORT = 5555           # Network port

# Client settings
'no_input_secs': 7,   # Seconds to wait with no motion
'fade_ms': 500,       # Fade duration
'vid_path': 'video.mp4'  # Video filename
```

## 📁 Project Structure

```
pir-motion-video-player/
├── README.md
├── LICENSE
├── requirements.txt
├── server/
│   ├── raspberry_pi_wifi_server.py
│   └── requirements.txt
├── client/
│   ├── video_player_pir_wifi.py
│   ├── requirements.txt
│   └── video.mp4 (not included)
├── docs/
│   ├── USER_MANUAL.md
│   ├── server-setup.md
│   ├── client-setup.md
│   ├── hotspot-setup.md
│   └── troubleshooting.md
└── examples/
    └── config-examples.py
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built with [pygame](https://www.pygame.org/) and [python-vlc](https://github.com/oaubert/python-vlc)
- PIR sensor handling with [RPi.GPIO](https://pypi.org/project/RPi.GPIO/)
- Inspired by interactive art installations

## 📞 Support

- Create an [Issue](https://github.com/yourusername/pir-motion-video-player/issues) for bug reports
- Check the [User Manual](docs/USER_MANUAL.md) for detailed instructions
- See [Troubleshooting](docs/troubleshooting.md) for common problems

## 🔮 Future Enhancements

- [ ] Multiple PIR sensor support
- [ ] Web-based configuration interface
- [ ] MQTT protocol option
- [ ] Video playlist support
- [ ] Motion detection zones
- [ ] Data logging and analytics

---

Made with ❤️ for interactive installations