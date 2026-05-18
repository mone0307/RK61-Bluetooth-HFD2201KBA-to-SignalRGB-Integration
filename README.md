The Bluetooth version of the RK61 often comes with a mystery chip labeled HFD2201KBA. Research and community findings confirm that this chip is a rebadged/cloned SONiX SN32F24x series (specifically compatible with SN32F248 instructions).

By identifying this chip as a SONiX clone, we can successfully flash SonixQMK firmware to unlock advanced features and real-time RGB control.

By flashing this custom firmware, you will PERMANENTLY LOSE Bluetooth functionality. The HFD2201KBA chip will run QMK code optimized for USB HID communication. The keyboard will become a wired-only device.

🛠 Technical Specifications
MCU: HFD2201KBA (Identified as SONiX SN32F248 clone).

Firmware Base: SonixQMK with VIA support.

Communication: Raw HID via Interface 1.

Endpoint Configuration: UsagePage: 0xff60, Usage: 0x61.

## Phase 1: Hardware Verification

Confirm your PCB uses the HFD2201KBA chip. Research confirms this is a rebadged SONiX SN32F248B (Cortex-M0). If you see this label, you can proceed using SN32F248B firmware targets.

📥 Download Tools
Sonix Flasher (for SN32F248B):
https://github.com/SonixQMK/sonix-flasher/releases

Jumploader: use the jumploader-generic.bin file.

QMK + SignalRGB Firmware:
Search for royal_kludge or rk61 in these official repositories:
https://github.com/SRGBmods/QMK-Binaries/tree/main/QMK%2BVIA-Firmware/0.15.12-sonix

## Phase 2: Enter Bootloader Mode
The HFD2201KBA / SN32F248B requires a physical bridge to enter bootloader mode:

Unplug the USB cable.

Open the keyboard case (remove the bottom screws).

Locate the two test pads under the spacebar area, typically labeled BOOT and GND.

Short these two pads using a metal screwdriver or tweezers while plugging the USB cable into your PC.

Hold the short for 2-3 seconds, then release.

The keyboard LEDs will remain off and keys won't respond: you are now in Bootloader Mode ✅.

## Phase 3: Flashing Process

ATTENTION — Flash the Jumploader First
You MUST install the Jumploader before flashing the main QMK firmware. Failure to do so may result in an unusable device.

Install Jumploader:

Open Sonix Flasher and select Chip: SN32F248B.

Click "Refresh" to detect the device.

On the right column, click "Flash Jumploader".

Select the jumploader-generic.bin file.

Wait for the progress bar at the bottom to turn green. The keyboard will reboot automatically.

Re-enter Bootloader:

Unplug the cable.

Hold down Backspace (or Space on some revisions) while plugging the USB back in. This is the software-triggered bootloader enabled by the Jumploader.

Flash QMK Firmware:

On the left column, click "Flash QMK...".

Ensure QMK Offset is set to 0x00 (default).

When the file explorer opens, select your firmware: royal_kludge_rk61_rgb_via.bin.

Ecco la traduzione tecnica e ben formattata da aggiungere alla sezione Phase 1 o Phase 3 del tuo README. L'ho adattata per essere chiara e professionale:

!!!Is your keyboard ANSI or ISO!!!!
It is crucial to select the correct firmware binary based on your physical layout to ensure all keys and LEDs are mapped correctly:

ANSI Layout (Small horizontal Enter key - US Layout):
  Use royal_kludge_rk61_rgb_via.bin

ISO Layout (Large vertical "L-shaped" Enter key - European/Italian Layout):
  Use royal_kludge_rk61_rgb_iso_via.bin

Once finished, unplug and replug the keyboard.

## Phase 4: SignalRGB Integration & Final Configuration
Install the Plugin:
Copy the RK_61_Keyboard.js file to the SignalRGB plugins directory:
%ProgramData%\WhirlwindFX\SignalRGB\Plugins

Identify New Hardware IDs:
After flashing QMK, the keyboard's identifiers will change.

Open SignalRGB.

Go to Settings -> Device Information.

Look for your keyboard in the list and take note of the VID and PID displayed.

Update the Plugin Code:
Open the RK_61_Keyboard.js file with a text editor (like Notepad++ or VS Code) and update the following lines with the IDs you just found:

JavaScript
export function VendorId() { return 0xXXXX; } // Replace with your new VID
export function ProductId() { return 0xXXXX; } // Replace with your new PID
Note: Ensure you keep the 0x prefix for hexadecimal values.

Final Restart:
Restart SignalRGB. The software should now correctly identify the keyboard and start streaming lighting data. Everything should be working perfectly!

## Credit

SonixQMK Project: https://github.com/SonixQMK 
For their incredible work in porting QMK to SONiX MCUs and providing the essential flashing tools.

EEVblog Community: https://www.eevblog.com/forum/microcontrollers/hfd2201kba-any-details-on-this-microcontroller/
Specifically the technical discussion that helped identify the HFD2201KBA as a rebadged SONiX SN32F24x series.

🎓 Author
Simone Segala - Computer Science Student @ Università del Piemonte Orientale.
