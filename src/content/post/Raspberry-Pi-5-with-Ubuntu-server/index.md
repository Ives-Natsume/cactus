---
title: "Raspberry Pi 5 with Ubuntu Server"
publishDate: "14 September 2024"
updatedDate: "14 September 2024"
description: "A brief introduction about how to install Ubuntu Server 0n Raspberry Pi 5, set up envs, and deploy servers."
tags: ["blog", "Ubuntu"]
coverImage:
    src: "./kumiko.png"
    alt: "kumiko"
---
Recently I bought a Raspberry Pi 5 with 8GB RAM. It is convinced that the processor of Pi 5 is much powerfull than its elder brothers. Thus it would be a fancy choice for buiding a lite personal server.

![Raspberry Pi 5](./pi-init.jpg)

## Manage netplan config file

Once ssh connected, run

```{Bash}
sudo nano /etc/netplan/01-<file-name>.yaml
```

 > Note that `.yaml` file demands for indentation.

By editing the file you should be able to manage Pi's IP preciously.

## Add SSD on your Pi

1. **Get disk info**

```{bash}
sudo fdisk -l
```

2. **Start partition**

Now suppose that you have a hardware named `nvme0n1`, use the `fdisk` tool to partition the `nvme0n1` hard drive:

```{Bash}
sudo fdisk /dev/nvme0n1
```

>- Press `n` for a new partition.
>- Choose `primary` in most case.
>- Repeating press `n` to make multiple partition.
>- Press `w` to exit `fdisk`
  
3. **Format partition**

After the partitioning is complete, a file system needs to be created for the new partition. Assuming the partition you created is `/dev/nvme0n1p1`, you can use the `mkfs` command to format it.

For example, format to ext4 file system:

```{bash}
sudo mkfs.ext4 /dev/nvme0n1p1
```

4. **Mount the new partition**

Create a mount point and mount the new partition to the specified location:

```{bash}
sudo mkdir /mnt/mydisk
sudo mount /dev/nvme0n1p1 /mnt/mydisk
```

5. **Manage automatic mounting**

Edit the `fstab` file to ensure that the partition is automatically mounted on system startup:

```{bash}
sudo nano /etc/fstab
```

Add this line to the bottom:

```{text}
/dev/nvme0n1p1 /mnt/mydisk ext4 defaults 0 0
```

Save and exit run the command to check if everything works well:

```{bash}
sudo mount -a
```

 > If eveything works well, there will have no output.

## Install Miniforge3 for Python envs

Since Raspberry Pi uses ARM as their arch, thus `Miniforge3` performs better on them than conda.

1. **Download Miniforge3**

Run the command to get Miniforge3 from GitHub:

```{Bash}
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-aarch64.sh
```

2. **Install Miniforge3**

Give permissions to the script and run for installation.

```{Bash}
$ chmod +x Miniforge3-Linux-aarch64.sh
$ ./Miniforge3-Linux-aarch64.sh
```

Follow the guidance to finish installation.

3. **Activate conda env**

Finally, time to activate conda env, just simply run:

```{bash}
source ~/.bashrc
```

Now conda env should be successfully installed and activated on your machine.

## Tool: Gopeed hi-speed downloader

[Gopeed](https://github.com/GopeedLab/gopeed) is a high speed downloader which supports Magnet, https and many other download protocol.

For Raspberry Pi, we would need a [arm46 version](https://github.com/GopeedLab/gopeed/releases/download/v1.5.9/gopeed-web-v1.5.9-linux-arm64.zip, "Github release, click to download.").

To install the software, run:

```{bash}
$ # Download software
$ wget https://github.com/GopeedLab/gopeed/releases/download/v1.5.9/gopeed-web-v1.5.9-linux-arm64.zip


$ # Unzip
$ unzip gopeed-web-v1.5.9-linux-arm64.zip


$ cd gopeed-web-v1.5.9-linux-arm64
$ chmod +x gopeed
$ ./gopeed
```

Then you would be able to visit Gopeed on http://localhost:9999

You can also add path to environment to run gopeed everywhere:

```{bash}
nano ~/.bashrc
```

 > Attention that `nano ~/.bashrc`only changes your user environment.

Then append `export PATH="/path/to/directory/gopeed:$PATH"` to the end. Finally run `source ~/.bashrc` again to make changes take effect immediately.
