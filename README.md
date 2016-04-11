DPDK: Data Plane Development Kit
===

* [dpdk.org][dpdk_org] 官网
* [开发手册][grop_guide]
* [示例文档][sample]
* source
  - [dpdk](git://dpdk.org/dpdk)
  - [pktgen](git://dpdk.org/apps/pktgen-dpdk)

[dpdk_org]:http://dpdk.org
[grop_guide]:http://dpdk.org/doc/guides/prog_guide
[sample]:http://dpdk.org/doc/guides/sample_app_ug/index.html


## quick start

#### install
```
wget http://www.dpdk.org/browse/dpdk/snapshot/dpdk-2.2.0.tar.gz
tar xzvf dpdk-2.2.0.tar.gz
sudo apt-get install linux-headers-`uname -r`
sudo apt-get install libpcap-dev

cd dpdk-2.2.0
export RTE_SDK=`pwd`
export RTE_TARGET=x86_64-native-linuxapp-gcc
make config T=x86_64-native-linuxapp-gcc
sed -ri 's,(PMD_PCAP=).*,\1y,' build/.config
make
make install T=x86_64-native-linuxapp-gcc

sudo mkdir /mnt/huge
sudo echo 'huge /mnt/huge hugetlbfs defaults 0 0' >> /etc/fstab
sudo mount /mnt/huge
sudo echo 256 > /sys/devices/system/node/node0/hugepages/hugepages-2048kB/nr_hugepages
sudo chmod 777 /mnt/huge

sudo vi /etc/sysctl.conf
#Add to the bottom of the file:
vm.nr_hugepages=256

#bind nic
./tools/dpdk_nic_bind.py --bind=igb_uio 01:00.0 01:00.1
```


#### build example
```shell
cd examples/load_balancer
make
```
