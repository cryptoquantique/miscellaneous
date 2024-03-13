


```sh
sudo nano /boot/firmware/cmdline.txt
```

add `cgroup_enable=memory cgroup_memory=1` at the end of the line

```sh
sudo reboot
```

run 
```sh
curl -sfL https://get.k3s.io | sh -`
```

To reinstall the agent, you will need to clear the `/etc/quarklink` directory:

```sh
sudo rm -rf /etc/quarklink
```
Then reinstall quarklink-agent (you will need to input your instance and its OEMRoot)
```sh
bash <(curl -fsSL https://agent.quarklink-staging.io)
```

download and install video-demo package
```sh
curl -sfL https://github.com/cryptoquantique/miscellaneous/raw/main/video-demo/video-demo.deb -o video-demo.deb


# TODO download instruction
sudo -E dpkg -i --force-all video-demo.deb
```

reboot

```sh
sudo reboot
```

Download the two deployment file from the following links:
  * Video demo purple - https://github.com/cryptoquantique/miscellaneous/blob/main/video-demo/deployment-purple.yaml
  * Video demo white - https://github.com/cryptoquantique/miscellaneous/blob/main/video-demo/deployment-white.yaml
  
And upload them to your quarklink instance via the UI