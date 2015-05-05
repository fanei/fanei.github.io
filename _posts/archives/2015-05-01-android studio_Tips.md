---
layout: article
title:  android studio code formatter
categories: archives
image:
---

使用Eclipse惯了的同学们一般都有一套自己的代码模板，今天在使用AS的时候发现代码的格式很让人着急，于是就想导入一套自己以前的格式模板，于是乎就安装下面的方法导入进去了。



  1. 准备好你的xxx.xml文件，当然你可以从往上下载，也可以从eclipse里导出来，这个相信大家的能力，一定能够导出来。
  2. 把文件拷贝到~/Library/Preferences/AndroidStudio/codestyles/文件夹中。
  这里需要特别的注意，我的androidStuido版本是从1.1直接升到1.2的，我的Preferences文件夹下面有两个文件 androidStuido和androidStuido1.2的文件夹，这个时候呢，需要把文件拷贝到androidStuido1.2的文件夹下面的codestyles。我在这里郁闷了很久，一开始不知道这里有两个文件夹，直接用命令拷贝到了androidStuido文件夹里，造成后面无法显示出来模板。
  3. 从起AS。这个是必须的。
  4. Android Studio > Preferences > Code Style > Java  Scheme里面直接选择你导入的名字即可。
  
  
    需要注意的地方：Android code sytle 行的宽度是100，这个太小了，建议大家改成200
    修改方法：Code Style > General, aRight margin (columns)改为 200.