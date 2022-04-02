# Redis

## 介绍

此 Chart 使用 [Helm](https://helm.sh) 包管理器在 [Kubernetes](http://kubernetes.io) 集群上引导 Redis 部署。

## 先决条件

- Kubernetes 1.10+ ，并且启用 Beta API

## 安装/更新 Chart

添加仓库：

```bash
# 添加仓库
helm repo add --username=<username> --password=<password> yidejia https://ydjharbor.jingzhuan.cn/chartrepo/base

# 查看仓库列表
helm repo list
```

安装/更新 Chart：

```bash
# 更新仓库
helm repo update

# 部署/更新 Chart
helm upgrade --install <releaseName> -n <namespace> yidejia/redis

# 列出 Release
helm list
```

例子：

```bash
## 说明
# 这里以部署/更新【jxs-cw】后端项目为例。

## 部署/更新 测试环境
helm upgrade --install cw-test-jxs-redis -n cw-back-test yidejia/redis \
    --set image.redisPassword=KneYgDdAvfZaSPeqCMSI9dEJ4RTXFfgq

## 部署/更新 生产环境（注意：生产环境使用云Redis服务，收下仅参考）
helm upgrade --install cw-prod-jxs-redis -n cw-back-prod yidejia/redis \
    --set image.redisPassword=GxwjqvTd1Iw8lF8LLxi4nka7SxB0DIPT \
    --set resources.limits.cpu=200m,resources.limits.memory=512Mi,resources.requests.cpu=150m,resources.requests.memory=128Mi
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
# 这里以卸载【jxs-cw】后端项目为例。

## 卸载测试环境
helm uninstall cw-test-jxs-redis -n cw-back-test
```

该命令将删除与 Chart 关联的所有 Kubernetes 资源并完全删除 Release。

## 配置

参数 | 描述 | 默认
---|---|---
nameOverride                |覆盖 .Chart.Name 名称          |""
fullnameOverride            |覆盖 redis.fullname 名称       |""
image.repository            |`redis` 镜像                           |ydjharbor.jingzhuan.cn/base/redis
image.tag                   |`redis` 标签                           |"4.0.14-alpine"
image.pullPolicy            |`redis` 镜像拉取策略                   |IfNotPresent
image.redisPassword         |`redis` 容器的 requirepass      |""
imagePullSecrets.name       |包含私有`registry`凭证的 Secret 资源的名称   |regcred
metrics.enabled             |启用 Metrics                | true
metrics.repository          |`metrics` 镜像              | ydjharbor.jingzhuan.cn/devops/redis_exporter
metrics.pullPolicy          |`metrics` 镜像拉取策略       | IfNotPresent
metrics.tag                 |`metrics` 标签              | "v1.6.1"
podAnnotations              |Pod 的注释                    | {}
podSecurityContext          |Pod 的安全上下文             | {}
securityContext             |设置安全上下文                | {}
resources                   |CPU、内存的资源请求、限制       | {}
nodeSelector                |Pod 分配的节点标签            | {}
tolerations                 |Pod 污点容忍度                | []
affinity                    |Pod 分配的亲和性规则           | {}
service.enabled             |启用服务                     | true
service.type                |服务类型                      | ClusterIP
service.port                |服务端口                      | 6379
service.annotations         |服务注释                      | prometheus.io/scrape: "true"<br>prometheus.io/port: "9121"  

