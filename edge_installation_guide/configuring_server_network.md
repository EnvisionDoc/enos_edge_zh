# 服务器网络配置

通过运行如下指令完成服务器的网络配置：

*表 1：操作一：编辑网络配置文件*

<table>
  <tr>
    <td>指令</td>
    <td>备注</td>
  </tr>
  <tr>
    <td>vim/etc/sysconfig/network-scripts/ifcfg-ens32</td>
    <td></td>
  </tr>
  <tr>
    <td>DEVICE=ens32</td>
    <td></td>
  </tr>
  <tr>
    <td>IPADDR=xxx.xxx.xxx.xxx</td>
    <td>使用已规划的IP地址</td>
  </tr>
  <tr>
    <td>NETMASK=255.255.255.0</td>
    <td></td>
  </tr>
  <tr>
    <td>GATEWAY=yyy.yyy.yyy.yyy</td>
    <td>使用已规划的网关</td>
  </tr>
  <tr>
    <td>ONBOOT=yes</td>
    <td></td>
  </tr>
  <tr>
    <td>NAME=ens32</td>
    <td></td>
  </tr>
  <tr>
    <td>DNS1=zzz.zzz.zzz.zzz</td>
    <td>使用已规划的DNS</td>
  </tr>
</table>

*表 2：操作二：启动ssh服务*

<table>
  <tr>
    <td>指令</td>
    <td>备注</td>
  </tr>
  <tr>
    <td>systemctl restart sshd</td>
    <td></td>
  </tr>
  <tr>
    <td>systemctl enable sshd</td>
    <td></td>
  </tr>
</table>

完成以上配置后，既可以远程管理Edge及进行后续的安装操作。
