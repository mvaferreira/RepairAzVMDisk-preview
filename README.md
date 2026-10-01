# Repair-AzVMDisk 0.9.4 (preview)

Temporary preview build of [Repair-AzVMDisk](https://github.com/mvaferreira/RepairAzVMDisk). This repository will be deleted once 0.9.4 is released in the main repository.

New in 0.9.4:

- When the Measured Boot logs record the ESP GUID under more than one GPT slot (for example after the disk was booted once on nested Hyper-V), `-GetUefiBootEntry`, `-SysCheck` and `-FixUefiBootEntry` now use the slot the firmware recorded most often - the one the Azure VM's saved 'Windows Boot Manager' entry points to - instead of the newest sighting. The other sightings are listed, and a tie is reported instead of guessed.
- `-SysCheck` and `-GetUefiBootEntry` report which Secure Boot CA signed the ESP boot manager (Windows Production PCA 2011 or Windows UEFI CA 2023), read from the file's embedded signature, and check it against the Secure Boot `db` / `dbx` recorded in the newest Measured Boot log. A CA 2023-signed boot manager on a platform whose `db` lacks that CA (such as an older Hyper-V Gen2 VM) is flagged, since Secure Boot refuses it there with "The boot loader failed".

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

`Repair-AzVMDisk.ps1` SHA256: `69F55C84F7B01DAF6C734AF2E2E156E2052A46A3F72677F33A7B23E10E43183A`

Usage and documentation: see the [main repository](https://github.com/mvaferreira/RepairAzVMDisk).
