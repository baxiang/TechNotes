 修改主机名
之前的版本可以直接在 etc/hostname 中 直接修改，但是新的版本有改动

vim /etc/cloud/cloud.cfg

需要将 preserve_hostname 修改为true

vim /etc/hostname
修改主机名后reboot
