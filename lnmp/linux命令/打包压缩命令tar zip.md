```
打包时，可以使用
.tar

打包：tar cvf FileName.tar DirName

解包：tar xvf FileName.tar
（注：tar是打包，不是压缩！）
--exclude=  打包时，跳过某个目录
使用示例
tar cvf www_data_www_androiddev.tar /data/www/androiddev/ --exclude=androiddev/imgfolder
打包时，跳过多个目录
tar cvf www_data_www_androiddev.tar /data/www/androiddev/ --exclude=androiddev/imgfolder   --exclude=androiddev/imgfolder

tar cvf www_data_www_mobile.tar /data/www/mobile/ --exclude= mobile/data --exclude=mobile/public/upload --exclude=mobile/public/www/news --exclude=mobile/public/www/zt

tar cvf www.tw_disk_cmstop.tar /disk/cmstop/ --exclude=cmstop
---------------------------------------------
.gz
解压1：gunzip FileName.gz
解压2：gzip -d FileName.gz
压缩：gzip FileName
 
.tar.gz
解压：tar zxvf FileName.tar.gz
压缩：tar zcvf FileName.tar.gz DirName
---------------------------------------------
.bz2
解压1：bzip2 -d FileName.bz2
解压2：bunzip2 FileName.bz2
压缩： bzip2 -z FileName
.tar.bz2
解压：tar jxvf FileName.tar.bz2
压缩：tar jcvf FileName.tar.bz2 DirName
---------------------------------------------
.bz
解压1：bzip2 -d FileName.bz
解压2：bunzip2 FileName.bz
压缩：未知
.tar.bz
解压：tar jxvf FileName.tar.bz
压缩：未知
---------------------------------------------
.Z
解压：uncompress FileName.Z
压缩：compress FileName
.tar.Z
解压：tar Zxvf FileName.tar.Z
压缩：tar Zcvf FileName.tar.Z DirName
---------------------------------------------
经过测试,这个好使!
.tgz
解压：tar zxvf FileName.tgz
压缩：未知
.tar.tgz
解压：tar zxvf FileName.tar.tgz
压缩：tar zcvf FileName.tar.tgz FileName
---------------------------------------------
.zip
-r 是递归处理的意思  自动压缩目录下的子目录
压缩：zip -r FileName.zip DirName
解压：unzip FileName.zip

---------------------------------------------
.rar
解压：rar a FileName.rar
压缩：rar e FileName.rar
rar请到：http://www.rarsoft.com/download.htm 下载！
解压后请将rar_static拷贝到/usr/bin目录（其他由$PATH环境变量指定的目录也可以）：

[root@www2 tmp]# cp rar_static /usr/bin/ra
```

