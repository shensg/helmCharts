# Nsqd

## 介绍

此 Chart 使用 [Helm](https://helm.sh) 包管理器在 [Kubernetes](http://kubernetes.io) 集群上引导 Nsqd 部署。

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
helm upgrade --install <releaseName> -n <namespace> stable/nsqd

# 列出 Release
helm list
```

例子：

```bash
## 说明

## 部署/更新 测试
helm upgrade --install me -n tool stable/nsqd \
     --set nsqadmin.ingress.hosts={nsq.example.com}

## 部署/更新 增加自定义参数
helm upgrade --install --debug me -n tool stable/nsqd \
     --set nsqd.replicaCount=4,nsqlookupd.replicaCount=4,nsqadmin.replicaCount=2 \
     --set nsqd.resources.limits.cpu=1,nsqd.resources.limits.memory=1Gi,nsqd.resources.requests.cpu=64m,nsqd.resources.requests.memory=128Mi \
     --set nsqlookupd.resources.limits.cpu=500m,nsqlookupd.resources.limits.memory=1Gi,nsqlookupd.requests.cpu=50m,nsqlookupd.resources.requests.memory=100Mi \
     --set nsqadmin.resources.limits.cpu=500m,nsqadmin.resources.limits.memory=1Gi,nsqadmin.requests.cpu=50m,nsqadmin.resources.requests.memory=100Mi \
     --set nsqadmin.ingress.hosts={nsq.example.com}
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
# 这里以卸载【cim】后端项目为例。

## 卸载测试环境
helm uninstall me -n tool
```

该命令将删除与 Chart 关联的所有 Kubernetes 资源并完全删除 Release。

## 配置

参数 | 描述 | 默认
---|---|---
image.repository                |镜像                           |nsq
image.tag                       |标签                           |"v1.1.0"
image.pullPolicy                |镜像拉取策略                     |IfNotPresent
imagePullSecrets.name           |包含私有`registry`凭证的 Secret 资源的名称   |imageSecret
nsqlookupd.replicaCount         |`nsqlookupd` Pod 副本数                   | 2
nsqlookupd.service.type         |`nsqlookupd` Service 类型                 | ClusterIP
nsqlookupd.podAnnotations       |`nsqlookupd` Pod 的注释                   | {}
nsqlookupd.podSecurityContext   |`nsqlookupd` Pod 的安全上下文              | {}
nsqlookupd.securityContext      |`nsqlookupd` 设置安全上下文                | {}
nsqlookupd.resources            |`nsqlookupd` CPU、内存的资源请求、限制       | {}
nsqlookupd.nodeSelector         |`nsqlookupd` 标签                         | {}
nsqlookupd.tolerations          |`nsqlookupd` 污点容忍度                    | []
nsqlookupd.affinity             |`nsqlookupd` 亲和性规则                    | {}
nsqd.replicaCount               |`nsqd` Pod 副本数                |2
nsqd.flags                      |`nsqd` 运行自定义参数              |[]
nsqd.podAnnotations             |`nsqd` Pod 的注释                | {}
nsqd.podSecurityContext         |`nsqd` Pod 的安全上下文           | {}
nsqd.securityContext            |`nsqd` 设置安全上下文              | {}
nsqd.resources                  |`nsqd` CPU、内存的资源请求、限制    | {}
nsqd.nodeSelector               |`nsqd` 标签                      | {}
nsqd.tolerations                |`nsqd` 污点容忍度                 | []
nsqd.affinity                   |`nsqd` 亲和性规则                 | podAntiAffinity:<br>&nbsp;&nbsp;requiredDuringSchedulingIgnoredDuringExecution:<br>&nbsp;&nbsp;&nbsp;&nbsp;- topologyKey: "kubernetes.io/hostname"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;labelSelector:<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;matchExpressions:<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- key: app<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;operator: In<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;values:<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-nsqd
nsqd.service.type               |`nsqd` Service 类型              | ClusterIP
nsqd.persistence.enabled        |`nsqd` 是否磁盘挂载                | false
nsqd.persistence.size           |`nsqd` 设置磁盘大小                | 5Gi
nsqd.persistence.storageClass   |`nsqd` 存储类型                   | ""
nsqd.persistence.accessModes    |`nsqd` 访问方式                   | "ReadWriteOnce"
nsqadmin.replicaCount           |`nsqadmin` Pod 副本数               | 1
nsqadmin.service.type           |`nsqadmin` Service 类型             | ClusterIP
nsqadmin.service.port           |`nsqadmin` Service 端口             | 80
nsqadmin.service.nodePort       |`nsqadmin` node节点端口              | ""
nsqadmin.podAnnotations         |`nsqadmin` Pod 的注释                | {}
nsqadmin.podSecurityContext     |`nsqadmin` Pod 的安全上下文           | {}
nsqadmin.securityContext        |`nsqadmin` 设置安全上下文             | {}
nsqadmin.ingress.enabled        |`nsqadmin` 是否启用 Ingress，默认添加hosts有配置即启用    | true
nsqadmin.ingress.className      |`nsqadmin` Ingress className        | ""
nsqadmin.ingress.annotations    |`nsqadmin` Ingress 注释              | kubernetes.io/ingress.class: edge<br>kubernetes.io/ingress.rule-mix: "false"<br>nginx.ingress.kubernetes.io/use-regex: "true"<br>nginx.ingress.kubernetes.io/ssl-redirect: "true"<br>nginx.ingress.kubernetes.io/auth-type: basic<br>nginx.ingress.kubernetes.io/auth-secret: basic-auth
nsqadmin.ingress.tls.enabled    |`nsqadmin` TLS是否绑定ssl证书         | true
nsqadmin.ingress.hosts          |`nsqadmin` Ingress 域名              | []
nsqadmin.resources              |`nsqadmin` CPU、内存的资源请求、限制    | {}
nsqadmin.nodeSelector           |`nsqadmin` 标签                     | {}
nsqadmin.tolerations            |`nsqadmin` 污点容忍度                 | []
nsqadmin.affinity               |`nsqadmin` 亲和性规则                 | {}
nsqexport.replicaCount          |`nsqexport` Pod 副本数               | 1
nsqexport.service.type          |`nsqexport` Service 类型             | ClusterIP
nsqexport.service.port          |`nsqexport` Service 端口             | 9527
nsqexport.service.annotations   |`nsqexport` service注释              | prometheus.io/scrape: "true"<br>prometheus.io/port: "9527"
nsqexport.podAnnotations        |`nsqexport` Pod 的注释                | {}
nsqexport.podSecurityContext    |`nsqexport` Pod 的安全上下文           | {}
nsqexport.securityContext       |`nsqexport` 设置安全上下文             | {}
nsqexport.enabled               |`nsqexport` 是否启用nsq监控           | true
nsqexport.image.repository      |`nsqexport` exporter_nsq镜像        | nsq-prometheus-exporter
nsqexport.image.pullPolicy      |`nsqexport` 拉取镜像策略              | IfNotPresent
nsqexport.image.tag             |`nsqexport` 镜像标签                 | v1.0
nsqexport.resources             |`nsqexport` CPU、内存的资源请求、限制   | {}
nsqexport.nodeSelector          |`nsqexport` 标签                     | {}
nsqexport.tolerations           |`nsqexport` 污点容忍度                 | []
nsqexport.affinity              |`nsqexport` 亲和性规则                 | {}
basicAuth.enabled               |`basicAuth` 是否开启配置basic-auth认证  | false
basicAuth.type                  |`basicAuth` secret类型                | Opaque
basicAuth.username              |`basicAuth` 认证的用户                 | "admin"
basicAuth.password              |`basicAuth` 认证的密码                 | "123456"
