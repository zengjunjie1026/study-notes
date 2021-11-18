把群晖 NAS 变成「时间返回舱」，轻松搞定 Time Machine 无线备份



2018年11月18日

相比 Windows 自带的系统还原功能，macOS 有着更加完善的备份还原机制：通过内置的 Time Machine，我们可以方便地进行整机备份，在关键时候成为系统以及重要资料一颗「后悔药」。

当然，如果需要使用 Time Machine 功能备份，苹果官方提供了两种方式，无论哪一种方式，你都需要外置的存储来解决：一种是准备一个较合适容量的移动硬盘，如果你正好有一个闲置不用的移动硬盘，拿出来做专门的 Mac 的 Time Machine 备份倒是不错，只不过就会比较麻烦——每一次备份都需要插入移动硬盘。

还有一种是使用网络存储器，比如说苹果官方的高存储容量 AirPort Time Capsule（时间返回舱），设置一次就可以轻松完成自动备份工作，但其售价高昂不说，现在也已经全面在苹果在线商店下架，显然无论是从经济角度还是便利性角度而言，以上两个方案似乎都并不合适。

其实，如果你恰好有一台群晖 NAS ，就可以将其轻松打造成一个稳定可靠的「时间返回舱」，而该功能从入门级的 J 系列就已经默认搭载，换句话说，你几乎没有什么额外成本就可以拥有和 AirPort Time Capsule 近乎一样的功能，那么如何使用 Time Machine 将 Mac 文件备份至群晖呢？

## 如何让群晖 NAS 支持 Time Machine 功能？

要想让群晖支持 Time Machine ，还需要对群晖进行一番设置，首先我们使用管理员账号登录群晖的 DSM 系统1 ，首先来创建一个共享文件夹，专门用来存放 Time Machine 的备份数据：

登录到 DSM 之后，点击「控制面板 - 共享文件夹」，点击「创建」来添加共享文件夹。

![img](https://cdn.sspai.com/2018/11/18/ac135adbe134d0703a7acac6bbc4a04c.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

然后输入共享文件夹的名字2 ，然后选择一个所在位置（单盘 NAS 选择默认）：

![img](https://cdn.sspai.com/2018/11/18/7992d79626fd2f571cae842bde49b2c1.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

后面一步就是是否需要加密，因为我是本地家庭环境使用，为了减少一些不必要的麻烦直接跳过，如果你是在公司中来操作则建议设置加密，然后点击下一步。

![img](https://cdn.sspai.com/2018/11/18/940f3d290ab4cb4f91a7e6ee58d08d49.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

后面是为共享文件夹配置高级功能，这里我就直接跳过，然后确认设置下面选择应用。

![img](https://cdn.sspai.com/2018/11/18/57993635211b319945fe96921a7ef000.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

最后会弹出读取权限的设置，这里面默认应该只有管理员（当前你登录的账户）读写权限，这里默认不修改，点击确定生成分享文件夹。

![img](https://cdn.sspai.com/2018/11/18/a715c3349fbeca61d476e5050bb05bcf.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

现在你应该可以看到名为 Time Machine Folder 新共享文件夹了，下面进入到下一步，为这个文件夹配可访问的账号。

## 在 NAS 中为 Time Machine 创建管理用户

Time Machine Folder 这个文件夹显然应该只需要有一个专门操作的账户来操作，使用管理员账号操作并不安全，因此在这里我们需要给 Time Machine 创建管理用户并为其配备的对应配额限制。

![img](https://cdn.sspai.com/2018/11/18/86234695bbbb0758f2aa1d1567eca86b.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

首先，还是通过群晖的管理员账户登录 DSM ，在「控制面板 - 用户」中，点击「创建用户」，输入用户名3 并设置一个复杂密码，然后点下一步，在群组中保持默认再点下一步。

![img](https://cdn.sspai.com/2018/11/18/9b3e81fc108b528526b47257a8c8e7a5.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

在文件夹的权限分配这个设置页中，找到之前设置的共享文件夹「Time Machine Folder」，**勾选中「读写」权限（一定要做），**然后点击下一步，在用户配合设置页面中，选择存储配额。

这一步需要注意的是，为了避免 Time Machine 备份占用你全部的存储空间，建议设置在一个额定的空间范围内，比如我的 MacBook Pro 是 128GB 的存储，而我的群晖 NAS 的存储「空间 1」剩余 2.0 TB ，因此从使用权衡来看，选择 200GB 完全是绰绰有余，制定好之后选择下一步。

![img](https://cdn.sspai.com/2018/11/18/85d0d591291cf483391f7ad169493d12.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

后续两个设置中都默认不选，直接点下一步，最后点击到应用后完成用户「Time Machine User」的新建，最后你在「控制面板 - 用户」中应该可以看到这个新建的用户了。

![img](https://cdn.sspai.com/2018/11/18/cf650e3ab4e93f9bb7ec52c84e61124b.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

p.s.  如果你是在企业生产环境中，可以通过生成配对的「共享文件夹」+「对应的用户」来创建多个 Time Machine 备份空间，这样可以为多台 Mac 设备开启单独的备份，管理也更方便。

## 在 NAS 中开启相关文件服务

前面共享文件夹和相关的访问用户已经设置完毕，那么接下来就是对相关的文件夹进行一系列的设置。首先在群晖的「控制面板 - 文件服务」设置中，找到 **SMB/AFP/NFS** 选项卡，然后点击勾选 启用 SMB 服务以及 AFP 服务。注意下方生成的访问地址，例如我这里生成的 SMB 访问地址：`smb://DS215J`。

![img](https://cdn.sspai.com/2018/11/18/9a0808946eb2a9d0c1e4cebe4919a8bb.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

然后切换到高级设置中，在 Bonjour 中勾选「启用 Bonjour 服务发现」，以及下方的「启用通过 SMB 进行 Bonjour Time Machine 播送」和「启用通过 AFP 进行 Bonjour Time Machine 播送」，然后点击下方的设置 Time machine 文件夹，选择之前我们建立的 「Time Machine Folder」共享文件夹点击确定即可。

![img](https://cdn.sspai.com/2018/11/18/dbf8339cefcd9273213e57735e8c8d4d.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

p.s. 如果你的环境中，Mac 设备的系统版本在 10.12 以上，那么只需要启用 SMB 相关服务即可，可以不用勾选 AFP 服务。

至此，在群晖上的相关设置就全部完成了，换言之，这时候的群晖已经脱胎换骨成为一台「时间返回舱」了。

## 让 Mac 的 Time Machine 备份至 NAS

接下来的步骤就是在 Mac 上设置时间机器，然后将备份目的地改成刚才设置好的群晖，首先在我们需要可以让 Mac 访问群晖中的备份文件夹。

![img](https://cdn.sspai.com/2018/11/18/c67463c703df1874de7c6318052b2430.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

打开访达，在菜单中点击「前往 - 连接服务器」，然后输入 NAS 的 smb 地址，例如我这里的 `smb://DS215J`，然后点击链接，在输入之前设置的账户名和密码之后，在弹出的文件夹中，选择时间机器的备份文件夹，之后点击好完成装载。

![img](https://cdn.sspai.com/2018/11/18/1f1c1dea15242ee5d673acbdbc7e7429.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

接下来从 Dock 栏中打开「系统偏好设置」，在「时间机器」中选择「备份磁盘」，然后选择之前链接的群晖对应的文件夹，然后点击使用磁盘。

这时候还有可能会要求输入之前在群晖中创建的账户和密码，然后点击链接，然后时间机器会自动绑定群晖设置到了备份目的磁盘，然后到这里你的时间机器就算是真正意义上的设置好了，如果不出意外的话就会自动开始进行备份工作。

通过一连串的操作之后，你的群晖可以轻松化身为一台「廉价版 AirPort Time Capsule」，为你的 Mac 设备保驾护航，而无论是性价比上还是便利性上都可以称作是 Mac 时间机器的最佳解决方案，如果你恰好有一台 Mac 和群晖设备，不妨试试这个方案来打造一个最廉价的时间机器备份方案。