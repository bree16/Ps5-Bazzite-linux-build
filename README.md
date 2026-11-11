# PS5 Bazzite Linux Lab

![Platform](https://img.shields.io/badge/platform-PS5%20(original)-003791)
![Firmware](https://img.shields.io/badge/tested%20firmware-11.60-informational)
![OS](https://img.shields.io/badge/OS-Bazzite%20KDE%20v2.7.3-8a2be2)
![Docs](https://img.shields.io/badge/type-documentation-success)

**Turn an original PlayStation 5 into a Linux gaming PC that runs Steam and Windows games.**

This project shows how I booted **Bazzite** (a gaming version of Linux, similar to what the Steam Deck runs) on my PS5 from an external SSD, signed into Steam, and played Windows games like *Elden Ring* through **Proton**, Valve's tool that lets Windows games run on Linux. I also tested a community launcher for the Aurora trainer app and wrote down what worked, what didn't, and what's still unsolved.

You don't need to be a Linux expert to follow along. If you can copy files to a USB drive and type a few commands, you can do this. Plan on an afternoon the first time. After that, getting back into Linux takes about five minutes.

> [!WARNING]
> **Check your firmware before you do anything.** This only works on PS5 firmware that the current jailbreak supports. Go to **Settings → System → System Software → Console Information** and write down your version. **Do not update your PS5**, and turn off automatic updates now. One update can lock you out of this whole process.

---

## Contents

- [What you need](#what-you-need)
- [How it works (the short version)](#how-it-works-the-short-version)
- [Step 1: Prepare the drives on a Windows PC](#step-1-prepare-the-drives-on-a-windows-pc)
- [Step 2: Get the PS5 ready](#step-2-get-the-ps5-ready)
- [Step 3: Boot into Linux](#step-3-boot-into-linux)
- [Step 4: Set up Steam and games](#step-4-set-up-steam-and-games)
- [Getting back into Linux later](#getting-back-into-linux-later)
- [When something goes wrong](#when-something-goes-wrong)
- [What I tested](#what-i-tested)
- [Full documentation](#full-documentation)
- [Disclaimer](#disclaimer)

---

## What you need

- **An original PS5** on a firmware version the jailbreak supports (mine was 11.60)
- **An external SSD** for Linux. I used a 2 TB Seagate. *Everything on it will be erased.*
- **A small USB flash drive** for one small file. Any size works.
- **A Windows PC** to prepare the drives
- **An Ethernet cable** long enough to reach your router (I used a 25 ft one)
- **A wired keyboard and mouse.** A keyboard with a built-in touchpad saves a USB port.
- **A USB hub** if you run out of ports
- **A controller** for playing

---

## How it works (the short version)

The PS5 can't boot Linux by itself. First you **jailbreak** it, which unlocks the console so it can run outside programs (called *payloads*). One of those payloads is the **Bazzite loader**. When you run the loader, the PS5 goes to sleep, and when you wake it up, it starts Linux from your external SSD instead of the normal PS5 system.

Your PS5's own storage is never touched. Linux lives entirely on the external SSD, and when you restart, the PS5 goes back to normal.

---

## Step 1: Prepare the drives on a Windows PC

Download these files from the **same release** on the [PS5 Bazzite Deck releases page](https://github.com/VivaLaVent/ps5-bazzite-deck/releases): every image archive part (`.7z.001`, `.7z.002`, and so on), the `.sha256` checksum file, and the loader file (`.elf`). Put them all in one folder. The image and the loader **must** come from the same version, or Linux won't boot.

1. **Extract the image.** Install [7-Zip](https://www.7-zip.org/), right-click the **`.001`** file only, and extract it. 7-Zip pulls the other parts in automatically. You end up with one `.img` file.
2. **Make sure the download isn't broken.** Open PowerShell in that folder and run:
   ```powershell
   Get-FileHash .\your-image-name.img -Algorithm SHA256
   ```
   The result has to match the number in the `.sha256` file exactly. If it doesn't, download it again.
3. **Write the image to the SSD.** Open [balenaEtcher](https://etcher.balena.io/), pick the `.img` file, pick your **external SSD** (check the name and size carefully, because this erases it), and click **Flash**.
4. **If Windows asks to format the drive afterward, click Cancel.** Windows just doesn't understand Linux drives. Formatting would wipe your work.
5. **Copy the loader `.elf` file** onto the small USB flash drive, then safely eject both drives.

---

## Step 2: Get the PS5 ready

This is where you jailbreak the console and set it up so Linux can start cleanly.

**Network.** Plug the PS5 into your router with Ethernet. In the network settings, set the DNS the way the [current jailbreak guide](https://github.com/C0rn3j/PS5) tells you to. Don't copy a DNS address from an old post without checking it's still the right one. The connection test may fail afterward. That's expected.

**Jailbreak.** Open **Settings → User's Guide**. Instead of Sony's help page, the jailbreak page should load. Accept the security prompt if it's the one you expected, press **Jailbreak**, and wait about 5 to 10 seconds for the success message. Press **Circle** to go back.

**Autoload.** In the **Payloads** area, open **Autoload**. Leave **WebKit** turned on. Make sure **etaHEN** and **kstuff** are **turned off**. Those two are useful for other things but can freeze the PS5 when Linux tries to start. If you turned them on before, turn them off and restart the PS5 once.

**Payload Manager.** Still in **Payloads**, download **PS5 Payload Manager** again and wait for the autoloader to finish loading **`54321`**. Then exit to the PS5 home screen and open **Payload Manager** from the **Media** tab. You'll know it loaded correctly when you see **icons** next to names like *ShadowMount Plus*. If the icons are black or blank, it wasn't ready yet. Go back and wait for `54321` to finish.

**Two settings.** Go to **Settings → System → Power Saving → Features Available in Rest Mode** and set **Supply Power to USB Ports** to **Always**. Then turn **HDMI Device Link** off. Linux needs both of these to start properly.

> [!NOTE]
> You only need to jailbreak **once each time the PS5 is turned on**. If you haven't restarted the PS5, you don't have to do it again.

---

## Step 3: Boot into Linux

Plug the **SSD** into a back USB port and the **small USB** into a front port.

In Payload Manager, choose **Scan USB Payloads**, then **Get Payloads**, and import the loader file. A Bazzite icon appears in the list. Make sure etaHEN and kstuff aren't running, then **launch the Bazzite loader**.

Now watch the light on the PS5:

| The light is... | What it means | What you do |
|---|---|---|
| **Blinking orange** | The PS5 is going to sleep | Wait |
| **Solid orange** | It's asleep and ready | Press the **power button once** |
| **White** | Linux is starting | Wait for the desktop to appear |

**Don't press the button while it's still blinking.** If the PS5 goes back to its normal home screen, you pressed too early, or the USB power setting isn't set to Always.

The first boot can take several minutes, and the screen may look frozen for a while. Give it time, and make sure your keyboard and mouse are plugged in.

---

## Step 4: Set up Steam and games

**Steam is already installed in Bazzite.** Don't install it again. Let it update, sign in, install a game, and **play it once normally** before changing anything. That way you know the game works on its own, so if something breaks later you know it wasn't the game.

If you want more game compatibility, open **Discover** (the app store), install **ProtonUp-Qt**, set the target to **Steam**, and add the newest **GE-Proton**. Restart Steam, then pick GE-Proton for a game under **Properties → Compatibility**.

The Aurora trainer launcher setup (Python, Tkinter, Distrobox, launch options, and first-run prompts) is longer, so it's in the [full guide](docs/PS5_Bazzite_Comprehensive_Guide.md#part-5--steam-aurora-python-dependencies-and-ge-proton).

---

## Getting back into Linux later

After the PS5 has been turned off or restarted, the steps are short:

1. Open **User's Guide** and press **Jailbreak**, then **Circle**
2. In **Payloads**, download **PS5 Payload Manager** and wait for **`54321`**
3. Open **Payload Manager** from **Media** and check the icons loaded
4. Make sure the SSD is plugged in and launch the **Bazzite loader**
5. Wait for **solid orange**, press **power once**, and wait for Linux

If the Bazzite icon is missing, plug the small USB back in and import the loader again.

---

## When something goes wrong

| Problem | Try this |
|---|---|
| User's Guide shows Sony's normal help page | Recheck the Ethernet and DNS settings, and confirm your firmware is supported |
| Payload Manager says the ELF isn't loaded, or icons are blank | You opened it too early. Wait for `54321` to finish, then reopen it |
| PS5 freezes when starting Linux | etaHEN or kstuff is running. Turn both off in Autoload and restart the PS5 |
| PS5 goes back to the normal home screen | You pressed power before solid orange, or USB power in Rest Mode isn't set to Always |
| Light is white but the screen is blank | Linux may be running. Wait, and plug in a keyboard and mouse |
| Linux won't boot at all | Check the image matches its checksum and the loader is from the same version |
| Windows wants to format the SSD | Click **Cancel** every time |
| Controller doesn't work in a game | Start the game from Steam. Launching it from a separate launcher outside Steam didn't pass controller input in my tests |

There's a longer troubleshooting section, including Steam and Aurora problems, in the [full guide](docs/PS5_Bazzite_Comprehensive_Guide.md#part-6--troubleshooting-and-observations).

---

## What I tested

| | |
|---|---|
| Console | Original PS5, firmware 11.60 |
| Linux | PS5 Bazzite Deck v2.7.3 (KDE Plasma desktop) |
| Storage | External 2 TB Seagate SSD |
| Compatibility | Steam Play / Proton, GE-Proton 12-11 |
| Games | *Elden Ring*, *Elden Ring Nightreign* |
| Controller | Victrix Pro BFG Reloaded Wireless |

**Worked:** booting Linux from the SSD, Steam sign-in, running *Elden Ring* on default Proton, and opening the Aurora trainer page.

**Still unsolved:** when a game is started from a separate launcher instead of Steam, the controller stops reaching the game. My best guess is that the launcher doesn't get Steam's controller setup, but that isn't confirmed.

---

## Full documentation

- **[Comprehensive guide](docs/PS5_Bazzite_Comprehensive_Guide.md):** every step explained, with the reasons behind each one, the Aurora setup, troubleshooting, and test notes
- **[Step-by-step checklist](docs/PS5_Bazzite_Step_by_Step_Checklist.md):** the same process as checkboxes, for when you just want to follow along

### Built with

[PS5 Bazzite Deck](https://github.com/VivaLaVent/ps5-bazzite-deck) · [PS5 Linux Wiki](https://wiki.ps5-linux.eu/) · [Bazzite](https://bazzite.gg/) · [Proton](https://github.com/ValveSoftware/Proton) · [ProtonUp-Qt](https://github.com/DavidoTek/ProtonUp-Qt) · [aurora-launcher](https://github.com/Merilairon/aurora-launcher) (unofficial community project)

---

## Disclaimer

This repository contains **documentation only**. It does not include firmware, jailbreak files, payloads, game files, trainer files, or passwords. It records one setup that worked for me, and it may not work on other firmware or software versions.

Jailbreaking can affect your console's stability, warranty, and access to PlayStation services. Use trainers only in **offline, single-player** games. Never use them online or try to get around anti-cheat. You can get banned. Follow at your own risk.
