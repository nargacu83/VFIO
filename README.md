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