# Setup Raid 1 on Ubuntu server 20.04

#### This tutorial is explaining how to create raid1 with ubuntu server 20.04 whle ypu are installing Ubuntu server.

## Requirment :
1. Two HDD with same Storage capacity.
2. Ubuntu server 20.04 setup on bootable device e.g.flash disk.

## Steps : 
1. Boot from Ububntu setup
2. Do the installation. Untill you reach **Storage configuration**, Choose custom storage layout.
3. reformat the two disks.
4. Head over the storages and select them as **Boot device**.
5. We need to create three partitions of the **two** disks, Read The underneath points **before** creation.
  * Create Three partitions for **/boot**,**/** and **swap** respectively with 10G,10G and 920G capacities  where I assummed 1TB HDD.
  * for **/boot** and **/** partitions, make field of **format** : **leave unformatted**.
  * for **swap** partition, make field of **format** : **swap**.
  * Repeat the **Creation** for the another disk.
6. After creating the partitions, WE need to create **raid1** as following :
  * For **md0** Head towards **Create software raid** choose raid level :  "1 mirrored".
  * For **md0** choose partition 2 from the both Hdds then press create.
  * For **md1** Head towards **Create software raid** choose raid level :  "1 mirrored".
  * For **md1** choose partition 3 from the both HDDs then press create.
7. After creating the md*, we to mount them to the root and to the boot as following :
  * Head toward md0. select Add GPT partition and modify mount  to  **/boot**.
  * Head toward md1. select Add GPT partition and modify mount  to  **/**.
8. By now, We are ready to go.

# THe END
