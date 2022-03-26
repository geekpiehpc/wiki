# 机器简介

Currently, we are using the SuperMicro 4124GS-TNR server.

* [Product Spec](https://www.supermicro.com/en/Aplus/system/4U/4124/AS-4124GS-TNR.cfm)
* [AS-4124GS-TNR User manual](https://www.supermicro.com/manuals/superserver/4U/MNL-2302.pdf)
* [H12DSG-O-CPU Motherboard manual](https://www.supermicro.com/manuals/motherboard/EPYC7000/MNL-2299.pdf)
* [Information for Lot 9 of ErP (Ecodesign)](https://www.supermicro.com/manuals/superserver/4U/Lot9_AS-4124GS-TNR.pdf)

## CPU: Epyc 7742

AMD claim that theoretical floating point performance can be calculated as: Double Precision theoretical Floating Point performance = #real\_cores \* 8 DP flop/clk * core frequency. For a 2 socket system =2 \* 64 cores \* 8 DP flops/ clk \* 2.2 GHz=2252.8 Gflops. This includes counting FMA as two flops.

![](https://en.wikichip.org/w/images/thumb/f/f2/zen_2_core_diagram.svg/1800px-zen_2_core_diagram.svg.png)

## GPU

[Dissecting theNVIDIA VoltaGPU Architecture via Microbenchmarking](https://arxiv.org/pdf/1804.06826.pdf)

## RDMA

```
a1:00.0 Infiniband controller [0207]: Mellanox Technologies MT27800 Family [ConnectX-5] [15b3:1017]
a1:00.1 Infiniband controller [0207]: Mellanox Technologies MT27800 Family [ConnectX-5] [15b3:1017]
```

* [Official Documention](https://docs.mellanox.com/display/MLNXOFEDv541030/)
* IB 卡的通讯协议 https://www.rdmamojo.com/2013/06/01/which-queue-pair-type-to-use/
* OpenMPI 使用 http://scc.ustc.edu.cn/zlsc/user\_doc/html/mpi-application/mpi-application.html

## RAID

```
e6:00.0 SATA controller [0106]: Marvell Technology Group Ltd. 88SE9230 PCIe SATA 6Gb/s Controller [1b4b:9230] (rev 11) (prog-if 01 [AHCI 1.0])
```

Official brief: https://www.marvell.com/content/dam/marvell/en/public-collateral/storage/marvell-storage-88se92xx-product-brief-2012-04.pdf

To configure the RAID controller, the easiest way is to press Ctrl+M during booting.

If you want to boot a system on RAID, please use Legacy mode.
If you switched to UEFI only, you can't find the controller even if you change it back later.
To solve it, see [Supermicro FAQ Entry](https://www.supermicro.com/support/faqs/faq.cfm?faq=33907)

### Firmware

It's possible to flash firmware, see [Marvell 9230 Firmware Updates and such](https://homeservershow.com/forums/topic/9179-marvell-9230-firmware-updates-and-such/page/11/). Our current firmware is `1070 (bios oprom version)`. If you want to flash another firmware, you might need to make a FreeDOS bootable disk.

> **Note**: Do backup before flashing!

Many links to firmware or utilities are broken. [Station Drivers](https://www.station-drivers.com/index.php?option=com_remository&Itemid=352&func=select&id=215&lang=en) may still work.
Also refer [Marvell 92xx A1 Firmware Image Repository](https://www.station-drivers.com/index.php/en/forum/news-bios/125-marvell-92xx-a1-firmware-image-repository?start=6#5542), it have a full collection of firmware images.

You can find supermicro's firmware from [official site](https://www.supermicro.com/wdl/ISO_Extracted/CDR-A1-UP_1.01_for_Intel_A1_UP_platform/Marvell/EEPROM_update_tool) but you can't download it. Try download from [http://members.iinet.net.au/\~michaeldd/](http://members.iinet.net.au/~michaeldd/).

## NVMe

Installed with [https://www.asus.com/us/Motherboards-Components/Motherboards/Accessories/HYPER-M-2-X16-CARD-V2/](https://www.asus.com/us/Motherboards-Components/Motherboards/Accessories/HYPER-M-2-X16-CARD-V2/).

```
21:00.0 Non-Volatile memory controller [0108]: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983 [144d:a808]
22:00.0 Non-Volatile memory controller [0108]: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983 [144d:a808]
23:00.0 Non-Volatile memory controller [0108]: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983 [144d:a808]
24:00.0 Non-Volatile memory controller [0108]: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983 [144d:a808]
```

⚠️ The PCIE socket of the NVME card must be configured as `4x4x4x4` so as to be recognized by the system correctly.

The card may have problems. If you find it doesn't work correctly, ask in Slack.



## Other links

* [SuperMicro FTP](https://www.supermicro.com/wdl/)
* [IMPI Vies](https://www.supermicro.com/manuals/other/IPMIView20.pdf)
* [IMPI Users Guide](https://www.supermicro.com/manuals/other/IPMI_Users_Guide.pdf)

### RAID Controller

#### MegaRAID

[LSI_SAS_EmbMRAID_SWUG.pdf](https://www.supermicro.com/wdl/ISO_Extracted/CDR-X8-O_1.01_for_Intel_X8_O_platform/MANUALS/LSI_SAS_EmbMRAID_SWUG.pdf)
[2006 LSI_SAS_EmbMRAID_SWUG.pdf](https://www.supermicro.com/wdl/driver/SAS/LSI/Documentation/2006%20-%20LSI_SAS_EmbMRAID_SWUG.pdf)

### ASrock

[faq1](https://www.asrockrack.com/support/faq.asp?id=14)

[faq2](https://www.asrockrack.com/support/faq.asp?k=Marvell9230)

### Win-Raid

[forum](https://www.win-raid.com/u162-chinobino-messages-3.html)

[Help-Problem-to-flash-the-Marvel-SE-card-resolve](https://www.win-raid.com/t5289f16-Help-Problem-to-flash-the-Marvel-SE-card-resolve.html)

[Syba-SI-PEX-PCIe-Card-with-Marvell-SATA-Controller](https://www.win-raid.com/t8641f51-Syba-SI-PEX-PCIe-Card-with-Marvell-SATA-Controller-1.html)

[firmware UEFI](https://www.supermicro.com/wdl/ISO_Extracted/CDR-A1-UP_1.01_for_Intel_A1_UP_platform/Marvell/EEPROM_update_tool/firmware/92xx/UEFI_MODE/UEFI/20151222_shell2.10_uefi64/)

[firmware DOS](https://www.supermicro.com/wdl/ISO_Extracted/CDR-A1-UP_1.01_for_Intel_A1_UP_platform/Marvell/EEPROM_update_tool/firmware/92xx/DOS_MODE/UEFI_PG/CpuAHCI/9230/uefi64/)

http://members.iinet.net.au/\~michaeldd/CDR-A1-UP\_1.01\_for\_Intel\_A1\_UP\_platform.zip

### Supermicro superserver bios change cause 960 nvme disappear

https://tinkertry.com/supermicro-superserver-bios-change-can-cause-960-pro-and-evo-to-hide-heres-the-fix

### Background knowlodge

* [Don't do it: consumer-grade solid-state drives (SSD) in Storage Spaces Direct](https://techcommunity.microsoft.com/t5/storage-at-microsoft/don-t-do-it-consumer-grade-solid-state-drives-ssd-in-storage/ba-p/425914)
* [PCI Option ROM](https://edk2-docs.gitbook.io/edk-ii-uefi-driver-writer-s-guide/32_distributing_uefi_drivers/321_pci_option_rom)
* [UEFI Validation Option ROM Guidance](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/uefi-validation-option-rom-validation-guidance?view=windows-10)
