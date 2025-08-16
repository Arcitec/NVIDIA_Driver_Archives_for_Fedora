# NVArchive: NVIDIA Driver Archives for Fedora.

## What is this?

This is a tool (NVArchive) that helps you build and maintain an archive of NVIDIA's drivers for Fedora, so that you always have a safe and easy way to reinstall working drivers.

It was created due to NVIDIA sometimes releasing buggy drivers that break your system. If you're lucky, they'll just give you terrible performance or broken games. But in the worst cases, their bugs are so severe that you may not even have any display output anymore. This tool was created after the author was greeted by a soft-bricked computer with a completely black screen, and had finally had enough of NVIDIA's shenanigans.

With this tool, you can always log in and restore your previous, working drivers again with one or two simple commands. You can even install and use this tool via the non-graphical terminal, in an emergency situation.

You can now relax and update Fedora with the confidence that you'll always be able to fix your system, no matter what NVIDIA does to it!


## Compatibility

- Supports **DNF5** (Fedora 41+) and **DNF4** (Fedora 40 and older).
- Supports all current *and **future*** versions of Fedora, thanks to using intelligent NVIDIA driver package dependency resolution via DNF.
- Supports **all variants** of the NVIDIA driver (including modern and legacy GPUs).


## Rescuing Unbootable Systems

If you've installed a broken NVIDIA driver, and you're no longer able to reach the desktop, then follow these steps to very easily rescue your system.

*You can skip this whole section and go to "Installing NVIDIA Drivers from the Archive" if you're already able to see the computer desktop.*

1. Reboot the machine.
2. Hold **Shift** during the startup process, to make the GRUB menu appear.
3. Select (highlight) the latest kernel, or whichever kernel you want to use.
4. Press **E** to "edit" its boot options.
5. Go down to the long `linux`-line with kernel flags, and **remove** *everything* related to *Nouveau, Nova and NVIDIA,* to ensure that you'll boot in 2D mode with Nouveau/Nova. The most common list of flags that you'll have to remove are `rd.driver.blacklist=nouveau,nova_core modprobe.blacklist=nouveau,nova_core nvidia-drm.modeset=1`. There may be others in the future. Just look for the words "nouveau", "nova" and "NVIDIA" and remove them.
6. Press **Ctrl + X** to boot with your changes. The changes are not saved permanently, so you don't have to worry about that.
7. You should now see the desktop, thanks to the open-source Nouveau/Nova drivers. It will be running in a low-performance mode, but it will be fast enough to let you do the driver installation.

Alternatively, if you aren't able to reach the desktop even with Nouveau/Nova, then you should boot in terminal mode (aka "single-user mode"), and do the installation process directly via the terminal.

1. The steps for single-user mode are almost the same. Do steps 1-5 from the previous list.
2. Add the word `single` at the end of the `linux`-line.
3. Do step 6 from the previous list.
4. You will now be at a terminal login prompt, where you can then log in with your username and password manually.

No matter which method you've used, you should now open a Terminal and use this tool to install the desired NVIDIA drivers. See the next section below for instructions.


## Installing NVIDIA Drivers from the Archive

1. If your current NVIDIA drivers are broken and you can't reach the desktop, then you should follow the steps in the "Rescuing Unbootable Systems" section first.
2. Open a Terminal window.
3. Ensure that you have `git`, and then clone this repository to a local directory.
```sh
sudo dnf install git
git clone "https://github.com/Arcitec/NVIDIA_Driver_Archives_for_Fedora.git"
```
4. Go into the project's directory, and execute the "install-deps" command to set up all of the remaining dependencies. **Warning:** If you skip/forget this step, then you won't be able to download the RPM packages (the archived drivers). So be sure to install the dependencies!
```sh
cd NVIDIA_Driver_Archives_for_Fedora
./nvarchive install-deps
```
5. Now just execute the command without any parameters, and then **read its help guide very carefully.** Everything is explained in detail. Read the **"Quick Start Guide"** section of the help-text to quickly learn how to use the tool.
```sh
./nvarchive
```
6. Have fun! :-)


## A Note About Immutable Desktops (Silverblue and Kinoite)

This tool might work for you too, if you've enabled DNF layering for the NVIDIA driver package. However, it hasn't been tested on such systems, since I personally don't use any immutable OS. Pull requests for improved compatibility are welcome (if any changes are necessary).

However, if the NVIDIA driver ever breaks for you on an immutable OS, then you can always revert your latest `rpm-ostree` upgrade to the previous state. How to do that is beyond the scope of this project, since that's a feature of the immutable OS itself.
