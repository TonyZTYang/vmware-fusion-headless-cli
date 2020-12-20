# vmware-fusion-headless-cli
A bash script to help with easy start/stop/ssh to your virtual machines.
## What it can do
- **Start/stop/pause/unpause virtual machines** without having to input their **full paths** every time.
- Get the virtual machine's current **ip address** for ssh connections and other operations.
- **Automatically load** the virtual machine's **ip address** in your **ssh client's config file** so that you don't have to look up and input the ip every time you make an ssh connection.

## How it works
### Setup
1. Put the file `vm` to one of your `$PATH` destinations, usually `/bin/` or `/usr/local/bin/`.
2. Grant the user executional permission to this file e.g. `usermod u+x file_path`.
3. Specify the virtual machine names and their corresponding path to the vmx file in the `vm` file.
   1. The section of code for this setup comes after the line of comment starting with `# NAME:`
   2. For each virtual machine you want to setup, there should be a block in the following formatt.
    ```bash
    vm_name)
        VM="path_to_vmx"
        ;;
    ```
    - Replace `vm_name` with the name you want to call the vm.
    - Replace `"path_to_vmx"` with the absolute path of the vmx file for the vm. [On how to find the vmx file](https://docs.vmware.com/en/VMware-Fusion/12/com.vmware.fusion.using.doc/GUID-212F0E8A-5D1B-4DCD-A60C-B34116BDD2D3.html)
### (Optional) Auto-ssh setup
If you want to be able to ssh to the machine with `ssh machine_name`, setup the following steps.
1. Uncomment the lines after the line of comment starting with `# SSH:`.
2. Set the network option for your virtual machine to **bridged** (other network method should work as well).
3. Configure each virtual machine's profile in `~/.ssh/config` as follows.
   ```
   Host vm_name
     HostName placeholder
   ```
   - Replace `vm_name` with the virtual machine's name set in the `vm` file.
   - Leave the `placeholder` as it is.
   - It's okay to specify ports, ssh keys or usernames as you wish as long as the `HostName` key exists.
### Usage
- **Start/stop/restart/suspend/pause/unpause a virtual machine:** `vm start/stop/restart/suspend/pause/unpause vm_name`
- **Check virtual machine ip address:** `vm ip vm_name`
- **(Optional) Connect to virtual machine through ssh:** `ssh vm_name`

## Why the script exists
This scripts gradually came into place when I tried to run my vmware fusion servers headless for simplicity and performance.

As you might know, vmware fusion has its own cli `vmrun` for command line tasks, however, its complexity and the fact that you have to input the complete path to your virtual machine's .vmx file makes it practically useless when you just want to simply spin up the machin, ssh to it (maybe through your IDE) to do some coding and then close it down.

When looking for a better solution, I came across [this article by Namshi](https://tech.namshi.io/blog/2015/08/02/vmware-fusion-headless/) and built upon the code in it.