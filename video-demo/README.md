# Video Demo

## What you need
1. Raspberry Pi
   1. Monitor
   2. Keyboard
   3. Internet connectivity
2. QuarkLink instance

## Setup
>Note: The following instructions are for a Raspberry Pi running Raspberry Pi OS 12 (bookworm).

You will need to type the commands listed in this section into a Terminal. This can either be directly on the Raspberri Pi or via an ssh session.

**Install K3s**
1. Edit the file `/boot/firmware/cmdline.txt`
    ```sh
    sudo nano /boot/firmware/cmdline.txt
    ```
2. Add `cgroup_enable=memory cgroup_memory=1` at the end of the line.
  It should look similar to this:
    ```
    console=serial0,115200 console=tty1 root=PARTUUID=0e7df85f-02 rootfstype=ext4 fsck.repair=yes rootwait quiet splash plymouth.ignore-serial-consoles cfg80211.ieee80211_regdom=GB cgroup_enable=memory cgroup_memory=1
    ```
3. Save the content and close the editor
     - Press `CTRL+X`
     - Type `Y`
     - Press `Enter`
4. Reboot   
    ```sh
    sudo reboot
    ```
1. Once rebooted, run the following command to install K3s
    ```sh
    curl -sfL https://get.k3s.io | sh -
    ```

**Re-install the quarklink-agent**
>**Important note:** if you are not using a TPM or Secure Element, when re-installing the agent your device will generate new keys, causing the device ID to change.

1. Clear the `/etc/quarklink` directory:
    ```sh
    sudo rm -rf /etc/quarklink
    ```
2. Re-install the quarklink-agent (you will need to input your instance and its OEMRoot)
```sh
bash <(curl -fsSL https://agent.quarklink-staging.io)
```

**Install the video-demo package**
1. Download the package
    ```sh
    curl -sfL https://github.com/cryptoquantique/miscellaneous/raw/main/video-demo/video-demo.deb -o video-demo.deb
    ```
2. Install the package
    ```sh
    sudo -E dpkg -i --force-all video-demo.deb
    ```
3. Reboot
    ```sh
    sudo reboot
    ```

[WIP]
Download the two deployment file from the following links:
  * Video demo purple - https://github.com/cryptoquantique/miscellaneous/blob/main/video-demo/deployment-purple.yaml
  * Video demo white - https://github.com/cryptoquantique/miscellaneous/blob/main/video-demo/deployment-white.yaml
  
And upload them to your quarklink instance via the UI