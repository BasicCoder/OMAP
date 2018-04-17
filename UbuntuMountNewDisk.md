# Ubuntu mount disk

虚拟机空间系统只占用了 80G ，剩余还有 300G 空间没有使用，需要重新进行格式化和挂载。

## 1.显示硬盘及所属分区情况
`sudo fdisk -lu`

## 2.对未使用的空间进行分区 | Hard disk add new partition
`sudo fdisk /dev/sda3` </br>
 在Command (m for help)提示符后面输入n，执行 add a new partition 指令给硬盘增加一个新分区。</br>
 出现Command action时，输入e，指定分区为扩展分区（extended）; 输入p，指定分区为主分区（primary partition）。</br>
 在Command (m for help)提示符后面输入p，显示分区表。</br>
 在Command (m for help)提示符后面输入w，保存分区表。 系统提示：The partition table has been altered! </br>

`sudo fdisk -lu` </br>
 显示系统已经识别了硬盘 /dev/sdb 的分区。

## 3.硬盘格式化 | Format hard disk
`sudo mkfs -t ext4 /dev/sda3` </br>
说明：
-t ext4 表示将分区格式化成ext4文件系统类型。

## 4.挂载硬盘分区 | Mount hard disk partition
显示硬盘挂载情况。在终端窗口中输入如下命令： </br>
`sudo df -l` </br>
新硬盘分区没有挂载，无法进入和查看。 </br>
在终端窗口中输入如下命令：</br>
`sudo mount -t ext4 /dev/sda3 /data` </br>
说明：
指定硬盘分区文件系统类型为ext4 ，同时将 /dev/sda3 分区挂载到目录 /data。 </br>
再次在终端窗口中输入如下命令：</br>
`sudo df -l`

## 5.设置开机自动挂载
编辑文件 `sudo vi /etc/fstab` </br>
加入 `/dev/sda3 /data ext4 defaults 0 2`

### 附录1：fdisk命令详解 | Appendix part 1:  fdisk command syntax
fdisk 命令的语法如下：</br>
<pre>
fdisk [-b sectorsize] device 
fdisk -l [-u] [device...]
fdisk -s partition...
fdisk -v 
</pre>
说明： 

- -b    指定每个分区的大小。也可以执行fdisk device（如：fdisk /dev/sdb）后，在系统提示时指定。
- -l    列出指定的外围设备的分区表状况。如果仅执行 fdisk -l ，系统会列出已知的分区。
- -u    搭配"-l"参数列表，会用分区数目取代柱面数目，来表示每个分区的起始地址。
- -s    将指定的分区的大小输出到标准输出上，单位为区块。
- -v   显示fdisk的版本信息。

### 附录2：mkfs命令详解 | Appendix part 2:  mkfs command syntax
mkfs 命令的语法如下：</br>

`mkfs [-V] [-t fstype] [fs-options] filesys`

说明：

- -V   显示简要的使用方法。
- -t   指定要建立何种文件系统，如：ext3, ext4。
- fs   指定建立文件系统时的参数。
- -v   显示版本信息与详细的使用方法。

### 附录3：mount命令详解 | Appendix part 3:  mount command syntax
mkfs 命令的语法如下：</br>

<pre>
mount [-afFnrsvw] [-t vfstype] [-L label]  [-o options] device dir
mount [-lhv]
</pre> 
说明：

- -a      		加载文件/etc/fstab中设置的所有设备。
- -f      		不实际加载设备。可与-v等参数同时使用以查看mount的执行过程。
- -F      		需与-a参数同时使用。所有在/etc/fstab中设置的设备会被同时加载，可加快执行速度。
- -t vfstype   	指定加载的文件系统类型，如：ext3, ext4。
- -L label     	给挂载点指定一个标签名称。
- -l      		显示分区的label。
- -h      		显示帮助信息。
- -v      		显示mount的版本信息。
- device  		要挂载的分区或文件。如果device是一个文件，挂载时须加上 -o loop参数。
- dir     		分区的挂载点。

### 附录4：fstab配置详解 | Appendix part 4:  fstab detail configuration
/etc/fstab 中一共有 6 列：

- file system：指定要挂载的文件系统的设备名称（如：/dev/sdb）。也可以采用UUID，UUID可以通过使用blkid命令来查看（如：blkid  /dev/sdb）指定设备的UUID号。
- mount point：挂载点。就是自己手动创建一个目录，然后把分区挂载到这个目录下。
- type：用来指定文件系统的类型。如：ext3, ext4, ntfs等。
- option dump：0 表示不备份；1表示要将整个中的内容备份。此处建议设置为 0。
- pass：用来指定fsck如何来检查硬盘。0 表示不检查；挂载点为分区/（根分区）必须设置为1，其他的挂载点不能设置为1；如果有挂载ass设置成大于1的值，则在检查完根分区后，然后按pass的值从小到大依次检查，相同数值的同时检查。如：/home　和 /boot　的pass 设置成2，/devdata 的pass 设置成3，则系统在检查完根分区，接着同时检查/boot和/home，再检查/devdata。