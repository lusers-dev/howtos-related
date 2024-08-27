# Linux Users - Poor's Man Approach to Patching a Stand-Alone ESXi Server

(NOTE: Since VMware acquisition by Broadcom, this could could be retired at any time by Broadcom)

(Leaving content for institutional knowlege only)

Managing software profiles on a VMware ESXi host using the ESXCLI command-line interface. 
(The Poor's Man Approach)

`linux_esxi_os_patch_by_cli.txt`

```shell

## Login to the ESXi server GUI and navigate to Host / Configuration
## and note the "Image profile" information.

## Download latest patch available from Broadcom
## Register with Broadcom Support
## (Don’t have an account? Register for Broadcom Support)
## site: https://support.broadcom.com/
## To see what patches are available:
## My Downloads / VMWare vSphere / Solutions / VMware vSphere - Essentials / <version> / <select-file-depoth>
## Once the depot patch zip file is downloaded, upload it to a ESXi server datastore
## Eg: datastore1
## you can create a "patches" folder to upload the patch zip file into
## Eg: patches


## Gracefully shutdown all Guest VMs and put your ESXi host into Maintenance Mode. 

## Once new patches are uploaded to your ESXi host..
## To list image profiles that came with the patch, run the following command: 

$ esxcli software sources profile list \
--depot=/vmfs/volumes/datastore1/patches/VMware-ESXi-7.0U3n-21930508-depot.zip

Name                            Vendor        Acceptance Level  Creation Time        Modification Time
------------------------------  ------------  ----------------  -------------------  -----------------
ESXi-7.0U3n-21930508-standard  VMware, Inc.  PartnerSupported  2023-07-06T00:00:00  2023-07-06T00:00:00
ESXi-7.0U3n-21930508-no-tools  VMware, Inc.  PartnerSupported  2023-07-06T00:00:00  2023-06-15T12:39:40

## apply patch profiles labeled with a "standard" label. 

$ esxcli software profile update \
--depot=/vmfs/volumes/datastore1/patches/VMware-ESXi-7.0U3n-21930508-depot.zip \
--profile=ESXi-7.0U3n-21930508-standard

Update Result
  Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
  Reboot Required: true
....
....
....

sync
sync
sync

##shutdown VMs and reboot ESXi server by GUI

## Verify the patch was applied
## Login to the ESXi server and navigate to Host / Configuration
## and note the "Image profile" information.

OR by CLI

$ esxcli system version get
   Product: VMware ESXi
   Version: 7.0.3
   Build: Releasebuild-21930508
   Update: 3
   Patch: 95

```

Let's break down the command step by step:

1. `esxcli`: This is the command-line utility used for managing various aspects of a VMware ESXi host, including software packages, hardware, networking, and more.

2. `software`: This is a subcommand that deals with software-related operations within ESXi.

3. `profile`: This subcommand is used for managing software profiles on the ESXi host.

4. `update`: This is the action you are instructing ESXCLI to perform. It means you want to update an existing software profile.

5. `--depot=/vmfs/volumes/datastore1/patches/VMware-ESXi-7.0U3n-21930508-depot.zip`: This is a parameter that specifies the location of the software depot, which is a ZIP archive file containing software packages and updates for ESXi. In this case, the depot is located at the specified path: `/vmfs/volumes/datastore1/patches/VMware-ESXi-7.0U3n-21930508-depot.zip`.

6. `--profile=ESXi-7.0U3n-21930508-standard`: This is another parameter that specifies the name of the software profile you want to update. In this case, you are targeting the profile named `ESXi-7.0U3n-21930508-standard`.


