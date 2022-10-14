---
title: 'Packer'
date: '2021-05-02'
draft: false
tags: ['Infrastructure as Code']
---

HashiCorp Packer is badass!!!

Here are some examples of how I use it:

* * *

1\. Create a Debian OVFs on macOS using VMware Fusion

Requirements:

Install packer (free):  
`brew install packer`

Install VMware Fusion Player (free): [https://my.vmware.com/group/vmware/downloads/details?downloadGroup=FUS-1211&productId=1040](https://my.vmware.com/group/vmware/downloads/details?downloadGroup=FUS-1211&productId=1040)

OVFTOOL (creates the OVA): [https://my.vmware.com/group/vmware/downloads/details?downloadGroup=OVFTOOL441&productId=742](https://my.vmware.com/group/vmware/downloads/details?downloadGroup=OVFTOOL441&productId=742)

* * *

Setup:

debian.pkr.hcl

```
source "vmware-iso" "debian10" {
  guest_os_type = "debian10_64Guest"
  disk_size = 4096
  cpus = 1
  memory = 1024
  vm_name = "Debian10"
  http_directory = "http"
  iso_url = "https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-10.9.0-amd64-netinst.iso"
  iso_checksum = "sha256:8660593d10de0ce7577c9de4dab886ff540bc9843659c8879d8eea0ab224c109"
  ssh_username = "debian"
  ssh_password = "default"
  shutdown_command = "sudo shutdown -P now"
  vmx_data = {
    "mks.noBeep" = "TRUE"
  }
  boot_command = [
    "<esc><wait>",
    "install <wait>",
    "preseed/url=http://{{ .HTTPIP }}:{{ .HTTPPort }}/preseed.cfg <wait>",
    "debian-installer=en_US.UTF-8 <wait>",
    "auto <wait>",
    "locale=en_US.UTF-8 <wait>",
    "kbd-chooser/method=us <wait>",
    "keyboard-configuration/xkb-keymap=us <wait>",
    "netcfg/get_hostname={{ .Name }} <wait>",
    "netcfg/get_domain=redtalks.lab <wait>",
    "fb=false <wait>",
    "debconf/frontend=noninteractive <wait>",
    "console-setup/ask_detect=false <wait>",
    "console-keymaps-at/keymap=us <wait>",
    "grub-installer/bootdev=/dev/sda <wait>",
    "<enter><wait>"
  ]
}

build {
  sources = ["sources.vmware-iso.debian10"]
}
```

http/preseed.cfg

```
# Setting the locales, country
# Supported locales available in /usr/share/i18n/SUPPORTED
d-i debian-installer/language string en
d-i debian-installer/country string us
d-i debian-installer/locale string en_US.UTF-8
# Keyboard setting
d-i console-setup/ask_detect boolean false
d-i keyboard-configuration/layoutcode string us
d-i keyboard-configuration/xkb-keymap us
d-i keyboard-configuration/modelcode string pc105
# User creation
d-i passwd/user-fullname string kopicloud
d-i passwd/username string kopicloud
d-i passwd/user-password password kopicloud
d-i passwd/user-password-again password kopicloud
d-i user-setup/allow-password-weak boolean true
# Disk and Partitioning setup
d-i partman-auto-lvm/guided_size string max
d-i partman-auto/choose_recipe select atomic
d-i partman-auto/method string lvm
d-i partman-lvm/confirm boolean true
d-i partman-lvm/confirm boolean true
d-i partman-lvm/confirm_nooverwrite boolean true
d-i partman-lvm/device_remove_lvm boolean true
d-i partman/choose_partition select finish
d-i partman/confirm boolean true
d-i partman/confirm_nooverwrite boolean true
d-i partman/confirm_write_new_label boolean true
# Set mirror
apt-mirror-setup apt-setup/use_mirror boolean true
choose-mirror-bin mirror/http/proxy string
d-i mirror/country string manual
d-i mirror/http/directory string /debian
d-i mirror/http/hostname string httpredir.debian.org
d-i mirror/http/proxy string
# Set root password
d-i passwd/root-login boolean false
d-i passwd/root-password-again password default
d-i passwd/root-password password default
d-i passwd/user-fullname string debian
d-i passwd/user-uid string 1000
d-i passwd/user-password password default
d-i passwd/user-password-again password default
d-i passwd/username string debian
# Package installations
popularity-contest popularity-contest/participate boolean false
tasksel tasksel/first multiselect standard, ssh-server, web-server
d-i pkgsel/include sudo wget curl open-vm-tools
d-i pkgsel/install-language-support boolean false
d-i pkgsel/update-policy select none
d-i pkgsel/upgrade select full-upgrade
d-i grub-installer/only_debian boolean true
d-i finish-install/reboot_in_progress note
#Skip scanning additional CDs
d-i apt-setup/cdrom/set-first boolean false
d-i apt-setup/cdrom/set-next boolean false   
d-i apt-setup/cdrom/set-failed boolean false 
```