# gradle和android gradle plugin的区别
gradle是一个打包编译工具，和android本身无关。而google推荐使用gradle进行构建和依赖管理，于是开发了android gradle 插件帮助进行android的打包编译，我们平常接触的gradle脚本其实就是agp

# android签名
为什么android需要签名？为了保证用户下载安装的release包没有被串改过。
如何保证呢？通过数字签名和数字证书

**数字签名**

 通过摘要算法，每个app会生成一个唯一的摘要，每次修改app都会生成一个不同的摘要。
通过非对称加密算法，apk签名的时候，使用私钥进行加密，用户下载到apk时会通过公钥进行解密，通过信息对比确定这个下载的apk就是没有被串改的
当然现在依然有一个问题，如何保证开发者打包的公钥是正确的？

**数字证书**

数字证书需要权威的机构下发，与开发者自行进行数字签名不同，数字证书的公钥在手机组装的时候已经确定了，一定是正确的，所以只需要对数字签名的公钥放在数字证书里包一层，在通过私钥加密。这样就能保证用户拿到的一定是正确的公钥


# minsdkversion、compilesdkversion、targetsdkversion的理解


- minsdkversion：表示对于该app允许被安装的最小的api版本（系统版本）
- compilesdkversion：在代码编写期间（编译期间），使用的api版本。比如有些新api版本才有的方法，在minsdkversion中还没有，那么就需要进行版本控制兼容
- targetsdkversion：代表了对于该app我们适配到了哪个版本，对于超过targetsdk版本的手机系统运行了该app，会默认调用targetsdkversion支持的api版本

