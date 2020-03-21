---
title: 编辑 OVA/OVF 文件，处理 VirtualBox 导入报错
author: 陈天傲
---

受人之托解决一个 VirtualBox 降级导致的 OVA 导入报错问题。

## 问题

VirtualBox 降级后发现在低版本导入由高版本导出的 OVA 文件时报错，错误大意是配置信息中某字段的值 "VBoxSVGA" 不合法。

查阅 Oracle® VM VirtualBox® 官方文档后发现，该较低版本的 VirtualBox 不支持 VBoxSVGA 图形控制器，亦不提供在图形化控制台手动或自动排除/忽略错误的能力。

该问题被诊断为由 VirtualBox 降级导致的配置兼容问题，用户需要 do it yourself。

## 解决

### 处理 OVA 文件

OVA 即 Open Virtualization Format Archive，是一个虚拟机实例的完整打包，其包含描述整个虚拟机所需的一切信息。

OVA 文件的本质是一个 tar 归档，使用 tar 兼容的解包工具即可解包。最简单粗暴的方式就是修改文件扩展名为 .tar，然后直接调用系统默认 tar 解包应用。

根据 OVF (Open Virtualization Format) 包结构规范，解包后应当可以看到如下几类文件：

- 扩展名为 .ovf 的 OVF 描述文件，是描述该虚拟机及其配置的文件，有且只有一个

- 虚拟硬盘等资源文件，视虚拟机本身及导出时的策略会有零个到多个

- 扩展名为 .cert 的 OVF 证书文件，可能有零个或一个

- 扩展名为 .mf 的 OVF 清单（manifest）文件，记录了上述所有文件的文件名和 SHA 摘要，可能有零个或一个

### 处理 OVF 描述文件

扩展名为 .ovf 的 OVF 描述文件本质是 XML 文件，使用文本编辑器打开即可直接编辑。这个文件通常有几百行，记得选一个顺手且带有查找功能的文本编辑器。

使用文本编辑器的查找功能定位到的 "VBoxSVGA" 一值，替换该键值为受该较低版本 VirtualBox 兼容的值，如 "VBoxVGA" 或 "VMSVGA"。通常情况下，这个值是唯一的。如果该值在全局不唯一，则根据前后的字段信息、报错内容及实际需求变通。在本案中，由于 OVF 描述文件包含了多个虚拟机，所以该值不唯一，为解决兼容问题，需要替换所有匹配的值。  
![Replace VBoxSVGA with VBoxVGA](/assets/modify-ova-ovf-files-handle-virtualbox-import-issues/Screen Shot 2020-01-02 at 21.17.59.png)  
其实没什么必要看图，但为了明确这里还是放一个。截图不涉敏感信息，勿念。

### 处理 OVF 清单文件

扩展名为 .mf 的 OVF 清单文件亦为纯文本文件，使用文本编辑器打开，更新刚才修改过的文件的 SHA1 值。根据 OVF 规范，这里亦可能为 SHA256。

这里有一种计算文件 SHA1 值的方法可供参考：  
命令行执行 `shasum -a 1 [文件路径/文件名]` 。

### 处理 OVF 证书文件

如果你没有看到扩展名为 .cert 的 OVF 证书文件，则可以忽略这一节。默认情况下，由 VirtualBox 导出的 OVA 文件或 OVF 包应当不会包含 OVF 证书文件。

OVF 证书通过对 OVF 清单文件签名实现对整个 OVF 包的签名，而 OVF 清单文件本身包含 OVF 描述文件的摘要，因此对 OVF 描述文件的任何修改都将使 OVF 证书失效。

根据 OVF 规范，任何 OVF 描述文件都不应在内部包含或引用 OVF 证书文件，这意味着证书文件不会成为描述文件的依赖，证书文件也非不可或缺。

因此，如果你看到了扩展名为 .cert 的 OVF 证书文件，直接删除可以避免证书报错。但是如果你确实需要此等安全性，则必须设法对 OVF 包执行重新签名。

### 收尾

此时，修改后的 OVF 描述文件已可直接导入 VirtualBox，VirtualBox 会在同路径下自动找到关联的其他文件。

如果你希望将这些文件再次打包为 OVA 文件，则需遵循 OVF 规范，这里不再赘述。
