# PS5 Bazzite Linux Gaming & Aurora Trainer Setup

A hands-on project documenting a Bazzite KDE Linux gaming environment on an original PlayStation 5, the boot process from external storage, Steam/Proton compatibility, and an unofficial community Aurora launcher workflow.

> **Scope and safety:** This repository documents the owner's setup and observations. It does not distribute console firmware, exploit files, jailbreak payload binaries, game files, proprietary trainer files, account credentials, or other copyrighted or sensitive material. Firmware-specific exploit behavior changes; verify the current documentation for your exact console and firmware before proceeding. This is a personal technical record, not a guarantee that the same process will work on every PS5.

## Project overview

The goal was to use an original PS5 as a Linux gaming system by booting Bazzite KDE from an external 2 TB Seagate M.2/SSD, then test Windows-oriented games and a third-party trainer workflow through Steam Play/Proton.

### Tested environment

| Component | Configuration |
|---|---|
| Hardware | Original PlayStation 5 |
| PS5 firmware tested | 11.60 |
| Linux distribution | PS5 Bazzite Deck, v2.7.3 image |
| Desktop | KDE Plasma |
| Linux boot storage | External 2 TB Seagate M.2/SSD |
| Loader-transfer storage | Separate 256 GB USB flash drive |
| Host computer used to prepare drives | Windows PC |
| Game platform | Steam |
| Compatibility layer | Steam Play / Proton |
| Tested game | Elden Ring |
| Controller | Victrix Pro BFG Reloaded Wireless |
| Community launcher | `aurora-launcher` (unofficial) |

Versions and compatibility can change. These results describe the setup tested during this project, not a guarantee for every release or hardware revision.

## Results

- Booted Bazzite KDE on an original PS5 from external storage.
- Used a Windows PC to download, extract, verify, and flash the Bazzite disk image.
- Used Payload Manager to import and launch the Bazzite Linux loader.
- Completed Steam's first-run update and signed in (Steam was preinstalled in the Bazzite image).
- Launched Elden Ring using Steam's default Proton selection.
- Cloned the community `aurora-launcher` repository.
- Prepared Python and GUI dependencies for the launcher.
- Installed the .NET 4.8 runtime in the relevant Wine/Proton environment through Winetricks.
- Opened Aurora and reached the Elden Ring trainer page.
- Documented a controller-input difference between normal Steam launch and a separate external trainer launcher.

---

# Part 1 — PS5 preparation and jailbreak entry

## 1. Confirm the console firmware first

The console used for this project was on **firmware 11.60**.

The jailbreak method referenced during setup has documentation covering PS5 firmware up to 13.60, but that does **not** mean every firmware from 3.00 through 13.60 uses the same exploit or is guaranteed to work. Some firmware versions require a different exploit path. Always check the current compatibility list for the exact firmware shown under:

**Settings → System → System Software → Console Information**

Do not update the console just to follow this guide. Keep automatic update downloads and automatic installation disabled before beginning.

## 2. Disable automatic system updates

From the PS5 home screen:

1. Open **Settings**.
2. Go to **System → System Software → System Software Update and Settings**.
3. Turn off **Download Update Files Automatically**.
4. Turn off **Install Update Files Automatically**.

The wording can vary slightly with system software revisions. Confirm both automatic download and automatic installation are disabled.

## 3. Configure the network connection

The tested setup used a **wired Ethernet connection**. Wi-Fi was not tested as part of this project.

Recommended hardware:

- An Ethernet cable long enough to reach from the router to the PS5. A 25-foot cable or longer provides flexibility if the console is not beside the router.
- A USB hub/dongle if the console's available USB ports are not sufficient for the external SSD, USB flash drive, keyboard, or other peripherals.
- Connect the Ethernet cable from the router to the PS5's LAN port.

The exploit guide used a manual DNS configuration to redirect the PS5 User Guide to the jailbreak page. The DNS address documented by the referenced guide was:

```text
45.56.67.85
```

On the PS5, open **Settings → Network → Settings → Set Up Internet Connection**, select the wired LAN connection, and open its advanced settings. Set DNS manually as directed by the current exploit guide. Do not guess the IP address, subnet mask, gateway, or other network values: those are different from the DNS server and depend on the local network. The network connectivity test may fail because the DNS configuration intentionally blocks Sony domains; that result is expected for this particular setup.

**Important:** This is the DNS value found in the referenced community guide, not a universal DNS setting. Confirm that the exploit guide you are following still recommends it before using it. Do not use it for ordinary networking after you are finished with the exploit workflow unless you understand its effect.

## 4. Open the PS5 User Guide

With the wired network and DNS configuration in place:

1. Return to the PS5 home screen.
2. Open **Settings → User's Guide, Health and Safety, and Other Information → User's Guide**. On some system versions the User Guide entry may appear under a slightly different Settings grouping.
3. The configured User Guide page should load the jailbreak landing page rather than the normal Sony help page.
4. If a certificate or security confirmation appears, read the prompt and confirm only if it is the expected prompt for the exploit page you intentionally configured. The exact prompt wording was not recorded in this project, so it should be checked against the current exploit instructions.

**Screenshot placeholder:** Add the user's screenshot of the jailbreak landing page here when supplied.

## 5. Run the jailbreak

The observed sequence on the tested console was:

1. On the jailbreak landing page, select **Jailbreak**.
2. Wait while the page initializes the exploit and kernel process.
3. The successful run displayed a completion message. The user's successful runs generally took about 5–10 seconds.
4. Press **Circle** to return to the jailbreak page/interface.
5. Confirm that the page reports the console is jailbroken (the observed page displayed “No more limits”).

A failed attempt may require returning to the User Guide page and trying again. If the console becomes unresponsive or the exploit fails, follow the current exploit maintainer's recovery instructions rather than repeatedly forcing power-off. A failed kernel exploit can leave the current boot unstable; use the normal PS5 shutdown process when it still responds, and consult the exploit guide if it does not.

## 6. Configure the payload autoloader for Linux-only use

This project is documenting **Linux boot only** at this stage. Emulation payloads and other optional payloads can be added in a later revision. The goal here is to keep the automatic startup list minimal so that etaHEN or kstuff do not start alongside the Linux loader.

The payload names discussed in this setup are **WebKit**, **etaHEN**, and **kstuff**. Download or make available the payload components required by the current jailbreak/payload guide, but distinguish between having a payload available in the payload list and having it enabled in the **Autoload** list.

1. Open the jailbreak page's **Payloads** area and locate the relevant payload entries.
2. Open the **Autoload** configuration.
3. For this Linux-only setup, leave **WebKit** enabled in Autoload.
4. **Do not enable etaHEN or kstuff in Autoload.** If either one was enabled previously—such as during an earlier emulation setup—remove/disable it from the Autoload list.
5. The **Payload Manager** entry may also be present in Autoload depending on the version and configuration being used. Keep it there only if your working Payload Manager setup requires it; WebKit is the required autoload entry recorded for this Linux-only stage.
6. Do not add the other emulation payloads yet. They will be documented separately when the emulation section is written.

Removing etaHEN and kstuff from **Autoload** does not necessarily mean deleting their downloaded files or entries from the general payload catalog. The important point for this Linux setup is that they must not be automatically started or running when the Bazzite loader is launched.

### If this PS5 was previously configured for emulation

If you previously enabled Autoload for emulation, review the Autoload list and remove/disable etaHEN, kstuff, and any other payloads that should not start during Linux boot. Then **restart the PS5 once** so the old automatically loaded payload state is cleared. After the restart, perform the jailbreak and Payload Manager initialization described below. If you never enabled those payloads, you do not need this cleanup/restart step just for this guide.

## 7. Run the jailbreak and initialize Payload Manager

**You do not need to repeat the jailbreak every time you open Payload Manager or launch Bazzite during the same PS5 session.** Run the jailbreak after the PS5 has started or restarted. Once it succeeds, downloading/launching PS5 Payload Manager lets the autoloader initialize for that session. Repeat this process only after another PS5 restart or shutdown.

After the PS5 has booted into its normal interface (and after the one-time cleanup restart above, if it was needed):

1. Open **User Guide** and load the configured jailbreak page.
2. If the expected certificate/security prompt appears, press **Yes**.
3. Select **Jailbreak** and wait for the completion message. The user's successful runs generally took about 5–10 seconds.
4. Press **Circle** to return to the jailbreak interface.
5. Open the **Payloads** area.
6. Locate **PS5 Payload Manager** and download/install it again.
7. Wait for the autoloader to initialize. In this setup, it automatically loads **`54321`**. Let this finish before opening Payload Manager.
8. Exit/back out to the normal PS5 interface.
9. Open the PS5 **Media** tab and launch **Payload Manager** there.
10. Check that the payload list has initialized correctly.

### How to recognize a successful initialization

When Payload Manager opens, look at the payload list:

- **Correct/ready:** Payload entries such as **ShadowMount Plus** and **Payload Manager** have their icons visible to the left of their names.
- **Not initialized correctly:** The payload names appear as text, but the icon areas to the left are black/blank and the icons have not loaded.
- **Opened too early:** Payload Manager may report that the payload ELF file is not loaded or may refuse to open properly.

If the ELF-not-loaded message appears, or the payload names appear without their icons, first make sure the jailbreak was run during the current PS5 session and that you waited for the `54321` autoloader initialization after downloading Payload Manager. If the PS5 has restarted since the jailbreak, repeat the sequence from User Guide. Do not keep rerunning the jailbreak during the same working session when the initialization has already succeeded.

The `54321` autoloader step and the visible payload icons are part of the user's observed working sequence. Exact on-screen wording can vary by release.

## 8. Payloads and settings relevant to Linux boot

For this Linux-only stage:

- **WebKit** should be the only required Autoload entry.
- **Payload Manager** may also be listed if the installed version/configuration requires it.
- **etaHEN and kstuff must not be enabled in Autoload or running** when starting the Linux loader.
- Do not enable the other emulation payloads yet.
- Keep Payload Manager available so the Linux loader can be selected.
- Configure **USB Power Supply in Rest Mode** to **Always**.
- Turn **HDMI Device Link** off.

The exact location of the USB power setting is generally **Settings → System → Power Saving → Features Available in Rest Mode → Supply Power to USB Ports**. Menu wording may vary. HDMI Device Link is under the HDMI/device settings.

Do not assume every payload in the general payload catalog is required. This guide records a minimal Linux-only Autoload configuration; the emulation payload list will be added in a later guide update.

---

# Part 2 — Prepare the Bazzite drives on Windows

## 9. Understand which drive does what

This setup used two separate external storage devices. They have different jobs.

| Device | Used for | Contents |
|---|---|---|
| External 2 TB Seagate M.2/SSD | Main Linux boot drive | The flashed Bazzite disk image, Linux partitions, operating system, Steam, games, and applications |
| Separate 256 GB USB flash drive | Transfer/import of the Linux loader | `bazzite-bootc-loader-v2.7.3.elf` |

A smaller USB flash drive (for example, 16 GB) should be sufficient if it is only being used to transfer the loader ELF. The 256 GB drive used in this project was larger than necessary for that purpose.

The Bazzite image is **not** copied onto the small USB as an ordinary file. It is flashed directly to the 2 TB external SSD using a disk-imaging tool.

## 10. Prepare a folder on the Windows PC

The drive-preparation process in this project used a Windows computer.

1. Create a folder such as:

```text
Documents\PS5-Bazzite
```

or:

```text
Downloads\PS5-Bazzite
```

2. Download the files for the same Bazzite release into that folder.
3. Keep the files for one release together. Do not mix an image from one release with a loader from another.

For the tested v2.7.3 release, the important files were:

- `ps5-bazzite-deck-bootc-v2.7.3.img.7z.001`
- `ps5-bazzite-deck-bootc-v2.7.3.img.7z.002`
- `ps5-bazzite-deck-bootc-v2.7.3.img.7z.003`
- `ps5-bazzite-deck-bootc-v2.7.3.img.sha256`
- `bazzite-bootc-loader-v2.7.3.elf`
- The loader's `.sha256` file, if provided with the release

Release names and the number of archive parts can change. Download every archive part listed for the release you actually choose.

## 11. Extract the split image archive with 7-Zip

Install 7-Zip on Windows if it is not already installed.

1. Confirm that all archive parts (`.001`, `.002`, `.003`, and any additional parts) are in the same folder.
2. Right-click the **`.001`** file.
3. Choose the 7-Zip extraction option.
4. Extract the archive. 7-Zip automatically reads the other numbered parts when they are present in the same folder.
5. The output should be one disk image file ending in `.img`.

The output is a **disk image (`.img`)**, not an ISO and not a ROM file.

Do not extract each numbered volume separately. Start the extraction from `.001` and leave all the other parts beside it until extraction has completed.

Do not delete the archive parts or checksum file until the extracted image has been verified and successfully flashed. Keeping the original downloads temporarily makes troubleshooting and re-creation easier.

## 12. Verify the download checksum

The `.sha256` file is used to verify that the downloaded files match the release publisher's published hashes.

In Windows PowerShell, calculate the hash of each downloaded archive part and compare it with the corresponding entry in the release checksum file:

```powershell
Get-FileHash .\ps5-bazzite-deck-bootc-v2.7.3.img.7z.001 -Algorithm SHA256
Get-FileHash .\ps5-bazzite-deck-bootc-v2.7.3.img.7z.002 -Algorithm SHA256
Get-FileHash .\ps5-bazzite-deck-bootc-v2.7.3.img.7z.003 -Algorithm SHA256
```

After extraction, verify the `.img` as well. From Command Prompt:

```bat
certutil -hashfile ps5-bazzite-deck-bootc-v2.7.3.img SHA256
```

Compare the resulting hash with the image hash in the `.sha256` file. Do not proceed if the hash does not match. Re-download the affected file and verify it again.

## 13. Flash the image to the 2 TB external SSD

Use **balenaEtcher** on Windows to flash the extracted `.img` file to the external 2 TB Seagate M.2/SSD.

1. Connect the 2 TB external SSD to the Windows PC.
2. Open balenaEtcher.
3. Select the extracted Bazzite `.img` file.
4. Select the **2 TB external SSD** as the target.
5. Double-check the selected target by its name and capacity.
6. Start the flash and wait for verification/completion.
7. If Windows later asks to format one of the Linux-created partitions, choose **Cancel**. Windows may not recognize Linux partitions; do not reformat the drive.

> **Data-loss warning:** Flashing a disk image erases the selected target drive. Check the drive name and capacity carefully before pressing Flash. Do not select the Windows system drive or a drive containing files you need.

The 2 TB SSD now contains the Bazzite disk image and Linux boot partitions. It is not just storing a copy of the `.img` file.

## 14. Prepare the small USB loader drive

1. Connect the separate USB flash drive to the Windows PC.
2. Copy the matching release's loader file to the root of the USB drive:

```text
bazzite-bootc-loader-v2.7.3.elf
```

3. Keep the loader filename intact.
4. Safely eject the USB drive.

The loader must match the Bazzite image release. Do not pair a v2.7.3 image with a loader from a different release unless the project's current instructions explicitly say they are compatible.

---

# Part 3 — Start Bazzite from the PS5

## 15. Connect the drives to the PS5

The connection order used in this project was:

- External 2 TB Seagate M.2/SSD connected to a rear USB port.
- Small USB flash drive containing the loader connected to a front USB port.

The user found this arrangement convenient. The order is not necessarily mandatory, but use the PS5 Bazzite project's current port requirements if they differ.

Connect the Ethernet cable and any keyboard/controller needed for setup.

## 16. Import the loader in Payload Manager

After the PS5 has been jailbroken again:

1. Open Payload Manager.
2. Use **Scan USB Payloads**.
3. Select **Get Payloads** (wording may vary by Payload Manager version).
4. Locate `bazzite-bootc-loader-v2.7.3.elf`.
5. Import it into Payload Manager's payload list.

In the user's workflow, the loader was imported from the USB drive and then made available as an icon in Payload Manager. If your Payload Manager version offers a copy/import-to-internal-storage option, use that so the small USB drive can be removed after the loader has been imported. The exact label and storage location should be confirmed against the version being used.

Once the loader is available in Payload Manager, the small USB drive is no longer needed for the boot itself if the loader has been successfully copied/imported into Payload Manager's internal list. Keep the 2 TB SSD connected.

## 17. Launch the Bazzite loader

Before launching:

- Confirm the 2 TB SSD with the flashed Bazzite image is connected.
- Confirm etaHEN and kstuff are not running.
- Confirm conflicting auto-start payloads are stopped.
- Confirm the loader is the payload being launched.

In Payload Manager, select the imported **Bazzite bootc loader** icon (the tested file was `bazzite-bootc-loader-v2.7.3.elf`).

The loader begins the Linux boot handoff. The PS5 may enter Rest Mode during this process.

## 18. Understand the PS5 LED during boot

The LED behavior documented by the PS5 Linux boot instructions is:

| LED state | Meaning / what to do |
|---|---|
| Blinking orange | The console is entering Rest Mode. Wait. |
| Solid orange | The console is in Rest Mode and ready to be woken. Press the physical power button once. |
| White | Linux has booted. Wait for the display and desktop to appear. |
| Returns to the normal PS5 OS | The wake may have happened too early, or the required Rest Mode USB power setting may not be enabled. |

The light may look yellow/amber depending on room lighting, but the boot documentation describes it as **orange**. Do not panic if the console turns off its display or enters Rest Mode during the handoff. Wait for the LED to become solid orange, then press the power button. There is no need to rush the button press; wait for the solid-orange state.

The first boot can take several minutes. Allow it time to complete.

## 19. First Bazzite boot and initial Steam setup

**Steam is already included in this Bazzite image. Do not install Steam separately.**

On the first boot:

1. Wait for Bazzite to finish loading.
2. Steam may automatically update itself. Allow the update to complete.
3. The screen may appear blank or temporarily unresponsive during the first boot. Give the system time to finish initializing before assuming the installation failed.
4. A blank or apparently unresponsive desktop can also happen if you do not have a keyboard and mouse connected. Connect them before troubleshooting the display or desktop.
5. A wired keyboard and mouse are recommended for initial setup.

### Keyboard and mouse recommendation

To conserve USB ports, a **wired keyboard with an integrated touchpad** is a convenient option. It combines keyboard and mouse control in one USB connection.

The setup used for this project was:

- One separate wired keyboard.
- One separate wired mouse.

Either arrangement can work. The integrated-touchpad keyboard is simply the recommended way to reduce the number of occupied USB ports.

Once Bazzite has finished loading and Steam has completed its initial update, the Steam sign-in screen should appear. Sign in with the Steam account you intend to use on this system.

Do not configure Proton compatibility or custom Steam launch options yet. Those steps come later, after the Aurora dependencies and GE-Proton tool have been prepared.

# Part 4 — Re-enter Bazzite after a PS5 reboot

The jailbreak and Payload Manager autoloader initialization are **per PS5 session**. If the PS5 has not restarted since you successfully ran the jailbreak and initialized Payload Manager, you do not need to run the jailbreak again just to launch Bazzite. Repeat the sequence below only after the PS5 restarts or shuts down.

1. Start the PS5 and return to the normal PS5 interface.
2. Open **User Guide**.
3. When the expected certificate/security prompt appears, press **Yes**.
4. On the jailbreak page, select **Jailbreak** and wait for the success/completion message.
5. Press **Circle** to return to the jailbreak interface.
6. Open **Payloads** and download/install **PS5 Payload Manager** again.
7. Wait for the autoloader to initialize and automatically load **`54321`**. Do not open Payload Manager before this has finished.
8. Exit/back out of the jailbreak and payload screens to the normal PS5 interface.
9. Open the **Media** tab and launch **Payload Manager**.
10. Confirm the payload list is fully initialized: icons should appear to the left of payload names such as ShadowMount Plus and Payload Manager.
11. Confirm **WebKit** is the only required Autoload entry for this Linux-only stage (Payload Manager may also be present if required by the installed configuration).
12. Confirm etaHEN and kstuff are not enabled in Autoload and are not running.
13. Confirm the 2 TB Bazzite SSD is connected.
14. Launch the imported Bazzite loader icon in Payload Manager.
15. Wait for the blinking-orange to solid-orange LED transition.
16. Press the physical power button once when the LED is solid orange.
17. Wait for Bazzite KDE to load.

If the Bazzite loader icon itself is missing after Payload Manager has initialized correctly, reconnect the small USB drive and use the **Scan USB Payloads / Get Payloads** workflow to import the matching loader ELF. Once the loader has been imported and is available internally, the small USB drive can be removed if the Payload Manager version supports that workflow.

Do not launch etaHEN or kstuff alongside the Linux loader. The PS5 Bazzite documentation warns that these can conflict with Linux boot.

# Part 5 — Steam, Aurora, Python dependencies, and GE-Proton

Steam is preinstalled in the PS5 Bazzite image. This section does **not** install Steam. It prepares the system for the community Aurora launcher and then configures the specific Steam game that will use it.

## 20. Confirm Steam is ready and establish a game baseline

1. Allow Steam's first-run update to finish.
2. Sign in to the Steam account.
3. Install the specific game you intend to use with Aurora.
4. Launch that game normally through Steam at least once before adding Aurora or custom launch options.
5. Confirm that the game reaches its normal playable state. This establishes a known-working baseline and helps separate game/Proton issues from Aurora-launcher issues.

Do not add the Aurora launch option until the game has successfully launched normally at least once.

## 21. Install ProtonUp-Qt and the latest GE-Proton

Bazzite includes Steam, but GE-Proton is a separate compatibility tool. Use ProtonUp-Qt to install it.

1. Open **Discover** in KDE Plasma.
2. Search for **ProtonUp-Qt** and install it.
3. Open ProtonUp-Qt.
4. Make sure the install target is **Steam**.
5. Choose **Add version**.
6. Set the compatibility tool to **GE-Proton**.
7. Select the latest GE-Proton version available for your system and install it.
8. Wait for installation to finish, then fully restart Steam so it detects the new compatibility tool.

The version used during this project was **GE-Proton 12-11**. New versions are released over time, so use the newest version available when following this guide rather than treating 12-11 as a permanent requirement.

## 22. Install the host Python dependencies

The Aurora launcher uses Python and a graphical interface. The host setup used Python, pip, Tkinter, and FreeSimpleGUI.

Bazzite is an atomic Fedora-based system. System-level Fedora packages should be layered with `rpm-ostree`, rather than using ordinary `dnf` directly on the host. Package layering creates a new deployment and requires a reboot; Bazzite recommends keeping host layering limited because it can affect updates.

On the **Bazzite host**, open Konsole and install the host packages together:

```bash
sudo rpm-ostree install python3-tkinter python3-pip
```

Reboot Bazzite once the package operation has completed.

Install the FreeSimpleGUI version used in this project:

```bash
python3 -m pip install --user FreeSimpleGUI==5.3.0.post1
```

Perform one Tkinter check on the host:

```bash
python3 -m tkinter
```

The Tkinter test window should open. The project environment had Python 3.15.0 and FreeSimpleGUI 5.3.0.post1 installed in the user's local Python package directory.

If `python3-pip` or `python3-tkinter` is already provided by the current Bazzite image, do not layer a duplicate package. The command above records the packages needed for this project; check the current Bazzite package state before layering anything already present. Bazzite package layering guidance: https://docs.bazzite.gg/Installing_and_Managing_Software/rpm-ostree/

## 23. Create the Fedora Distrobox and install Tkinter inside it

A separate Fedora Distrobox named `aurora-dev` was also created during setup. This gives the launcher a Fedora userspace environment without turning the container into the Bazzite host.

Create and enter the container:

```bash
distrobox create --name aurora-dev --image fedora:latest
distrobox enter aurora-dev
```

Inside the `aurora-dev` container, install Tkinter:

```bash
sudo dnf install python3-tkinter -y
```

The host and the Distrobox container are separate environments. Installing Tkinter in one does not install it in the other. This project used Tkinter on the Bazzite host and also installed Tkinter inside the Fedora Distrobox.

## 24. Clone the community Aurora launcher

Repository:

https://github.com/Merilairon/aurora-launcher

Open Konsole on the Bazzite host and clone the repository into the user's home directory:

```bash
cd ~
git clone https://github.com/Merilairon/aurora-launcher
cd ~/aurora-launcher
chmod -R ug+x .
```

The launcher entry point used in this setup is:

```text
/var/home/ps5/aurora-launcher/aurora
```

This is an **unofficial community launcher**. It is not the official Cheat Happens Aurora application and is not affiliated with or endorsed by Cheat Happens. Review the repository and its current instructions before running its code.

## 25. Configure GE-Proton and the Aurora launch option for the specific game

These settings are applied to the individual game you intend to use with Aurora, not globally to every Steam game.

1. In Steam, open **Library**.
2. Right-click the specific game and select **Properties**.
3. Open **Compatibility**.
4. Enable **Force the use of a specific Steam Play compatibility tool**.
5. Select the GE-Proton version installed through ProtonUp-Qt. In this project the tested selection was **GE-Proton 12-11**; choose the latest version you installed if the version number has moved on.
6. Open the game's **General** tab.
7. Find **Launch Options**.
8. Paste this exact launch option:

```text
"/var/home/ps5/aurora-launcher/aurora" %command%
```

Keep the quotation marks around the full Aurora path and keep `%command%` outside those quotation marks. The path shown is the path used on this project's Bazzite account (`ps5`). If the repository is stored under a different Linux username or directory, update only the path to match that installation; preserve the `%command%` token and its position.

## 26. First Aurora launch and initialization

After the dependencies, GE-Proton selection, and game-specific launch option are in place:

1. Start the game from its Steam Library page.
2. Aurora will begin initializing. The first launch can take a while.
3. The process may initialize Vulkan and other Wine/Proton components before the game and Aurora appear.
4. If Aurora presents a prompt asking whether to **Build** or **Download** a required patch/component, choose **Build** at that first prompt.
5. Aurora may then offer a recommended download or configuration intended to improve compatibility/performance. Choose the recommended **Download** option presented by Aurora and allow it to finish.
6. Wait for Aurora to report that setup/initialization completed successfully. Wine/Proton-related windows may appear during this process.
7. The game and Aurora should then open.

The exact prompt text and recommended component can change between launcher releases. This section records the sequence observed in the project; follow the current prompt text if a later version changes the workflow.

## 27. Keep Aurora open when the game-start warning appears

During the first launch, Aurora may display a message saying the game closed too quickly or asking whether Aurora should remain open.

1. Press **Alt+Tab** to bring the warning window into view if it is behind the game or another window.
2. When asked whether to keep Aurora open, choose **Yes**.
3. A second warning/console window (described during testing as the **BAT** warning/window) may explain that it must remain open while Aurora is running.
4. **Minimize** that window. Do not close it. Closing it can terminate the launcher process and close Aurora.

After minimizing the warning window, Aurora should remain available for sign-in and trainer selection.

## 28. Sign in and access trainers

A Cheat Happens account is required for account-based trainer features. Some trainer access also requires an active membership.

- Create or use an existing Cheat Happens account.
- Sign in through Aurora's supported login flow.
- Choose a membership plan if needed. The project account used a Lifetime membership; a monthly membership may also provide access depending on the provider's current plan terms.
- Once signed in and the game is detected, Aurora should display the trainer associated with the running game.
- Open the trainer and configure only the options you intend to use.

Membership terms, trainer availability, and plan entitlements can change. Check the provider's current plan details rather than assuming every trainer is included in every plan.

## 29. Optional: connect the Aurora mobile app

Aurora can also be paired with a mobile device through its mobile feature.

1. Install the supported Aurora mobile app on the phone.
2. In the desktop Aurora interface, select the **mobile** icon near the top of the window.
3. Follow the pairing instructions shown there.
4. The desktop app provides a separate mobile username/ID and password for the mobile connection. In the user's setup, these were numeric credentials and were different from the normal Cheat Happens account login.
5. Enter the mobile pairing credentials in the phone app.

Treat the mobile pairing credentials as private. They are not the same as the main website/Aurora account password.

## 30. Controller setup and external-launcher limitation

Connect a controller before starting the game. The controller used for this project was the **Victrix Pro BFG Reloaded Wireless Controller**.

Normal launch through Steam was the known-working controller path in this setup.

Some games or trainers may provide instructions to start the game through a separate external launcher. That can introduce a controller-input problem:

- Steam continued to recognize the controller.
- When the game was started by double-clicking the supplied external launcher from Dolphin, the game itself did not reliably receive controller input.
- The suspected cause is that the external launcher may not inherit the same Steam Input/Proton launch environment as a normal Steam launch. This remains a hypothesis, not a confirmed diagnosis.
- The issue is still unresolved in this project.

If a trainer requires a separate launcher, be aware that controller input may not work the same way as it does through Steam. Do not bypass or disable anti-cheat protections to solve this. For now, normal Steam launch is the controller-compatible baseline.

# Part 6 — Troubleshooting and observations

## PS5 jailbreak and Linux boot troubleshooting

| Symptom | What to check |
|---|---|
| User Guide opens the normal Sony help page | Check that the wired LAN connection and DNS settings match the current exploit guide. Verify the exact firmware and exploit compatibility. |
| Jailbreak page does not load | Recheck the network/DNS configuration and the current exploit instructions. Do not assume the same method supports every firmware. |
| Jailbreak attempt fails | Return to the exploit page and follow the exploit maintainer's retry/recovery instructions. A failed kernel exploit may leave the current boot unstable. |
| Payload list appears empty | Wait briefly, refresh/reopen the payload list, and check again. In the user's setup, scrolling/refreshing caused entries to populate. |
| Payload Manager/autoloader is not initialized | Confirm the jailbreak was run after the most recent PS5 restart, then download/install PS5 Payload Manager again and wait for the `54321` autoloader initialization before opening it from the Media tab. |
| Loader is missing from Payload Manager | Reconnect the USB containing the matching ELF and repeat Scan USB Payloads/Get Payloads. Confirm the filename and release match the image. |
| PS5 freezes or fails during Linux launch | Confirm etaHEN and kstuff are off and conflicting auto-start payloads are stopped. Confirm the loader matches the Bazzite image. |
| PS5 goes to Rest Mode during loader handoff | This is expected. Wait for solid orange, then press the physical power button once. |
| PS5 returns to the PS5 operating system | Check that the button was not pressed before solid orange and that USB power in Rest Mode is set to Always. |
| Linux boots but no display appears | A white LED can mean Linux is running even if the display is stuck. Consult the current PS5 Linux troubleshooting guide before forcing shutdown. |
| Windows asks to format the flashed SSD | Cancel. Windows may not recognize the Linux partitions. Do not reformat the drive. |
| Bazzite does not boot after flashing | Recheck the image checksum, ensure all archive parts were present, extract from `.001`, and flash the `.img` to the intended external SSD. |

## Steam/Aurora troubleshooting

| Symptom | Finding / next check |
|---|---|
| Game launches without Aurora but not with it | Preserve the known-working default Proton baseline and isolate the launcher/prefix issue. |
| `rundll32.exe` could not be started | Observed with GE-Proton 12-11 in this setup. Default Proton launched the game successfully. |
| Tkinter missing | Check the Python environment actually used by the launcher. Host Python, Distrobox Python, and a launcher-created virtual environment are distinct. |
| FreeSimpleGUI missing | Check whether the launcher is using its own virtual environment; a host-level pip install may not satisfy it. |
| `dotnet48` prompt appears | Install it into the intended prefix using Winetricks, then reuse that same prefix. |
| Prefix initialization warning | Avoid deleting/recreating prefixes as a first response. Inspect the launcher's log and identify the prefix in use. |
| Aurora reports that the game closed too quickly | This may be a detection-timing issue if the game actually starts. Keep Aurora open only if the game is running and the launcher remains usable. |

## External trainer launcher: controller compatibility (unresolved)

During testing of **Elden Ring Nightreign**, the trainer instructions supplied a separate Windows launcher executable (`EldenRingNightreignLauncher.exe`) to start the game outside its normal Steam launch path.

Observed behavior on this PS5/Bazzite setup:

- Launching Nightreign normally through Steam: the connected Victrix controller worked.
- Launching Nightreign by double-clicking the supplied external launcher from the game's folder in Dolphin: controller input did not work reliably in-game.
- The issue was specific to that external-launcher method in the tests so far; it has not been established as a controller hardware fault.
- Steam's interface could still respond to input, while the game itself did not reliably receive controller input.

**Status: unresolved / fix to be determined.** One possible explanation is that launching the Windows executable outside Steam does not preserve the same Steam Input and Proton launch environment as starting the game through Steam. This is a hypothesis, not a confirmed root cause.

For now, normal Steam launch is the known-working controller baseline. Do not assume that an external trainer launcher will preserve controller support for a connected PS5 or other gamepad. Further troubleshooting is needed to determine whether a supported launch method can retain controller input without bypassing the game's anti-cheat protections.

This finding is specific to the tested configuration and may differ with other controllers, Proton versions, launcher versions, or game updates.

## Nightreign trainer observations

The trainer page observed during testing displayed App Version 1.03.3. The user's notes included:

- Rune and level editor values appeared to take effect fully only after a character death or run reset.
- Unlimited flasks appeared to work: the displayed flask count reached 20 and did not decrement when used.
- Invisible Player appeared to work in some situations; enemies initially ignored the player until provoked or aggroed.
- HP/FP/stamina-related options and automatic ultimate-skill recharge did not appear to work reliably in the tested environment.
- The user manually edited some stat fields, so those edited values should not be treated as the game's original/default stat values.

Trainer value editing and runtime trainer hooks are different functions. A value appearing in an editor does not prove the live game process accepted or retained it. Compatibility may be partial and can change with game, trainer, and Proton updates. These are observations, not confirmed explanations of the trainer's internal behavior.

---

# Part 7 — Security and responsible-use notes

- Restrict trainer use to offline, single-player modes.
- Do not use trainers in multiplayer, competitive, or anti-cheat-protected sessions; doing so can result in account penalties or bans.
- Do not attempt to bypass or disable anti-cheat protections.
- Back up files before changing anything in a game directory.
- Do not execute trainer attachments from untrusted sources.
- Do not commit game executables, proprietary trainer files, jailbreak payload binaries, account credentials, or tokens to GitHub.
- Review the relevant game, platform, exploit-project, and trainer-provider terms before use.
- Jailbreaking and console modification can carry security, stability, warranty, service, and legal risks. This guide is not legal advice.

---

# Reproducibility checklist

## PS5 and Linux boot

- [ ] Record the exact PS5 model and firmware.
- [ ] Confirm the exploit method supports that exact firmware.
- [ ] Disable automatic update downloads and installation.
- [ ] Configure wired LAN/DNS according to the current exploit guide.
- [ ] Successfully open the User Guide exploit page.
- [ ] Run the jailbreak and confirm its success message.
- [ ] Install/open Payload Manager.
- [ ] Configure the relevant WebKit Autoloader behavior.
- [ ] Ensure etaHEN and kstuff are off before Linux boot.
- [ ] Set USB power in Rest Mode to Always.
- [ ] Turn HDMI Device Link off.
- [ ] Download all matching Bazzite image archive parts, checksum, and loader.
- [ ] Verify the downloaded files.
- [ ] Extract the `.001` archive with 7-Zip.
- [ ] Flash the extracted `.img` to the external SSD with balenaEtcher.
- [ ] Import the matching loader ELF into Payload Manager.
- [ ] Wait for solid orange before pressing the power button.
- [ ] Confirm KDE Plasma loads.

## Steam and Aurora

- [ ] Steam's first-run update completes and the user signs in.
- [ ] The target game launches normally through Steam once before custom launch options are added.
- [ ] ProtonUp-Qt is installed and the latest GE-Proton version is installed for Steam.
- [ ] The host Python dependencies are installed once; Tkinter opens on the host.
- [ ] The `aurora-dev` Fedora Distrobox is created and Tkinter is installed inside it.
- [ ] The community launcher repository has been cloned and reviewed.
- [ ] The target game's Compatibility and Launch Options settings are configured.
- [ ] Aurora initializes, the required Build/Download prompts are completed, and its warning windows are handled without closing the BAT window.
- [ ] Aurora opens and the trainer page loads.
- [ ] Trainer use is restricted to offline single-player testing.

---

# Resume-ready project summary

**PS5 Linux Gaming & Windows Compatibility Lab**

Configured a Bazzite Linux gaming environment on PlayStation 5 hardware using external SSD storage. Prepared and verified a Linux disk image on Windows, documented the console-to-Linux boot workflow, and evaluated Windows game compatibility through Steam Play/Proton. Integrated and troubleshot an unofficial Python-based Aurora community launcher, investigated Python GUI dependencies and Wine prefixes, compared Proton configurations to isolate launch failures, and documented controller compatibility findings and safe offline testing practices.

### Skills demonstrated

- Linux desktop administration (Bazzite/Fedora ecosystem)
- Command-line troubleshooting
- Windows disk-image preparation and checksum verification
- External storage and boot troubleshooting
- Git and GitHub documentation
- Python environments, virtual environments, and GUI dependencies
- Wine/Proton compatibility troubleshooting
- Steam configuration and application testing
- Technical documentation and reproducibility

## References

- PS5 Bazzite Deck releases: https://github.com/VivaLaVent/ps5-bazzite-deck/releases
- PS5 Linux Wiki — Bazzite Deck: https://wiki.ps5-linux.eu/choose/bazzite-deck/
- PS5 Linux Wiki — Linux loader: https://wiki.ps5-linux.eu/getting-started/loader/
- PS5 Linux Wiki — Booting Linux: https://wiki.ps5-linux.eu/installation/booting/
- PS5 jailbreak reference guide: https://github.com/C0rn3j/PS5
- Bazzite: https://bazzite.gg/
- Bazzite package layering documentation: https://docs.bazzite.gg/Installing_and_Managing_Software/rpm-ostree/
- ProtonUp-Qt (GE-Proton installer/manager): https://github.com/DavidoTek/ProtonUp-Qt
- ProtonUp-Qt on Flathub: https://flathub.org/en/apps/net.davidotek.pupgui2
- Steam Play / Proton: https://github.com/ValveSoftware/Proton
- Community Aurora launcher: https://github.com/Merilairon/aurora-launcher
- Cheat Happens: https://www.cheathappens.com/

---

*This document records one tested setup. Update hardware, software versions, and results as the environment changes. Distinguish confirmed observations from hypotheses, and do not describe unverified steps as successful results.*
