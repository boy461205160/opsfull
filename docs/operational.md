
## 一、服务重启
```
#master
systemctl restart kube-scheduler
systemctl restart kube-controller-manager
systemctl restart kube-apiserver
systemctl restart flannel
systemctl restart etcd

systemctl stop kube-scheduler
systemctl stop kube-controller-manager
systemctl stop kube-apiserver
systemctl stop flannel
systemctl stop etcd

systemctl status kube-apiserver
systemctl status kube-scheduler
systemctl status kube-controller-manager
systemctl status etcd

#node
systemctl restart kubelet
systemctl restart kube-proxy
systemctl restart flannel
systemctl restart etcd

systemctl stop kubelet
systemctl stop kube-proxy
systemctl stop flannel
systemctl stop etcd

systemctl status kubelet
systemctl status kube-proxy
systemctl status flannel
systemctl status etcd

#查询健康状况
[root@linux-node1 ~]# kubectl get cs
NAME                 STATUS    MESSAGE             ERROR
controller-manager   Healthy   ok
scheduler            Healthy   ok
etcd-0               Healthy   {"health":"true"}
etcd-2               Healthy   {"health":"true"}
etcd-1               Healthy   {"health":"true"}

#查询node
[root@linux-node1 ~]# kubectl get node -o wide
NAME            STATUS    ROLES     AGE       VERSION   EXTERNAL-IP   OS-IMAGE                KERNEL-VERSION          CONTAINER-RUNTIME
192.168.56.12   Ready     <none>    2m        v1.10.3   <none>        CentOS Linux 7 (Core)   3.10.0-862.el7.x86_64   docker://18.6.1
192.168.56.13   Ready     <none>    2m        v1.10.3   <none>        CentOS Linux 7 (Core)   3.10.0-862.el7.x86_64   docker://18.6.1

#创建测试deployment
[root@linux-node1 ~]# kubectl run net-test --image=alpine --replicas=2 sleep 360000

#查询pod
[root@linux-node1 ~]# kubectl get pod -o wide
NAME                        READY     STATUS    RESTARTS   AGE       IP          NODE
net-test-5767cb94df-6smfk   1/1       Running   1          1h        10.2.69.3   192.168.56.12
net-test-5767cb94df-ctkhz   1/1       Running   1          1h        10.2.17.3   192.168.56.13

#查询service
[root@linux-node1 ~]# kubectl get service
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.1.0.1     <none>        443/TCP   4m
```