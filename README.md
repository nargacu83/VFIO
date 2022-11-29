# VFIO KVM Passthrough configuration

CPU : AMD Ryzen 5 2600 Six-Core Processor

GPU : Radeon RX 590 Series

## About

This is a Single GPU Passthrough configuration for AMD CPU and GPU.

It is also supposed to work with anti-cheat vm detection, though i don't play any games with heavy anti-cheat.

Works on Arch Linux with the amdgpu driver and PipeWire (should work fine with pulseaudio).

Note: You should check the `start.sh` to change the user.

If you are using Nvidia GPU you can check [Mageas's repository](https://gitlab.com/Mageas/vfio-single-gup-passthrough).

## Credits

- [Mageas](https://gitlab.com/Mageas)
- [Mocan Cosmin](https://github.com/cosminmocan) for his [vfio-single-amdgpu-passthrough](https://github.com/cosminmocan/vfio-single-amdgpu-passthrough)
- [QaidVoid](https://github.com/QaidVoid) for his [Complete Single GPU Passthrough](https://github.com/QaidVoid/Complete-Single-GPU-Passthrough)
- [Bryan Steiner]() for his [gpu-passthrough-tutorial](https://github.com/bryansteiner/gpu-passthrough-tutorial)
- [SomeOrdinaryGamers](https://www.youtube.com/channel/UCtMVHI3AJD4Qk4hcbZnI9ZQ) for his [video](https://youtu.be/BUSrdUoedTo)


## Troubleshooting

- No internet in VM:

https://wiki.libvirt.org/page/Networking#NAT_forwarding_.28aka_.22virtual_networks.22.29

Save in a file called `default.xml` [src](https://github.com/libvirt/libvirt/blob/master/src/network/default.xml.in):
Add `dev="enp5s0"` to `<forward mode="nat" />`
```
<network>
  <name>default</name>
  <uuid>9a05da11-e96b-47f3-8253-a3a482e445f5</uuid>
  <forward mode='nat' dev="enp5s0" />
  <bridge name='virbr0' stp='on' delay='0'/>
  <mac address='52:54:00:0a:cd:21'/>
  <ip address='192.168.122.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.122.2' end='192.168.122.254'/>
    </dhcp>
  </ip>
</network>
```

Remove default NAT configuration
```
sudo virsh net-undefine default
sudo virsh net-destroy default
sudo virsh net-list
```

Make sure everything is enabled and restart libvirt
```
sudo systemctl enable --now libvirtd
sudo systemctl enable virtlogd.socket
sudo systemctl restart libvirtd.service
```

Redefine default configuration with the `default.xml` file.
```
sudo virsh net-define default.xml
sudo virsh net-autostart default
sudo virsh net-start default
```