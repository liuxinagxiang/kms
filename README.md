# kms-activate
---
##  How to activate windows server:
```
slmgr /upk  #卸载产品密钥

slmgr /ipk 489J6-VHDMP-X63PK-3K798-CPX3Y  #Windows Server 2008 R2 企业版秘钥

slmgr /skms kms.zhangyi.cf	#激活服务器

slmgr /ato #配置生效

slmgr /dlv #查看激活状态
```
**KMS 激活有 180 天期限,默认情况下系统每 7 天自动进行一次激活续订尝试。在续订客户端激活之后，激活有效期时间间隔将重新开始计算，重置为180天**<br/>

kms激活的前提是你的系统是批量授权版本，即VL版，一般企业版都是VL版，专业版有零售和VL版，家庭版旗舰版OEM版等等那就肯定不能用kms激活。一般建议从http://msdn.itellyou.cn上面下载系统 
VL版本的镜像一般内置GVLK key，用于kms激活。如果你手动输过其他key，那么这个内置的key就会被替换掉，这个时候如果你想用kms，那么就需要把GVLK key输回去。

首先到 https://technet.microsoft.com/en-us/library/jj612867.aspx 或者 https://learn.microsoft.com/zh-cn/windows-server/get-started/kms-client-activation-keys 获取你对应版本的KEY

如果不知道自己的系统是什么版本，可以运行以下命令查看系统版本：
```
wmic os get caption
```
得到对应key之后，使用管理员权限运行cmd执行安装key：
```
slmgr /ipk xxxxx-xxxxx-xxxxx-xxxxx
```
**Windows10 家庭版（RTL版）升级为企业版（VOL版）并激活：**

Windows10 最新版微软官方下载地址：https://www.microsoft.com/zh-cn/software-download/windows10ISO/
请使用“手机” 访问按提示选择版本和语言获取iOS镜像下载链接（电脑直接访问会跳转到更新助手页面）
该版本安装完默认为 Windows10 家庭版（RTL版）依照以下命令升级为企业版（VOL版）并激活。

1.使用 Win+i 快捷键打开「设置」- 点击「更新和安全」- 在左侧点击「激活」选项卡点击右侧的「更改产品密钥按钮」，输入Key：NPPR9-FWDCX-D2C8J-H872K-2YT43
如果提示密钥错误，请先输入专业版密钥：VK7JG-NPHTM-C97JM-9MPGT-3V66T 升级为专业版后，再输入企业版或者其他版本密钥进行升级

2.按提示升级后打开命令提示符(管理员)逐行执行以下命令：
```
slmgr /ipk NPPR9-FWDCX-D2C8J-H872K-2YT43
slmgr /skms kms.zhangyi.cf && slmgr /ato
```
同样的方法可以升级为专业版、教育版等，以及版本退回切换（政府版升级不可逆）

##  How to activate office:

首先你的OFFICE必须是VOL版本，否则无法激活。 找到你的office安装目录，比如:
```
C:\Program Files (x86)\Microsoft Office\Office16
```
64位的就是
```
C:\Program Files\Microsoft Office\Office16
```
office16是office2016，office15就是2013，office14就是2010.
接下来我们就cd到这个目录下面，例如：
```
cd C:\Program Files (x86)\Microsoft Office\Office16
```
然后执行注册kms服务器地址：
```
cscript ospp.vbs /sethst:kms.zhangyi.cf
```
一般来说，“一句命令已经完成了”，但一般office不会马上连接kms服务器进行激活，所以我们额外补充一条手动激活命令：
```
cscript ospp.vbs /act
```
如果提示看到successful的字样，那么就是激活成功了，重新打开office就好。

###如果遇到报错，请检查：

&emsp;&emsp;你的系统/OFFICE是否是批量VL版本,需要转换为VOL版才能激活

&emsp;&emsp;是否以管理员权限运行CMD

&emsp;&emsp;你的系统/OFFICE是否修改过KEY/未安装GVLK KEY：执行命令安装密钥后重新激活：
```
  cscript ospp.vbs /inpkey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX
```
&emsp;&emsp;检查你的网络连接

&emsp;&emsp;根据出错代码自己搜索出错原因

&emsp;&emsp;服务器繁忙，稍后再试

**Office365 家庭版（RTL版）转换专业版（VOL版）并激活（以64位默认安装目录为例）：**

Office365 最新版微软官方下载地址：https://products.office.com/zh-cn/try
该版本安装完默认为 Office365 家庭版（RTL版）依照以下命令升级为专业版（VOL版）并激活。

1.打开命令提示符(管理员)执行以下命令进入OSPP.VBS目录
```
cd C:\Program Files\Microsoft Office\Office16
```
2.将Office365家庭版RTL版转换为专业版VOL版
```
for /f %x in ('dir /b ..\root\Licenses16\proplusvl_kms*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%x"
for /f %x in ('dir /b ..\root\Licenses16\proplusvl_mak*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%x"
```
3.安装KMS激活密钥
```
cscript ospp.vbs /inpkey:XQNVK-8JYDB-WJ9W3-YJ8YR-WFG99
```
4.激活
```
cscript ospp.vbs /sethst:kms.zhangyi.cf && cscript ospp.vbs /act
```
**查看已安装的Windows或Office是否为VOL版：**

1.查验Windows在命令提示符执行：slmgr /dlv

2.查验Office命令提示符进入OSPP.VBS目录执行：cscript ospp.vbs /dstatus
在“描述（DESCRIPTION）”这一行内有 VOLUME 字样就是VOL版，就是支持KMS激活

**Windows GVLK密钥对照表（KMS激活专用）**

以下key来源于微软官网：https://technet.microsoft.com/en-us/library/jj612867.aspx

**微软官方文档：**

[Windows] https://docs.microsoft.com/zh-cn/windows/deployment/volume-activation/activate-using-key-management-service-vamt<br/>
[Windows] https://docs.microsoft.com/zh-cn/windows/deployment/volume-activation/use-the-volume-activation-management-tool-client<br/>
[Windows] https://docs.microsoft.com/zh-cn/windows/deployment/volume-activation/volume-activation-windows-10<br/>
[Windows] https://docs.microsoft.com/zh-cn/windows/deployment/volume-activation/activate-windows-10-clients-vamt<br/>
[Windows] https://docs.microsoft.com/zh-cn/windows/deployment/volume-activation/plan-for-volume-activation-client<br/>
[Windows] https://docs.microsoft.com/zh-cn/windows-server/get-started-19/activation-19<br/>
[Windows] https://docs.microsoft.com/zh-cn/windows-server/get-started/kmsclientkeys<br/>
[Office] https://docs.microsoft.com/zh-cn/deployoffice/vlactivation/plan-volume-activation-of-office<br/>
[Office 2016/2019] https://docs.microsoft.com/zh-cn/DeployOffice/vlactivation/gvlks<br/>
[Office 2013] https://docs.microsoft.com/zh-cn/previous-versions/office/dn385360(v=office.15)<br/>
[Office 2010] https://docs.microsoft.com/zh-cn/previous-versions/office/office-2010/ee624355(v=office.14)<br/>
[Windows] https://docs.microsoft.com/zh-cn/search/index?search=kms<br/>

**Make your own activation-server with docker:**
>    [docker pull liuxiangxiang/kms-activate](https://hub.docker.com/r/liuxiangxiang/kms-activate)
---
