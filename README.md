# VMWare Conversion Tools
Offering VMWare refugees conversion to nearly all formats, with now-offline but required utilities included.   
This repo focuses on converting VMware .vmdk files to .vhdx for use in Hyper-V, but these tools are highly flexible.  
A Windows 10/11/Server environment is needed to run the `mvmc` and `dsfok` utilities.  
Qemu is also a great option for both [Linux](https://www.qemu.org/docs/master/about/build-platforms.html) and [Windows](https://cloudbase.it/qemu-img-windows).    

### The Process  
(Credit to <https://gist.github.com/rahilwazir/69a750b70348459875cbf40935af02cb>)      

### USE INCLUDED OVF FILES + PDF MANUALS TO CONVERT FROM .OVA + OTHER VM FORMATS
These are only necessary if you need to convert from/to .OVA, .OVF, and .VMX.  

### The VHDX format, not VHD, is supported by Windows 11. Windows Server has not been tested.
Install `mvmc_setup.msi`  
Open Powershell as admin and run:  
`Import-Module 'C:\Program Files\Microsoft Virtual Machine Converter\MvmcCmdlet.psd1'`  
Then run:   
`ConvertTo-MvmcVirtualHardDisk -SourceLiteralPath d:\scratch\vmx\VM-disk1.vmdk -VhdType DynamicHardDisk -VhdFormat vhdx -destination c:\vm-disk1`  

#### If you see this error: 
ConvertTo-MvmcVirtualHardDisk : "The entry 1 (_or any number_) is not a supported disk database entry for the descriptor."

### FIX ENTRY [NUMBER] ERROR WITH DSFOK TOOLS
Download and extract dsfok tools  
Use `dsfo.exe "c:\temp\disk2.vmdk" 512 1024 descriptor1.txt` to extract the descriptor  
Edit the descriptor file in Notepad++: comment out this line as so: **#ddb.toolsInstallType = "4"**  

_(This could be another number. Original Gist author deleted one NULL character from the end to keep the file size of 1024 bytes (not sure if this is needed.))_  

Use `dsfi.exe "c:\temp\disk2.vmdk" 512 1024 descriptor1.txt` to inject the descriptor back into the VMDK  
Repeat these steps for any other .vmdk  
Reissue the `ConvertTo-MvmcVirtualHardDisk` command  
Place the new .vhdx file in your Hyper-V virtual disk directory  


