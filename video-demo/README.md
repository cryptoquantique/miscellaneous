# Video Demo

## What you need
1. Raspberry Pi
   1. Monitor
   2. Keyboard
   3. Internet connectivity
2. QuarkLink instance
3. Laptop or computer to access the QuarkLink instance and upload the firmware

## Setup
> Note: The following instructions are for a Raspberry Pi running Raspberry Pi OS 12 (bookworm).

You will need to type the commands listed in this section into a Terminal. This can either be directly on the Raspberry Pi or via an ssh session.

**Install K3s**
1. Edit the file `/boot/firmware/cmdline.txt`
    ```sh
    sudo nano /boot/firmware/cmdline.txt
    ```
2. Add the following at the end of the line.
    ```
    cgroup_enable=memory cgroup_memory=1
    ```
  It should look similar to this:
    ```
    console=serial0,115200 console=tty1 root=PARTUUID=0e7df85f-02 rootfstype=ext4 fsck.repair=yes rootwait quiet splash plymouth.ignore-serial-consoles cfg80211.ieee80211_regdom=GB cgroup_enable=memory cgroup_memory=1
    ```
1. Save the content and close the editor
     - Press `CTRL+X`
     - Type `Y`
     - Press `Enter`
2. Reboot   
    ```sh
    sudo reboot
    ```
3. Once rebooted, run the following command to install K3s
    ```sh
    curl -sfL https://get.k3s.io | sh -
    ```

**Re-install the quarklink-agent**
>**Important note:** if you are not using a TPM or Secure Element, when re-installing the agent your device will generate new keys, causing the device ID to change.

1. Clear the `/etc/quarklink` directory:
    ```sh
    sudo rm -rf /etc/quarklink
    ```
2. Re-install the quarklink-agent (you will need to input your instance and its OEMRoot). Full instructions can be found [here](https://docs.quarklink.io/docs/provision-new-device-with-quarklink-agent).
    ```sh
    bash <(curl -fsSL https://agent.quarklink-staging.io)
    ```

    The script will prompt you for the endpoint and the root certificate. Your endpoint will look something like this:

    ```shell
    <your-instance>.quarklink.io
    ```

    > NOTE: You should not include the `https://` in the endpoint.

    The root certificate can be found by logging into your quarklink instance, navigating to `HSM` and selecting the `OEMRoot` certificate. From there press `See` and click the copy button to copy the certificate to your clipboard and paste it into the terminal when prompted.

    The script will then install the Quarklink agent and create a deviceID for you to add to your quarklink instance. The output will look something like this:

    ```
    Device provisioned with ID: b81e1af6b13f35c8ead2ca916b93ccfde2dc27d6f742624350f3b0d5a13cfc94
    Add device to batch to successfully run the agent
    Exiting...
    ```

    You should now add your device to your quarklink instance. Once successfully added to a batch, the agent should enrol the device. 

    To view the logs of the agent at any time run the following command:

    ```bash
    journalctl -u quarklink-agent-go -f
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

**Upload firmware**

This should be done from a separate computer (such as a laptop)

1. Download the two deployment file from the following links:
     * **Video demo purple** - https://github.com/cryptoquantique/miscellaneous/blob/main/video-demo/deployment-purple.yaml
     * **Video demo white** - https://github.com/cryptoquantique/miscellaneous/blob/main/video-demo/deployment-white.yaml
  
2. Upload them to your quarklink instance via the UI