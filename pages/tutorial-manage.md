# 使用管理端

双击刚创建的`manage.bat`脚本即可启动管理端。管理端使用交互式命令行来完成各种操作，即使是在黑框命令行下也非常简单易用

## 创建第一个更新包

创建第一个更新包之前，要先把要更新的文件复制到`workspace`里来，这是一些栗子：（workspace目录里各种子文件夹仍然需要手动创建）

+ 要更新所有模组，复制`.minecraft/mods`目录
  + 到`mp/workspace/.minecraft/mods`

+ 要更新资源包，复制`.minecraft/resourcepacks`目录
  + 到`mp/workspace/.minecraft/resourcepacks`

+ 要更新.minecraft目录旁边的`新玩家进服教程.txt`，复制`新玩家进服教程.txt`文件
  + 到`mp/workspace/新玩家进服教程.txt`

+ 如果你开了版本隔离，就需要复制`.minecraft/versions/your-version/mods`目录
  + 到`mp/workspace/.minecraft/versions/your-version/mods`


到这里你可能已经看出规律了：`workspace`相当于本地的`.minecraft`目录，唯一的区别是`workspace`下只需要复制要更新的文件，不更新的文件暂时不用复制，会占用太多的存储空间

> 如果你打算配置一键启动，那么游戏核心文件和资源文件就不能加入更新，会导致更新失败！

然后运行manage.bat，输入`1`来开始创建第一个更新包。第一个版本号通常输入1.0，当然你也可以输入任何你喜欢的版本号风格，好了之后按Enter确定

>  版本号只能包括大小写字母数字，以及`!@#$()_+-=;',.`切勿使用中文或者空格或其它字符

输入版本号之后，程序会列出你对文件的所有更改。因为我们是第一次打更新包，文件很多，我们粗略看一下就好，后续打包中建议仔细审阅这个列表以确保对文件的修改都是没问题的

如果你要给这个版本写更新记录的话，可以在此时打开`changelogs.txt`文件，把你的更新记录粘贴进去并保存（更新记录只能使用纯文本，不支持富文本格式）。如果你不想写更新记录，请直接跳过这一步

接着输入`y`开始正式打包，首次打包内容一般都较大，可能会花费相当多的时间，请耐心等待

等到出现`创建版本完成`的字样后，就说明打包成功了（更新包文件会保存在public目录下）

## 容易出错的地方

如果你打包完成之后才发现这个版本的内容有问题时，应该重新再打一个更加新的版本来修复这个问题，而不是选择手动删掉更新包文件

已发布的版本千万不能手动删除文件，会导致后续更新全部出错！若你实在需要撤回这个版本，请参考[版本发错了怎么办](tutorial-notices.md#版本发错了怎么办)

## 后续发布新版

1. 后续发布新版本很简单，只需要对`workspace`目录下的文件做修改（就像对本地文件一样修改就好），然后打出更新包就完成了
2. 比如我想要删除客户端的一个模组a.jar再添加一个新的模组b.jar，那么只需要在workspace目录下删掉a.jar然后复制进去b.jar，接着打包新版本就好
3. 若新旧文件同名，但文件内容被修改了也只一样的做法：直接覆盖旧文件就好，程序也能自动检测到
    + 注意：如果遇到新旧文件同名但是大小写不一样，这种情况客户端会更新出错，请尽量人为避免这种情况出现（这是个bug等待修复）
4. 对目录的新建和删除也是一样，该怎么新建怎么新建，该什么删除怎么删除，就就像对本地文件一样进行这些操作
5. 如果你在workspace目录改了一些文件，但又觉得不妥，想要丢弃这些修改，可以启动管理端，输入`4`来还原

## 目录用途说明

1. history目录：用来作为对比，以计算你对workspace目录做了哪些修改的目录
   1. 这个目录正常情况下和workspace目录里的文件一致，当你修改了workspace目录下的文件通过history目录进行对比就能算出你改了哪些文件目录
   2. history目录下的内容由程序自动维护，千万不要手动修改history目录下的文件，否则程序计算出来补丁二进制数据就是错乱的，会导致客户端无法更新到新版本
   3. 若不小心修改了history目录的文件，可以参考[这里](tutorial-notices.md#不小心修改了history目录)来修复
2. public目录：用来存放历史更新包，每个更新包有2个文件：二进制数据文件(.bin)和元数据文件(.json)，还有一个版本列表文件mc-patch-versions.txt
   1. 如果在一个版本中你仅做了删除文件，删除目录，新建目录的操作，同时又没有进行任何修改文件和新增文件的时候，最终打出来的更新包就只会包含一个json文件，而没有bin文件，这是正常现象
3. worksapce目录：服主日常维护客户端文件内容的地方，要对客户端做更新可以直接对整个文件做修改，然后打出更新包，客户端就会自动更新到新版本了