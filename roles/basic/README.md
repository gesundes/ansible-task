# Basic Host Configuration Ansible Role

## What is this role doing?
- Configures CPU performance profile
- Sets maximum level of c-state
- Encrypts drives or partitions with LUKS
- Updates a name of the default network interface
- Shows basic information about CPU

## A few considerations
- While this role do a lot of different things it's gole to provide a basic host configuration so it can be concidered as a whole thing
- At the same time tasks in the role are splitted by files with related tasks
- Some details will be provided later but overall this role is pretty simple and self documented
- The role was made to be used with Debian-based Linux distributions

## Available variables
They are pretty good described in the [defaults/main.yaml](defaults/main.yaml) file.

## Available tags
All groups of tasks compiled into separate files have their own tags:

- **cpufreq** — to set the "performance" profile (governor)
- **cstate** — to set a maximum values for the c-state
- **luks** — to encrypt drives with LUKS
- **netrename** — to rename the default network interface
- **lscpu** — to get information about CPU

So they can be applied separatly.

## cpufreq
It's not possible to apply "performance" or "powersave" governor on virtual machines so the first step is to check is it possible on a particalar host.

## cstate
Final applicaton of the cstate limitation requires host reboot. Just in case there is a *basic_reboot_allowed* option that prevents host from reboot by default and must be enabled in the group_vars.

## luks
This role can encrypt any number of drives/partitions but it doesn't preserve its content. **So be careful using it.**

It can encrypt multiple disks/partitions, just describe them as a list.

To be able to open volumes during reboot volume labels are used in the */etc/crypttab* file. Why is so? Because during reboot of EC2 instances disk names (like /dev/nvme2n1) can be changed.
