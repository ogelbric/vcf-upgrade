# VCF-Upgrade 9.0 -> 9.0.1

## Create off line Depot

```
A Server with disk space is need!
I used my Linix jump server and added an extra disk with 2TB
And then a https server with auth is needed vor the VCF upgrade
```

# Adding disk space
```
#in vCenter add a disk to your linux jump host VM
```

![GitHub](AddDisk1.png)

```
#Disk shows up as adb
	lsblk
#sdb                         8:16   0     2T  0 disk
#
# looks like sdb      8:16   0  150G  0 disk  is my new disk 
	sudo parted /dev/sdb
	mklabel gpt
	print
	mkpart primary 0 1995GB
	Ignore
	quit
	sudo mkfs.ext4 /dev/sdb1
	sudo mkdir /bigdisk
	sudo mount /dev/sdb1 /bigdisk
	
	sudo blkid /dev/sdb1

#/dev/sdb1: UUID="9dd30ea6-ddb9-4396-98fa-f5eeddface3d" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="primary" PARTUUID="4c81fe73-c946-41b4-9890-268dd2ca2683"
#UUID=9dd30ea6-ddb9-4396-98fa-f5eeddface3d /mnt/mydrive ext4 defaults 0 0

#add this to vfstab: /dev/disk/by-uuid/9dd30ea6-ddb9-4396-98fa-f5eeddface3d /mnt/mydrive ext4 defaults 0 0

sudo vi /etc/vfstab
```

# Creating quick https server

```
Following William: https://williamlam.com/2025/01/quick-tip-easily-host-vmware-cloud-foundation-vcf-offline-depot-using-python-simplehttpserver-with-authentication.html

#Quick test for the web server
sudo python3 -m http.server 9000

#Test with above sript
sudo python3 http_server_auth.py --bind 127.0.0.1 --user vcf --password vcf123! --port 8888 --directory depot


a

```


