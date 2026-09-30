# Repair-AzVMDisk 0.9.2 (preview)

Temporary preview build of [Repair-AzVMDisk](https://github.com/mvaferreira/RepairAzVMDisk). This repository will be deleted once 0.9.2 is released in the main repository.

New in 0.9.2:

- `-GetUefiBootEntry` reads the original EFI System Partition GUID and GPT slot from the guest's Measured Boot logs, and reports whether the saved UEFI boot entry still matches the disk.
- `-FixUefiBootEntry` restores the original ESP GUID so the VM's saved UEFI boot entry resolves again (no VM guest state changes needed).
- `-RecreateBootPartition` recreates a missing ESP at its original location and GUID, and renames stray `\EFI` folders on other FAT partitions that would otherwise take precedence at boot.

## Download

[Download ZIP](https://github.com/mvaferreira/RepairAzVMDisk-preview/archive/refs/heads/main.zip)

`Repair-AzVMDisk.ps1` SHA256: `A2BCCFE365CE4224C1919DDF6BA46CFAAFFA7C4E5E603525D58BA9AEC82040FA`

Usage and documentation: see the [main repository](https://github.com/mvaferreira/RepairAzVMDisk).
