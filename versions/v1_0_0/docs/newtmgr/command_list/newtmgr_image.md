## <font color="#F2853F" style="font-size:24pt">newtmgr image </font>
Manage images on a device.

#### Usage:

```no-highlight
    newtmgr image [command] -c <connection_profile> [flags] 
```

#### Flags:
The coredownload subcommand uses the following local flags:

```no-highlight
    -n, --bytes uint32         Number of bytes of the core to download 
    -e, --elfify               Create an ELF file 
        --offset unint32       Offset of the core file to start the download 

```
#### Global Flags:

```no-highlight
    -c, --conn string       connection profile to use
    -h, --help              Help for newtmgr commands
    -l, --loglevel string   log level to use (default "info")
    -t, --trace             print all bytes transmitted and received
```

####Description
The image command provides subcommands to manage core and image files on a device.  Newtmgr uses the `conn_profile` connection profile to connect to the device.

Sub-command  | Explanation
-------------| ------------------------
confirm      | The newtmgr image confirm [hex-image-hash] command makes an image setup permanent on a device. If a `hex-image-hash` hash value is specified, Mynewt permanently switches to the image identified by the hash value. If a hash value is not specified, the current image is made permanent.
coreconvert  | The newtmgr image coreconvert &lt;core-filename&gt; &lt;elf-file&gt; command converts the `core-filename` core file to an ELF format and names it `elf-file`. <br><br> **Note**: This command does not download the core file from a device. The core file must exist on your host.
coredownload | The newtmgr image coredownload &lt;core-filename&gt; command downloads the core file from a device and names the file `core-filename` on your host. Use the local flags under Flags to customize the command.
coreerase    | The newtmgr image coreerase command erases the core file on a device.
corelist     | The newtmgr image corelist command lists the core(s) on a device.
list         | The newtmgr image list command displays information for the images on a device.
test         | The newtmgr test &lt;hex-image-hash&gt; command tests the image, identified by the `hex-image-hash` hash value, on next reboot.
upload       | The newtmgr image upload &lt;image-file&gt; command uploads the `image-file` image file to a device.

####Examples

Sub-command  | Usage                  | Explanation
-------------| -----------------------|-----------------
confirm       | newtmgr confirm<br>-c profile01 | Makes the current image setup on a device permanent.  Newtmgr connects to the device over a connection specified in the `profile01` connection profile.
confirm       | newtmgr confirm<br>be9699809a049...73d77f<br>-c profile01 | Makes the image, identified by the `be9699809a049...73d77f` hash value, setup on a device permanent.  Newtmgr connects to the device over a connection specified in the `profile01` connection profile.
coreconvert    | newtmgr image coreconvert mycore mycore.elf | Converts the `mycore` file to the ELF format and saves it in the `mycore.elf` file.
coredownload | newtmgr image coredownload <br>mycore -c profile01 | Downloads the core from a device and saves it in the `mycore` file.   Newtmgr connects to the device over a connection specified in the `profile01` connection profile.
coredownload | newtmgr image coredownload <br>mycore -e <br>-c profile01 | Downloads the core from a device, converts the core file into the ELF format, and saves it in the `mycore` file.   Newtmgr connects to the device over a connection specified in the `profile01` connection profile.
coredownload | newtmgr image coredownload <br>mycore <br>--offset 10 -n 30<br>-c profile01 | Downloads 30 bytes, starting at offset 10, of the core from a device and saves it in the `mycore` file.   Newtmgr connects to the device over a connection specified in the `profile01` connection profile.
coreerase    | newtmgr image coreerase <br>-c profile01 | Erases the core file on a device.  Newtmgr connects to the device over a connection specified in the `profile01` connection profile.
corelist     | newtmgr image corelist<br>-c profile01 | Lists the core files on a device.  Newtmgr connects to the device over a connection specified in the `profile01` connection profile.
list         | newtmgr image list<br>-c profile01 | Lists the images on a device.  Newtmgr connects to the device over a connection specified in the `profile01` connection profile.
test         | newtmgr image test <br>be9699809a049...73d77f | Tests the image, identified by the `be9699809a049...73d77f` hash value, during the next reboot on a device. Newtmgr connects to the device over a connection specified in the `profile01` connection profile.  
upload       | newtmgr image <br>upload bletiny.img<br>-c profile01 | Uploads the `bletiny.img` image to a device.  Newtmgr connects to the device over a connection specified in the `profile01` connection profile.
