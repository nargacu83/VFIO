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

Save in default.xml:
```
<network>
  <name>default</name>
  <uuid>afd4e923-66cb-45ca-9120-1e46e72899a3</uuid>
  <forward mode='nat'>
    <nat>
      <port start='1024' end='65535'/>
    </nat>
  </forward>
  <bridge name='virbr0' stp='on' delay='0'/>
  <ip address='192.168.122.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.122.2' end='192.168.122.254'/>
    </dhcp>
  </ip>
</network>
```

Then execute these lines with sudo:
```
virsh net-undefine default
virsh net-destroy default
virsh net-list
systemctl enable --now libvirtd
systemctl enable virtlogd.socket
systemctl restart libvirtd.service
virsh net-define default.xml
virsh net-autostart default
virsh net-start default
```