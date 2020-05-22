##sftp

### 适用场景

>我们平时习惯了使用FTP来上传下载文件，尤其是很多Linux的环境下，我们一般都会通过第三方的SSH工具连接到Linux的，但是当我们需要传输文件到Linux的服务器当中，很多人习惯用FTP来传输，其实Linux的默认是不提供FTP的，需要你额外安装FTP服务器。而且FTP服务器端会占用一定的VPS服务器资源。其实笔者更建议使用SFTP代替FTP。
>
>　　主要因为：一，可以不用额外安装任何服务器端程序（我比较中意这个，哈哈~~，很多公司为了安全性的Linux没有外网环境，只有SSH的时候，想传输文件是很悲催的问题）二，会更省系统资源。三，SFTP使用加密传输认证信息和传输数据，相对来说会更安全。四，也不需要单独配置，对新手来说比较简单（开启SSH默认就开启了SFTP）。
>
>

### sftp是什么

>FTP是一种文件传输协议，一般是为了方便数据共享的。包括一个FTP服务器和多个FTP客户端.FTP客户端通过FTP协议在服务器上下载资源。而SFTP协议是在FTP的基础上对数据进行加密，使得传输的数据相对来说更安全。但是这种安全是以牺牲效率为代价的，也就是说SFTP的传输效率比FTP要低（不过现实使用当中，没有发现多大差别）。

### sftp的好处

>SFTP不要安装; SFTP更安全，但更安全带来副作用就是的效率比FTP要低些。　　
>
>SFTP同样是使用加密传输认证信息和传输的数据，所以，使用SFTP是非常安全的。

###和ftp的优劣点

>　FTP是一种文件传输协议，一般是为了方便数据共享的。包括一个FTP服务器和多个FTP客户端.FTP客户端通过FTP协议在服务器上下载资源。而SFTP协议是在FTP的基础上对数据进行加密，使得传输的数据相对来说更安全。但是这种安全是以牺牲效率为代价的，也就是说SFTP的传输效率比FTP要低（不过现实使用当中，没有发现多大差别）。个人肤浅的认为就是：一; FTP要安装，SFTP不要安装二; SFTP更安全，但更安全带来副作用就是的效率比FTP要低些。

### Java中如何使用

[Java实现的SFTP](https://blog.csdn.net/weixin_36795183/article/details/78626215)

[hutool]()

```
hutool封装了这些方法的使用
```

### sftp常用命令

```shell
## linux远程连接
sftp -oPort=2222 weizhi_2020@218.94.106.239

```



```
put()：      文件上传
get()：      文件下载
cd()：       进入指定目录
ls()：       得到指定目录下的文件列表
rename()：   重命名指定文件或目录
rm()：       删除指定文件
mkdir()：    创建目录
rmdir()：    删除目录
```

###代码使用

```xml
//依赖
     <!--工具类-->
            <dependency>
                <groupId>cn.hutool</groupId>
                <artifactId>hutool-sftp</artifactId>
                <version>4.6.8</version>
            </dependency>
```



```java
  public boolean pushGOIPCallToJiangsu(String day) {
        Sftp sftp = null;
        String tmpFile = null;
        try {
            sftp = JschUtil.createSftp(FzPropConstant.GUANGZHOU_SFTP_HOST, FzPropConstant.GUANGZHOU_SFTP_PORT,
                    FzPropConstant.GUANGZHOU_SFTP_USER, FzPropConstant.GUANGZHOU_SFTP_PASSWORD);
            List<String> fileNames = sftp.lsFiles(FzPropConstant.GUANGZHOU_GOIP_PATH);
            Date date = new Date();
            if (!Strings.isNullOrEmpty(day)) {
                date = DateUtil.str2Date(day, DateUtil.YYYY_MM_DD);
            }
            //默认删除30天之前的数据
            for (String fileName : fileNames) {
                //删除30天以外的数据
                try {
                    String fileNameSuffix = fileName.substring(0, fileName.indexOf("."));
                    Date dateFile = DateUtil.str2Date(fileNameSuffix, DateUtil.YYYYMMDDHHMMSS_FULL);
                    int compareDay = DateUtil.daysBetween(dateFile, date);
                    //删除超过30天文件
                    if (compareDay > GOIP_TMP_DAY) {
                        log.info("删除文件{}" + FzPropConstant.GUANGZHOU_GOIP_PATH + fileName);
                        sftp.delFile(FzPropConstant.GUANGZHOU_GOIP_PATH + fileName);
                    }
                    //上传当天的文件
                    if (compareDay == 0) {
                        //文件上传到管局服务器
                        if (!new File(GOIP_TASTIC_DIR).exists()) {
                            FileUtil.mkdir(GOIP_TASTIC_DIR);
                        }
                        //临时存放地方
                        tmpFile = GOIP_TASTIC_DIR + fileName;
                        //从广州下载
                        sftp.get(FzPropConstant.GUANGZHOU_GOIP_PATH + fileName, tmpFile);
                        log.info("===SFTP 连接江苏管局===");
                        SFTPUtils sFTPUtils = new SFTPUtils(FzPropConstant.GUANJU_SFTP_HOST, FzPropConstant.GUANJU_SFTP_PORT,
                                FzPropConstant.GUANJU_SFTP_USER, FzPropConstant.GUANJU_SFTP_PASSWORD);
                        //上传到管局
                        log.info("===准备上传文件到江苏管局===");
                        sFTPUtils.uploadFile(new File(tmpFile), FzPropConstant.GUANJU_GOIP_PATH, false, false);
                        log.info("===结束上传文件到江苏管局===");
                    }
                    log.info("文件名 fileName={} 距今天{}", fileName, compareDay);
                } catch (Exception e) {
                    log.error("fileName 文件名操作失败={}", e.getMessage());
                    e.printStackTrace();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            IoUtil.close(sftp);
        }
        return true;
    }
```

```java
public class SFTPUtils {
    private static final String TMP_SUFFIX = ".tmp";
    private String host;
    private int port;
    private String user;
    private String password;

    public SFTPUtils(String host, int port, String user, String password) {
        this.host = host;
        this.port = port;
        this.user = user;
        this.password = password;
    }

    public void uploadFile(File file, String dest, boolean useTmpSuffix, boolean cleanFile) throws Exception {
        Assert.notEmpty(dest, "上传目录为空", new Object[0]);
        Assert.notNull(file, "要上传的文件为空", new Object[0]);
        if (!file.exists()) {
            throw new RuntimeException("要上传的文件不存在,path=" + file.getAbsolutePath());
        } else {
            Sftp sftp = new Sftp(this.host, this.port, this.user, this.password, CharsetUtil.CHARSET_UTF_8);
            File tmpFile = null;

            try {
                String originalFileName = file.getName();
                if (useTmpSuffix) {
                    String tmpFileName = file.getName() + ".tmp";
                    String tmpFileLocation = file.getAbsolutePath() + ".tmp";
                    tmpFile = new File(tmpFileLocation);
                    tmpFile.createNewFile();
                    FileUtil.move(file, tmpFile, true);
                    sftp.put(tmpFileLocation, dest);
                    sftp.cd(dest);
                    sftp.getClient().rename(tmpFileName, originalFileName);
                } else {
                    sftp.put(file.getAbsolutePath(), dest);
                }
            } finally {
                sftp.close();
                if (null != tmpFile) {
                    if (!FileUtil.del(tmpFile)) {
                        throw new RuntimeException("临时文件删除失败,path=" + tmpFile.getAbsolutePath());
                    }

                    if (cleanFile && !FileUtil.del(file)) {
                        throw new RuntimeException("删除文件失败,path=" + file.getAbsolutePath());
                    }
                }

            }

        }
    }
```

