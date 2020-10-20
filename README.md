# SSJ - Your everyday Linux distribution gone Super Saiyan.
![ssj](https://raw.githubusercontent.com/thirdbyte/ssj/main/ssj.png)

## Introduction
SSJ is a silly little script that makes use of **docker** installed on your everyday Linux distribution (Ubuntu, Debian, etc.) and magically arms it with hundreds of penetration testing and forensics tools. All of these run with almost native performance (as containers utilize the host kernel) and thus is a slightly better alternative to Virtual Machines in terms of speed, performance and convenience.

## Technical Details
SSJ is a Docker image that uses `kalilinux/kali` as the base image and installs `google-chrome`, `firefox-esr`, `sublime-text`, `tmux`, `kali-linux-large`, etc. packages. It uses the `kali.download/kali` mirror and `kali-last-snapshot` branch. It also allows you to run GUI applications like Burpsuite, Wireshark, Ettercap, etc. from within the container on your everyday Linux distribution using `--privileged` docker capabilities and `--net=host` argument. This script builds the docker image and creates a `.desktop` file (the Application Launcher) for you. So, the only thing you need to do is, find SSJ in you aplicaiton drawer/menu and click on it to launch it. An `xfce4-terminal` will popup with all your pentesting and infosec tools in it. Execute `burpsuite` to fire up the proxy, `firefox` to fire up the browser and like that, you have access to hundreds of tools and packages that are there in Kali Linux (particularly the `kali-linux-large` metapackage), right on your everyday Linux distribution. Also, contrary to virtual machines, that are either networked behind a virtual NAT or bridged along with the host operating system, SSJ utilizes the host network stack as it is (using `--net=host`) which means that the SSJ container will have direct access to all the network interfaces as the host and will also share the same IP address.

This script is just an extension to [demon-docker](https://github.com/thirdbyte/demon-docker). SSJ goes a few steps ahead to make the setup super easy and convenient for you. 

## Prerequisites
+ Docker (User must be in the `docker` group)
+ Internet connection

## Installation
`wget https://raw.githubusercontent.com/thirdbyte/ssj/main/ssj.sh && chmod +x ssj.sh && sudo ./ssj.sh`

This might take half an hour to full depending upon your Internet speed. The script needs to download 3-4G of data.

## Usage
1. Access the application drawer/menu on your Linux distribution to find SSJ.
2. Launch SSJ.
3. An `xfce4-terminal` will pop up.
4. Use this terminal to launch any tool by executing them using their respective package names. For an example: `msfconsole`, `burpsuite`, `chromium`, `wireshark`, etc.
5. You can save any file in the `/root` directory inside the container and find it at `/home/ssj` on your host Linux distribution.

## Screenshots

**Tested on:** Ubuntu 20.04.1 LTS

![Application Launcher](https://raw.githubusercontent.com/thirdbyte/ssj/main/screenshots/ssj_ss_application_launcher.png)

![Burpsuite](https://raw.githubusercontent.com/thirdbyte/ssj/main/screenshots/ssj_ss_burpsuite.png)

![Wireshark](https://raw.githubusercontent.com/thirdbyte/ssj/main/screenshots/ssj_ss_wireshark.png)

![Ettercap](https://raw.githubusercontent.com/thirdbyte/ssj/main/screenshots/ssj_ss_ettercap.png)

![Metasploit & Nmap](https://raw.githubusercontent.com/thirdbyte/ssj/main/screenshots/ssj_ss_msf_nmap.png)

## Troubleshooting
+ The Kali Linux repositories are updated very frequently. Sometimes, when the packages are being migrated to the `/kali` repository, you might get a `404` error finding some packages while the image is building. The only way to resolve this as of now is to wait a few hours and try again.
+ Since the container runs with the root user, the files created in the `/root` directory have the owner set to root. On the host, this directory is `/home/ssj`. All the files and sub directories inside `/home/ssj` will require the root user on the host in case any data needs to be written to or deleted from this directory.
+ If the `firefox-esr`/`firefox` is executed from within the container at the time there is already an instance of it running on the host, the new instance of the Firefox Browser will be created from the `firefox-esr`/`firefox` installed on the host and not that installed inside the container. You can avoid this by renaming `default-esr` profile for the `firefox-esr`/`firefox` on your host by executing `firefox -p` and then selecting `default-esr` and renaming it to somthing else and hitting `Start Firefox`.

## Why use SSJ?
+ Requires lesser amount of disk space (around 10G) as compared to a VM.
+ Faster and more convenient than a VM.
+ Everything runs as if it is running on your host Linux distribution both in terms of performance and experience.
+ SSJ is isolated from host Linux distribution in terms of filesystem (except `/root` which is mounted from `/home/ssj` on the host) and processes. This means that whatever you do inside the SSJ container will not interfere with anything on your host Linux distribution.


## Limitations
+ Wireless hacking tools that require a patched kernel, the one that is found in Kali Linux, will not work on SSJ. The simple reason for that is SSJ utilizes the Linux kernel of your host machine which isn't patched or modified to support packet injection.
+ SSJ uses docker `--privileged` capabilities and `--net=host`. It also adds a universal access control to `xhost` for making GUI applications work, but immidiately closes it once SSJ's `xfce4-terminal` is exited. This might allow any application to access the X server or GUI in particular for the time SSJ is running which can be a security or a privacy concern for many.
+ Audio ouput does not work as of now.

## And...
This script was created out of curiosity. This might solve a lot of problems. This might create new ones as well. It comes with no commitments. You are solely responsible for anything you may wish to do with this script. You can still feel free to file issues in case you experience any of them. Cheers!
