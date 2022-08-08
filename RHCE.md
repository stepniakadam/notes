# redhat_exams
Materials used for RedHat exams preparation

#### What is the difference between MBR and GPT parition schames?

They are both parition schames - describe how the data is organized. But they differ in it's properties:

#### MBR
- Has maximum number of 4 paritions
- Maximum size of partion is 2TB

#### GPT
- Hax maximum number of 128 paritions
- Maximum size of partion is 18 exabytes
- Good support of multiple boot
- Easy parition recovery

#### Useful commands:

```
df - report file system disk space usage
```

```
blkid - locate/print block device attributes
```

```
fdisk - manipulate disk partition table (adding, removing paritions ect)
```

```
gdisk - Interactive GUID partition table (GPT) manipulator
```

#### Block device vs character device?

In the UNIX world there are two categories of device files and thus device drivers: character and block. This division is done by the speed, volume and way of organizing the data to be transferred from the device to the system and vice versa. In the first category, there are slow devices, which manage a small amount of data, and access to data does not require frequent seek queries. Examples are devices such as keyboard, mouse, serial ports, sound card, joystick. In general, operations with these devices (read, write) are performed sequentially byte by byte. The second category includes devices where data volume is large, data is organized on blocks, and search is common. Examples of devices that fall into this category are hard drives, cdroms, ram disks, magnetic tape drives. For these devices, reading and writing is done at the data block level.




