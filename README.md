# Android Device Flash Script

An automated script for flashing Android devices with KSuNext packages (or any fastboot-compatible zip packages).

## Features

### Smart Package Selection
- Automatically finds and selects the **latest zip file** based on the date in the filename
- Supports format: `MMDDYY-waffle-KSuNext-KPkg.zip`
- Example: Automatically picks `092625-waffle-KSuNext-KPkg.zip` over `092325-waffle-KSuNext-KPkg.zip`

### Intelligent Device Detection
- **5-second timeout detection** for both ADB and fastboot modes
- No more hanging or infinite waiting
- Automatically detects if device is already in bootloader/fastboot mode
- Smart retry mechanism with troubleshooting guidance

### Flexible Operation
- **Automatic reboot**: Press Y or just Enter to reboot to bootloader
- **Manual reboot**: Press N to reboot manually using hardware buttons
- **Safety confirmations**: Always asks before flashing
- **Error handling**: Comprehensive checks for tools, packages, and connections

### Device State Management
- Handles ADB → Bootloader → Fastboot transitions seamlessly
- Waits for proper device states at each step
- Verifies device comes back online after flashing

## Requirements

- **Platform Tools**: ADB and Fastboot must be available
- **USB Debugging**: Enabled if starting from Android
- **USB Drivers**: Proper device drivers installed
- **Zip Package**: KSuNext or compatible fastboot package

## Installation

### Windows
1. Download the script file: `Waffle-KSU-N-Update.bat`
2. Place it in your `platform-tools` directory
3. Place your zip packages in the same directory
4. Run as Administrator (recommended)

### Linux/macOS
1. Download and extract: `Waffle-KSU-N-Update-Linux-macOS.tar.gz`
2. Install platform tools:
   - **Linux (Ubuntu/Debian)**: `sudo apt install android-tools-adb android-tools-fastboot`
   - **Linux (Fedora)**: `sudo dnf install android-tools`
   - **Linux (Arch)**: `sudo pacman -S android-tools`
   - **macOS (Homebrew)**: `brew install android-platform-tools`
   - **macOS (MacPorts)**: `sudo port install android-platform-tools`
3. Place your zip packages in the same directory

## Usage

### Quick Start

**Windows:**
```cmd
# Just run the script - it handles everything automatically
Waffle-KSU-N-Update.bat
```

**Linux/macOS:**
```bash
# Extract the archive
tar -xzf Waffle-KSU-N-Update-Linux-macOS.tar.gz

# Run the script - no chmod needed!
./Waffle-KSU-N-Update.sh
```

### File Structure

**Windows:**
```
platform-tools/
├── adb.exe
├── fastboot.exe
├── Waffle-KSU-N-Update.bat          # This script
├── 092325-waffle-KSuNext-KPkg.zip
└── 092625-waffle-KSuNext-KPkg.zip  # ← Will auto-select this (latest)
```

**Linux/macOS:**
```
project-folder/
├── Waffle-KSU-N-Update.sh           # This script
├── 092325-waffle-KSuNext-KPkg.zip
└── 092625-waffle-KSuNext-KPkg.zip  # ← Will auto-select this (latest)
```

### Typical Flow
1. **Package Detection**: Script finds all matching zip files and selects the latest
2. **Device Detection**: Checks for ADB device (5s timeout) → then fastboot device (5s timeout)  
3. **Reboot Handling**: Offers automatic or manual reboot to bootloader
4. **Safety Check**: Shows selected package and asks for confirmation
5. **Flashing**: Runs `fastboot update` with progress indicators
6. **Verification**: Confirms device boots back to Android

## Example Output

```
==========================================
   Waffle KSU-N Package Flash Script
==========================================

Looking for flash packages...
Found flash packages:
092325-waffle-KSuNext-KPkg.zip
092625-waffle-KSuNext-KPkg.zip

Selected latest package by date in filename: 092625-waffle-KSuNext-KPkg.zip

Step 1: Detecting device mode...
Checking for ADB device (5 seconds timeout)...
Device detected in ADB mode!
db279bf5        device

Step 2: Ready to reboot to bootloader
Reboot device to bootloader now? (Y/N, or just press Enter for Yes): 
Rebooting to bootloader...

Step 3: Verifying fastboot connection...
db279bf5        fastboot

WARNING: This will flash 092625-waffle-KSuNext-KPkg.zip
Make sure you have the correct file!

Continue with flashing? (Y/N): Y

Step 4: Starting flash process...
[Fastboot output...]
Flash completed successfully!
```

## Supported Platforms

- ✅ **Windows**: Batch file (`.bat`)
- ✅ **Linux**: Bash script (`.sh`)
- ✅ **macOS**: Bash script (`.sh`)

## Error Handling

The script handles common issues automatically:

- **Missing Tools**: Checks for ADB/Fastboot availability with platform-specific installation guidance
- **No Device**: Clear troubleshooting steps and retry options  
- **Wrong Mode**: Automatically handles device state transitions
- **No Package**: Validates zip files exist before starting
- **Flash Failures**: Reports errors with clear messages

## Safety Features

- **Confirmation Prompts**: Always asks before destructive operations
- **Package Verification**: Shows exactly which file will be flashed
- **Timeout Protection**: Won't hang indefinitely waiting for devices
- **State Validation**: Ensures device is in correct mode before flashing
- **Recovery Guidance**: Provides troubleshooting steps when issues occur

## Troubleshooting

### Device Not Detected
- **Linux/macOS**: Ensure your user is in the `plugdev` group or has proper USB permissions
- **All platforms**: Ensure USB debugging is enabled (for ADB mode)
- Check USB cable and drivers
- Try different USB ports
- Manually boot to bootloader and retry

### Permission Issues (Linux/macOS)
```bash
# Add udev rules for Android devices (Linux)
sudo wget -S -O - http://source.android.com/source/51-android.rules | sudo tee >/dev/null /etc/udev/rules.d/51-android.rules
sudo udevadm control --reload-rules

# Add user to plugdev group
sudo usermod -a -G plugdev $USER
# Log out and back in for changes to take effect
```

### Script Hangs
- The script includes 5-second timeouts to prevent hanging
- Use Ctrl+C to exit if needed
- Check device connection and try again

### Flash Fails
- Verify the zip package is compatible with your device
- Ensure bootloader is unlocked
- Check available storage space
- Try a different USB cable

## Contributing

Feel free to submit issues, feature requests, or pull requests to improve the script.

## License

This script is provided as-is for educational and personal use. Use at your own risk.

## Disclaimer

⚠️ **Warning**: Flashing firmware can brick your device if done incorrectly. Always ensure you have the correct files for your specific device model. The authors are not responsible for any damage to your device.
