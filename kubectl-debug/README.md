## Kubectl-debug

### Install Client
```
export PLUGIN_VERSION=0.1.1
# linux x86_64
curl -Lo kubectl-debug.tar.gz https://github.com/aylei/kubectl-debug/releases/download/v${PLUGIN_VERSION}/kubectl-debug_${PLUGIN_VERSION}_linux_amd64.tar.gz
# macos
curl -Lo kubectl-debug.tar.gz https://github.com/aylei/kubectl-debug/releases/download/v${PLUGIN_VERSION}/kubectl-debug_${PLUGIN_VERSION}_darwin_amd64.tar.gz

tar -zxvf kubectl-debug.tar.gz kubectl-debug
sudo mv kubectl-debug /usr/local/bin/
```
### Install Daemonset debug-agent
```
kubectl apply -f https://raw.githubusercontent.com/aylei/kubectl-debug/master/scripts/agent_daemonset.yml
# 或者使用 helm 安装
helm install -n=debug-agent ./contrib/helm/kubectl-debug
```

### Config
```
# debug-agent 映射到宿主机的端口
# 默认 10027
agentPort: 10027

# 是否开启ageless模式
# 默认 false
agentless: true
# agentPod 的 namespace, agentless模式可用
# 默认 default
agentPodNamespace: default
# agentPod 的名称前缀，后缀是目的主机名, agentless模式可用
# 默认 debug-agent-pod
agentPodNamePrefix: debug-agent-pod
# agentPod 的镜像, agentless模式可用
# 默认 aylei/debug-agent:latest
agentImage: aylei/debug-agent:latest

# debug-agent DaemonSet 的名字, port-forward 模式时会用到
# 默认 'debug-agent'
debugAgentDaemonset: debug-agent
# debug-agent DaemonSet 的 namespace, port-forward 模式会用到
# 默认 'default'
debugAgentNamespace: kube-system
# 是否开启 port-forward 模式
# 默认 false
portForward: true
# image of the debug container
# default as showed
image: nicolaka/netshoot:latest
# start command of the debug container
# default ['bash']
command:
- '/bin/bash'
- '-l
```

### Usage
```
# kubectl 1.12.0 或更高的版本, 可以直接使用:
kubectl debug -h
# 假如安装了 debug-agent 的 daemonset, 可以略去 --agentless 来加快启动速度
# 之后的命令里会略去 --agentless
kubectl debug POD_NAME --agentless

# 假如 Pod 处于 CrashLookBackoff 状态无法连接, 可以复制一个完全相同的 Pod 来进行诊断
kubectl debug POD_NAME --fork

# 假如 Node 没有公网 IP 或无法直接访问(防火墙等原因), 请使用 port-forward 模式
kubectl debug POD_NAME --port-forward --daemonset-ns=kube-system --daemonset-name=debug-agent

# 老版本的 kubectl 无法自动发现插件, 需要直接调用 binary
kubect-debug POD_NAME
```
