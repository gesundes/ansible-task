# Task notes

## First
This code isn't designed to support multiple operating systems. It was tested with Ubuntu only.\
And probably will work with other Debain-based distributions.

## Second
While one of the best practices is to design roles to focus on a specific task or service, main goal here is to prepare the server for operation and all these tasks can be concidered as parts of a single whole.

And it's much easier for everyday work to have all tasks in files in one directory than browse between multiple small roles. It more readable, understandable and easier to edit.

## Third
A part of the task says: "Implement a procedure to encrypt the partition that is present on the disk next to the root partition."

So, we have these partitions on the main disk:
```bash
Disk /dev/xvda: 18 GiB, 19327352832 bytes, 37748736 sectors

...

Device        Start      End  Sectors  Size Type
/dev/xvda1  2099200 37748702 35649503   17G Linux filesystem
/dev/xvda14    2048    10239     8192    4M BIOS boot
/dev/xvda15   10240   227327   217088  106M EFI System
/dev/xvda16  227328  2097152  1869825  913M Linux extended boot
```

I think it's possible to concider */dev/xvda14* as "the partition that is present on the disk next to the root partition.". Or even all other partitions.

All these partitions are boot partitions. It's not a good idea to encrypt any of that partitions and that's why:

- If you will not provide a keyfile, GRUB will ask you for a passcode. It will not work in a cloud as it will need a manual intervention to boot an instance. It wouldn't be dynamic enough.
- To decrypt an encrypted boot partition without human help a bootloader needs access to the keyfile in some unecrypted place. It makes encryption meaningless because a bad guy can get that keyfile and decrypt a partition.
- NitroTPM can help here. In this case a passcode will be stored in the trusted place outside of instance. However t2.small instances don't support NitroTPM so it's not possible to use it in our case.

As a result I did nothing here.

Nevertheless, the basic role supports encryption (with data loss) of multiple disks/partitions. One can describe a list of entities to encrypt in a loop.

## Fourth
Regarding "Switching CPU operation from power-saving mode to more productive mode."

I realized (probably wrong) that it's only possible on a real hardware but not on virtual machines.

So I implemented a check as a first step is it possible to apply a governor change. And if not all other tasks will be skipped.

In case of t2.small instances (and all other instances) it's not possibly to apply this configuration.

## Fifth
Even if I didn't add enough debug information everywhere I hope I was able to show that I can do it. Especially in the [roles/basic/tasks/luks.yaml](roles/basic/tasks/luks.yaml) file.

## Sixth
Why we have a *basic_* prefix in variables names? In case if we apply multiple roles to hosts these roles can have the same variable names and we can get a mess. So it's better to have prefixes with same value as role names to avoid that kind of situations