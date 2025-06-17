# PIR Motion-Activated Video Player - User Manual

## Overview

This system consists of two Python programs that work together to create a motion-activated video player:

1. **PIR WiFi Server** (`raspberry_pi_wifi_server_continuous.py`) - Runs on a Raspberry Pi with a PIR sensor
2. **Video Player Client** (`video_player_pir_wifi.py`) - Runs on any computer to display video

When motion is detected by the PIR sensor, the video fades to black. When no motion is detected for 7 seconds, the video fades back in.

## System Requirements

### For the PIR Server (Raspberry Pi):
- Raspberry Pi (any model with GPIO pins)
- PIR motion sensor connected to GPIO pin 24
- Python 3 with RPi.GPIO library
- Network connection (WiFi or Ethernet)

### For the Video Player Client:
- Computer running Windows, macOS, or Linux
- Python 3 with the following libraries:
  - pygame
  - python-vlc
  - asyncio
- VLC media player installed
- Video file: `projection version04.mp4`

## Installation

### On the Raspberry Pi (Server):

1. Connect the PIR sensor:
   - VCC to 5V power
   - GND to ground
   - OUT to GPIO pin 24

2. Install required library:
   ```bash
   pip3 install RPi.GPIO
   ```

3. Copy `raspberry_pi_wifi_server_continuous.py` to the Raspberry Pi

### On the Client Computer:

1. Install VLC media player from https://www.videolan.org/

2. Install Python libraries:
   ```bash
   pip3 install pygame python-vlc
   ```

3. Copy `video_player_pir_wifi.py` to the client computer

4. Place your video file (`projection version04.mp4`) in the same directory

## Usage

### Step 1: Start the PIR Server

On the Raspberry Pi, run:
```bash
python3 raspberry_pi_wifi_server_continuous.py
```

The server will display:
- Its IP address (e.g., 192.168.1.100)
- The port number (5555)
- Current PIR sensor status
- Number of connected clients

### Step 2: Start the Video Player

On the client computer, run:
```bash
python3 video_player_pir_wifi.py
```

You'll be prompted to:
1. **Auto-detect PIR server** - Scans the network automatically
2. **Enter IP manually** - Type the Raspberry Pi's IP address

Or provide the IP as a command line argument:
```bash
python3 video_player_pir_wifi.py 192.168.1.100
```

### Step 3: Operation

- The video will play in fullscreen mode
- When motion is detected: video fades to black (0.5 seconds)
- When no motion for 7 seconds: video fades back in (1 second)
- Press **SPACE** for manual trigger (testing)
- Press **ESC** to exit

## Configuration

### Server Settings (in `raspberry_pi_wifi_server_continuous.py`):

```python
PIR_PIN = 24          # GPIO pin for PIR sensor
PORT = 5555           # Network port
SEND_INTERVAL = 0.1   # Send state every 100ms
```

### Client Settings (in `video_player_pir_wifi.py`):

```python
SETTINGS = {
    'res': (1280, 720),           # Video resolution
    'no_input_secs': 7,           # Seconds to wait with no motion
    'motion_debounce_ms': 200,    # Debounce time in milliseconds
    'fps': 30,                    # Frame rate
    'fade_ms': 500,               # Fade duration in milliseconds
    'vid_path': 'projection version04.mp4',  # Video filename
    'port': 5555,                 # Must match server port
}
```

## Troubleshooting

### "No PIR server found on network"
- Ensure the Raspberry Pi server is running
- Check that both devices are on the same network
- Try manual IP entry instead of auto-detect
- Check firewall settings

### "Video not found"
- Ensure `projection version04.mp4` is in the same directory as the script
- Check the filename matches exactly (case-sensitive on Linux/macOS)

### Motion detection not working
- Check PIR sensor connections
- Verify GPIO pin number (default: 24)
- Test PIR sensor with a simple GPIO test script
- Check for electromagnetic interference

### Video won't play
- Ensure VLC is installed
- Try playing the video directly in VLC first
- Check video codec compatibility

### Connection drops
- Check network stability
- Ensure power supply is adequate for Raspberry Pi
- Check for WiFi interference

## Network Protocol

The server sends JSON messages continuously:
```json
{
    "type": "pir_state",
    "timestamp": "2024-01-15T10:30:45",
    "state": 1,
    "motion": true,
    "motion_count": 42,
    "clients_connected": 1
}
```

- `state`: 0 (no motion) or 1 (motion detected)
- `motion`: Boolean version of state
- `motion_count`: Total motion events since server start

## Safety Notes

- The PIR sensor requires 5V power - ensure proper connections
- Use appropriate resistors if needed for your PIR model
- The system hides the mouse cursor - press ESC to exit cleanly
- Video plays in fullscreen - ensure you can access ESC key

## Advanced Usage

### Multiple Clients
The server supports multiple simultaneous clients. Each client will receive the same motion data.

### Custom Video Files
Change the `vid_path` setting to use different videos. Supported formats depend on VLC installation.

### Network Security
The system uses unencrypted TCP connections. For secure environments, consider:
- Using a VPN
- Implementing authentication
- Adding SSL/TLS encryption

## Support

For issues:
1. Check all connections
2. Verify network connectivity
3. Test components individually
4. Check Python library versions
5. Review error messages in console output