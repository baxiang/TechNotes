![图片.png](https://upload-images.jianshu.io/upload_images/143845-4178bfa396df10eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#删除所有容器
sudo docker rm -f $(sudo docker ps -qa)

#删除/var/etcd目录
sudo rm -rf /var/etcd

#删除/var/lib/kubelet/目录，删除前先卸载
for m in $(sudo tac /proc/mounts | sudo awk '{print $2}'|sudo grep /var/lib/kubelet);do
 sudo umount $m||true
done
sudo rm -rf /var/lib/kubelet/

#删除/var/lib/rancher/目录，删除前先卸载
for m in $(sudo tac /proc/mounts | sudo awk '{print $2}'|sudo grep /var/lib/rancher);do
 sudo umount $m||true
done
sudo rm -rf /var/lib/rancher/

#删除/run/kubernetes/ 目录
sudo rm -rf /run/kubernetes/

#删除所有的数据卷
sudo docker volume rm $(sudo docker volume ls -q)

#再次显示所有的容器和数据卷，确保没有残留
sudo docker ps -a
sudo docker volume ls
