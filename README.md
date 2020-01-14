# Percobaan 1

date: 20200114

## Referensi
* https://kubernetes.io/docs/setup/production-environment/container-runtimes/#docker
* https://docs.okd.io/latest/getting_started/administrators.html

## Langkah
### Install docker
```
yum install yum-utils device-mapper-persistent-data lvm2 -y
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum update -y
yum install containerd.io-1.2.10 docker-ce-19.03.4 docker-ce-cli-19.03.4 -y

## Create /etc/docker directory.
mkdir /etc/docker

# Setup daemon.
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": { "max-size": "100m" },
  "storage-driver": "overlay2",
  "storage-opts": ["overlay2.override_kernel_check=true"],
  "insecure-registries": ["172.30.0.0/16"]
}
EOF
mkdir -p /etc/systemd/system/docker.service.d

# Restart Docker
systemctl daemon-reload
systemctl restart docker
systemctl enable docker
```
### Install okd
```
wget https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-server-v3.11.0-0cbc58b-linux-64bit.tar.gz
tar xzf openshift-origin-server-v3.11.0-0cbc58b-linux-64bit.tar.gz
cd openshift-origin-server-v3.11.0-0cbc58b-linux-64bit
./oc cluster up
```
And you will got this after finished
```
Login to server ...                                                                                                                                                                          
Creating initial project "myproject" ...                                                                                                                                                      
Server Information ...                                                                                                                                                                       
OpenShift server started.                                                                                                                                                                     
                                                                                                                                                                                             
The server is accessible via web console at:                                                                                                                                                  
    https://127.0.0.1:8443                                                                                                                                                                   
                                                                                                                                                                                              
You are logged in as:                                                                                                                                                                        
    User:     developer                                                                                                                                                                       
    Password: <any value>                                                                                                                                                                    
                                                                                                                                                                                              
To login as administrator:                                                                                                                                                                   
    oc login -u system:admin     
```

