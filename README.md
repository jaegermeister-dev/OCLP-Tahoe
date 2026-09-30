# OCLP-Tahoe

Run macOS Tahoe on selected unsupported MacBook Pro models.

OCLP-Tahoe is an independently maintained project built on OpenCore and OpenCore Legacy Patcher. It is **not affiliated with the official OpenCore Legacy Patcher team or subreddit**. Please bring OCLP-Tahoe support requests here or to [r/OCLP_Tahoe](https://www.reddit.com/r/OCLP_Tahoe/), not to the official OCLP support channels.

> Private release draft for maintainer review. No release download has been published in this repository yet.

## Supported hardware and macOS

| Target | Status |
| --- | --- |
| **MacBookPro11,5** — 15-inch Mid 2015, Intel Iris Pro and AMD Radeon R9 M370X | Primary tested model. A fresh USB creation, installation, EFI installation, root patching and final boot have completed successfully. |
| **MacBookPro11,4** — 15-inch Mid 2015, Intel Iris Pro only | **Experimental and untested on actual 11,4 hardware.** Intel Iris Pro has worked on the 11,5, but this does not establish complete 11,4 compatibility. |
| Other Mac models | Not supported by this release. |

The root patcher targets **macOS Tahoe 26.7.1, build 25G241**. Do not assume newer or older Tahoe builds are compatible. This application does not create installers or apply root patches for Big Sur, Monterey, Ventura, Sonoma or Sequoia.

## Read before starting

- **Follow every installation step below, in order, even if a step does not seem important.** In particular, selecting the correct EFI at each stage matters.
- Back up your files and retain a way to boot a working macOS installation. Do not erase your only recovery option.
- Use a **32 GB or larger USB drive**. The stock Tahoe 26.7.1 installer is approximately **18.4 GB**; the completed USB uses approximately **22 GB**, including OCLP-Tahoe, its offline patch assets and the EFI. Creating the installer erases the selected USB drive.
- Connect the Mac to power. Allow enough free internal storage for the installer download, extraction and macOS installation.
- Internet is needed to obtain the application and download the macOS installer. **Root patching is self-contained and requires no downloads**: the application contains its required patch assets and is also bundled onto the installer USB. A working Wi-Fi connection or Ethernet dock is not required to apply root patches.
- USB creation from **Monterey** has been tested successfully. Other host macOS versions have not all been tested.
- **Sleep/wake is not working reliably.** Disable automatic system sleep before leaving the patched system unattended. Do not use manual Sleep or close the lid to put it to sleep. Turning off the display alone is not the same as disabling system sleep.
- Do not install macOS version updates until this project explicitly supports them. See [Updates](#updates).

### The two EFI choices

| Choice | When to use it |
| --- | --- |
| **Installer/First Boot** | On the USB: boots the Tahoe installer, installation restarts and the first unpatched Tahoe boot. Based on the captured official OCLP 3.0.0 beta-generated OpenCore 1.0.5 EFI. |
| **Installed Tahoe** | On the internal SSD: installed after reaching the Tahoe desktop, before applying root patches. Reproduces the tested installed-system EFI. |

Do not substitute one for the other. **Before installing an EFI, carefully check both the selected EFI option and the destination drive:** install **Installer/First Boot to the USB drive**, and **Installed Tahoe to the internal SSD** after reaching the Tahoe desktop.

The macOS installer and operating-system installation use Apple's installer. The USB additionally carries the OCLP-Tahoe application and offline assets; root patches are applied afterwards, not during the macOS installation itself.

### Use OCLP-Tahoe to create the USB

**Use OCLP-Tahoe's Create macOS Installer feature rather than creating the USB manually with `createinstallmedia`.** Manually creating the installer and separately building/installing its EFI could work, but this is **not the tested or recommended route**.

Using `createinstallmedia` on its own does not add OCLP-Tahoe and its offline patch assets to the USB. You would need to arrange those separately, install OCLP-Tahoe yourself on the new Tahoe installation, manually build and install the **Installed Tahoe** EFI to the internal SSD, and then apply the root patches. Do not assume the automatic prompts from the tested procedure will be available on a manually prepared USB.

Unpatched Tahoe can be extremely slow, making those extra manual steps time-consuming. **Creating the USB through OCLP-Tahoe is simpler:** it bundles the application and offline assets and prepares the guided installation process used in the successful test. Follow that route for this release.

## Installation instructions

### 1. Create the USB and install its EFI

1. On your working macOS installation, install OCLP-Tahoe using its installer `.pkg`, then open the application. Monterey was used for the successful test.
2. Use **Create macOS Installer** to download/select Tahoe 26.7.1 and create the installer on your USB drive. Double-check which drive you select: it will be erased.
3. Wait for creation to finish. **The maintainer's successful USB creation took over 40 minutes.** A long creation time can be normal; elapsed time alone does not mean it has frozen. Keep the USB connected and wait for completion or an explicit error.
4. After the USB has been created, OCLP-Tahoe asks whether you want to build/install OpenCore. Clicking **Yes automatically selects Installer/First Boot**. Check that this is the selected EFI option, then install it to the **same USB drive**, not the internal SSD at this stage. Carefully check the destination drive before confirming, and wait for the installation-success message.

### 2. Install Tahoe using the USB EFI throughout

5. Restart while holding **Option (Alt)**. In Apple's startup picker, select the **USB EFI**, then select the Tahoe installer in OpenCore.
6. Install Tahoe onto your intended destination volume. Check the destination carefully; do not erase unrelated volumes or your recovery installation.
7. The installer will restart the Mac multiple times. **On every restart, hold Option and select the USB EFI again.** Within that OpenCore session, continue the installation entry for the target volume; later, boot the installed Tahoe volume. Do not accidentally use an existing internal EFI during these stages.

### 3. Complete Setup Assistant

8. Proceed through Setup Assistant. **Skip signing in with an Apple ID** for this installation procedure; Apple services have not been validated.
9. At the software-update choice, select **Download Automatically**, rather than the default **Continue** choice. This keeps automatic update installation off from the start, saving you from having to disable it after booting into Tahoe and reducing the risk of an unintended macOS update. Downloads remain enabled, but updates should not install automatically. The unpatched setup can be extremely slow.
10. In the successful test, Setup Assistant remained stuck at this exact step. After waiting approximately **one minute**, the maintainer held the power button until the Mac turned off. On the next boot, Tahoe reached the login screen. **If you encounter that same persistent Setup Assistant hang, this is the workaround used in the tested sequence.** If setup progresses normally, let it finish. A forced power-off is not risk-free: do not use this workaround while macOS installation, EFI installation or root patching is running.
11. Start the Mac holding **Option**, select the **USB EFI**, then boot the installed Tahoe volume and log in.

### 4. Install the internal EFI, then the root patches

12. Accept the popup offering to install OpenCore to the internal drive. **Installed Tahoe** should already be highlighted. Confirm that profile, build it and install it to the **internal SSD**. Wait for success, then accept the reboot prompt.
13. During this reboot, hold **Option** and explicitly select the **internal OpenCore EFI**, then boot Tahoe and log in. From this point onward, use the internal installed-system EFI, not the USB Installer/First Boot profile.
14. Accept the popup to open OCLP-Tahoe and install the root patches. Enter the administrator password when requested, and wait until patching reports successful completion. Do not interrupt it.
15. Click **Reboot** when patching finishes. Boot Tahoe through the internal OpenCore EFI again. This completed the successful fresh-install test.

If an EFI build/install or root-patching step reports an error, **stop at that step and save the error/log for support**. Do not treat a failed operation as completed or continue rebooting in the hope that it succeeded.

## What has been confirmed working

These observations are from the **MacBookPro11,5** development and acceptance tests. The latest fresh installation also completed the sequence above successfully; that does not mean every individual feature below was separately retested in that final run.

| Area | Confirmed observations |
| --- | --- |
| Installation | USB creation on Monterey, USB EFI installation, installer boots and restarts, first unpatched boot, internal EFI installation, root patching and patched desktop boot. |
| Graphics | AMD M370X acceleration; Intel Iris Pro recognized with Metal support and tested rendering/compute; internal display operation through the Intel framebuffer. This is not a claim of validated automatic GPU switching or complete AMD power-off. |
| Displays and HDMI | Laptop display; external display output and HDMI audio through both the laptop HDMI port and the tested dock; HDMI unplug/replug; internal Retina scaling. |
| Icons and interface | Application icons, Settings panes, Light/Dark/Clear/Tinted icon appearances, and Dock right-click menus. The previously observed login-screen beach-ball issue has also been addressed. |
| Wi-Fi | Networks detected and connection established. See the remaining label caveat below. |
| Audio | Internal speakers and microphone; headphone output and microphone input through the laptop 3.5 mm jack; tested dock 3.5 mm input/output. |
| Other hardware | Bluetooth, tested dock Ethernet, keyboard and function keys, keyboard backlight adjustment, trackpad and SD card reader. This does not establish every Bluetooth profile or every third-party dock. |
| Camera | Normal live camera picture in a browser webcam test. Photo Booth is a separate known issue. |
| Power reporting and cooling | Model-specific CPU/GPU reporting, dynamic CPU behaviour, temperature sensors, automatic fan response and active thermal protection observed under the measured conditions. Macs Fan Control sensor readings and fan adjustment also worked. This does not validate every power state or battery endurance. |
| Other software | Siri worked. A standalone Safari update through Software Update completed successfully. |

## Known issues and limitations

- **Sleep/wake can leave the Mac unresponsive and require a hard restart.** Keep automatic system sleep disabled and avoid initiating sleep. Closing the lid while an external display remains active is not proof that sleep/wake works. Sleep repair is deferred to a later update.
- **Photo Booth crashes**, despite the camera working in the browser test. Photo Booth repair is deferred.
- **Setup Assistant can hang at the update-choice screen.** The specific tested workaround is documented above.
- A normally broadcast Wi-Fi network was incorrectly labelled **Hidden Network**. A saved-profile correction persisted in development testing, but a general fix and final visual acceptance have not been established. Connectivity worked. Apple's **Known Networks / Other Networks** grouping is normal and is not a defect.
- Some interface operations and initial icon loading can still be slow. No promise of native-supported-Mac performance is made. A performance optimiser is planned separately and is **not included** in this release.

## What is not yet confirmed

- Actual **MacBookPro11,4** operation, including its display routing and external outputs. If you test this model, report how far it boots and whether the problem occurs before or after root patching.
- Replacement **NVMe internal SSD** configurations. The tested machine's internal SSD appears under SATA in System Information.
- Comprehensive automatic graphics switching, complete M370X power-off, battery endurance, and extended graphics or thermal stress testing.
- Apple ID/iCloud, App Store, Apple Music, iMessage, FaceTime and other Apple services. The individual Siri and Safari results above do not validate all Apple services.
- Every older macOS installation booting through either generated EFI, including every official-OCLP-patched point release. Upstream EFI ancestry alone is not proof of all those combinations. Keep a known-working recovery boot option.
- Using OCLP-Tahoe to create the USB on macOS versions other than Monterey. It may work, however it has not been tested. If you have issues creating the USB using another macOS version, it is advised to use Monterey.
- Rejection of a real, newly offered macOS version update by the included update protection. Do not rely on that protection as permission to attempt an unsupported update.

## Updates

**Do not install macOS version updates**, including another 26.7.x release, until OCLP-Tahoe explicitly supports the exact version/build. Do not enable automatic installation of macOS updates. A working current installation does not prove that its patches will work after an OS update.

This restriction concerns **macOS itself**, not a blanket ban on standalone Safari updates. A Safari update was successfully tested. That success does not establish macOS version-update compatibility.

## Uninstaller

The companion OCLP-Tahoe uninstaller removes the OCLP-Tahoe application and its application integration. It **does not undo root patches, remove the installed EFI, or delete patch-version records, backups and diagnostic state**. It does not uninstall official OpenCore Legacy Patcher. Removing the app is not a way to make a patched system safe for an unsupported macOS update.

## Support

- [OCLP-Tahoe GitHub issues](https://github.com/jaegermeister-dev/OCLP-Tahoe/issues)
- [OCLP-Tahoe subreddit](https://www.reddit.com/r/OCLP_Tahoe/)

Include your model identifier, macOS version/build, OCLP-Tahoe version, which EFI profile you used, the stage that failed, and the exact error or relevant logs. Review logs for personal information before posting. For an early boot failure, a clear phone recording of the verbose screen can be useful when disk logging has not started.

Please **do not send OCLP-Tahoe bug reports to the official OCLP team or subreddit**. This is a separate project with its own support channels.

Credit to the [OpenCore](https://github.com/acidanthera/OpenCorePkg) and [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher) projects and the upstream components on which this work builds.
