---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# 集群故障诊断
{: #cs_troubleshoot}

在使用 {{site.data.keyword.containershort_notm}} 时，请考虑这些用于故障诊断和获取帮助的方法。您还可以检查 [{{site.data.keyword.Bluemix_notm}} 系统的状态 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/bluemix/support/#status)。

您可以执行一些常规步骤来确保集群是最新的：
- 定期[重新启动工作程序节点](cs_cli_reference.html#cs_worker_reboot)，以确保已安装 IBM 自动部署到操作系统的更新和安全补丁
- 针对 {{site.data.keyword.containershort_notm}}，将集群更新到 [Kubernetes 的最新缺省版本](cs_versions.html)

{: shortdesc}

<br />


## 调试集群
{: #debug_clusters}

复查可用于调试集群并查找失败根本原因的选项。

1.  列出集群并找到集群的 `State`。

  ```
  bx cs clusters
  ```
  {: pre}

2.  复查集群的 `State`。

  <table summary="每个表行都应从左到右阅读，其中第一列是集群状态，第二列是描述。">
  <thead>
    <th>集群状态</th>
    <th>描述</th>
    </thead>
    <tbody>
      <tr>
        <td>Deploying</td>
        <td>Kubernetes 主节点尚未完全部署。无法访问集群。</td>
       </tr>
       <tr>
        <td>Pending</td>
        <td>Kubernetes 主节点已部署。正在供应工作程序节点，这些节点在集群中尚不可用。您可以访问集群，但无法将应用程序部署到集群。</td>
      </tr>
      <tr>
        <td>Normal</td>
        <td>集群中的所有工作程序节点都已启动并正在运行。您可以访问集群，并将应用程序部署到集群。</td>
     </tr>
     <tr>
        <td>Warning</td>
        <td>集群中至少有一个工作程序节点不可用，但其他工作程序节点可用，并且可以接管工作负载。</td>
     </tr>
     <tr>
      <td>Critical</td>
      <td>无法访问 Kubernetes 主节点，或者集群中的所有工作程序节点都已停止运行。</td>
     </tr>
    </tbody>
  </table>

3.  如果集群处于 **Warning** 或 **Critical** 状态，或者长时间卡在 **Pending** 状态，请复查工作程序节点的状态。如果集群处于 **Deploying** 状态，请等待集群完全部署后，再复查集群的运行状况。集群处于 **Normal** 状态时，被视为正常运行，此时不需要执行操作。

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

  <table summary="每个表行都应从左到右阅读，其中第一列是集群状态，第二列是描述。">
  <thead>
    <th>工作程序节点状态</th>
    <th>描述</th>
    </thead>
    <tbody>
      <tr>
       <td>Unknown</td>
       <td>由于以下某种原因，Kubernetes 主节点不可访问：<ul><li>您请求了更新 Kubernetes 主节点。在更新期间无法检索到工作程序节点的状态。</li><li>您可能有其他防火墙在保护工作程序节点，或者最近更改了防火墙设置。{{site.data.keyword.containershort_notm}} 需要打开特定 IP 地址和端口，以允许工作程序节点与 Kubernetes 主节点之间进行通信。有关更多信息，请参阅[工作程序节点卡在重新装入循环中](#cs_firewall)。</li><li>Kubernetes 主节点已停止运行。请通过开具 [{{site.data.keyword.Bluemix_notm}} 支持凭单](/docs/support/index.html#contacting-support)来联系 {{site.data.keyword.Bluemix_notm}} 支持。</li></ul></td>
      </tr>
      <tr>
        <td>Provisioning</td>
        <td>正在供应工作程序节点，该节点在集群中尚不可用。您可以在 CLI 输出的 **Status** 列中监视供应过程。如果工作程序节点长时间卡在此状态，并且在 **Status** 列中看不到任何进度，请继续执行下一步以查看供应期间是否发生了问题。</td>
      </tr>
      <tr>
        <td>Provision_failed</td>
        <td>无法供应工作程序节点。继续执行下一步以找到失败的详细信息。</td>
      </tr>
      <tr>
        <td>Reloading</td>
        <td>正在重新装入工作程序节点，该节点在集群中不可用。您可以在 CLI 输出的 **Status** 列中监视重新装入过程。如果工作程序节点长时间卡在此状态，并且在 **Status** 列中看不到任何进度，请继续执行下一步以查看重新装入期间是否发生了问题。</td>
       </tr>
       <tr>
        <td>Reloading_failed</td>
        <td>无法重新装入工作程序节点。继续执行下一步以找到失败的详细信息。</td>
      </tr>
      <tr>
        <td>Normal</td>
        <td>工作程序节点已完全供应并准备就绪，可以在集群中使用。</td>
     </tr>
     <tr>
        <td>Warning</td>
        <td>工作程序节点将达到内存或磁盘空间的限制。</td>
     </tr>
     <tr>
      <td>Critical</td>
      <td>工作程序节点的磁盘空间不足。</td>
     </tr>
    </tbody>
  </table>

4.  列出工作程序节点的详细信息。

  ```
  bx cs worker-get <worker_node_id>
  ```
  {: pre}

5.  查看常见错误消息并了解如何解决这些错误。

  <table>
    <thead>
    <th>错误消息</th>
    <th>描述和解决方法
    </thead>
    <tbody>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure 异常：当前禁止您的帐户订购“计算实例”。</td>
        <td>您的 IBM Bluemix Infrastructure (SoftLayer) 帐户可能受到限制，无法订购计算资源。请通过开具 [{{site.data.keyword.Bluemix_notm}} 支持凭单](/docs/support/index.html#contacting-support)来联系 {{site.data.keyword.Bluemix_notm}} 支持。</td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure 异常：无法下单。路由器“router_name”后的资源不足，无法实现以下访客的请求：“worker_id”。</td>
        <td>所选的 VLAN 与数据中心内没有足够空间来供应工作程序节点的 pod 相关联。有以下选项可供选择：<ul><li>使用其他数据中心来供应工作程序节点。运行 <code>bx cs locations</code> 以列出可用的数据中心。<li>如果您有与数据中心内另一个 pod 相关联的现有公用和专用 VLAN 对，请改为使用此 VLAN 对。<li>请通过开具 [{{site.data.keyword.Bluemix_notm}} 支持凭单](/docs/support/index.html#contacting-support)来联系 {{site.data.keyword.Bluemix_notm}} 支持。</ul></td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure 异常：无法获取标识为 &lt;vlan id&gt; 的网络 VLAN。</td>
        <td>由于以下某种原因而找不到所选的 VLAN 标识，因此无法供应工作程序节点：<ul><li>您可能指定的是 VLAN 编号，而不是 VLAN 标识。VLAN 编号的长度为 3 位或 4 位，而 VLAN 标识的长度为 7 位。运行 <code>bx cs vlans &lt;location&gt;</code> 以检索 VLAN 标识。<li>该 VLAN 标识可能未与所使用的 IBM Bluemix Infrastructure (SoftLayer) 帐户相关联。运行 <code>bx cs vlans &lt;location&gt;</code> 以列出您帐户的可用 VLAN 标识。要更改 IBM Bluemix Infrastructure (SoftLayer) 帐户，请参阅 [bx cs credentials-set](cs_cli_reference.html#cs_credentials_set)。</ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation：为此订单提供的位置无效。(HTTP 500)</td>
        <td>IBM Bluemix Infrastructure (SoftLayer) 未设置为订购所选数据中心内的计算资源。请联系 [{{site.data.keyword.Bluemix_notm}} 支持](/docs/support/index.html#contacting-support)，以验证您的帐户是否正确设置。</td>
       </tr>
       <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure 异常：用户没有必需的 {{site.data.keyword.Bluemix_notm}} Infrastructure 许可权来添加服务器
</br></br>
        {{site.data.keyword.Bluemix_notm}} Infrastructure 异常：必须具有许可权才能订购“项”。</td>
        <td>您可能没有必需的许可权来从 IBM Bluemix Infrastructure (SoftLayer) 产品服务组合供应工作程序节点。请参阅[配置对 IBM Bluemix Infrastructure (SoftLayer) 产品服务组合的访问权以创建标准的 Kubernete 集群](cs_planning.html#cs_planning_unify_accounts)。</td>
      </tr>
    </tbody>
  </table>

<br />


## 调试应用程序部署
{: #debug_apps}

复查您可用于调试应用程序部署并查找失败根本原因的选项。

1. 通过运行 `describe` 命令，在服务或部署资源中查找异常情况。

 示例：
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [检查容器是否卡在 ContainerCreating 状态](#stuck_creating_state)。

3. 检查集群是否处于 `Critical` 状态。如果集群处于 `Critical` 状态，请检查防火墙规则，并验证主节点是否能与工作程序节点进行通信。

4. 验证该服务是否正在侦听正确的端口。
   1. 获取 pod 的名称。
<pre class="pre"><code>        kubectl get pods
        </code></pre>
   2. 登录到容器。
<pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. 从容器中 Curl 应用程序。如果该端口不可访问，那么该服务可能未在侦听正确的端口，或者应用程序可能存在问题。使用正确的端口更新服务的配置文件，并重新部署或调查应用程序的潜在问题。<pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. 验证服务是否已正确链接到 Pod。
   1. 获取 pod 的名称。
<pre class="pre"><code>        kubectl get pods
        </code></pre>
   2. 登录到容器。
<pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Curl 服务的集群 IP 地址和端口。如果 IP 地址和端口不可访问，请查看服务的端点。如果没有端点，那么服务的选择器与 pod 不匹配。如果存在端点，请查看服务上的目标端口字段，并确保目标端口与用于这些 pod 的目标端口相同。
<pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. 对于 Ingress 服务，请验证可从集群内访问该服务。
   1. 获取 pod 的名称。
<pre class="pre"><code>        kubectl get pods
        </code></pre>
   2. 登录到容器。
<pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Curl 为 Ingress 服务指定的 URL。如果该 URL 不可访问，请检查集群和外部端点之间是否存在防火墙问题。
<pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

<br />


## 确定 kubectl 的本地客户机和服务器版本

要检查您正在本地运行或您的集群正在运行的 Kubernetes CLI 版本，请运行以下命令并检查版本。

```
        kubectl version  --short
        ```
{: pre}

输出示例：

```
        Client Version: v1.5.6
        Server Version: v1.5.6
        ```
{: screen}

<br />


## 在创建集群时无法连接到 IBM Bluemix Infrastructure (SoftLayer) 帐户
{: #cs_credentials}

{: tsSymptoms}
创建新的 Kubernetes 集群时，收到以下消息。

```
我们无法连接到您的 IBM Bluemix Infrastructure (SoftLayer) 帐户。创建标准集群要求您有一个链接到 IBM Bluemix Infrastructure (SoftLayer) 帐户条款的“现买现付”帐户，或者您已使用 IBM {{site.data.keyword.Bluemix_notm}} Container Service CLI 设置 {{site.data.keyword.Bluemix_notm}} Infrastructure API 密钥。
```
{: screen}

{: tsCauses}
具有未链接 {{site.data.keyword.Bluemix_notm}} 帐户的用户必须创建新的“现买现付”帐户，或者使用 {{site.data.keyword.Bluemix_notm}} CLI 手动添加 IBM Bluemix Infrastructure (SoftLayer) API 密钥。

{: tsResolve}
要向您的 {{site.data.keyword.Bluemix_notm}} 帐户添加凭证，请执行以下操作：

1.  请联系 IBM Bluemix Infrastructure (SoftLayer) 管理员，以获取您的 IBM Bluemix Infrastructure (SoftLayer) 用户名和 API 密钥。

    **注**：您使用的 IBM Bluemix Infrastructure (SoftLayer) 帐户必须设置有“超级用户”许可权才可成功创建标准集群。

2.  添加凭证。

  ```
  bx cs credentials-set --infrastructure-username <username> --infrastructure-api-key <api_key>
  ```
  {: pre}

3.  创建标准集群。

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u1c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

<br />


## 使用 SSH 访问工作程序节点失败
{: #cs_ssh_worker}

{: tsSymptoms}
使用 SSH 连接无法访问工作程序节点。

{: tsCauses}
工作程序节点上禁用了通过密码进行的 SSH。

{: tsResolve}
将 [DaemonSets ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) 用于必须在每个节点上运行的任何操作，或者将作业用于必须执行的任何一次性操作。

<br />


## Pod 保持暂挂状态
{: #cs_pods_pending}

{: tsSymptoms}
运行 `kubectl get pods` 时，可能看到 pod 保持**暂挂**状态。

{: tsCauses}
如果刚刚创建了 Kubernetes 集群，那么工作程序节点可能仍在配置中。如果这是现有集群，那么可能是集群中没有足够的容量来部署 pod。

{: tsResolve}
此任务需要[管理员访问策略](cs_cluster.html#access_ov)。验证您当前的[访问策略](cs_cluster.html#view_access)。

如果刚创建了 Kubernetes 集群，请运行以下命令并等待工作程序节点初始化。

```
kubectl get nodes
```
{: pre}

如果这是现有集群，请检查集群容量。

1.  使用缺省端口号设置代理。

  ```
  kubectl proxy
  ```
   {: pre}

2.  打开 Kubernetes 仪表板。

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  检查集群中是否有足够的容量来部署 pod。

4.  如果集群中没有足够的容量，请向集群另外添加一个工作程序节点。

  ```
  bx cs worker-add <cluster name or id> 1
  ```
  {: pre}

5.  如果在完全部署工作程序节点后，pod 仍然保持 **pending** 状态，请查看 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) 以进一步对 pod 的暂挂状态进行故障诊断。

<br />


## pod 陷入创建状态
{: #stuck_creating_state}

{: tsSymptoms}
当您运行 `kubectl get pods -o wide` 时，您会看到在同一工作程序节点上运行的多个 pod 陷入 `ContainerCreating` 状态。

{: tsCauses}
工作程序节点上的文件系统是只读的。

{: tsResolve}
1. 备份可能存储在工作程序节点或容器中的任何数据。
2. 通过运行以下命令来重建工作程序节点。

<pre class="pre"><code>bx cs worker-reload &lt;cluster_name&gt; &lt;worker_id&gt;</code></pre>

<br />


## 容器不启动
{: #containers_do_not_start}

{: tsSymptoms}
pod 会成功部署到集群，但容器不启动。

{: tsCauses}
当达到注册表限额时，容器可能不会启动。

{: tsResolve}
[在 {{site.data.keyword.registryshort_notm}} 中释放存储器。](../services/Registry/registry_quota.html#registry_quota_freeup)

<br />


## 访问新工作程序节点上的 pod 失败，并返回超时错误
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
您删除了集群中的工作程序节点，然后添加了工作程序节点。部署 pod 或 Kubernetes 服务时，资源无法访问新创建的工作程序节点，并且连接超时。

{: tsCauses}
如果从集群中删除工作程序节点，然后添加工作程序节点，那么可能会将已删除工作程序节点的专用 IP 地址分配给新工作程序节点。Calico 使用此专用 IP 地址作为标记，并继续尝试访问已删除的节点。

{: tsResolve}
手动更新专用 IP 地址的引用以指向正确的节点。

1.  确认您是否有两个工作程序节点使用相同的**专用 IP** 地址。记下已删除的工作程序的**专用 IP** 和**标识**。

  ```
  bx cs workers <CLUSTER_NAME>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   192.0.2.0.12   203.0.113.144   b1c.4x16       normal    Ready
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   192.0.2.0.16   203.0.113.144   b1c.4x16       deleted    -
  ```
  {: screen}

2.  安装 [Calico CLI](cs_security.html#adding_network_policies)。
3.  列出 Calico 中的可用工作程序节点。将 <path_to_file> 替换为 Calico 配置文件的本地路径。

  ```
  calicoctl get nodes --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  删除 Calico 中的重复工作程序节点。将 NODE_ID 替换为工作程序节点标识。

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  重新引导未删除的工作程序节点。

  ```
  bx cs worker-reboot CLUSTER_ID NODE_ID
  ```
  {: pre}


已删除的节点不会再在 Calico 中列出。

<br />


## 工作程序节点连接失败
{: #cs_firewall}

{: tsSymptoms}
当工作程序节点无法连接时，您可能会看到各种不同的症状。当 kubectl 代理失败时，或者您尝试访问集群中的某个服务并且连接失败时，您可能会看到以下消息之一。

  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}

如果运行 kubectl exec、attach 或 log，那么可能会看到以下消息。

  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

如果 kubectl 代理成功，但仪表板不可用，那么您可能会看到以下消息。

  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}



{: tsCauses}
您可能已在 IBM Bluemix Infrastructure (SoftLayer) 帐户中额外设置防火墙或定制现有防火墙设置。{{site.data.keyword.containershort_notm}} 需要打开特定 IP 地址和端口，以允许工作程序节点与 Kubernetes 主节点之间进行通信。另一个原因可能是工作程序节点陷入重新装入循环。

{: tsResolve}
此任务需要[管理员访问策略](cs_cluster.html#access_ov)。验证您当前的[访问策略](cs_cluster.html#view_access)。

在定制防火墙中打开以下端口和 IP 地址。

1.  记下用于集群中所有工作程序节点的公共 IP 地址：

  ```
  bx cs workers '<cluster_name_or_id>'
  ```
  {: pre}

2.  在来自工作程序节点的出站连接的防火墙中，允许出局网络流量从源工作程序节点流至目标 TCP/UDP 端口范围 20000-32767 和端口 443（用于 `<each_worker_node_publicIP>`），以及以下 IP 地址和网络组。
    - **重要事项**：必须允许流至端口 443 以及区域内所有位置的出站流量流至彼此，以便均衡引导过程中的负载。例如，如果集群位于美国南部，那么必须允许流量从端口 443 流至 dal10 和 dal12 以及从 dal10 和 dal12 流至彼此。<p>
  <table summary="表中的第一行跨两列。其余行应从左到右阅读，其中第一列是服务器位置，第二列是要匹配的 IP 地址。">
      <thead>
      <th>区域</th>
      <th>位置</th>
      <th>IP 地址</th>
      </thead>
    <tbody>
      <tr>
         <td>亚太地区南部</td>
         <td>mel01<br>syd01</td>
         <td><code>168.1.97.67</code><br><code>168.1.8.195</code></td>
      </tr>
      <tr>
         <td>欧洲中部</td>
         <td>ams03<br>fra02</td>
         <td><code>169.50.169.110</code><br><code>169.50.56.174</code></td>
        </tr>
      <tr>
        <td>英国南部</td>
        <td>lon02<br>lon04</td>
        <td><code>159.122.242.78</code><br><code>158.175.65.170</code></td>
      </tr>
      <tr>
        <td>美国东部</td>
         <td>wdc06<br>wdc07</td>
         <td><code>169.60.73.142</code><br><code>169.61.83.62</code></td>
      </tr>
      <tr>
        <td>美国南部</td>
        <td>dal10<br>dal12<br>dal13</td>
        <td><code>169.46.7.238</code><br><code>169.47.70.10</code><br><code>169.60.128.2</code></td>
      </tr>
      </tbody>
    </table>
</p>

3.  允许出站网络流量从工作程序节点流至 {{site.data.keyword.registrylong_notm}}：
    - `TCP port 443 FROM <each_worker_node_publicIP> TO <registry_publicIP>`
    - 将 <em>&lt;registry_publicIP&gt;</em> 替换为要允许流量的注册表区域的所有地址：<p>      
<table summary="表中的第一行跨两列。其余行应从左到右阅读，其中第一列是服务器位置，第二列是要匹配的 IP 地址。">
      <thead>
        <th colspan=2><img src="images/idea.png"/> 注册表 IP 地址</th>
        </thead>
      <tbody>
        <tr>
          <td>registry.au-syd.bluemix.net</td>
          <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></td>
        </tr>
        <tr>
          <td>registry.eu-de.bluemix.net</td>
          <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></td>
         </tr>
         <tr>
          <td>registry.eu-gb.bluemix.net</td>
          <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></td>
         </tr>
         <tr>
          <td>registry.ng.bluemix.net</td>
          <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></td>
         </tr>
        </tbody>
      </table>
</p>

4.  可选：允许出站网络流量从工作程序节点流至 {{site.data.keyword.monitoringlong_notm}} 和 {{site.data.keyword.loganalysislong_notm}} 服务：
    - `TCP port 443, port 9095 FROM <each_worker_node_publicIP> TO <monitoring_publicIP>`
    - 将 <em>&lt;monitoring_publicIP&gt;</em> 替换为要允许流量的监视区域的所有地址：<p><table summary="表中的第一行跨两列。其余行应从左到右阅读，其中第一列是服务器位置，第二列是要匹配的 IP 地址。">
      <thead>
        <th colspan=2><img src="images/idea.png"/> 监视公共 IP 地址</th>
        </thead>
      <tbody>
        <tr>
         <td>metrics.eu-de.bluemix.net</td>
         <td><code>159.122.78.136/29</code></td>
        </tr>
        <tr>
         <td>metrics.eu-gb.bluemix.net</td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>metrics.ng.bluemix.net</td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
    - `TCP port 443, port 9091 FROM <each_worker_node_publicIP> TO <logging_publicIP>`
    - 将 <em>&lt;logging_publicIP&gt;</em> 替换为要允许流量的日志记录区域的所有地址：<p><table summary="表中的第一行跨两列。其余行应从左到右阅读，其中第一列是服务器位置，第二列是要匹配的 IP 地址。">
      <thead>
        <th colspan=2><img src="images/idea.png"/> 日志记录公共 IP 地址</th>
        </thead>
      <tbody>
        <tr>
         <td>ingest.logging.eu-de.bluemix.net</td>
         <td><code>169.50.25.125</code></td>
        </tr>
        <tr>
         <td>ingest.logging.eu-gb.bluemix.net</td>
         <td><code>169.50.115.113</code></td>
        </tr>
        <tr>
          <td>ingest.logging.ng.bluemix.net</td>
          <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
         </tr>
        </tbody>
      </table>
</p>

5. 如果您有专用防火墙，请允许适当的 IBM Bluemix Infrastructure (SoftLayer) 专用 IP 范围。请从**后端（专用）网络**部分开始查阅[此链接](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall)。
    - 添加您正在使用的所有[区域内的位置](cs_regions.html#locations)
    - 请注意，必须添加 dal01 位置（数据中心）
    - 打开端口 80 和 443 以允许集群引导过程

<br />


## 更新或重新装入工作程序节点后，出现重复的节点和 pod
{: #cs_duplicate_nodes}

{: tsSymptoms}
运行 `kubectl get nodes` 时，您看到状态为 **NotReady** 的重复工作程序节点。状态为 **NotReady** 的工作程序节点具有公共 IP 地址，而状态为 **Ready** 的工作程序节点具有专用 IP 地址。

{: tsCauses}
较旧的集群具有按集群的公共 IP 地址列出的工作程序节点。现在，工作程序节点按集群的专用 IP 地址列出。当您重新装入或更新节点时，IP 地址将更改，但对公共 IP 地址的引用将保持不变。

{: tsResolve}
由于这些重复，因此不存在服务中断，但您应该从 API 服务器中除去旧的工作程序节点引用。

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## 通过 Ingress 连接到应用程序失败
{: #cs_ingress_fails}

{: tsSymptoms}
您已通过为集群中的应用程序创建 Ingress 资源来向公众公开应用程序。但尝试通过 Ingress 控制器的公共 IP 地址或子域连接到应用程序时，连接失败或超时。

{: tsCauses}
由于以下原因，Ingress 可能未正常运行：
<ul><ul>
<li>集群尚未完全部署。
<li>集群设置为 Lite 集群，或设置为仅具有一个工作程序节点的标准集群。
<li>Ingress 配置脚本包含错误。
</ul></ul>

{: tsResolve}
要对 Ingress 进行故障诊断，请执行以下操作：

1.  检查是否设置了完全部署的标准集群，以及该集群是否至少有两个工作程序节点，以确保 Ingress 控制器具有高可用性。

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

    在 CLI 输出中，确保工作程序节点的 **Status** 显示 **Ready**，并且 **Machine Type** 显示除了 **free** 之外的机器类型。

2.  检索 Ingress 控制器子域和公共 IP 地址，然后对每一项执行 ping 操作。

    1.  检索 Ingress 控制器子域。

      ```
      bx cs cluster-get <cluster_name_or_id> | grep "Ingress subdomain"
      ```
      {: pre}

    2.  对 Ingress 控制器子域执行 ping 操作。

      ```
      ping <ingress_controller_subdomain>
      ```
      {: pre}

    3.  检索 Ingress 控制器的公共 IP 地址。

      ```
      nslookup <ingress_controller_subdomain>
      ```
      {: pre}

    4.  对 Ingress 控制器公共 IP 地址执行 ping 操作。

      ```
      ping <ingress_controller_ip>
      ```
      {: pre}

    如果对于 Ingress 控制器的公用 IP 地址或子域，CLI 返回超时，并且您已设置定制防火墙来保护工作程序节点，那么可能需要在[防火墙](#cs_firewall)中打开其他端口和联网组。

3.  如果使用的是定制域，请确保定制域已通过域名服务 (DNS) 提供程序映射到 IBM 提供的 Ingress 控制器的公共 IP 地址或子域。
    1.  如果使用的是 Ingress 控制器子域，请检查规范名称记录 (CNAME)。
    2.  如果使用的是 Ingress 控制器公共 IP 地址，请检查定制域是否已映射到指针记录 (PTR) 中的可移植公共 IP 地址。
4.  检查 Ingress 配置文件。

    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingress
    spec:
      tls:
      - hosts:
        - <ingress_subdomain>
        secretName: <ingress_tlssecret>
      rules:
      - host: <ingress_subdomain>
        http:
          paths:
          - path: /
            backend:
              serviceName: myservice
              servicePort: 80
    ```
    {: codeblock}

    1.  检查 Ingress 控制器子域和 TLS 证书是否正确。要找到 IBM 提供的子域和 TLS 证书，请运行 bx cs cluster-get <cluster_name_or_id>。
    2.  确保应用程序侦听的是在 Ingress 的 **path** 部分中配置的路径。如果应用程序设置为侦听根路径，请包含 **/** 以作为路径。
5.  检查 Ingress 部署，并查找潜在的错误消息。

  ```
  kubectl describe ingress <myingress>
  ```
  {: pre}

6.  检查 Ingress 控制器的日志。
    1.  检索正在集群中运行的 Ingress pod 的标识。

      ```
      kubectl get pods -n kube-system |grep ingress
      ```
      {: pre}

    2.  检索每个 Ingress pod 的日志。

      ```
      kubectl logs <ingress_pod_id> -n kube-system
      ```
      {: pre}

    3.  在 Ingress 控制器日志中查找错误消息。

<br />


## 通过 LoadBalancer 服务连接到应用程序失败
{: #cs_loadbalancer_fails}

{: tsSymptoms}
您已通过在集群中创建 LoadBalancer 服务来向公众公开应用程序。但尝试通过负载均衡器的公共 IP 地址连接到应用程序时，连接失败或超时。

{: tsCauses}
由于以下某种原因，LoadBalancer 服务可能未正常运行：

-   集群为 Lite 集群，或者为仅具有一个工作程序节点的标准集群。
-   集群尚未完全部署。
-   LoadBalancer 服务的配置脚本包含错误。

{: tsResolve}
要对 LoadBalancer 服务进行故障诊断，请执行以下操作：

1.  检查是否设置了完全部署的标准集群，以及该集群是否至少有两个工作程序节点，以确保 LoadBalancer 服务具有高可用性。

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

    在 CLI 输出中，确保工作程序节点的 **Status** 显示 **Ready**，并且 **Machine Type** 显示除了 **free** 之外的机器类型。

2.  检查 LoadBalancer 服务的配置文件是否准确。

  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: myservice
  spec:
    type: LoadBalancer
    selector:
      <selectorkey>:<selectorvalue>
    ports:
     - protocol: TCP
       port: 8080
  ```
  {: pre}

    1.  检查是否已将 **LoadBalancer** 定义为服务类型。
    2.  确保使用的是部署应用程序时在 **label/metadata** 部分中所用的 **<selectorkey>** 和 **<selectorvalue>**。
    3.  检查是否使用的是应用程序侦听的**端口**。

3.  检查 LoadBalancer 服务，并查看 **Events** 部分以查找潜在错误。

  ```
  kubectl describe service <myservice>
  ```
  {: pre}

    查找以下错误消息：
    <ul><ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>要使用 LoadBalancer 服务，您必须有至少包含两个工作程序节点的标准集群。
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the load balancer service request. Add a portable subnet to the cluster and try again</code></pre></br>此错误消息指示没有任何可移植公共 IP 地址可供分配给 LoadBalancer 服务。请参阅[向集群添加子网](cs_cluster.html#cs_cluster_subnet)，以了解有关如何为集群请求可移植公共 IP 地址的信息。有可移植的公共 IP 地址可供集群使用后，将自动创建 LoadBalancer 服务。
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips</code></pre></br>您使用 **loadBalancerIP** 部分为 LoadBalancer 服务定义了可移植公共 IP 地址，但此可移植公共 IP 地址在可移植公共子网中不可用。请更改 LoadBalancer 服务配置脚本，并选择其中一个可用的可移植公共 IP 地址，或者从脚本中除去 **loadBalancerIP** 部分，以便可以自动分配可用的可移植公共 IP 地址。
    <li><pre class="screen"><code>No available nodes for load balancer services</code></pre>您没有足够的工作程序节点可部署 LoadBalancer 服务。一个原因可能是您已部署了包含多个工作程序节点的标准集群，但供应这些工作程序节点失败。
    <ol><li>列出可用的工作程序节点。</br><pre class="codeblock"><code>kubectl get nodes</code></pre>
    <li>如果找到了至少两个可用的工作程序节点，请列出工作程序节点详细信息。</br><pre class="screen"><code>bx cs worker-get <worker_ID></code></pre>
    <li>确保分别由“kubectl get nodes”和“bx cs worker-get”命令返回的工作程序节点的公共和专用 VLAN 标识相匹配。</ol></ul></ul>

4.  如果使用定制域来连接到 LoadBalancer 服务，请确保定制域已映射到 LoadBalancer 服务的公共 IP 地址。
    1.  找到 LoadBalancer 服务的公共 IP 地址。

      ```
      kubectl describe service <myservice> | grep "LoadBalancer Ingress"
      ```
      {: pre}

    2.  检查定制域是否已映射到指针记录 (PTR) 中 LoadBalancer 服务的可移植公共 IP 地址。

<br />


## 检索用于 Calico CLI 配置的 ETCD URL 失败
{: #cs_calico_fails}

{: tsSymptoms}
当您检索 `<ETCD_URL>` 以[添加网络策略](cs_security.html#adding_network_policies)时，您获得 `calico-config not found` 错误消息。

{: tsCauses}
集群不是 [Kubernetes V1.7](cs_versions.html) 或更高版本。

{: tsResolve}
[更新集群](cs_cluster.html#cs_cluster_update)或使用与较早版本 Kubernetes 兼容的命令检索 `<ETCD_URL>`。

要检索 `<ETCD_URL>`，请运行以下其中一个命令：

- Linux 和 OS X：

    ```
              kubectl describe pod -n kube-system `kubectl get pod -n kube-system | grep calico-policy-controller | awk '{print $1}'` | grep ETCD_ENDPOINTS | awk '{print $2}'
              ```
    {: pre}

- Windows：<ol>
    <li> 获取 kube-system 名称空间中 pod 的列表，并找到 Calico 控制器 pod。</br><pre class="codeblock"><code>kubectl get pod -n kube-system</code></pre></br>示例：</br><pre class="screen"><code>calico-policy-controller-1674857634-k2ckm</code></pre>
    <li> 查看 Calico 控制器 pod 的详细信息。</br> <pre class="codeblock"><code>kubectl describe pod -n kube-system calico-policy-controller-&lt;ID&gt;</code></pre>
    <li> 找到 ETCD 端点值。示例：<code>https://169.1.1.1:30001</code></ol>

当您检索 `<ETCD_URL>` 时，继续执行[添加网络策略](cs_security.html#adding_network_policies)中列出的步骤。

<br />


## 已知问题
{: #cs_known_issues}

了解已知问题。
{: shortdesc}

### 集群
{: #ki_clusters}

<dl>
  <dt>同一 {{site.data.keyword.Bluemix_notm}} 空间中的 Cloud Foundry 应用程序不能访问集群</dt>
    <dd>创建 Kubernetes 集群时，该集群是在帐户级别创建的，并不使用空间，但绑定 {{site.data.keyword.Bluemix_notm}} 服务时会使用空间。如果您有希望集群访问的 Cloud Foundry 应用程序，那么必须使该 Cloud Foundry 应用程序公共可用，或者必须使您的应用程序在集群中[公共可用](cs_planning.html#cs_planning_public_network)。</dd>
  <dt>Kube 仪表板 NodePort 服务已禁用</dt>
    <dd>出于安全原因，禁用了 Kubernetes 仪表板 NodePort 服务。要访问 Kubernetes 仪表板，请运行以下命令。</br><pre class="codeblock"><code>kubectl proxy</code></pre></br>然后，可以通过 `http://localhost:8001/ui` 访问 Kubernetes 仪表板。</dd>
  <dt>对负载均衡器的服务类型的限制</dt>
    <dd><ul><li>无法在专用 VLAN 上使用负载均衡。<li>您无法使用 service.beta.kubernetes.io/external-traffic 和 service.beta.kubernetes.io/healthcheck-nodeport 服务注释。有关这些注释的更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tutorials/services/source-ip/)。</ul></dd>
  <dt>水平自动扩展在某些集群中不起作用</dt>
    <dd>出于安全原因，由 Heapster (10255) 使用的标准端口在旧集群的所有工作程序节点中都已关闭。由于此端口已关闭，因此 Heapster 无法报告工作程序节点的度量值，并且水平自动扩展无法如 Kubernetes 文档中[水平 Pod 自动扩展 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) 内所记录的那样工作。请创建其他集群以避免此问题。</dd>
</dl>

### 持久性存储器
{: #persistent_storage}

`kubectl describe <pvc_name>` 命令对于持久性卷申领显示 **ProvisioningFailed**：
<ul><ul>
<li>创建持久性卷申领时，没有持久性卷可用，所以 Kubernetes 返回消息 **ProvisioningFailed**。
<li>创建了持久性卷并绑定到申领时，Kubernetes 会返回消息 **ProvisioningSucceeded**。此过程可能需要几分钟时间。
</ul></ul>

## 获取帮助和支持
{: #ts_getting_help}

从何处开始对容器进行故障诊断？

-   要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，请[检查 {{site.data.keyword.Bluemix_notm}} 状态页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/bluemix/support/#status)。
-   在 [{{site.data.keyword.containershort_notm}} Slack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com) 中发布问题。如果未将 IBM 标识用于 {{site.data.keyword.Bluemix_notm}} 帐户，请联系 [crosen@us.ibm.com](mailto:crosen@us.ibm.com) 并请求针对此 Slack 的邀请。
-   请复查论坛，以查看是否有其他用户遇到相同的问题。使用论坛进行提问时，请使用适当的标记来标注您的问题，以方便 {{site.data.keyword.Bluemix_notm}} 开发团队识别。

    -   如果您有关于使用 {{site.data.keyword.containershort_notm}} 开发或部署集群或应用程序的技术问题，请在 [Stack Overflow ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://stackoverflow.com/search?q=bluemix+containers) 上发布您的问题，并使用 `ibm-bluemix`、`kubernetes` 和 `containers` 标记您的问题。
    -   有关服务的问题和入门指示信息，请使用 [IBM developerWorks dW Answers ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 论坛。请加上 `bluemix` 和 `containers` 标记。
    有关使用论坛的更多详细信息，请参阅[获取帮助](/docs/support/index.html#getting-help)。

-   联系 IBM 支持。有关提交 IBM 支持凭单或支持级别和凭单严重性的信息，请参阅[联系支持人员](/docs/support/index.html#contacting-support)。
