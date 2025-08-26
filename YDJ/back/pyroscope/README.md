# Pyroscope

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
    --set replicaCount=<replicas> \
    --set image.port=<ports> \
    --set image.readinessProbeEnabled=true \
    --set persistence.enabled=true \
    --set ingress.hosts={<domainName>}

# 列出 Release
helm list
```

```bash
#【配置】部分列出了安装期间可以配置的参数。
helm upgrade --install <deployName> -n <Namespace> <stableName>/<templateName> \
    --set image.repository=<DockerImage> \
    --set image.tag=<dockerTag> \
    --set replicaCount=<replicas> \
    --set image.port=<ports> \
    --set image.readinessProbeEnabled=true \
    --set persistence.enabled=true \
    --set ingress.hosts={<domainName>}
```
## 更新 Chart

更新 Chart：

```bash
# 更新仓库
helm repo update

# 更新 Chart
helm upgrade --install <deployName> -n <Namespace> <stableName>/<templateName> \
    --set image.repository=<DockerImage> \
    --set image.tag=<dockerTag> \
    --set replicaCount=<replicas> \
    --set image.port=<ports> \
    --set image.readinessProbeEnabled=true \
    --set persistence.enabled=true \
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
revisionHistoryLimit        |历史版本数限制         |2
image.repository            |镜像                        |pyroscope
image.tag                   |标签                     |""
image.pullPolicy            |镜像拉取策略             |IfNotPresent
image.port                  |容器的监听端口           |4040
image.readinessProbeEnabled |容器启用 存活探针        |false
readinessProbe.httpGet.path |容器的存活探针 httpGet.path |/
readinessProbe.httpGet.port |容器的存活探针 httpGet.port |http
args                        |容器的运行参数              |""
timezone                    |时区 |"Asia/Shanghai"
imagePullSecrets            |包含私有`registry`凭证的 Secret 资源的名称   |- name: imageSecret
podAnnotations              |Pod 的注释    |{}
serviceAnnotations          |Service 的注释    |{}
podSecurityContext          |Pod 的安全上下文    |{}
securityContext             |设置安全上下文            |{}
podManagementPolicy         |pod管理策略           |Parallel
updateStrategy.type         |更新策略           |RollingUpdate
service.enable              |启用service               |true
service.type                |服务类型                   |ClusterIP
service.port                |服务端口                   |80
ingress.enabled             |启用 Ingress 默认添加hosts有配置即启用  |true
ingress.className           |Ingress className       |""
ingress.annotations         |Ingress 注释             |kubernetes.io/ingress.class: edge<br>kubernetes.io/ingress.rule-mix: "false"<br>nginx.ingress.kubernetes.io/use-regex: "true"<br>nginx.ingress.kubernetes.io/ssl-redirect: 'true'
ingress.hosts               |Ingress 域名               |{chart-example.local}
ingress.tls.enabled         |启用 Ingress TLS           |true
resources                   |CPU、内存的资源请求、限制  |limits<br>&nbsp;&nbsp;cpu=1<br>&nbsp;&nbsp;memory=2Gi<br>requests<br>&nbsp;&nbsp;cpu=100m<br>&nbsp;&nbsp;memory=128Mi
readinessProbe:             |就绪探针  |httpGet:<br>&nbsp;&nbsp;path: /<br>&nbsp;&nbsp;port: http<br>initialDelaySeconds: 10<br>periodSeconds: 30<br>timeoutSeconds: 60<br>successThreshold: 15<br>failureThreshold: 15
autoscaling.enabled         |是否开启自动伸缩配置          |false
autoscaling.minReplicas     |自动伸缩最小副本集下限          |1
autoscaling.maxReplicas     |自动伸缩最大副本集上限        |100
autoscaling.targetCPUUtilizationPercentage        |通过CPU使用率触发自动伸缩的机制          |80
hostAliases                 |绑定容器本地hosts              |[]
nodeSelector                |Pod 分配的节点标签    |{}
tolerations                 |Pod 污点容忍度      |[]
affinity                    |Pod 分配的亲和性规则   |{}
persistence:                |持久化逻辑卷，nfc格式  |enabled: false<br>storageClassName: nfs-default<br>storage: 10Gi<br>accessModes:<br>&nbsp;&nbsp;- ReadWriteOnce
basicAuth:                  |nginx入口验证用户  |enabled: false<br>type: Opaque<br>username: "admin"<br>password: "123456"
