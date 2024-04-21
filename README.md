# RedmiBook Pro 16 2024 Intel and Ubuntu 24.04

Ubuntu 24.04, 23.10 and etc have issues with sound setup. Windows works well "from the box", 
drivers are pulled automatically through updates.

Only **Dummy output** was available both in LiveISO and installed system.

Teken from [here](https://aaroalhainen.medium.com/how-i-fixed-my-ubuntu-20-04-no-audio-dummy-output-issue-eaa525838e0d) helped a lot:

```
$ echo "options snd-hda-intel model=generic" | sudo tee -a /etc/modprobe.d/alsa-base.conf
$ echo "options snd-hda-intel dmic_detect=0" | sudo tee -a /etc/modprobe.d/alsa-base.conf
$ echo "blacklist snd_soc_skl" | sudo tee -a /etc/modprobe.d/blacklist.conf
```

This was the last step actually (and **reboot**). Not sure if this relevant, but I tried to install also
sof drivers from github. This action did not give me any result at all. 

Links to previous action:

https://askubuntu.com/questions/1421513/22-04-lts-no-sound-intel-sound-card-snd-hda-intel-not-working

I tried sof versions 2.2 and newer. Not helped at all, but not sure if it was cleaned after properly.

Package **firmware-sof-signed** was reinstalled btw. 


Alsamixer displays **PipeWire** as card now. At first time it was something like **sof-hda-dsp**.

My actual configuration:

```
$ sudo hwinfo --sound
[sudo] password for oleg: 
16: PCI 1f.3: 0401 Multimedia audio controller                  
  [Created at pci.386]
  Unique ID: nS1_.W26PfiWanu5
  SysFS ID: /devices/pci0000:00/0000:00:1f.3
  SysFS BusID: 0000:00:1f.3
  Hardware Class: sound
  Model: "Intel Multimedia audio controller"
  Vendor: pci 0x8086 "Intel Corporation"
  Device: pci 0x7e28 
  SubVendor: pci 0x1d72 "Xiaomi"
  SubDevice: pci 0x2309 
  Revision: 0x20
  Driver: "snd_hda_intel"
  Driver Modules: "snd_hda_intel"
  Memory Range: 0x48192c0000-0x48192c3fff (rw,non-prefetchable)
  Memory Range: 0x4819000000-0x48191fffff (rw,non-prefetchable)
  IRQ: 190 (1069 events)
  Module Alias: "pci:v00008086d00007E28sv00001D72sd00002309bc04sc01i00"
  Driver Info #0:
    Driver Status: snd_hda_intel is active
    Driver Activation Cmd: "modprobe snd_hda_intel"
  Driver Info #1:
    Driver Status: snd_sof_pci_intel_mtl is active
    Driver Activation Cmd: "modprobe snd_sof_pci_intel_mtl"
  Config Status: cfg=new, avail=yes, need=no, active=unknown

```

