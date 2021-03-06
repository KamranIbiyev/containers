---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# 设置集群
{: #cs_cluster}

设计集群设置以实现最高可用性和容量。
{:shortdesc}

开始之前，请查看[高可用性集群配置](cs_planning.html#cs_planning_cluster_config)的选项。

![集群的高可用性阶段](images/cs_cluster_ha_roadmap.png)

<br />


## 使用 GUI 创建集群
{: #cs_cluster_ui}

Kubernetes 集群是组织成网络的一组工作程序节点。集群的用途是定义一组资源、节点、网络和存储设备，以便使应用程序保持高可用性。要部署应用程序，必须先创建集群，并在该集群中设置工作程序节点的定义。
{:shortdesc}

对于 {{site.data.keyword.Bluemix_notm}} Dedicated 用户，请转而参阅[在 {{site.data.keyword.Bluemix_notm}} Dedicated 中通过 GUI 创建 Kubernetes 集群（封闭 Beta 版）](#creating_ui_dedicated)。

要创建集群，请执行以下操作：
1. 在目录中，选择 **Kubernetes 集群**。
2. 选择集群套餐的类型。您可以选择 **Lite** 或**现买现付**。使用“现买现付”套餐，可以向标准集群提供诸如多个工作程序节点等功能，以获取高可用性环境。
3. 配置集群详细信息。
    1. 为集群提供名称，选择 Kubernetes 版本，然后选择要部署的位置。请选择实际离您最近的位置，以获得最佳性能。
请记住，如果您选择所在国家或地区以外的位置，那么您可能需要法律授权，才能将数据实际存储到国外。
    2. 选择机器类型并指定所需的工作程序节点数。机器类型用于定义在每个工作程序节点中设置并可供容器使用的虚拟 CPU 和内存量。
        - 微机器类型指示最小的选项。
        - 均衡机器具有分配给每个 CPU 的相等内存量，从而优化性能。
    3. 从 IBM Bluemix Infrastructure (SoftLayer) 帐户中选择公用和专用 VLAN。两个 VLAN 在工作程序节点之间进行通信，但公用 VLAN 还与 IBM 管理的 Kubernetes 主节点进行通信。可以对多个集群使用相同的 VLAN。
**注**：如果选择不选择公用 VLAN，那么必须配置备用解决方案。
    4. 选择硬件的类型。共享是足以适合大多数情况的选项。
        - **专用**：确保完全隔离您的物理资源。
        - **共享**：允许将您的物理资源存储在与其他 IBM 客户相同的硬件上。
4. 单击**创建集群**。您可以在**工作程序节点**选项卡中查看工作程序节点部署的进度。完成部署后，您可以在**概述**选项卡中看到集群已就绪。**注**：为每个工作程序节点分配了唯一的工作程序节点标识和域名，在创建集群后，不得手动更改该标识和域名。更改标识或域名会阻止 Kubernetes 主节点管理集群。


**接下来要做什么？**

集群启动并开始运行后，可查看以下任务：

-   [安装 CLI 以开始使用集群。](cs_cli_install.html#cs_cli_install)
-   [在集群中部署应用程序。](cs_apps.html#cs_apps_cli)
-   [在 {{site.data.keyword.Bluemix_notm}} 中设置自己的专用注册表，以存储 Docker 映像并与其他用户共享这些映像。](/docs/services/Registry/index.html)


### 在 {{site.data.keyword.Bluemix_notm}} Dedicated 中通过 GUI 创建集群（封闭 Beta 版）
{: #creating_ui_dedicated}

1.  使用您的 IBM 标识登录到 {{site.data.keyword.Bluemix_notm}} Public 控制台 ([https://console.bluemix.net ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net))。
2.  从帐户菜单中，选择 {{site.data.keyword.Bluemix_notm}} Dedicated 帐户。控制台会以 {{site.data.keyword.Bluemix_notm}} Dedicated 实例的服务和信息更新。
3.  在目录中，选择**容器**，然后单击 **Kubernetes 集群**。
4.  输入**集群名称**。
5.  选择**机器类型**。机器类型用于定义在每个工作程序节点中设置并可用于节点中部署的所有容器的虚拟 CPU 量和内存量。
    -   微机器类型指示最小的选项。
    -   均衡机器类型分配给每个 CPU 的内存量相等，从而优化性能。
6.  选择需要的**工作程序节点数**。选择 `3` 以确保集群的高可用性。
7.  单击**创建集群**。这将打开集群的详细信息，但供应集群中的工作程序节点还需要几分钟的时间。在**工作程序节点**选项卡上，可以查看工作程序节点部署的进度。当工作程序节点已准备就绪时，状态会更改为 **Ready**。

**接下来要做什么？**

集群启动并开始运行后，可查看以下任务：

-   [安装 CLI 以开始使用集群。](cs_cli_install.html#cs_cli_install)
-   [在集群中部署应用程序。](cs_apps.html#cs_apps_cli)
-   [在 {{site.data.keyword.Bluemix_notm}} 中设置自己的专用注册表，以存储 Docker 映像并与其他用户共享这些映像。](/docs/services/Registry/index.html)

<br />


## 使用 CLI 创建集群
{: #cs_cluster_cli}

集群是组织成网络的一组工作程序节点。集群的用途是定义一组资源、节点、网络和存储设备，以便使应用程序保持高可用性。要部署应用程序，必须先创建集群，并在该集群中设置工作程序节点的定义。
{:shortdesc}

对于 {{site.data.keyword.Bluemix_notm}} Dedicated 用户，请转而参阅[在 {{site.data.keyword.Bluemix_notm}} Dedicated 中通过 CLI 创建 Kubernetes 集群（封闭 Beta 版）](#creating_cli_dedicated)。

要创建集群，请执行以下操作：
1.  安装 {{site.data.keyword.Bluemix_notm}} CLI 和 [{{site.data.keyword.containershort_notm}} 插件](cs_cli_install.html#cs_cli_install)。
2.  登录到 {{site.data.keyword.Bluemix_notm}} CLI。根据提示，输入您的 {{site.data.keyword.Bluemix_notm}} 凭证。要指定 {{site.data.keyword.Bluemix_notm}} 区域，请[包含 API 端点](cs_regions.html#bluemix_regions)。

    ```
    bx login
    ```
    {: pre}

    **注**：如果您有联合标识，请使用 `bx login --sso` 登录到 {{site.data.keyword.Bluemix_notm}} CLI。输入您的用户名，并使用 CLI 输出中提供的 URL 来检索一次性密码。如果不使用 `--sso` 时登录失败，而使用 `--sso` 选项时登录成功，说明您拥有的是联合标识。

3. 如果有多个 {{site.data.keyword.Bluemix_notm}} 帐户，请选择要在其中创建 Kubernetes 集群的帐户。

4.  指定在其中创建 Kubernetes 集群的 {{site.data.keyword.Bluemix_notm}} 组织和空间。
    ```
    bx target --cf
    ```
    {: pre}

    **注**：集群是特定于帐户和组织的，但又独立于 {{site.data.keyword.Bluemix_notm}} 空间。例如，如果您在组织的 `test` 空间中创建集群，那么在您稍后转向 `dev` 空间时仍可使用该集群。

5.  如果要在先前所选的 {{site.data.keyword.Bluemix_notm}} 区域以外的区域中创建或访问 Kubernetes 集群，请[指定 {{site.data.keyword.containershort_notm}} 区域 API 端点](cs_regions.html#container_login_endpoints)。

    **注**：如果要在美国东部创建集群，那么必须使用 `bx cs init --host https://us-east.containers.bluemix.net` 命令，指定美国东部容器区域 API 端点。

7.  创建集群。
    1.  查看可用的位置。显示的位置取决于您登录到的 {{site.data.keyword.containershort_notm}} 区域。

        ```
        bx cs locations
        ```
        {: pre}

        CLI 输出与[容器区域的位置](cs_regions.html#locations)相匹配。

    2.  选择位置并查看该位置中可用的机器类型。机器类型指定可供每个工作程序节点使用的虚拟计算资源。


        ```
        bx cs machine-types <location>
        ```
        {: pre}

        ```
        Getting machine types list...
        OK
        Machine Types
        Name         Cores   Memory   Network Speed   OS             Storage   Server Type   
        u1c.2x4      2       4GB      1000Mbps        UBUNTU_16_64   100GB     virtual   
        b1c.4x16     4       16GB     1000Mbps        UBUNTU_16_64   100GB     virtual   
        b1c.16x64    16      64GB     1000Mbps        UBUNTU_16_64   100GB     virtual   
        b1c.32x128   32      128GB    1000Mbps        UBUNTU_16_64   100GB     virtual   
        b1c.56x242   56      242GB    1000Mbps        UBUNTU_16_64   100GB     virtual 
        ```
        {: screen}

    3.  请检查 IBM Bluemix Infrastructure (SoftLayer) 中是否存在此帐户的公用和专用 VLAN。

        ```
        bx cs vlans <location>
        ```
        {: pre}

        ```
        ID        Name                Number   Type      Router  
        1519999   vlan   1355     private   bcr02a.dal10  
        1519898   vlan   1357     private   bcr02a.dal10 
        1518787   vlan   1252     public   fcr02a.dal10 
        1518888   vlan   1254     public    fcr02a.dal10 
        ```
        {: screen}

        如果公用和专用 VLAN 已经存在，请记下匹配的路由器。专用 VLAN 路由器始终以 `bcr`（后端路由器）开头，而公用 VLAN 路由器始终以 `fcr`（前端路由器）开头。这两个前缀后面的数字和字母组合必须匹配，才可在创建集群时使用这些 VLAN。在示例输出中，任一专用 VLAN 都可以与任一公用 VLAN 一起使用，因为路由器全都包含 `02a.dal10`。

    4.  运行 `cluster-create` 命令。您可以选择 Lite 集群（包含设置有 2 个 vCPU 和 4GB 内存的一个工作程序节点）或标准集群（可以包含您在 IBM Bluemix Infrastructure (SoftLayer) 帐户中选择的任意数量的工作程序节点）。缺省情况下，创建标准集群时，工作程序节点的硬件由多个 IBM 客户共享，并且会按小时对使用量计费。</br>标准集群的示例：

        ```
        bx cs cluster-create --location dal10 --public-vlan <public_vlan_id> --private-vlan <private_vlan_id> --machine-type u1c.2x4 --workers 3 --name <cluster_name>
        ```
        {: pre}

        Lite 集群的示例：

        ```
        bx cs cluster-create --name my_cluster
        ```
        {: pre}

        <table>
        <caption>表 1. 了解此命令的组成部分</caption>
        <thead>
        <th colspan=2><img src="images/idea.png"/> 了解此命令的组成部分</th>
        </thead>
        <tbody>
        <tr>
        <td><code>cluster-create</code></td>
        <td>此命令在 {{site.data.keyword.Bluemix_notm}} 组织中创建集群。</td>
        </tr>
        <tr>
        <td><code>--location <em>&lt;location&gt;</em></code></td>
        <td>将 <em>&lt;location&gt;</em> 替换为要在其中创建集群的 {{site.data.keyword.Bluemix_notm}} 位置标识。[可用位置](cs_regions.html#locations)取决于您登录到的 {{site.data.keyword.containershort_notm}} 区域。</td>
        </tr>
        <tr>
        <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
        <td>如果要创建标准集群，请选择机器类型。机器类型指定可供每个工作程序节点使用的虚拟计算资源。有关更多信息，请查看[比较 {{site.data.keyword.containershort_notm}} 的 Lite 和标准集群](cs_planning.html#cs_planning_cluster_type)。对于 Lite 集群，无需定义机器类型。</td>
        </tr>
        <tr>
        <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
        <td><ul>
          <li>对于 Lite 集群，无需定义公用 VLAN。Lite 集群会自动连接到 IBM 拥有的公用 VLAN。</li>
          <li>对于标准集群，如果已经在 IBM Bluemix Infrastructure (SoftLayer) 帐户中为该位置设置了公用 VLAN，请输入该公用 VLAN 的标识。如果您的帐户中没有公用和专用 VLAN，请勿指定此选项。{{site.data.keyword.containershort_notm}} 会自动为您创建公用 VLAN。<br/><br/>
          <strong>注</strong>：专用 VLAN 路由器始终以 <code>bcr</code>（后端路由器）开头，而公用 VLAN 路由器始终以 <code>fcr</code>（前端路由器）开头。这两个前缀后面的数字和字母组合必须匹配，才可在创建集群时使用这些 VLAN。</li>
        </ul></td>
        </tr>
        <tr>
        <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
        <td><ul><li>对于 Lite 集群，无需定义专用 VLAN。Lite 集群会自动连接到 IBM 拥有的专用 VLAN。</li><li>对于标准集群，如果已经在 IBM Bluemix Infrastructure (SoftLayer) 帐户中为该位置设置了专用 VLAN，请输入该专用 VLAN 的标识。如果您的帐户中没有公用和专用 VLAN，请勿指定此选项。{{site.data.keyword.containershort_notm}} 会自动为您创建公用 VLAN。<br/><br/><strong>注</strong>：专用 VLAN 路由器始终以 <code>bcr</code>（后端路由器）开头，而公用 VLAN 路由器始终以 <code>fcr</code>（前端路由器）开头。这两个前缀后面的数字和字母组合必须匹配，才可在创建集群时使用这些 VLAN。</li></ul></td>
        </tr>
        <tr>
        <td><code>--name <em>&lt;name&gt;</em></code></td>
        <td>将 <em>&lt;name&gt;</em> 替换为集群的名称。</td>
        </tr>
        <tr>
        <td><code>--workers <em>&lt;number&gt;</em></code></td>
        <td>要包含在集群中的工作程序节点的数目。如果未指定 <code>--workers</code> 选项，那么会创建 1 个工作程序节点。</td>
        </tr>
        </tbody></table>

8.  验证是否请求了创建集群。

    ```
    bx cs clusters
    ```
    {: pre}

    **注**：可能需要最长 15 分钟时间，才能订购好工作程序节点计算机，并且在您的帐户中设置并供应集群。

    当完成集群供应时，集群的状态会更改为**已部署**。

    ```
    Name         ID                                   State      Created          Workers
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1
    ```
    {: screen}

9.  检查工作程序节点的状态。

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    当工作程序节点已准备就绪时，状态会更改为 **normal**，而阶段状态为 **Ready**。节点阶段状态为 **Ready** 时，可以访问集群。

    **注**：为每个工作程序节点分配了唯一的工作程序节点标识和域名，在创建集群后，不得手动更改该标识和域名。更改标识或域名会阻止 Kubernetes 主节点管理集群。

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1   169.47.223.113   10.171.42.93   free           normal    Ready
    ```
    {: screen}

10. 将所创建的集群设置为此会话的上下文。每次使用集群时都完成这些配置步骤。
    1.  获取命令以设置环境变量并下载 Kubernetes 配置文件。

        ```
        bx cs cluster-config <cluster_name_or_id>
        ```
        {: pre}

        配置文件下载完成后，会显示一个命令，您可以使用该命令将本地 Kubernetes 配置文件的路径设置为环境变量。

        OS X 的示例：

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    2.  复制并粘贴终端中显示的命令，以设置 `KUBECONFIG` 环境变量。
    3.  验证已正确设置 `KUBECONFIG` 环境变量。

        OS X 的示例：

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        输出：

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml

        ```
        {: screen}

11. 使用缺省端口 `8001` 启动 Kubernetes 仪表板。
    1.  使用缺省端口号设置代理。

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  在 Web 浏览器中打开以下 URL 以查看 Kubernetes 仪表板。

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}


**接下来要做什么？**

-   [在集群中部署应用程序。](cs_apps.html#cs_apps_cli)
-   [使用 `kubectl` 命令行管理集群。![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/user-guide/kubectl/)
-   [在 {{site.data.keyword.Bluemix_notm}} 中设置自己的专用注册表，以存储 Docker 映像并与其他用户共享这些映像。](/docs/services/Registry/index.html)

### 在 {{site.data.keyword.Bluemix_notm}} Dedicated 中通过 CLI 创建集群（封闭 Beta 版）
{: #creating_cli_dedicated}

1.  安装 {{site.data.keyword.Bluemix_notm}} CLI 和 [{{site.data.keyword.containershort_notm}} 插件](cs_cli_install.html#cs_cli_install)。
2.  登录到 {{site.data.keyword.containershort_notm}} 的公用端点。在提示时输入 {{site.data.keyword.Bluemix_notm}} 凭证并选择 {{site.data.keyword.Bluemix_notm}} Dedicated 帐户。

    ```
    bx login -a api.<region>.bluemix.net
    ```
    {: pre}

    **注**：如果您有联合标识，请使用 `bx login --sso` 登录到 {{site.data.keyword.Bluemix_notm}} CLI。输入您的用户名，并使用 CLI 输出中提供的 URL 来检索一次性密码。如果不使用 `--sso` 时登录失败，而使用 `--sso` 选项时登录成功，说明您拥有的是联合标识。

3.  使用 `cluster-create` 命令创建集群。创建标准集群时，将按小时数对工作程序节点硬件的使用量计费。

    示例：

    ```
    bx cs cluster-create --location <location> --machine-type <machine-type> --name <cluster_name> --workers <number>
    ```
    {: pre}

    <table>
    <caption>表 2. 了解此命令的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>此命令在 {{site.data.keyword.Bluemix_notm}} 组织中创建集群。</td>
    </tr>
    <tr>
    <td><code>--location <em>&lt;location&gt;</em></code></td>
    <td>将 &lt;location&gt; 替换为要在其中创建集群的 {{site.data.keyword.Bluemix_notm}} 位置标识。[可用位置](cs_regions.html#locations)取决于您登录到的 {{site.data.keyword.containershort_notm}} 区域。</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>如果要创建标准集群，请选择机器类型。机器类型指定可供每个工作程序节点使用的虚拟计算资源。有关更多信息，请查看[比较 {{site.data.keyword.containershort_notm}} 的 Lite 和标准集群](cs_planning.html#cs_planning_cluster_type)。对于 Lite 集群，无需定义机器类型。</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>将 <em>&lt;name&gt;</em> 替换为集群的名称。</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>要包含在集群中的工作程序节点的数目。如果未指定 <code>--workers</code> 选项，那么会创建 1 个工作程序节点。</td>
    </tr>
    </tbody></table>

4.  验证是否请求了创建集群。

    ```
    bx cs clusters
    ```
    {: pre}

    **注**：可能需要最长 15 分钟时间，才能订购好工作程序节点计算机，并且在您的帐户中设置并供应集群。

    当完成集群供应时，集群的状态会更改为**已部署**。


    ```
    Name         ID                                   State      Created          Workers
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1
    ```
    {: screen}

5.  检查工作程序节点的状态。

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    当工作程序节点已准备就绪时，状态会更改为 **normal**，而阶段状态为 **Ready**。节点阶段状态为 **Ready** 时，可以访问集群。

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1   169.47.223.113   10.171.42.93   free           normal    Ready
    ```
    {: screen}

6.  将所创建的集群设置为此会话的上下文。每次使用集群时都完成这些配置步骤。

    1.  获取命令以设置环境变量并下载 Kubernetes 配置文件。

        ```
        bx cs cluster-config <cluster_name_or_id>
        ```
        {: pre}

        配置文件下载完成后，会显示一个命令，您可以使用该命令将本地 Kubernetes 配置文件的路径设置为环境变量。

        OS X 的示例：

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    2.  复制并粘贴终端中显示的命令，以设置 `KUBECONFIG` 环境变量。
    3.  验证已正确设置 `KUBECONFIG` 环境变量。

        OS X 的示例：

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        输出：

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml

        ```
        {: screen}

7.  使用缺省端口 8001 访问 Kubernetes 仪表板。
    1.  使用缺省端口号设置代理。

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  在 Web 浏览器中打开以下 URL 以查看 Kubernetes 仪表板。

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}


**接下来要做什么？**

-   [在集群中部署应用程序。](cs_apps.html#cs_apps_cli)
-   [使用 `kubectl` 命令行管理集群。![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/user-guide/kubectl/)
-   [在 {{site.data.keyword.Bluemix_notm}} 中设置自己的专用注册表，以存储 Docker 映像并与其他用户共享这些映像。](/docs/services/Registry/index.html)

<br />


## 使用专用和公共映像注册表
{: #cs_apps_images}

Docker 映像是所创建的每一个容器的基础。映像通过 Dockerfile 进行创建，此文件包含用于构建映像的指令。Dockerfile 可能会在其单独存储的指令中引用构建工件，例如应用程序、应用程序配置及其依赖项。映像通常会存储在可以公共访问的注册表（公共注册表）中，或者存储在设置为仅供一小组用户访问的注册表（专用注册表）中。
{:shortdesc}

查看以下选项以找到有关如何设置映像注册表以及如何通过注册表使用映像的信息。

-   [访问 {{site.data.keyword.registryshort_notm}} 中的名称空间以使用 IBM 提供的映像和您自己的专用 Docker 映像](#bx_registry_default)。
-   [访问 Docker Hub 中的公共映像](#dockerhub)。
-   [访问存储在其他专用注册表中的专用映像](#private_registry)。

### 访问 {{site.data.keyword.registryshort_notm}} 中的名称空间以使用 IBM 提供的映像和自己的专用 Docker 映像
{: #bx_registry_default}

可以通过 IBM 提供的公共映像将容器部署到集群，也可以通过存储在 {{site.data.keyword.registryshort_notm}} 的名称空间中的专用映像来部署。

开始之前：

1. [在 {{site.data.keyword.Bluemix_notm}} Public 或 {{site.data.keyword.Bluemix_notm}} Dedicated 的 {{site.data.keyword.registryshort_notm}} 中设置名称空间并将映像推送至此名称空间](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add)。
2. [创建集群](#cs_cluster_cli)。
3. [设定 CLI 的目标为集群](cs_cli_install.html#cs_cli_configure)。

创建集群时，会自动为该集群创建不到期的注册表令牌。此令牌用于授予对 {{site.data.keyword.registryshort_notm}} 中设置的任一名称空间的只读访问权，以便您可以使用 IBM 提供的公共 Docker 映像和您自己的专用 Docker 映像。令牌必须存储在 Kubernetes `imagePullSecret` 中，才能在部署容器化应用程序时供 Kubernetes 集群访问。创建集群时，{{site.data.keyword.containershort_notm}} 会自动将此令牌存储在 Kubernetes `imagePullSecret` 中。`imagePullSecret` 会添加到缺省 Kubernetes 名称空间、该名称空间的 ServiceAccount 中的缺省私钥列表以及 kube-system 名称空间。

**注**：使用此初始设置时，可以通过 {{site.data.keyword.Bluemix_notm}} 帐户的名称空间中可用的任何映像，将容器部署到集群的**缺省**名称空间。如果要将容器部署到集群的其他名称空间，或者如果要使用存储在其他 {{site.data.keyword.Bluemix_notm}} 区域或其他 {{site.data.keyword.Bluemix_notm}} 帐户中的映像，您必须[为集群创建自己的 imagePullSecret](#bx_registry_other)。

要将容器部署到集群的**缺省**名称空间，请创建配置文件。

1.  创建名为 `mydeployment.yaml` 的部署配置文件。
2.  定义部署以及要在 {{site.data.keyword.registryshort_notm}} 的名称空间中使用的映像。

    要使用 {{site.data.keyword.registryshort_notm}} 的名称空间中的专用映像，请使用以下内容：


    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: ibmliberty-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: ibmliberty
        spec:
          containers:
          - name: ibmliberty
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **提示**：要检索名称空间信息，请运行 `bx cr namespace-list`。

3.  在集群中创建部署。

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **提示：**您还可以部署现有配置文件，如 IBM 提供的其中一个公共映像。此示例使用美国南部区域中的 **ibmliberty** 映像。

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-{{site.data.keyword.Bluemix_notm}}/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}

### 将映像部署到其他 Kubernetes 名称空间或者访问其他 {{site.data.keyword.Bluemix_notm}} 区域和帐户中的映像
{: #bx_registry_other}

通过创建您自己的 imagePullSecret，可以将容器部署到其他 Kubernetes 名称空间，使用存储在其他 {{site.data.keyword.Bluemix_notm}} 区域或帐户中的映像，或者使用存储在 {{site.data.keyword.Bluemix_notm}} Dedicated 中的映像。

开始之前：

1.  [在 {{site.data.keyword.Bluemix_notm}} Public 或 {{site.data.keyword.Bluemix_notm}} Dedicated 的 {{site.data.keyword.registryshort_notm}} 中设置名称空间并将映像推送至此名称空间](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add)。
2.  [创建集群](#cs_cluster_cli)。
3.  [设定 CLI 的目标为集群](cs_cli_install.html#cs_cli_configure)。

要创建自己的 imagePullSecret，请执行以下操作：

**注**：ImagePullSecret 仅对于创建它们时所用于的 Kubernetes 名称空间有效。对要部署容器的每个名称空间，重复这些步骤。来自 [DockerHub](#dockerhub) 的映像不需要 ImagePullSecrets。

1.  如果还没有令牌，请[为要访问的注册表创建令牌](/docs/services/Registry/registry_tokens.html#registry_tokens_create)。
2.  列出您的 {{site.data.keyword.Bluemix_notm}} 帐户中的令牌。

    ```
    bx cr token-list
    ```
    {: pre}

3.  记下要使用的令牌标识。
4.  检索令牌的值。将 <em>&lt;token_id&gt;</em> 替换为在上一步中检索到的令牌的标识。


    ```
    bx cr token-get <token_id>
    ```
    {: pre}

    令牌值会显示在 CLI 输出的**令牌**字段中。

5.  创建 Kubernetes 私钥以用于存储令牌信息。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>表 3. 了解此命令的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必需。要使用私钥并将容器部署到的集群的 Kubernetes 名称空间。运行 <code>kubectl get namespaces</code> 可列出集群中的所有名称空间。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必需。要用于 imagePullSecret 的名称。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_url&gt;</em></code></td>
    <td>必需。在其中设置名称空间的映像注册表的 URL。<ul><li>对于在美国南部和美国东部设置的名称空间：registry.ng.bluemix.net</li><li>对于在英国南部设置的名称空间：registry.eu-gb.bluemix.net</li><li>对于在欧洲中部（法兰克福）设置的名称空间：registry.eu-de.bluemix.net</li><li>对于在澳大利亚（悉尼）设置的名称空间：registry.au-syd.bluemix.net</li><li>对于在 {{site.data.keyword.Bluemix_notm}} Dedicated 中设置的名称空间：registry.<em>&lt;dedicated_domain&gt;</em></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>必需。用于登录到专用注册表的用户名。对于 {{site.data.keyword.registryshort_notm}}，用户名设置为 <code>token</code>。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必需。先前检索到的注册表令牌的值。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必需。如果您有 Docker 电子邮件地址，请输入该地址。如果没有，请输入虚构的电子邮件地址，例如 a@b.c。此电子邮件对于创建 Kubernetes 私钥是必需的，但在创建后不会再使用此电子邮件。</td>
    </tr>
    </tbody></table>

6.  验证私钥是否已成功创建。将 <em>&lt;kubernetes_namespace&gt;</em> 替换为在其中创建 imagePullSecret 的名称空间的名称。


    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  创建引用 imagePullSecret 的 pod。
    1.  创建名为 `mypod.yaml` 的 pod 配置文件。
    2.  定义要用于访问专用 {{site.data.keyword.Bluemix_notm}} 注册表的 pod 和 imagePullSecret。

        来自名称空间的专用映像：

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<region>.bluemix.net/<my_namespace>/<my_image>:<tag>  
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        {{site.data.keyword.Bluemix_notm}} 公共映像：

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<region>.bluemix.net/
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>表 4. 了解 YAML 文件的组成部分</caption>
        <thead>
        <th colspan=2><img src="images/idea.png"/> 了解 YAML 文件的组成部分</th>
        </thead>
        <tbody>
        <tr>
        <td><code><em>&lt;container_name&gt;</em></code></td>
        <td>要部署到集群的容器的名称。</td>
        </tr>
        <tr>
        <td><code><em>&lt;my_namespace&gt;</em></code></td>
        <td>存储映像的名称空间。要列出可用名称空间，请运行 `bx cr namespace-list`。</td>
        </tr>
        <tr>
        <td><code><em>&lt;my_image&gt;</em></code></td>
        <td>要使用的映像的名称。要列出 {{site.data.keyword.Bluemix_notm}} 帐户中的可用映像，请运行 `bx cr image-list`。</td>
        </tr>
        <tr>
        <td><code><em>&lt;tag&gt;</em></code></td>
        <td>要使用的映像的版本。如果未指定标记，那么缺省情况下会使用标记为 <strong>latest</strong> 的映像。</td>
        </tr>
        <tr>
        <td><code><em>&lt;secret_name&gt;</em></code></td>
        <td>先前创建的 imagePullSecret 的名称。</td>
        </tr>
        </tbody></table>

   3.  保存更改。
   4.  在集群中创建部署。

        ```
        kubectl apply -f mypod.yaml
        ```
        {: pre}


### 访问 Docker Hub 中的公共映像
{: #dockerhub}

可以使用存储在 Docker Hub 中的任何公共映像将容器部署到集群，而无需任何其他配置。

开始之前：

1.  [创建集群](#cs_cluster_cli)。
2.  [设定 CLI 的目标为集群](cs_cli_install.html#cs_cli_configure)。

创建部署配置文件。

1.  创建名为 `mydeployment.yaml` 的配置文件。
2.  定义部署以及 Docker Hub 中要使用的公共映像。以下配置文件使用 Docker Hub 中可用的公用 NGINX 映像。

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: nginx-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx
    ```
    {: codeblock}

3.  在集群中创建部署。

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **提示：**或者，部署现有配置文件。以下示例使用相同的公共 NGINX 映像，但会将其直接应用于集群。

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-{{site.data.keyword.Bluemix_notm}}/kube-samples/master/deploy-apps-clusters/deploy-nginx.yaml
    ```
    {: pre}


### 访问存储在其他专用注册表中的专用映像
{: #private_registry}

如果您已经拥有要使用的专用注册表，那么必须将相应的注册表凭证存储在 Kubernetes imagePullSecret 中，并在配置文件中引用此私钥。

开始之前：

1.  [创建集群](#cs_cluster_cli)。
2.  [设定 CLI 的目标为集群](cs_cli_install.html#cs_cli_configure)。

要创建 imagePullSecret，请执行以下操作：

**注**：ImagePullSecret 对于创建它们时所用于的 Kubernetes 名称空间有效。对要通过专用 {{site.data.keyword.Bluemix_notm}} 注册表中的映像来部署容器的每个名称空间，重复这些步骤。

1.  创建 Kubernetes 私钥以用于存储专用注册表凭证。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>表 5. 了解此命令的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必需。要使用私钥并将容器部署到的集群的 Kubernetes 名称空间。运行 <code>kubectl get namespaces</code> 可列出集群中的所有名称空间。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必需。要用于 imagePullSecret 的名称。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_url&gt;</em></code></td>
    <td>必需。要在其中存储专用映像的注册表的 URL。</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>必需。用于登录到专用注册表的用户名。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必需。先前检索到的注册表令牌的值。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必需。如果您有 Docker 电子邮件地址，请输入该地址。如果没有，请输入虚构的电子邮件地址，例如 a@b.c。此电子邮件对于创建 Kubernetes 私钥是必需的，但在创建后不会再使用此电子邮件。</td>
    </tr>
    </tbody></table>

2.  验证私钥是否已成功创建。将 <em>&lt;kubernetes_namespace&gt;</em> 替换为在其中创建 imagePullSecret 的名称空间的名称。


    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  创建引用 imagePullSecret 的 pod。
    1.  创建名为 `mypod.yaml` 的 pod 配置文件。
    2.  定义要用于访问专用 {{site.data.keyword.Bluemix_notm}} 注册表的 pod 和 imagePullSecret。要使用专用注册表中的专用映像，请使用以下内容：



        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: <my_image>:<tag>
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>表 6. 了解 YAML 文件的组成部分</caption>
        <thead>
        <th colspan=2><img src="images/idea.png"/> 了解 YAML 文件的组成部分</th>
        </thead>
        <tbody>
        <tr>
        <td><code><em>&lt;pod_name&gt;</em></code></td>
        <td>要创建的 pod 的名称。</td>
        </tr>
        <tr>
        <td><code><em>&lt;container_name&gt;</em></code></td>
        <td>要部署到集群的容器的名称。</td>
        </tr>
        <tr>
        <td><code><em>&lt;my_image&gt;</em></code></td>
        <td>专用注册表中要使用的映像的完整路径。</td>
        </tr>
        <tr>
        <td><code><em>&lt;tag&gt;</em></code></td>
        <td>要使用的映像的版本。如果未指定标记，那么缺省情况下会使用标记为 <strong>latest</strong> 的映像。</td>
        </tr>
        <tr>
        <td><code><em>&lt;secret_name&gt;</em></code></td>
        <td>先前创建的 imagePullSecret 的名称。</td>
        </tr>
        </tbody></table>

  3.  保存更改。
  4.  在集群中创建部署。

        ```
        kubectl apply -f mypod.yaml
        ```
        {: pre}

<br />


## 将 {{site.data.keyword.Bluemix_notm}} 服务添加到集群
{: #cs_cluster_service}

将现有 {{site.data.keyword.Bluemix_notm}} 服务实例添加到集群，以支持集群用户在将应用程序部署到集群时，访问和使用 {{site.data.keyword.Bluemix_notm}} 服务。
{:shortdesc}

开始之前：

1. [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群。
2. [在空间中请求 {{site.data.keyword.Bluemix_notm}} 服务](/docs/services/reqnsi.html#req_instance)的实例。**注：**要在华盛顿位置创建服务的实例，必须使用 CLI。
3. 对于 {{site.data.keyword.Bluemix_notm}} Dedicated 用户，请转而参阅[在 {{site.data.keyword.Bluemix_notm}} Dedicated 中向集群添加 {{site.data.keyword.Bluemix_notm}} 服务（封闭 Beta 版）](#binding_dedicated)。

**注：**
<ul><ul>
<li>只能添加支持服务密钥的 {{site.data.keyword.Bluemix_notm}} 服务。如果服务不支持服务密钥，请参阅[使外部应用程序能够使用 {{site.data.keyword.Bluemix_notm}} 服务](/docs/services/reqnsi.html#req_instance)。</li>
<li>必须完全部署集群和工作程序节点后，才能添加服务。</li>
</ul></ul>


要添加服务，请执行以下操作：
2.  列出 {{site.data.keyword.Bluemix_notm}} 空间中的所有现有服务。

    ```
    bx service list
    ```
    {: pre}

    示例 CLI 输出：

    ```
    name                      service           plan    bound apps   last operation   
    <service_instance_name>   <service_name>    spark                create succeeded
    ```
    {: screen}

3.  记下要添加到集群的服务实例的**名称**。
4.  确定要用于添加服务的集群名称空间。在以下选项之间进行选择。

    -   列出现有名称空间，并选择要使用的名称空间。


        ```
        kubectl get namespaces
        ```
        {: pre}

    -   在集群中创建新的名称空间。

        ```
        kubectl create namespace <namespace_name>
        ```
        {: pre}

5.  将服务添加到集群。

    ```
    bx cs cluster-service-bind <cluster_name_or_id> <namespace> <service_instance_name>
    ```
    {: pre}

    服务成功添加到集群后，将创建集群私钥，用于保存服务实例的凭证。示例 CLI 输出：

    ```
    bx cs cluster-service-bind mycluster mynamespace cleardb 
    Binding service instance to namespace...
    OK
    Namespace: mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

6.  验证是否已在集群名称空间中创建私钥。

    ```
    kubectl get secrets --namespace=<namespace>
    ```
    {: pre}


要在集群中部署的 pod 中使用服务，集群用户可以[将 Kubernetes 私钥作为私钥卷安装到 pod](cs_apps.html#cs_apps_service)，以访问 {{site.data.keyword.Bluemix_notm}} 服务的服务凭证。

### 在 {{site.data.keyword.Bluemix_notm}} Dedicated 中向集群添加 {{site.data.keyword.Bluemix_notm}} 服务（封闭 Beta 版）
{: #binding_dedicated}

**注**：必须完全部署集群和工作程序节点后，才能添加服务。

1.  将本地 {{site.data.keyword.Bluemix_notm}} Dedicated 配置文件的路径设置为 `DEDICATED_BLUEMIX_CONFIG` 环境变量。

    ```
    export DEDICATED_BLUEMIX_CONFIG=<path_to_config_directory>
    ```
    {: pre}

2.  将以上定义的相同路径设置为 `BLUEMIX_HOME` 环境变量。

    ```
    export BLUEMIX_HOME=$DEDICATED_BLUEMIX_CONFIG
    ```
    {: pre}

3.  登录到要创建服务实例的 {{site.data.keyword.Bluemix_notm}} Dedicated 环境。

    ```
    bx login -a api.<dedicated_domain> -u <user> -p <password> -o <org> -s <space>
    ```
    {: pre}

4.  列出 {{site.data.keyword.Bluemix_notm}} 目录中的可用服务。

    ```
    bx service offerings
    ```
    {: pre}

5.  创建要绑定到集群的服务的实例。

    ```
    bx service create <service_name> <service_plan> <service_instance_name>
    ```
    {: pre}

6.  通过列出 {{site.data.keyword.Bluemix_notm}} 空间中的所有现有服务来验证是否已创建服务实例。

    ```
    bx service list
    ```
    {: pre}

    示例 CLI 输出：

    ```
    name                      service           plan    bound apps   last operation   
    <service_instance_name>   <service_name>    spark                create succeeded
    ```
    {: screen}

7.  取消设置 `BLUEMIX_HOME` 环境变量以重新使用 {{site.data.keyword.Bluemix_notm}} Public。

    ```
    unset $BLUEMIX_HOME
    ```
    {: pre}

8.  在 {{site.data.keyword.Bluemix_notm}} Dedicated 环境中，登录到 {{site.data.keyword.containershort_notm}} 的公用端点，并设定 CLI 的目标为集群。
    1.  使用 {{site.data.keyword.containershort_notm}} 的公用端点登录到帐户。在提示时输入 {{site.data.keyword.Bluemix_notm}} 凭证并选择 {{site.data.keyword.Bluemix_notm}} Dedicated 帐户。

        ```
        bx login -a api.ng.bluemix.net
        ```
        {: pre}

        **注**：如果您有联合标识，请使用 `bx login --sso` 登录到 {{site.data.keyword.Bluemix_notm}} CLI。输入您的用户名，并使用 CLI 输出中提供的 URL 来检索一次性密码。如果不使用 `--sso` 时登录失败，而使用 `--sso` 选项时登录成功，说明您拥有的是联合标识。

    2.  获取可用集群的列表，并在 CLI 中识别要设定为目标的集群名称。

        ```
        bx cs clusters
        ```
        {: pre}

    3.  获取命令以设置环境变量并下载 Kubernetes 配置文件。

        ```
        bx cs cluster-config <cluster_name_or_id>
        ```
        {: pre}

        配置文件下载完成后，会显示一个命令，您可以使用该命令将本地 Kubernetes 配置文件的路径设置为环境变量。

        OS X 的示例：

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    4.  复制并粘贴终端中显示的命令，以设置 `KUBECONFIG` 环境变量。

9.  确定要用于添加服务的集群名称空间。在以下选项之间进行选择。

    * 列出现有名称空间，并选择要使用的名称空间。

        ```
        kubectl get namespaces
        ```
        {: pre}

    * 在集群中创建新的名称空间。
        ```
        kubectl create namespace <namespace_name>
        ```
        {: pre}

10.  将服务实例绑定到集群。

      ```
    bx cs cluster-service-bind <cluster_name_or_id> <namespace> <service_instance_name>
    ```
      {: pre}

<br />


## 管理集群访问权
{: #cs_cluster_user}

您可以授予其他用户对集群的访问权，以便这些用户可以访问该集群，管理该集群以及将应用程序部署到该集群。
{:shortdesc}

使用 {{site.data.keyword.containershort_notm}} 的每个用户都必须在“身份和访问权管理”中分配有特定于服务的用户角色，以确定此用户可以执行的操作。“身份和访问权管理”会区分以下访问许可权。


-   {{site.data.keyword.containershort_notm}} 访问策略

    访问策略用于确定可以在集群上执行的集群管理操作，例如创建或除去集群，以及添加或除去多余的工作程序节点。

<!-- If you want to prevent a user from deploying apps to a cluster or creating other Kubernetes resources, you must create RBAC policies for the cluster. -->

-   Cloud Foundry 角色

    每个用户必须分配有一个 Cloud Foundry 用户角色。此角色将确定用户可以在 {{site.data.keyword.Bluemix_notm}} 帐户上执行的操作，例如邀请其他用户或查看配额使用情况。要查看每个角色的许可权，请参阅 [Cloud Foundry 角色](/docs/iam/users_roles.html#cfroles)。

-   RBAC 角色

    分配有 {{site.data.keyword.containershort_notm}} 访问策略的每个用户都会自动分配有 RBAC 角色。RBAC 角色将确定可以在集群内部的 Kubernetes 资源上执行的操作。只会为缺省名称空间设置 RBAC 角色。集群管理员可以为集群中的其他名称空间添加 RBAC 角色。有关更多信息，请参阅 Kubernetes 文档中的[使用 RBAC 授权 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview)。


从以下操作中进行选择以继续：

-   [查看使用集群所必需的访问策略和许可权](#access_ov)。
-   [查看当前访问策略](#view_access)。
-   [更改现有用户的访问策略](#change_access)。
-   [将其他用户添加到 {{site.data.keyword.Bluemix_notm}} 帐户](#add_users)。

### 必需的 {{site.data.keyword.containershort_notm}} 访问策略和许可权概述
{: #access_ov}

查看可以授予 {{site.data.keyword.Bluemix_notm}} 帐户中用户的访问策略和许可权。

|访问策略|集群管理许可权|Kubernetes 资源许可权|
|-------------|------------------------------|-------------------------------|
|<ul><li>角色：管理员</li><li>服务实例：所有当前服务实例</li></ul>|<ul><li>创建 Lite 或标准集群</li><li>为 {{site.data.keyword.Bluemix_notm}} 帐户设置凭证以访问 IBM Bluemix Infrastructure (SoftLayer) 产品服务组合</li><li>除去集群</li><li>为此帐户中的其他现有用户分配和更改 {{site.data.keyword.containershort_notm}} 访问策略。</li></ul><br/>此角色会继承此帐户中所有集群的“编辑者”、“操作员”和“查看者”角色的许可权。|<ul><li>RBAC 角色：集群管理员</li><li>对每个名称空间中资源的读/写访问权</li><li>在名称空间内创建角色</li><li>访问 Kubbernees 仪表板</li><li>创建使应用程序公共可用的 Ingress 资源</li></ul>|
|<ul><li>角色：管理员</li><li>服务实例：特定集群标识</li></ul>|<ul><li>除去特定集群。</li></ul><br/>此角色会继承所选集群的“编辑者”、“操作员”和“查看者”角色的许可权。|<ul><li>RBAC 角色：集群管理员</li><li>对每个名称空间中资源的读/写访问权</li><li>在名称空间内创建角色</li><li>访问 Kubbernees 仪表板</li><li>创建使应用程序公共可用的 Ingress 资源</li></ul>|
|<ul><li>角色：操作员</li><li>服务实例：所有当前服务实例/特定集群标识</li></ul>|<ul><li>向集群添加其他工作程序节点</li><li>从集群中除去工作程序节点</li><li>重新引导工作程序节点</li><li>重新装入工作程序节点</li><li>向集群添加子网</li></ul>|<ul><li>RBAC 角色：管理员</li><li>具有对缺省名称空间内部资源的读/写访问权，但对名称空间本身没有读/写访问权</li><li>在名称空间内创建角色</li></ul>|
|<ul><li>角色：编辑者</li><li>服务实例：所有当前服务实例/特定集群标识</li></ul>|<ul><li>将 {{site.data.keyword.Bluemix_notm}} 服务绑定到集群。</li><li>取消将 {{site.data.keyword.Bluemix_notm}} 服务绑定到集群。</li><li>创建 Webhook。</li></ul><br/>请将此角色用于应用程序开发者。|<ul><li>RBAC 角色：编辑</li><li>对缺省名称空间内部资源的读/写访问权</li></ul>|
|<ul><li>角色：查看者</li><li>服务实例：所有当前服务实例/特定集群标识</li></ul>|<ul><li>列出集群</li><li>查看集群的详细信息</li></ul>|<ul><li>RBAC 角色：查看</li><li>对缺省名称空间内部资源的读访问权</li><li>对 Kubernetes 私钥无读访问权</li></ul>|
|<ul><li>Cloud Foundry 组织角色：管理员</li></ul>|<ul><li>将其他用户添加到 {{site.data.keyword.Bluemix_notm}} 帐户</li></ul>| |
|<ul><li>Cloud Foundry 空间角色：开发者</li></ul>|<ul><li>创建 {{site.data.keyword.Bluemix_notm}} 服务实例</li><li>将 {{site.data.keyword.Bluemix_notm}} 服务实例绑定到集群</li></ul>| |
{: caption="表 7. 必要 {{site.data.keyword.containershort_notm}} 访问权策略和许可权的概述" caption-side="top"}

### 验证 {{site.data.keyword.containershort_notm}} 访问策略
{: #view_access}

可以查看和验证为 {{site.data.keyword.containershort_notm}} 分配的访问策略。访问策略将确定可以执行的集群管理操作。

1.  选择要在其中验证 {{site.data.keyword.containershort_notm}} 访问策略的 {{site.data.keyword.Bluemix_notm}} 帐户。
2.  在菜单栏中，单击**管理** > **安全性** > **身份和访问权**。**用户**窗口会显示用户列表及其电子邮件地址和所选帐户的当前状态。
3.  选择要检查其访问策略的用户。
4.  在**服务策略**部分中，查看用户的访问策略。要找到有关可以使用此角色执行的操作的详细信息，请参阅[必需的 {{site.data.keyword.containershort_notm}} 访问策略和许可权概述](#access_ov)。
5.  可选：[更改当前访问策略](#change_access)。

    **注**：只有分配有对 {{site.data.keyword.containershort_notm}} 中所有资源的管理员服务策略的用户才能更改现有用户的访问策略。要将更多用户添加到 {{site.data.keyword.Bluemix_notm}} 帐户，您必须具有该帐户的“管理员”Cloud Foundry 角色。要找到 {{site.data.keyword.Bluemix_notm}} 帐户所有者的标识，请运行 `bx iam accounts` 并查找**所有者用户标识**。


### 更改现有用户的 {{site.data.keyword.containershort_notm}} 访问策略
{: #change_access}

可以更改现有有用户的访问策略，以授予对 {{site.data.keyword.Bluemix_notm}} 帐户中集群的集群管理许可权。

开始之前，请[验证您是否已分配有管理员访问策略](#view_access)（针对 {{site.data.keyword.containershort_notm}} 中的所有资源）。

1.  选择要在其中更改现有用户的 {{site.data.keyword.containershort_notm}} 访问策略的 {{site.data.keyword.Bluemix_notm}} 帐户。
2.  在菜单栏中，单击**管理** > **安全性** > **身份和访问权**。**用户**窗口会显示用户列表及其电子邮件地址和所选帐户的当前状态。
3.  找到要更改其访问策略的用户。如果找不到要寻找的用户，请[邀请此用户加入 {{site.data.keyword.Bluemix_notm}} 帐户](#add_users)。
4.  在**操作**选项卡中，单击**分配策略**。
5.  从**服务**下拉列表中，选择 **{{site.data.keyword.containershort_notm}}**。
6.  从**角色**下拉列表，选择要分配的访问策略。选择对特定区域或集群没有任何限制的角色，会自动将此访问策略应用于在此帐户中创建的所有集群。如果要将访问权仅限于特定集群或区域，请从**服务实例**和**区域**下拉列表中选择相应项。要找到每个访问策略支持的操作的列表，请参阅[必需的 {{site.data.keyword.containershort_notm}} 访问策略和许可权概述](#access_ov)。要找到特定集群的标识，请运行 `bx cs clusters`。
7.  单击**分配策略**以保存更改。

### 向 {{site.data.keyword.Bluemix_notm}} 帐户添加用户
{: #add_users}

可以将其他用户添加到 {{site.data.keyword.Bluemix_notm}} 帐户以授予对集群的访问权。

开始之前，请验证您是否已分配有对 {{site.data.keyword.Bluemix_notm}} 帐户的“管理员”Cloud Foundry 角色。

1.  选择要在其中添加用户的 {{site.data.keyword.Bluemix_notm}} 帐户。
2.  在菜单栏中，单击**管理** > **安全性** > **身份和访问权**。“用户”窗口会显示用户列表及其电子邮件地址和所选帐户的当前状态。
3.  单击**邀请用户**。
4.  在**电子邮件地址或现有 IBM 标识**中，输入要添加到 {{site.data.keyword.Bluemix_notm}} 帐户的用户的电子邮件地址。
5.  在**访问权**部分中，展开**支持身份和访问权的服务**。
6.  从**服务**下拉列表中，选择 **{{site.data.keyword.containershort_notm}}**。
7.  从**区域**下拉列表中，选择区域。如果您想要的区域未列出，并且 [{{site.data.keyword.containershort_notm}} 支持](cs_regions.html)，请选择**所有区域**。
8.  从**角色**下拉列表，选择要分配的访问策略。选择对特定区域或集群没有任何限制的角色，会自动将此访问策略应用于在此帐户中创建的所有集群。要限制对特定集群或区域的访问，请从**服务实例**和**区域**下拉列表中选择值。要找到每个访问策略支持的操作的列表，请参阅[必需的 {{site.data.keyword.containershort_notm}} 访问策略和许可权概述](#access_ov)。要找到特定集群的标识，请运行 `bx cs clusters`。
9.  展开 **Cloud Foundry 访问权**部分，并从**组织**下拉列表中选择要添加用户的 {{site.data.keyword.Bluemix_notm}} 组织。
10.  从**空间角色**下拉列表中，选择任何角色。Kubernetes 集群独立于 {{site.data.keyword.Bluemix_notm}} 空间。
11. 单击**邀请用户**。
12. 可选：要允许此用户将其他用户添加到 {{site.data.keyword.Bluemix_notm}} 帐户，请为该用户分配 CloudFoundry 组织角色。
    1. 在**用户**概述表的**操作**列中，选择**管理用户**。
    2. 在 **Cloud Foundry 角色**部分中，找到已授予先前步骤中所添加用户的 Cloud Foundry 组织角色。
    3. 在**操作**选项卡中，选择**编辑组织角色**。
    4. 从**组织角色**下拉列表中，选择**管理员**。
    5. 单击**保存角色**。

<br />


## 更新 Kubernetes 主节点
{: #cs_cluster_update}

更新集群的过程分为两个步骤。首先，必须更新 Kubernetes 主节点，然后才能更新每个工作程序节点。

**注意**：如果不相应地进行计划，那么更新_可能_会导致应用程序停机和中断。

Kubernetes 提供以下更新类型：

|更新类型|版本标签|更新者|影响
|-----|-----|-----|-----|
|主要|示例：1.x.x|用户|可能涉及对集群的操作进行更改，并且可能需要对脚本或部署进行更改。|
|次要|示例：x.5.x|用户|可能涉及对集群的操作进行更改，并且可能需要对脚本或部署进行更改。
|补丁|示例：x.x.3|IBM/用户|非中断性的小修订。补丁不需要对脚本或部署进行更改。IBM 会自动更新主节点，但用户必须更新工作程序节点以应用补丁。|
{: caption="Kubernetes 更新的类型" caption-side="top"}


在进行_主要_或_次要_更新时，请完成以下步骤。在更新生产环境之前，请使用测试集群。不能将集群回滚到先前版本。

1. 查看 [Kubernetes 更改](cs_versions.html)，并对任何更新标记为_在更新主节点之前更新_。
2. 使用 GUI 或运行 [CLI 命令](cs_cli_reference.html#cs_cluster_update)来更新 Kubernetes 主节点。当您更新 Kubernetes 主节点时，主节点将关闭约 5 到 10 分钟。在更新期间，您无法访问或更改集群。但是，不会修改集群用户已部署的工作程序节点、应用程序和资源，并继续运行。
3. 确认更新已完成。在 {{site.data.keyword.Bluemix_notm}} 仪表板上查看 Kubernetes 版本，或运行 `bx cs clusters`。

当 Kubernetes 主节点更新完成时，您可以将工作程序节点更新为最新版本。

<br />


## 更新工作程序节点
{: #cs_cluster_worker_update}

工作程序节点可以更新为 Kubernetes 主节点的 Kubernetes 版本。当 IBM 自动将补丁应用于 Kubernetes 主节点时，工作程序节点需要同时用于更新和补丁的用户命令。

**注意**：更新工作程序节点版本可能会导致应用程序和服务的停机时间。如果数据未存储在 pod 外，那么将删除数据。在部署中使用 [replicas ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas) 以允许 pod 重新安排到可用节点。

更新生产级别集群：
- 使用测试集群验证工作负载和交付过程是否不受更新影响。您无法将工作程序节点回滚到先前版本。
- 生产级别集群应该能够在工作程序节点发生故障时生存。如果您的集群不存在，请在更新集群之前先添加工作程序节点。
- 更新过程不会放弃更新之前的节点。请考虑使用 [`drain` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_drain/) 和 [`uncordon` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_uncordon/)，以帮助避免应用程序的停机时间。

开始之前，请安装与 Kubernetes 主节点的 Kubernetes 版本相匹配的 [`kubectl cli` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/tools/install-kubectl/) 版本。

1. 查看 [Kubernetes 更改](cs_versions.html)，并在需要时对您的部署脚本进行任何标记为_更新主节点之后更新_的更改。

2. 更新工作程序节点。要从 {{site.data.keyword.Bluemix_notm}} 仪表板进行更新，请浏览到集群的 `Worker Nodes` 部分，然后单击 `Update Worker`。要获取工作程序节点标识，请运行 `bx cs workers <cluster_name_or_id>`。如果选择多个工作程序节点，那么一次更新一个工作程序节点。

    ```
    bx cs worker-update <cluster_name_or_id> <worker_node_id1> <worker_node_id2>
    ```
    {: pre}

3. 验证您的工作程序节点是否已更新。在 {{site.data.keyword.Bluemix_notm}} 仪表板上查看 Kubernetes 版本，或运行 `bx cs workers <cluster_name_or_id>`。此外，确认 `kubectl` 列示的 Kubernetes 版本已更新。在某些情况下，较旧的集群可能会在更新后列出具有 **NotReady** 状态的重复工作程序节点。要除去重复项，请参阅[故障诊断](cs_troubleshoot.html#cs_duplicate_nodes)。

    ```
kubectl get nodes
```
    {: pre}

5. 检查 Kubernetes 仪表板。如果未在 Kubernetes 仪表板中显示利用率图形，请删除 `kube-dashboard` pod 以强制重新启动。将使用 RBAC 策略重新创建 pod，以访问 heapster 获取利用率信息。

    ```
    kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
    ```
    {: pre}

完成更新后，请对其他集群重复更新过程。此外，通知在集群中工作的开发者将其 `kubectl` CLI 更新到 Kubernetes 主节点的版本。

<br />


## 向集群添加子网
{: #cs_cluster_subnet}

通过向集群添加子网，来更改可用可移植公共 IP 地址的池。
{:shortdesc}

在 {{site.data.keyword.containershort_notm}} 中，您可以通过将网络子网添加到集群来为 Kubernetes 服务添加稳定的可移植 IP。创建标准集群时，{{site.data.keyword.containershort_notm}} 会自动供应可移植公共子网和 5 个 IP 地址。可移植公共 IP 地址是静态的，不会在除去工作程序节点甚至集群时更改。

其中一个可移植公共 IP 地址用于 [Ingress 控制器](cs_apps.html#cs_apps_public_ingress)，此控制器可用于通过一个公共路径公开集群中的多个应用程序。剩余 4 个可移植公共 IP 地址可用于通过[创建 LoadBalancer 服务](cs_apps.html#cs_apps_public_load_balancer)向公众公开单个应用程序。

**注**：可移植公共 IP 地址按月收费。如果在供应集群后选择除去可移植公共 IP 地址，那么即使只使用了很短的时间，您也仍然必须支付一个月的费用。

### 为集群请求其他子网
{: #add_subnet}

可以通过向集群分配子网，将稳定的可移植公共 IP 添加到集群。

对于 {{site.data.keyword.Bluemix_notm}} Dedicated 用户，您必须[开具支持凭单](/docs/support/index.html#contacting-support)以创建子网，然后使用 [`bx cs cluster-subnet-add`](cs_cli_reference.html#cs_cluster_subnet_add) 命令将子网添加到集群，而不是使用此任务。

开始之前，请确保可以通过 {{site.data.keyword.Bluemix_notm}} GUI 访问 IBM Bluemix Infrastructure (SoftLayer) 产品服务组合。要访问该产品服务组合，必须设置或使用现有的 {{site.data.keyword.Bluemix_notm}}“现买现付”帐户。

1.  在目录中的**基础架构**部分中，选择**网络**。
2.  选择**子网/IP**，然后单击**创建**。
3.  从**选择要添加到此帐户的子网类型**下拉菜单中，选择**可移植公共**。
4.  选择要从可移植子网中添加的 IP 地址数。

    **注**：添加子网的可移植公共 IP 地址时，会使用 3 个 IP 地址来建立集群内部联网，所以不能将这 3 个地址用于 Ingress 控制器或用于创建 LoadBalancer 服务。例如，如果请求 8 个可移植公共 IP 地址，那么可以使用其中 5 个地址向公众公开应用程序。

5.  选择要将可移植公共 IP 地址路由到的公共 VLAN。必须选择现有工作程序节点连接到的公用 VLAN。查看工作程序节点的公用 VLAN。

    ```
    bx cs worker-get <worker_id>
    ```
    {: pre}

6.  填写调查表，然后单击**下单**。

    **注**：可移植公共 IP 地址按月收费。如果在创建可移植公共 IP 地址后选择将其除去，那么即便使用时间不足一个月，您也仍然必须支付一个月的费用。

7.  供应子网后，使该子网可供 Kubernetes 集群使用。
    1.  在 Infrastructure 仪表板上，选择创建的子网并记下该子网的标识。
    2.  登录到 {{site.data.keyword.Bluemix_notm}} CLI。要指定 {{site.data.keyword.Bluemix_notm}} 区域，请[包含 API 端点](cs_regions.html#bluemix_regions)。

        ```
        bx login
        ```
        {: pre}

        **注**：如果您有联合标识，请使用 `bx login --sso` 登录到 {{site.data.keyword.Bluemix_notm}} CLI。输入您的用户名，并使用 CLI 输出中提供的 URL 来检索一次性密码。如果不使用 `--sso` 时登录失败，而使用 `--sso` 选项时登录成功，说明您拥有的是联合标识。

    3.  列出您帐户中的所有集群，并记下要使子网可供其使用的集群的标识。

        ```
        bx cs clusters
        ```
        {: pre}

    4.  将子网添加到集群。使子网可供集群使用后，将创建 Kubernetes 配置映射，其中包含所有可用的可移植公共 IP 地址以供您使用。如果不存在用于集群的 Ingress 控制器，那么会自动使用一个可移植公共 IP 地址来创建 Ingress 控制器。其他所有可移植公共 IP 地址都可用于为应用程序创建 LoadBalancer 服务。


        ```
        bx cs cluster-subnet-add <cluster name or id> <subnet id>
        ```
        {: pre}

8.  验证子网是否已成功添加到集群。子网 CIDR 在 **VLAN** 部分中列出。

    ```
    bx cs cluster-get --showResources <cluster name or id>
    ```
    {: pre}

### 向 Kibernetes 集群添加定制和现有子网
{: #custom_subnet}

您可以向 Kubernetes 集群添加现有可移植公用子网。

开始之前，请[设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群。

如果 IBM Bluemix Infrastructure (SoftLayer) 产品服务组合中存在具有要使用的定制防火墙规则或可用 IP 地址的现有子网，请创建不具有子网的集群，并在集群供应时使现有子网可用于集群。

1.  识别要使用的子网。记下子网的标识和 VLAN 标识。在此示例中，子网标识为 807861，VLAN 标识为 1901230。

    ```
    bx cs subnets
    ```
    {: pre}

    ```
    Getting subnet list...
    OK
    ID        Network                                      Gateway                                   VLAN ID   Type      Bound Cluster
    553242    203.0.113.0/24                               10.87.15.00                               1565280   private
    807861    192.0.2.0/24                                 10.121.167.180                            1901230   public

    ```
    {: screen}

2.  确认 VLAN 所在的位置。在此示例中，位置为 dal10。

    ```
    bx cs vlans dal10
    ```
    {: pre}

    ```
    Getting VLAN list...
    OK
    ID        Name                  Number   Type      Router   
    1900403   vlan                    1391     private   bcr01a.dal10   
    1901230   vlan                    1180     public   fcr02a.dal10 
    ```
    {: screen}

3.  使用您识别的位置和 VLAN 标识来创建集群。包含 `--no-subnet` 标志，以阻止自动创建新的可移植公共 IP 子网。

    ```
    bx cs cluster-create --location dal10 --machine-type u1c.2x4 --no-subnet --public-vlan 1901230 --private-vlan 1900403 --workers 3 --name my_cluster 
    ```
    {: pre}

4.  验证是否请求了创建集群。

    ```
    bx cs clusters
    ```
    {: pre}

    **注**：可能需要最长 15 分钟时间，才能订购好工作程序节点计算机，并且在您的帐户中设置并供应集群。

    当完成集群供应时，集群的状态会更改为**已部署**。

    ```
    Name         ID                                   State      Created          Workers   
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   3   
    ```
    {: screen}

5.  检查工作程序节点的状态。

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    当工作程序节点已准备就绪时，状态会更改为 **normal**，而阶段状态为 **Ready**。节点阶段状态为 **Ready** 时，可以访问集群。

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1   169.47.223.113   10.171.42.93   free           normal    Ready
    ```
    {: screen}

6.  通过指定子网标识，向集群添加子网。使子网可供集群使用后，将创建 Kubernetes 配置映射，其中包含所有可用的可移植公共 IP 地址以供您使用。如果尚不存在用于集群的 Ingress 控制器，那么会自动使用一个可移植公共 IP 地址来创建 Ingress 控制器。其他所有可移植公共 IP 地址都可用于为应用程序创建 LoadBalancer 服务。


    ```
    bx cs cluster-subnet-add mycluster 807861
    ```
    {: pre}

### 将用户管理的子网和 IP 地址添加到 Kubernetes 集群
{: #user_subnet}

从要 {{site.data.keyword.containershort_notm}} 访问的内部部署网络中提供自己的子网。然后，可以将专用 IP 地址从该子网添加到 Kubernetes 集群中的 LoadBalancer 服务。

需求：
- 用户管理的子网只能添加到专用 VLAN。
- 子网前缀长度限制为 /24 到 /30。例如，`203.0.113.0/24` 指定了 253 个可用的专用 IP 地址，而 `203.0.113.0/30` 指定了 1 个可用的专用 IP 地址。
- 子网中的第一个 IP 地址必须用作子网的网关。

开始之前：配置网络流量在外部子网的输入和输出的路径。此外，请确认内部部署数据中心网关设备与 IBM Bluemix Infrastructure (SoftLayer) 产品服务组合中的专用网络 Vyatta 之间具有 VPN 连接。有关更多信息，请参阅此[博客帖子 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/)。

1. 查看集群的专用 VLAN 的标识。找到 **VLAN** 部分。在 **Is Public?** 字段中，标识具有 _false_ 的 VLAN 标识。

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR         Is Public?   Is BYOIP?
    1555503   192.0.2.0/24        true         false
    1555505   198.51.100.0/24     false        false
    ```
    {: screen}

2. 将外部子网添加到专用 VLAN。可移植专用 IP 地址将添加到集群的配置映射中。

    ```
    bx cs cluster-user-subnet-add <subnet_CIDR> <VLAN_ID>
    ```
    {: pre}

    示例：

    ```
    bx cs cluster-user-subnet-add 203.0.113.0/24 1555505
    ```
    {: pre}

3. 验证是否添加了用户提供的子网。**Is BYOIP?** 字段为 _true_。

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR         Is Public?   Is BYOIP?
    1555503   192.0.2.0/24        true         false
    1555505   198.51.100.0/24     false        false
    1555505   203.0.113.0/24      false        true
    ```
    {: screen}

4. 添加专用负载均衡器以通过专用网络访问应用程序。如果要使用添加的子网中的专用 IP 地址，那么必须在创建专用负载均衡器时指定 IP 地址。否则，将在专用 VLAN 上的 IBM Bluemix Infrastructure (SoftLayer) 子网或用户提供的子网中随机选择 IP 地址。有关更多信息，请参阅[配置对应用程序的访问权](cs_apps.html#cs_apps_public_load_balancer)。

    具有指定 IP 地址的专用 LoadBalancer 服务的配置文件示例：

    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: <myservice>
      annotations:
        service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: private
    spec:
      type: LoadBalancer
      selector:
        <selectorkey>:<selectorvalue>
      ports:
       - protocol: TCP
         port: 8080
      loadBalancerIP: <private_ip_address>
    ```
    {: codeblock}

<br />


## 在集群中使用 NFS 文件共享
{: #cs_cluster_volume_create}

如果在您的 IBM Bluemix Infrastructure (SoftLayer) 帐户中已经有要用于 Kubernetes 的现有 NFS 文件共享，那么可以通过在现有 NFS 文件共享上创建持久性卷来使用 NFS 文件共享。持久性卷是一块实际硬件，用作 Kubernetes 集群资源，并可以由集群用户使用。
{:shortdesc}

开始之前，请确保您拥有可用于创建持久性卷的现有 NFS 文件共享。

[![创建持久性卷和持久性卷申领](images/cs_cluster_pv_pvc.png)](https://console.bluemix.net/docs/api/content/containers/images/cs_cluster_pv_pvc.png)

Kubernetes 会区分持久性卷（代表实际硬件）和持久性卷申领（通常由集群用户发起的对存储器的请求）。要使现有 NFS 文件共享可用于 Kubernetes，必须创建具有特定大小和访问方式的持久性卷，并创建与持久性卷规范相匹配的持久性卷申领。如果持久性卷和持久性卷申领相匹配，那么会将它们相互绑定。只有绑定的持久性卷申领才能由集群用户用于将相应卷安装到 pod。此过程称为持久性存储器的静态供应。

**注**：持久性存储器的静态供应仅适用于现有 NFS 文件共享。如果没有现有 NFS 文件共享，那么集群用户可以使用[动态供应](cs_apps.html#cs_apps_volume_claim)过程来添加持久性卷。

要创建持久性卷及匹配的持久性卷申领，请执行以下步骤。

1.  在您的 IBM Bluemix Infrastructure (SoftLayer) 帐户中，查找要在其中创建持久性卷对象的 NFS 文件共享的标识和路径。
    1.  登录到 IBM Bluemix Infrastructure (SoftLayer) 帐户。
    2.  单击**存储器**。
    3.  单击**文件存储器**，并记下要使用的 NFS 文件共享的标识和路径。
2.  打开首选的编辑器。
3.  为持久性卷创建存储器配置文件。

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
     name: mypv
    spec:
     capacity:
       storage: "20Gi"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "nfslon0410b-fz.service.softlayer.com"
       path: "/IBM01SEV8491247_0908"
    ```
    {: codeblock}

    <table>
    <caption>表 8. 了解 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>输入要创建的持久性卷对象的名称。</td>
    </tr>
    <tr>
    <td><code>storage</code></td>
    <td>输入现有 NFS 文件共享的存储器大小。输入的存储器大小必须以千兆字节为单位，例如 20Gi (20 GB) 或 1000Gi (1 TB)，并且大小必须与现有文件共享的大小相匹配。</td>
    </tr>
    <tr>
    <td><code>accessMode</code></td>
    <td>访问方式定义了持久性卷申领可以安装到工作程序节点的方式。<ul><li>ReadWriteOnce (RWO)：此持久性卷只能安装到单个工作程序节点中的 pod。安装到此持久性卷的 pod 可以对该卷执行读写操作。</li><li>ReadOnlyMany (ROX)：此持久性卷可以安装到在多个工作程序节点上托管的 pod。安装到此持久性卷的 pod 只能对该卷执行读操作。</li><li>ReadWriteMany (RWX)：此持久性卷可以安装到在多个工作程序节点上托管的 pod。安装到此持久性卷的 pod 可以对该卷执行读写操作。</li></ul></td>
    </tr>
    <tr>
    <td><code>server</code></td>
    <td>输入 NFS 文件共享服务器标识。</td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>输入要在其中创建持久性卷对象的 NFS 文件共享的路径。</td>
    </tr>
    </tbody></table>

4.  在集群中创建持久性卷对象。

    ```
    kubectl apply -f <yaml_path>
    ```
    {: pre}

    示例

    ```
    kubectl apply -f deploy/kube-config/pv.yaml
    ```
    {: pre}

5.  验证持久性卷是否已创建。

    ```
    kubectl get pv
    ```
    {: pre}

6.  创建另一个配置文件以创建持久性卷申领。为了使持久性卷申领与先前创建的持久性卷对象相匹配，必须为 `storage` 和 `accessMode` 选择相同的值。`storage-class` 字段必须为空。如果其中任何字段与持久性卷不匹配，那么会改为自动创建新的持久性卷。


    ```
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
     annotations:
       volume.beta.kubernetes.io/storage-class: ""
    spec:
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: "20Gi"
    ```
    {: codeblock}

7.  创建持久性卷申领。

    ```
    kubectl apply -f deploy/kube-config/mypvc.yaml
    ```
    {: pre}

8.  验证持久性卷申领是否已创建并绑定到持久性卷对象。此过程可能需要几分钟时间。

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    输出类似于以下内容。

    ```
    Name: mypvc
    Namespace: default
    StorageClass: ""
    Status: Bound
    Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels: <none>
    Capacity: 20Gi
    Access Modes: RWX
    Events:
      FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
      --------- -------- ----- ----        ------------- -------- ------ -------
      3m 3m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m 1m  10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m 1m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    ```
    {: screen}


您已成功创建持久性卷对象，并将其绑定到持久性卷申领。现在，集群用户可以[安装持久性卷申领](cs_apps.html#cs_apps_volume_mount)至其 pod，并开始对持久性卷对象执行读写操作。

<br />


## 配置集群日志记录
{: #cs_logging}

日志可帮助您对集群和应用程序的问题进行故障诊断。您可以对各种集群日志源启用日志转发并选择日志的转发位置。
{:shortdesc}

### 查看日志
{: #cs_view_logs}

要查看集群和容器的日志，可以使用标准的 Kubernetes 和 Docker 日志记录功能。
{:shortdesc}

#### {{site.data.keyword.loganalysislong_notm}}
{: #cs_view_logs_k8s}

对于标准集群，日志位于您创建 Kubernetes 集群时登录的 {{site.data.keyword.Bluemix_notm}} 空间中。容器日志在容器外部监视和转发。可以使用 Kibana 仪表板来访问容器的日志。有关日志记录的更多信息，请参阅 [{{site.data.keyword.containershort_notm}} 的日志记录](/docs/services/CloudLogAnalysis/containers/logging_containers_ov.html#logging_containers_ov)。

要访问 Kibana 仪表板，请转至以下某个 URL，然后选择您在其中创建集群的 {{site.data.keyword.Bluemix_notm}} 组织和空间。
- 美国南部和美国东部：https://logging.ng.bluemix.net
- 英国南部：https://logging.eu-gb.bluemix.net
- 欧洲中部：https://logging.eu-de.bluemix.net

#### Docker 日志
{: #cs_view_logs_docker}

可以利用内置 Docker 日志记录功能来查看标准 STDOUT 和 STDERR 输出流上的活动。有关更多信息，请参阅[查看在 Kubernetes 集群中运行的容器的容器日志](/docs/services/CloudLogAnalysis/containers/logging_containers_ov.html#logging_containers_ov_methods_view_kube)。

### 为 Docker 容器名称空间配置日志转发
{: #cs_configure_namespace_logs}

缺省情况下，{{site.data.keyword.containershort_notm}} 会将 Docker 容器名称空间日志转发到 {{site.data.keyword.loganalysislong_notm}}。您还可以通过创建新的日志转发配置，将容器名称空间日志转发到外部 syslog 服务器。
{:shortdesc}

**注**：要查看悉尼位置的日志，必须将日志转发到外部 syslog 服务器。

#### 启用日志转发到 syslog
{: #cs_namespace_enable}

开始之前：

1. 设置可以接受 syslog 协议的服务器。您可以通过两种方式运行 syslog 服务器：
  * 设置和管理自己的服务器，或者让提供者为您管理服务器。如果提供者为您管理服务器，请从日志记录提供者获取日志记录端点。
  * 从容器运行 syslog。例如，您可以使用此[部署 .yaml 文件](https://github.com/IBM-Bluemix/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube)来访存在 Kubernetes 集群中运行容器的 Docker 公共映像。该映像在公共集群 IP 地址上发布端口 `30514`，并使用此公共集群 IP 地址来配置 syslog 主机。

2. [设置 CLI 的目标](cs_cli_install.html#cs_cli_configure)为名称空间所在的集群。

要将名称空间日志转发到 syslog 服务器，请执行以下操作：

1. 创建日志记录配置。

    ```
    bx cs logging-config-update <my_cluster> --namespace <my_namespace> --hostname <log_server_hostname> --port <log_server_port> --type syslog
    ```
    {: pre}

    <table>
    <caption>表 1. 了解此命令的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>logging-config-create</code></td>
    <td>用于为您的名称空间创建日志转发配置的命令。</td>
    </tr>
    <tr>
    <td><code><em>&lt;my_cluster&gt;</em></code></td>
    <td>将 <em>&lt;my_cluster&gt;</em> 替换为集群的名称或标识。</td>
    </tr>
    <tr>
    <td><code>--namespace <em>&lt;my_namespace&gt;</em></code></td>
    <td>将 <em>&lt;my_namespace&gt;</em> 替换为名称空间的名称。<code>ibm-system</code> 和 <code>kube-system</code> Kubernetes 名称空间不支持日志转发。如果未指定名称空间，那么容器中的所有名称空间都将使用此配置。</td>
    </tr>
    <tr>
    <td><code>--hostname <em>&lt;log_server_hostname&gt;</em></code></td>
    <td>将 <em>&lt;log_server_hostname&gt;</em> 替换为日志收集器服务器的主机名或 IP 地址。</td>
    </tr>
    <tr>
    <td><code>--port <em>&lt;log_server_port&gt;</em></code></td>
    <td>将 <em>&lt;log_server_port&gt;</em> 替换为日志收集器服务器的端口。如果未指定端口，那么将对 syslog 使用标准端口 <code>514</code>。</td>
    </tr>
    <tr>
    <td><code>--type syslog</code></td>
    <td>syslog 的日志类型。</td>
    </tr>
    </tbody></table>

2. 验证是否已创建日志转发配置。

  * 要列出集群中的所有日志记录配置，请执行以下操作：
    ```
    bx cs logging-config-get <my_cluster>
    ```
    {: pre}

    输出示例：

    ```
    Logging Configurations
    ---------------------------------------------
    Id                                    Source      Protocol Host       Port
    f4bc77c0-ee7d-422d-aabf-a4e6b977264e  kubernetes  syslog   localhost  5514
    5bd9c609-13c8-4c48-9d6e-3a6664c825a9  ingress     ibm      -          -

    Container Log Namespace configurations
    ---------------------------------------------
    Namespace         Protocol    Host        Port
    default           syslog      localhost   5514
    my-namespace      syslog      localhost   5514   
    ```
    {: screen}

  * 要仅列出名称空间日志记录配置，请执行以下操作：
    ```
    bx cs logging-config-get <my_cluster> --logsource namespaces
    ```
    {: pre}

    输出示例：

    ```
    Namespace         Protocol    Host        Port
    default           syslog      localhost   5514
    myapp-namespace   syslog      localhost   5514
    ```
    {: screen}

#### 更新 syslog 服务器配置
{: #cs_namespace_update}

如果要更新当前 syslog 服务器配置的详细信息或更改为其他 syslog 服务器，那么可以更新日志记录转发配置。
{:shortdesc}

开始之前：

1. [设置 CLI 的目标](cs_cli_install.html#cs_cli_configure)为名称空间所在的集群。

要更改 syslog 转发配置的详细信息，请执行以下操作：

1. 更新日志转发配置。

    ```
    bx cs logging-config-update <my_cluster> --namespace <my_namespace> --hostname <log_server_hostname> --port <log_server_port> --type syslog
    ```
    {: pre}

    <table>
    <caption>表 2. 了解此命令的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>logging-config-update</code></td>
    <td>用于为您的名称空间更新日志转发配置的命令。</td>
    </tr>
    <tr>
    <td><code><em>&lt;my_cluster&gt;</em></code></td>
    <td>将 <em>&lt;my_cluster&gt;</em> 替换为集群的名称或标识。</td>
    </tr>
    <tr>
    <td><code>--namepsace <em>&lt;my_namespace&gt;</em></code></td>
    <td>将 <em>&lt;my_namespace&gt;</em> 替换为具有日志记录配置的名称空间的名称。</td>
    </tr>
    <tr>
    <td><code>--hostname <em>&lt;log_server_hostname&gt;</em></code></td>
    <td>将 <em>&lt;log_server_hostname&gt;</em> 替换为日志收集器服务器的主机名或 IP 地址。</td>
    </tr>
    <tr>
    <td><code>--port <em>&lt;log_collector_port&gt;</em></code></td>
    <td>将 <em>&lt;log_server_port&gt;</em> 替换为日志收集器服务器的端口。如果未指定端口，那么将使用标准端口 <code>514</code>。</td>
    </tr>
    <tr>
    <td><code>--type syslog</code></td>
    <td><code>syslog</code> 的日志记录类型。</td>
    </tr>
    </tbody></table>

2. 验证日志转发配置是否已更新。
    ```
    bx cs logging-config-get <my_cluster> --logsource namespaces
    ```
    {: pre}

    输出示例：

    ```
    Namespace         Protocol    Host        Port
    default           syslog      localhost   5514
    myapp-namespace   syslog      localhost   5514
    ```
    {: screen}

#### 停止 syslog 的日志转发
{: #cs_namespace_delete}

您可以通过删除日志记录配置来停止从名称空间转发日志。

**注**：此操作仅删除用于将日志转发到 syslog 服务器的配置。名称空间的日志将继续转发到 {{site.data.keyword.loganalysislong_notm}}。

开始之前：

1. [设置 CLI 的目标](cs_cli_install.html#cs_cli_configure)为名称空间所在的集群。

要停止将日志转发到 syslog，请执行以下操作：

1. 删除日志记录配置。

    ```
    bx cs logging-config-rm <my_cluster> --namespace <my_namespace>
    ```
    {: pre}
将 <em>&lt;my_cluster&gt;</em> 替换为日志记录配置所在集群的名称，并将 <em>&lt;my_namespace&gt;</em> 替换为名称空间的名称。
<br />


## 可视化 Kubernetes 集群资源
{: #cs_weavescope}

Weave Scope 提供了 Kubernetes 集群内资源（包括服务、pod、容器、进程、节点等等）的可视图。此外，Weave Scope 还提供了 CPU 和内存的交互式度量值，以及用于跟踪和执行到容器中的多种工具。
{:shortdesc}

开始之前：

-   切勿在公共因特网上公开您的集群信息。完成以下步骤以安全地部署 Weave Scope 并在本地从 Web 浏览器对其进行访问。
-   如果还没有标准集群，请[创建标准集群](#cs_cluster_ui)。Weave Scope 可以是 CPU 密集型，尤其是应用程序。因此请使用更大的标准集群运行 Weave Scope，而不要使用 Lite 集群。
-   [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群以运行 `kubectl` 命令。


要将 Weave Scope 用于集群，请执行以下操作：
2.  在集群中部署其中一个提供的 RBAC 许可权配置文件。

    启用读/写许可权：

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-{{site.data.keyword.Bluemix_notm}}/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    启用只读许可权：

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-{{site.data.keyword.Bluemix_notm}}/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    输出：

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  部署 Weave Scope 服务，此服务专供集群 IP 地址访问。

    <pre class="pre">
    <code>kubectl apply --namespace kube-system -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    输出：

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  运行端口转发命令以在计算机上启动该服务。现在，Weave Scope 已配置用于集群；下次访问 Weave Scope 时，可以运行以下端口转发命令，而不用再次完成先前的配置步骤。

    ```
    kubectl port-forward -n kube-system "$(kubectl get -n kube-system pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    输出：

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  打开 Web 浏览器并转至 `http://localhost:4040`。选择查看集群中 Kubernetes 资源的拓扑图或表。

     <img src="images/weave_scope.png" alt="Weave Scope 中的示例拓扑" style="width:357px;" />


[了解有关 Weave Scope 功能的更多信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.weave.works/docs/scope/latest/features/)。

<br />


## 除去集群
{: #cs_cluster_remove}

使用集群完毕后，可以将该集群除去，使其不再耗用资源。
{:shortdesc}

对于使用标准或 {{site.data.keyword.Bluemix_notm}}“现买现付”帐户创建的 Lite 和标准集群，在不再需要时，必须由用户手动除去。在免费试用期结束后，将自动除去使用免费试用帐户创建的 Lite 集群。

删除集群时，还会删除该集群上的资源，包括容器、pod、绑定的服务和私钥。如果在删除集群时未删除存储器，那么可以通过 {{site.data.keyword.Bluemix_notm}} GUI 中的 IBM Bluemix Infrastructure (SoftLayer) 仪表板删除存储器。受每月记帐周期的影响，无法在一个月的最后一天删除持久性卷申领。如果在一个月的最后一天删除持续性卷申领，那么删除操作会保持暂挂，直到下个月开始再执行。

**警告**：不会在持久存储器中创建集群或数据的备份。删除集群是永久性的，无法撤销。

-   通过 {{site.data.keyword.Bluemix_notm}} GUI
    1.  选择集群，然后单击**更多操作...** 菜单中的**删除**。
-   通过 {{site.data.keyword.Bluemix_notm}} CLI
    1.  列出可用的集群。


        ```
        bx cs clusters
        ```
        {: pre}

    2.  删除集群。

        ```
        bx cs cluster-rm my_cluster
        ```
        {: pre}

    3.  遵循提示并选择是否删除集群资源。

除去集群时，不会自动除去可移植公共和专用子网。子网用于将可移植公共 IP 地址分配给 LoadBalancer 服务或 Ingress 控制器。可以选择手动删除子网或将其复用于新集群。
