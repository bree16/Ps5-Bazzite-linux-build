# PS5 Bazzite Setup — Step-by-Step Checklist

A condensed execution checklist based on the comprehensive project guide. Use the comprehensive guide for explanations, release-specific caveats, and troubleshooting.

> **Important:** This checklist records one tested setup: original PS5, firmware 11.60, PS5 Bazzite Deck v2.7.3, and an external 2 TB SSD. Exploit compatibility and software releases change. Confirm the current instructions for your exact firmware and the exact Bazzite release before proceeding. Do not treat any DNS value or exploit path as universal.

## A. Before you begin

- [ ] Record the PS5 model and exact firmware: **Settings → System → System Software → Console Information**.
- [ ] Confirm that the current exploit instructions support that exact firmware.
- [ ] Do not update the PS5 just to follow this guide.
- [ ] Turn off automatic system software downloads and installation.
- [ ] Gather:
  - [ ] Windows PC.
  - [ ] External SSD for Bazzite (the tested setup used a 2 TB Seagate M.2/SSD).
  - [ ] Separate USB flash drive for the loader ELF (a small drive is sufficient; the tested drive was 256 GB).
  - [ ] Wired Ethernet cable (25 ft or longer is convenient when needed).
  - [ ] USB hub/dongle if you need more ports.
  - [ ] Wired keyboard and mouse, or preferably a wired keyboard with an integrated touchpad.
  - [ ] Controller.
- [ ] Back up anything important on the external SSD and USB drive. Flashing the SSD will erase it.

## B. Prepare Bazzite on Windows

- [ ] Download the Bazzite image archive parts, matching `.sha256` checksum, and matching loader ELF from the same release.
- [ ] Keep all split archive parts in the same folder.
- [ ] Confirm the image and loader versions match.
- [ ] Use 7-Zip to extract from the `.001` file only.
- [ ] Confirm the extracted result is a `.img` disk image.
- [ ] Verify the downloaded files and extracted image against the published SHA-256 checksums.
- [ ] Do not proceed if a checksum does not match.
- [ ] Open balenaEtcher.
- [ ] Select the extracted `.img` file.
- [ ] Select the external SSD by checking its name and capacity carefully.
- [ ] Flash the image and wait for completion/verification.
- [ ] If Windows asks to format a Linux partition, choose **Cancel**.
- [ ] Copy the matching loader ELF to the root of the separate USB flash drive.
- [ ] Safely eject both drives.

## C. Prepare the PS5

- [ ] Connect the PS5 to the network using wired Ethernet (the tested setup used Ethernet).
- [ ] Configure network/DNS exactly as directed by the current exploit guide. Do not copy an old DNS value without confirming it is still recommended.
- [ ] Open **Settings → User's Guide** (menu wording/location can vary).
- [ ] Confirm the expected jailbreak landing page opens.
- [ ] Accept a certificate/security prompt only if it is the expected prompt for the page you intentionally configured.
- [ ] Select **Jailbreak**.
- [ ] Wait for the completion message (about 5–10 seconds in the tested setup).
- [ ] Press **Circle** to return to the jailbreak interface.
- [ ] Open **Payloads**.
- [ ] Download/install **PS5 Payload Manager** again.
- [ ] Wait for the autoloader to load **`54321`**.
- [ ] Exit to the normal PS5 interface.
- [ ] Open Payload Manager from the PS5 **Media** tab.
- [ ] Confirm payload icons appear beside entries such as ShadowMount Plus and Payload Manager.
- [ ] If the ELF-not-loaded message appears or icons are blank/black, repeat the full sequence from User Guide. Do not continue with a partially initialized list.
- [ ] Set **Supply Power to USB Ports** in Rest Mode to **Always**.
- [ ] Turn **HDMI Device Link** off.
- [ ] Make sure **etaHEN** and **kstuff** are not running before Linux boot.

## D. Import the loader and boot Bazzite

- [ ] Connect the external Bazzite SSD to a rear PS5 USB port.
- [ ] Connect the loader USB to a front port.
- [ ] In Payload Manager, choose **Scan USB Payloads**.
- [ ] Choose **Get Payloads** (wording can vary).
- [ ] Import the loader ELF that matches the Bazzite image release.
- [ ] Confirm the Bazzite loader icon appears in Payload Manager.
- [ ] Keep the external SSD connected.
- [ ] If the loader has been imported into Payload Manager's internal list, remove the small USB only if the current Payload Manager workflow supports doing so.
- [ ] Launch the Bazzite loader.
- [ ] Wait while the PS5 LED blinks orange.
- [ ] When the LED becomes solid orange, press the physical power button once.
- [ ] Wait for the LED to turn white and for Bazzite to finish booting. First boot can take several minutes.

## E. First Bazzite desktop and Steam sign-in

- [ ] Connect a wired keyboard and mouse. A wired keyboard with integrated touchpad saves a USB port; the tested setup used separate wired keyboard and mouse.
- [ ] Do not assume a temporarily blank/unresponsive screen means the boot failed; allow first-boot initialization to finish.
- [ ] Steam is **preinstalled** in this Bazzite image. Do not install it separately.
- [ ] Allow Steam to update itself.
- [ ] Sign in when the Steam login screen appears.
- [ ] Do not configure Proton or custom launch options yet.

## F. Repeat the boot process after a PS5 reboot

- [ ] Return to the normal PS5 interface.
- [ ] Open User Guide and accept the expected certificate/security prompt.
- [ ] Select **Jailbreak** and wait for success.
- [ ] Press **Circle**.
- [ ] Open **Payloads** and download/install **PS5 Payload Manager** again.
- [ ] Wait for autoloader **`54321`** to finish.
- [ ] Exit to the PS5 interface.
- [ ] Open Payload Manager from **Media**.
- [ ] Confirm the payload icons are visible.
- [ ] Confirm etaHEN and kstuff are off.
- [ ] Confirm the Bazzite SSD is connected.
- [ ] Launch the Bazzite loader.
- [ ] Wait for solid orange, then press the physical power button once.
- [ ] Wait for Bazzite to load.

## G. Prepare Steam, GE-Proton, Python, and Aurora

### Steam and GE-Proton

- [ ] Confirm Steam has completed its first-run update and you are signed in.
- [ ] Install the target game.
- [ ] Launch it normally through Steam once and confirm it reaches a playable state.
- [ ] Install **ProtonUp-Qt** from KDE Discover.
- [ ] In ProtonUp-Qt, set the target to **Steam**.
- [ ] Choose **Add version → GE-Proton**.
- [ ] Install the latest GE-Proton version available.
- [ ] Restart Steam so it detects the new compatibility tool.

### Python and Distrobox

- [ ] On the Bazzite host, install `python3-tkinter` and `python3-pip` with `rpm-ostree` only if they are not already present. Avoid layering duplicate packages.
- [ ] If packages were layered, reboot once after the operation completes.
- [ ] Install the project-tested FreeSimpleGUI version:
  ```bash
  python3 -m pip install --user FreeSimpleGUI==5.3.0.post1
  ```
- [ ] Run the one-time host Tkinter check:
  ```bash
  python3 -m tkinter
  ```
- [ ] Create and enter the Fedora Distrobox:
  ```bash
  distrobox create --name aurora-dev --image fedora:latest
  distrobox enter aurora-dev
  ```
- [ ] Inside the container, install Tkinter:
  ```bash
  sudo dnf install python3-tkinter -y
  ```

### Clone Aurora and configure the game

- [ ] Review the unofficial community Aurora launcher repository: https://github.com/Merilairon/aurora-launcher
- [ ] On the Bazzite host, clone it:
  ```bash
  cd ~
  git clone https://github.com/Merilairon/aurora-launcher
  cd ~/aurora-launcher
  chmod -R ug+x .
  ```
- [ ] In Steam Library, open the target game's **Properties → Compatibility**.
- [ ] Enable **Force the use of a specific Steam Play compatibility tool**.
- [ ] Select the GE-Proton version installed through ProtonUp-Qt.
- [ ] Open **Properties → General → Launch Options**.
- [ ] Enter the exact launch option:
  ```text
  "/var/home/ps5/aurora-launcher/aurora" %command%
  ```
- [ ] Keep the quotes around the Aurora path and keep `%command%` outside the quotes. Change the path only if your Linux username or install directory differs.
- [ ] Start the game from Steam.
- [ ] During first initialization, choose **Build** at the first Build/Download prompt.
- [ ] Choose the recommended **Download** option when Aurora offers it.
- [ ] Wait for Aurora to report successful initialization.
- [ ] If the “game closed too quickly” prompt appears, use **Alt+Tab** and choose **Yes** to keep Aurora open.
- [ ] Minimize the second BAT/console warning window. **Do not close it** while Aurora is running.
- [ ] Sign in to Cheat Happens through Aurora.
- [ ] Confirm your membership includes the trainer access you want; plan terms can change.
- [ ] Optional: pair the mobile app using the separate mobile ID/password shown by desktop Aurora. Do not confuse these with the main account login.

## H. Controller and trainer-use notes

- [ ] Connect the controller before launching the game.
- [ ] Normal Steam launch was the known-working controller path with the Victrix Pro BFG Reloaded Wireless controller in this project.
- [ ] A separate external trainer launcher opened from Dolphin did not reliably pass controller input to the game in testing. The cause is unconfirmed.
- [ ] Do not assume an external launcher will preserve Steam Input.
- [ ] Use trainers only in offline single-player modes.
- [ ] Do not use trainers in multiplayer, competitive, or anti-cheat-protected sessions.
- [ ] Do not bypass or disable anti-cheat protections.
- [ ] Do not upload game executables, proprietary trainer files, jailbreak payload binaries, passwords, or tokens to GitHub.

## I. Completion notes to record

- [ ] Record the exact PS5 model and firmware.
- [ ] Record the Bazzite image and loader versions.
- [ ] Record the external SSD model/capacity.
- [ ] Record the GE-Proton version.
- [ ] Record the target game's version and whether it launches normally.
- [ ] Record Aurora initialization results and any warnings.
- [ ] Separate confirmed results from hypotheses or unresolved issues.
- [ ] Update the comprehensive guide when software versions or steps change.
