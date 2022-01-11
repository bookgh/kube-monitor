## imgutils

功能: 下载镜像加速,打包镜像

### 使用方法 1

> 下载脚本

    curl -so /usr/local/bin/imgutils https://cdn.jsdelivr.net/gh/bookgh/kube-monitor@main/imgutils
    chmod +x /usr/local/bin/imgutils

> 下载镜像

    imgutils -P quay.io/prometheus/blackbox-exporter:v0.19.0
    imgutils -P k8s.gcr.io/prometheus-adapter/prometheus-adapter:v0.9.1

> 保存镜像

    imgutils -S -d /mnt -r k8s.gcr.io
 

### 使用方法 2

> 下载镜像 Example 1

    bash <(curl https://cdn.jsdelivr.net/gh/bookgh/kube-monitor@main/imgutils) -P k8s.gcr.io/prometheus-adapter/prometheus-adapter:v0.9.1

> 下载镜像 Example 2

    curl -s https://cdn.jsdelivr.net/gh/bookgh/kube-monitor@main/imgutils | bash -s -- -P k8s.gcr.io/prometheus-adapter/prometheus-adapter:v0.9.1

### 可用镜像

    imgutils -P k8s.gcr.io/prometheus-adapter/prometheus-adapter:v0.9.1
    imgutils -P k8s.gcr.io/kube-state-metrics/kube-state-metrics:v2.3.0
    

### 下载 kube-prometheus 镜像

> 下载 kube-prometheus 仓库

    git clone https://github.com/prometheus-operator/kube-prometheus.git

> 列出仓库内使用的镜像名

    cd kube-prometheus-main/manifests
    find ./ -type f |xargs grep 'image: '|sort|uniq|awk '{print $3}'|grep ^[a-zA-Z]|grep -Evw 'error|kubeRbacProxy'|sort -rn|uniq

> 下载仓库内使用的镜像

    for image in $(find ./ -type f |xargs grep 'image: '|sort|uniq|awk '{print $3}'|grep ^[a-zA-Z]|grep -Evw 'error|kubeRbacProxy'|sort -rn|uniq); do
        imgutils -P $image
    done

> 导出镜像

    for url in $(find ./ -type f |xargs grep 'image: '|sort|uniq|awk '{print $3}'|grep ^[a-zA-Z]|grep -Evw 'error|kubeRbacProxy'|sort -rn|awk -F/ '{print $1}'|uniq); do
        imgutils -S -d /mnt/kube-prometheus -r $url
    done

> 导入镜像

    for file in $(ls /mnt/kube-prometheus/*); do
        docker load -i $file
    done
