---
title: "How to install Ubuntu 20.04 on Hyper-V with enhanced session"
description: "How to configure an Hyper-V virtual machine running Ubuntu 20.04 desktop with enhanced session enabled."
author: "Francesco Tonini"
tags: 
  - "ubuntu"
  - "20.04"
  - "lts"
  - "hyperv"
  - "windows10"
  - "installation"
  - "vm"
date: 2020-05-02T09:27:00+02:00
lastmod: 2020-05-02T09:27:00+02:00
slug: "ubuntu-20-04-hyperv"
---

A few days ago Canonical officially released the latest version of Ubuntu. The first of the two releases of 2020 marks a new long term support (LTS).

This post aims at configuring an Hyper-V virtual machine running Ubuntu 20.04 desktop with enhanced session enabled. At the time of writing this the Hyper-V wizard of Windows 10 1909 supports Ubuntu 18.04 and 19.10, but we can easily bypass this limitation by installing the tools manually.

# Create VM

Open the *Hyper-V Manager* and on the right panel click on *New*, then *Virtual Machine*.

{{<figure src="/images/ubuntu-20-04-hyperv/step1.png" >}}

A new window will appear. Choose an appropriate name for your VM and click *Next*.

{{<figure src="/images/ubuntu-20-04-hyperv/step2.png" >}}

Make sure you select *Generation 2* on the next page. This will ensure that UEFI is enabled. Then, click *Next*.

{{<figure src="/images/ubuntu-20-04-hyperv/step3.png" >}}

As for the amount of memory, 4GB is the minimum requirement for a good experience. I suggest you to go to 8GB if you can. Then, click *Next*.

{{<figure src="/images/ubuntu-20-04-hyperv/step4.png" >}}

Be sure to select at least one network interface so that Ubuntu can download updates while installing and later on we can download drivers too. Then, click *Next*.

{{<figure src="/images/ubuntu-20-04-hyperv/step5.png" >}}

By default the wizard will create a 127GB disk. For my purpose that amount of storage is enough. Then, you guessed it, click *Next*.

{{<figure src="/images/ubuntu-20-04-hyperv/step6.png" >}}

Last but not least, select the ISO downloaded from the [Ubuntu website.](https://ubuntu.com/) To end the wizard, click on *Finish*.

{{<figure src="/images/ubuntu-20-04-hyperv/step7.png" >}}

Before you can start the VM, we have to disable secure boot as Ubuntu doesn't support it. Select the VM, then on the right panel click *Settings*.

{{<figure src="/images/ubuntu-20-04-hyperv/step8.png" >}}

On the left panel click *Security* and make sure that *Enable secure boot* is unchecked. Then, click *Ok*.

{{<figure src="/images/ubuntu-20-04-hyperv/step9.png" >}}

We are now ready to start the VM and run the OS setup (remember to **disable auto-login**, otherwise the enhanced session won't work). Once you have finished, we can start installing the *linux-tools* provided by Microsoft.

# Setup enhanced session

On Ubuntu, open a terminal, download and run the setup script.

```bash
wget https://raw.githubusercontent.com/Microsoft/linux-vm-tools/master/ubuntu/18.04/install.sh
sudo chmod +x install.sh
sudo ./install.sh
```

If you receive an error at the end of the install script, there is one more thing to do. Using your favorite editor, open **/etc/xrdp/xrdp.ini** (sudo required) and edit the following lines:

```
port=vsock://-1:3389
use_vsock=false
```

Save the file and shutdown the VM. From Windows, open a PowerShell prompt with admin privileges and type:

```powershell
Set-VM -VMName <your_vm_name>  -EnhancedSessionTransportType HvSocket
```

where *<your_vm_name>* is, well, the name of the VM (the one chosen at the start of the creation wizard).
There you go! Now start the VM and in a matter of seconds you will be redirected to the XRDP login page.

![Step 10](/images/ubuntu-20-04-hyperv/step10.png)