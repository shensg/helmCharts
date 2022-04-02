# Golang

## 介绍

此 Chart 使用 [Helm](https://helm.sh) 包管理器在 [Kubernetes](http://kubernetes.io) 集群上引导【Golang项目】部署。

## 先决条件

- Kubernetes 1.10+ ，并且启用 Beta API

## 安装 Chart

添加仓库：

```bash
# 添加仓库
helm repo add --username=<username> --password=<password> <stableName> <helmRegistryRepoUrl>

# 查看仓库列表
helm repo list
```

安装 Chart：

```bash
# 更新仓库
helm repo update

# 部署 Chart
helm upgrade --install <deployName> -n <Namespace> <stableName>/<templateName> \
    --set image.repository=<DockerImage> \
    --set image.tag=<dockerTag> \
    --set image.command=<command> \
    --set replicaCount=<replicas> \
    --set image.containerPort=<ports> \
    --set image.livenessProbe=true \
    --set filebeat.enabled=true \
    --set filebeat.configMapName=<filebeatConfigName> \
    --set image.configMapName=<configName> \
    --set hostAliases.ip=<ipaddress> \
    --set hostAliases.hostnames=<hostname> \
    --set resources.limits.cpu=<cpuResource>,resources.limits.memory=<MemResource> \
    --set resources.requests.cpu=<cpuResource>,resources.requests.memory=<MemResource> \
    --set ingress.hosts={<domainName>}

# 列出 Release
helm list
```

```bash
#【配置】部分列出了安装期间可以配置的参数。
helm install --dry-run --debug <deployName> -n <Namespace> <stableName>/<templateName> \
    --set image.repository=<DockerImage> \
    --set image.tag=<dockerTag> \
    --set image.command=<command> \
    --set replicaCount=<replicas> \
    --set image.containerPort=<ports> \
    --set image.livenessProbe=true \
    --set filebeat.enabled=true \
    --set filebeat.configMapName=<filebeatConfigName> \
    --set image.configMapName=<configName> \
    --set hostAliases.ip=<ipaddress> \
    --set hostAliases.hostnames=<hostname> \
    --set resources.limits.cpu=<cpuResource>,resources.limits.memory=<MemResource> \
    --set resources.requests.cpu=<cpuResource>,resources.requests.memory=<MemResource> \
    --set ingress.hosts={<domainName>}
```
## 更新 Chart

更新 Chart：

```bash
# 更新仓库
helm repo update

# 更新 Chart
helm upgrade <deployName> -n <Namespace> <stableName>/<templateName> \
    --set image.repository=<DockerImage> \
    --set image.tag=<dockerTag> \
    --set image.command=<command> \
    --set replicaCount=<replicas> \
    --set image.containerPort=<ports> \
    --set image.livenessProbe=true \
    --set filebeat.enabled=true \
    --set filebeat.configMapName=<filebeatConfigName> \
    --set image.configMapName=<configName> \
    --set hostAliases.ip=<ipaddress> \
    --set hostAliases.hostnames=<hostname> \
    --set resources.limits.cpu=<cpuResource>,resources.limits.memory=<MemResource> \
    --set resources.requests.cpu=<cpuResource>,resources.requests.memory=<MemResource> \
    --set ingress.hosts={<domainName>}
```

## 调试 Chart

命令：

    helm lint：检查 Chart 中可能出现的问题。

    helm install --dry-run --debug 或 helm template --debug：让服务器渲染模板，然后返回生成的清单文件。

    helm get manifest：查看安装在服务器上的模板。
    
    helm upgrade --history-max=1：设置helm版本保留数

## 卸载 Chart

卸载 Chart：

```bash
helm uninstall <releaseName> -n <namespace>
# 或 
helm delete <releaseName> -n <namespace>
```

该命令将删除与 Chart 关联的所有 Kubernetes 资源并完全删除 Release。

## 配置

参数 | 描述 | 默认
---|---|---
nameOverride                |覆盖 .Chart.Name 名称          |""
fullnameOverride            |覆盖 nginx.fullname 名称       |""
replicaCount                |Pod 副本数           |1
image.repository            |`golang` 镜像                        |golang
image.tag                   |`golang` 标签                     |""
image.pullPolicy            |`golang` 镜像拉取策略             |IfNotPresent
image.containerPort         |`golang` 容器的监听端口           |8080
image.livenessProbeEnabled  |`golang` 容器启用 存活探针        |false
image.livenessProbePath     |`golang` 容器的存活探针 httpGet.path |/ping
image.command               |`golang` 容器的运行命令 /wwwroot/main/\<serverName\> |""
image.configMapName         |`golang` 容器的 ConfigMap 资源名称 |${team}-${env}-${project}-conf
filebeat.enabled            |`filebeat` 启用filebeat日志搜集           |false
filebeat.configMapName      |`filebeat` filebeat配置文件           |filebeat-conf
filebeat.repository         |`filebeat` 镜像              |filebeat
filebeat.tag                |`filebeat` 标签              |"7.10.1"
filebeat.logsMountPath      |`filebeat` 容器需收集的日志文件路径      | /logs
configmap.enabled           |`额外参数` 启用 ConfigMap                 |false
configmap.payEnabled        |`额外参数` 挂载payCertPublicKey          |false
configmap.payConfName       |`额外参数` 挂载支付使用公钥文件名           |${team}-${env}-${project}-crt-conf
hostAliases                 |`额外参数` 绑定容器本地hosts              |[]
imagePullSecrets.name       |包含私有`registry`凭证的 Secret 资源的名称   |regcred
podAnnotations              |Pod 的注释    |{}
serviceAnnotations          |Service 的注释    |{}
podSecurityContext          |Pod 的安全上下文    |{}
securityContext             |设置安全上下文            |{}
resources                   |CPU、内存的资源请求、限制  |{}
nodeSelector                |Pod 分配的节点标签    |{}
tolerations                 |Pod 污点容忍度      |[]
affinity                    |Pod 分配的亲和性规则   |{}
service.enable              |启用service               |true
service.type                |服务类型                   |ClusterIP
service.port                |服务端口                   |80
ingress.enabled             |启用 Ingress 默认添加hosts有配置即启用  |true
ingress.className           |Ingress className       |""
ingress.annotations         |Ingress 注释             |kubernetes.io/ingress.class: edge<br>kubernetes.io/ingress.rule-mix: "false"<br>nginx.ingress.kubernetes.io/use-regex: "true"<br>nginx.ingress.kubernetes.io/ssl-redirect: 'true'
ingress.hosts               |Ingress 域名               |{chart-example.local}
ingress.tls.enabled         |启用 Ingress TLS           |true
autoscaling.enabled         |是否开启自动伸缩配置          |false
autoscaling.minReplicas     |自动伸缩最小副本集下限          |3
autoscaling.maxReplicas     |自动伸缩最大副本集上限        |20
autoscaling.targetCPUUtilizationPercentage        |通过CPU使用率触发自动伸缩的机制          |60
autoscaling.targetMemoryUtilizationPercentage     |通过Memory使用率触发自动伸缩的机制       |60
