## imgutils

功能: 下载镜像加速,打包镜像

### 使用方法 1

> 下载脚本

    curl -so /usr/local/bin/imgutils https://cdn.jsdelivr.net/gh/bookgh/kube-monitor@main/imgutils
    chmod +x /usr/local/bin/imgutils

> 下载镜像

    imgutils quay.io/prometheus/blackbox-exporter:v0.19.0
    imgutils k8s.gcr.io/prometheus-adapter/prometheus-adapter:v0.9.1

> 保存镜像

    imgutils -S -d /mnt -r k8s.gcr.io
 

### 使用方法 2

> 下载镜像 Example 1

    bash <(curl https://cdn.jsdelivr.net/gh/bookgh/kube-monitor@main/imgutils) -P k8s.gcr.io/prometheus-adapter/prometheus-adapter:v0.9.1

> 下载镜像 Example 2

    curl -s https://cdn.jsdelivr.net/gh/bookgh/kube-monitor@main/imgutils | bash -s -- -P k8s.gcr.io/prometheus-adapter/prometheus-adapter:v0.9.1


### 列出当前目录下所有镜像

    find ./ -type f |xargs grep 'image: '|sort|uniq|awk '{print $3}'|grep ^[a-zA-Z]|grep -Evw 'error|kubeRbacProxy'|sort -rn|uniq

### 可用镜像

    imgutils k8s.gcr.io/prometheus-adapter/prometheus-adapter:v0.9.1
    imgutils k8s.gcr.io/kube-state-metrics/kube-state-metrics:v2.3.0
