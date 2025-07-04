# 🧩 My Linux Kernel Contributions (So Far)
As part of my deep dive into systems programming and LFX Mentroship Summer 2025 I’ve been actively contributing to the Linux kernel — submitting patches, getting feedback from maintainers, and learning from the process. Here's a log of patches I've worked on, along with their current status and some notes about the outcomes.

## Short Summary 

#1. [[PATCH] btrfs: remove ASSERT in populate_free_space_tree for empty extent tree btrfs_search_slot_for_read() returns 1 when no items are found](https://lore.kernel.org/linux-btrfs/20250607111531.28065-1-abinashsinghlalotra@gmail.com/ ) Already Fixed

#2. [[RFC PATCH] mfd: twl4030-irq: remove redundant 'node' variable](https://lore.kernel.org/all/20250613122251.1033078-1-abinashsinghlalotra@gmail.com/)
Accepted by Maintainer in mfd tree ; mrege conflict with mfd-fixes ; Dropped

#3. [\[RFC PATCH\] fsnotify: initialize destroy_next to avoid KMSAN uninit-value warning](https://lore.kernel.org/all/20250619104140.105835-1-abinashsinghlalotra@gmail.com/)                                               False postive by syzbot ; 

#4. [[RFC PATCH] KVM: x86: Dynamically allocate bitmap to fix -Wframe-larger-than error](https://lore.kernel.org/kvm/aEylI-O8kFnFHrOH@google.com/)                                            Maintainer got a better fix ; Got Reported by Tag

#5. [[RFC PATCH] usb_wwan : add locking around shared port data in two FIXME-marked places](https://lore.kernel.org/all/20250620101747.39037-1-abinashsinghlalotra@gmail.com/) Discussions Going On

#6. [[PATCH] f2fs: fix KMSAN uninit-value in extent_info usage](https://lore.kernel.org/all/20250625110537.22806-1-abinashsinghlalotra@gmail.com/) Discussions Going on

#7. [[RFC PATCH] xen/gntdev: reduce stack usage by dynamically allocating gntdev_copy_batch](https://lore.kernel.org/all/20250629204215.1651573-1-abinashsinghlalotra@gmail.com/) Discussions going on

## 🔧 Patch Log
### ✅ #1. btrfs: remove ASSERT in populate_free_space_tree
`Subsystem`: Btrfs

`Summary`: Removed an unnecessary ASSERT() when the extent tree is empty.

`Outcome`: Already fixed by someone else. Still, useful for learning about btrfs_search_slot_for_read() behavior.

### ❌ #2. mfd: twl4030-irq: remove redundant 'node' variable
`Subsystem`: MFD (Multi-Function Device)

`Summary`: Removed a dead local variable.

`Outcome`: Accepted by the maintainer, but later dropped due to merge conflicts with mfd-fixes.

### ⚠️ #3. fsnotify: initialize destroy_next to avoid KMSAN uninit-value warning
`Subsystem`: fsnotify

`Summary`: Attempted to silence a KMSAN warning by initializing a field.

`Outcome`: Turned out to be a false positive reported by syzbot.

### 🛠️ #4. KVM/x86: dynamically allocate bitmap to fix -Wframe-larger-than
`Subsystem`: KVM (x86)

`Summary`: Proposed a fix for large stack frame by heap allocation.

`Outcome`: Maintainer had a better fix; but patch was acknowledged and discussed.

### 🔄 #5. usb_wwan: add locking around shared port data
`Subsystem`: USB Serial (usb_wwan)

`Summary`: Added locking around shared port->data to prevent race conditions (FIXME comment resolution).

`Status`: Discussions ongoing.

Very relevant to proper driver behavior under multithreading.

### 🔄 #6. f2fs: fix KMSAN uninit-value in extent_info usage
`Subsystem`: F2FS

`Summary`: Fixed uninitialized read caught by KMSAN in extent_info.

`Status`: Under discussion.

### 🔄 #7. xen/gntdev: reduce stack usage via dynamic gntdev_copy_batch
`Subsystem`: Xen

`Summary`: Converted a large stack structure to dynamically allocated memory.

`Status`: Ongoing discussions.

## 🧠 Lessons Learned
Upstreaming is iterative: Even rejected or dropped patches spark useful discussions.

Subsystems behave differently: What’s okay in one driver may be an anti-pattern in another.

Tooling helps: Static analysis tools like KMSAN and syzbot are powerful — but not always right.

Communication matters: Learning how to write clear commit messages, respond to maintainers, and track mailing list threads is part of the journey.

## 📌 What's Next?
I’m continuing to explore various kernel subsystems, and aim to upstream more fixes and improvements — especially in device drivers, filesystems, and memory management.

Stay tuned for more posts as I document the deeper technical side of these patches and what I learn from reviewing the kernel source.
 
