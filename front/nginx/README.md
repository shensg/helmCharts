# Nginx

## 介绍

此 Chart 使用 [Helm](https://helm.sh) 包管理器在 [Kubernetes](http://kubernetes.io) 集群上引导【前端项目】部署。

## 先决条件

- Kubernetes 1.10+ ，并且启用 Beta API

## 安装/更新 Chart

添加仓库：

```bash
# 添加仓库
helm repo add --username=<username> --password=<password> stable https://harhor.example.local/chartrepo/base

# 查看仓库列表
helm repo list
```

安装/更新 Chart：

```bash
# 更新仓库
helm repo update

# 部署/更新 Chart
helm upgrade --install <releaseName> -n <namespace> yidejia/nginx \
    --set image.repository=<dockerRepo> \
    --set image.tag=<dockerTag> \
    --set ingress.hosts={<domainName>}

# 列出 Release
helm list
```

例子：

```bash
## 说明

## 部署/更新
# 单域名
helm upgrade --install my-nginx -n front statle/nginx \
    --set replicaCount=3 \
    --set image.repository=nginx \
    --set image.tag=16-alpine \
    --set ingress.hosts={test.example.com}

# 双域名
helm upgrade --install my-nginx -n front statle/nginx \
    --set replicaCount=3 \
    --set image.repository=nginx \
    --set image.tag=16-alpine \
    --set ingress.hosts={test.example.com\,test.example.com}
```

【配置】部分列出了安装期间可以配置的参数。

## 调试 Chart

命令：

    helm lint：检查 Chart 中可能出现的问题。

    helm install --dry-run --debug 或 helm template --debug：让服务器渲染模板，然后返回生成的清单文件。

    helm get manifest：查看安装在服务器上的模板。

## 卸载 Chart

卸载 Chart：

```bash
helm uninstall <releaseName> -n <namespace>
```

例子：

```bash
## 说明

## 卸载
helm uninstall my-nginx -n front
```

该命令将删除与 Chart 关联的所有 Kubernetes 资源并完全删除 Release。

## 配置

参数 | 描述 | 默认
---|---|---
nameOverride                |覆盖 .Chart.Name 名称          |""
fullnameOverride            |覆盖 nginx.fullname 名称       |""
replicaCount                |Pod 副本数           |1
revisionHistoryLimit        |历史版本数限制        |2
image.repository            |`nginx` 镜像                        |nginx
image.tag                   |`nginx` 标签                     |""
image.pullPolicy            |`nginx` 镜像拉取策略             |IfNotPresent
image.containerPort         |`nginx` 容器的监听端口           |80
image.livenessProbeEnabled  |`nginx` 容器启用 存活探针        |true
image.livenessProbePath     |`nginx` 容器的存活探针 httpGet.path |/
imagePullSecrets.name       |包含私有`registry`凭证的 Secret 资源的名称   |imageSecret
podAnnotations              |Pod 的注释    |{}
podSecurityContext          |Pod 的安全上下文    |{}
securityContext             |设置安全上下文            |{}
resources                   |CPU、内存的资源请求、限制  |{}
nodeSelector                |Pod 分配的节点标签    |{}
tolerations                 |Pod 污点容忍度      |[]
affinity                    |Pod 分配的亲和性规则   |{}
hostAliases                 |Pod 的 hostAliases 配置       | []
service.type                |服务类型                   |ClusterIP
service.port                |服务端口                   |80
ingress.enabled             |启用 Ingress                 |true
ingress.className           |Ingress className       |""
ingress.annotations         |Ingress 注释             |kubernetes.io/ingress.class: edge<br>kubernetes.io/ingress.rule-mix: "false"<br>nginx.ingress.kubernetes.io/use-regex: "true"<br>nginx.ingress.kubernetes.io/ssl-redirect: 'true'
ingress.hosts               |Ingress 域名               |{chart-example.local}
ingress.tls.enabled         |启用 Ingress TLS           |true


