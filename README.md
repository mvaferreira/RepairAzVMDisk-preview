# Repair-AzVMDisk 0.9.3 (preview)

Temporary preview build of [Repair-AzVMDisk](https://github.com/mvaferreira/RepairAzVMDisk). This repository will be deleted once 0.9.3 is released in the main repository.

New in 0.9.3:

- On Trusted Launch and Confidential VMs, the saved 'Windows Boot Manager' entry only resolves when the ESP has both the original partition GUID **and** the original GPT slot (entry index). Start offset and size do not need to match.
- `-GetUefiBootEntry` and `-SysCheck` now report a GPT slot mismatch as "will NOT boot" on Trusted Launch / CVM. The earlier 0.9.2 assumption that the firmware falls back to `\EFI\Boot\bootx64.efi` was wrong for Trusted Launch and has been removed.
- `-FixUefiBootEntry` restores the original ESP GUID and also moves the ESP back to its original GPT slot (swapping entries with the partition that occupies it). Both GPT copies are backed up and rewritten with valid CRCs, the BCD is re-pointed, and the Windows drive letter is preserved. Re-running it is a no-op.
- `-EspSlot <1..128>` overrides the target GPT slot for `-FixUefiBootEntry`.

New in 0.9.2:

- `-GetUefiBootEntry` reads the original EFI System Partition GUID and GPT slot from the guest's Measured Boot logs, and reports whether the saved UEFI boot entry still matches the disk.
- `-FixUefiBootEntry` restores the original ESP GUID so the VM's saved UEFI boot entry resolves again (no VM guest state changes needed).
- `-RecreateBootPartition` recreates a missing ESP at its original location and GUID, and renames stray `\EFI` folders on other FAT partitions that would otherwise take precedence at boot.

## Download

[Download ZIP](https://github.com/mvaferreira/RepairAzVMDisk-preview/archive/refs/heads/main.zip)

`Repair-AzVMDisk.ps1` SHA256: `05EFC8F5E76B708DE57EA41A710EB2520CC0E317DE72A83E6C9F1702020839CA`

Usage and documentation: see the [main repository](https://github.com/mvaferreira/RepairAzVMDisk).
