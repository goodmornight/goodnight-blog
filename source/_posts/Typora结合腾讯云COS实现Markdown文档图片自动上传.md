---
title: Typora结合腾讯云COS实现Markdown文档图片自动上传
categories: notes
date: 2020-07-14 09:26:38
tags:
	- 建站
	- 图床
	- Markdown
photos:
---





下面参考链接中，腾讯云大学给的流程步骤已经够详细够小白了。

但我在实现的过程中还是有些出入，所以我在此记录一下。（文中很多图片直接用的腾讯云大学文档中的图片，如有侵权，请联系我删除）

# 存储桶创建

1. 访问腾讯云对象存储COS的控制台，进入 [存储桶列表](https://console.cloud.tencent.com/cos5/bucket) 页面。
2. 创建存储桶

![img](https://course-public-resources-1252758970.cos.ap-chengdu.myqcloud.com/%E5%AE%9E%E6%88%98%E8%AF%BE/%E5%88%A9%E7%94%A8%E8%85%BE%E8%AE%AF%E4%BA%91COS%E4%BD%9C%E4%B8%BA%E5%9B%BE%E5%BA%8A%E4%B8%BAMarkdown%E6%96%87%E6%A1%A3%E6%8F%90%E4%BE%9B%E5%9B%BE%E7%89%87%E9%93%BE%E6%8E%A5/20200103144443-133736.png)

3. 在存储桶中创建文件夹，文件名随意

![img](https://course-public-resources-1252758970.cos.ap-chengdu.myqcloud.com/%E5%AE%9E%E6%88%98%E8%AF%BE/%E5%88%A9%E7%94%A8%E8%85%BE%E8%AE%AF%E4%BA%91COS%E4%BD%9C%E4%B8%BA%E5%9B%BE%E5%BA%8A%E4%B8%BAMarkdown%E6%96%87%E6%A1%A3%E6%8F%90%E4%BE%9B%E5%9B%BE%E7%89%87%E9%93%BE%E6%8E%A5/20200103144520-456884.png)

4. 获取存储桶相关信息（敲黑板，这是重点，后面要考的）

![img](https://course-public-resources-1252758970.cos.ap-chengdu.myqcloud.com/%E5%AE%9E%E6%88%98%E8%AF%BE/%E5%88%A9%E7%94%A8%E8%85%BE%E8%AE%AF%E4%BA%91COS%E4%BD%9C%E4%B8%BA%E5%9B%BE%E5%BA%8A%E4%B8%BAMarkdown%E6%96%87%E6%A1%A3%E6%8F%90%E4%BE%9B%E5%9B%BE%E7%89%87%E9%93%BE%E6%8E%A5/20200103144544-431705.png)

![image-20200714093845099](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/14/093846-607528.png)

# 子用户创建与密钥获取

建议创建一个专门用于向腾讯云COS传图片的用户。

1. 前往访问管理页面，创建子用户

   前往访问管理CAM的 [用户列表](https://console.cloud.tencent.com/cam) 页面。

   1）新建用户

   ![img](https://course-public-resources-1252758970.cos.ap-chengdu.myqcloud.com/%E5%AE%9E%E6%88%98%E8%AF%BE/%E5%88%A9%E7%94%A8%E8%85%BE%E8%AE%AF%E4%BA%91COS%E4%BD%9C%E4%B8%BA%E5%9B%BE%E5%BA%8A%E4%B8%BAMarkdown%E6%96%87%E6%A1%A3%E6%8F%90%E4%BE%9B%E5%9B%BE%E7%89%87%E9%93%BE%E6%8E%A5/20200103144746-239165.png)

   2）自定义创建

   ![img](https://course-public-resources-1252758970.cos.ap-chengdu.myqcloud.com/%E5%AE%9E%E6%88%98%E8%AF%BE/%E5%88%A9%E7%94%A8%E8%85%BE%E8%AE%AF%E4%BA%91COS%E4%BD%9C%E4%B8%BA%E5%9B%BE%E5%BA%8A%E4%B8%BAMarkdown%E6%96%87%E6%A1%A3%E6%8F%90%E4%BE%9B%E5%9B%BE%E7%89%87%E9%93%BE%E6%8E%A5/20200103144746-239165.png)

   3）选择子用户类型

   ![img](https://course-public-resources-1252758970.cos.ap-chengdu.myqcloud.com/%E5%AE%9E%E6%88%98%E8%AF%BE/%E5%88%A9%E7%94%A8%E8%85%BE%E8%AE%AF%E4%BA%91COS%E4%BD%9C%E4%B8%BA%E5%9B%BE%E5%BA%8A%E4%B8%BAMarkdown%E6%96%87%E6%A1%A3%E6%8F%90%E4%BE%9B%E5%9B%BE%E7%89%87%E9%93%BE%E6%8E%A5/20200103144851-834770.png)

   4）填写用户信息

   ![img](https://course-public-resources-1252758970.cos.ap-chengdu.myqcloud.com/%E5%AE%9E%E6%88%98%E8%AF%BE/%E5%88%A9%E7%94%A8%E8%85%BE%E8%AE%AF%E4%BA%91COS%E4%BD%9C%E4%B8%BA%E5%9B%BE%E5%BA%8A%E4%B8%BAMarkdown%E6%96%87%E6%A1%A3%E6%8F%90%E4%BE%9B%E5%9B%BE%E7%89%87%E9%93%BE%E6%8E%A5/20200103144909-565999.png)

   5）设置用户权限，接下来的操作只涉及到对象存储COS的读写操作，此处也只需授予相关的权限。

   在策略列表中勾选 `QcloudCOSDataReadOnly`（对象存储COS数据只读的访问权限）和`QcloudCOSDataWriteOnly`（对象存储COS数据只写的访问权限）：

   ![img](https://course-public-resources-1252758970.cos.ap-chengdu.myqcloud.com/%E5%AE%9E%E6%88%98%E8%AF%BE/%E5%88%A9%E7%94%A8%E8%85%BE%E8%AE%AF%E4%BA%91COS%E4%BD%9C%E4%B8%BA%E5%9B%BE%E5%BA%8A%E4%B8%BAMarkdown%E6%96%87%E6%A1%A3%E6%8F%90%E4%BE%9B%E5%9B%BE%E7%89%87%E9%93%BE%E6%8E%A5/20200103145001-173781.png)

   最后一步，审阅用户权限，确认前面配置的所有信息无误后，点击 “完成” 按钮，即可完成子用户的创建。

2. 获取子用户密钥

   我个人觉得可以给子用户绑定邮箱，密钥通过邮箱的方式获取，也可在页面上查看。

   完成刚才的用户创建后，**不要跳出页面，在当前页面中便可以获取到用户密钥**（需要获取的信息为`SecretId`和`SecretKey`）：

   ![img](https://course-public-resources-1252758970.cos.ap-chengdu.myqcloud.com/%E5%AE%9E%E6%88%98%E8%AF%BE/%E5%88%A9%E7%94%A8%E8%85%BE%E8%AE%AF%E4%BA%91COS%E4%BD%9C%E4%B8%BA%E5%9B%BE%E5%BA%8A%E4%B8%BAMarkdown%E6%96%87%E6%A1%A3%E6%8F%90%E4%BE%9B%E5%9B%BE%E7%89%87%E9%93%BE%E6%8E%A5/20200103145029-796472.png)

# Typora插件实现图片自动上传

该插件项目地址：[Thobian/typora-plugins-win-img](https://github.com/Thobian/typora-plugins-win-img)

在git上下载插件到本地。

```bash
git clone https://github.com/Thobian/typora-plugins-win-img.git
```

1. 将插件中的`plugins`文件夹

   ![img](https://course-public-resources-1252758970.cos.ap-chengdu.myqcloud.com/%E5%AE%9E%E6%88%98%E8%AF%BE/%E5%88%A9%E7%94%A8%E8%85%BE%E8%AE%AF%E4%BA%91COS%E4%BD%9C%E4%B8%BA%E5%9B%BE%E5%BA%8A%E4%B8%BAMarkdown%E6%96%87%E6%A1%A3%E6%8F%90%E4%BE%9B%E5%9B%BE%E7%89%87%E9%93%BE%E6%8E%A5/20200103145220-454162.png)

   复制到你的Typora软件目录`/Typora/resources/app`下

   ![image-20200714094955892](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/14/094958-575370.png)

2. 修改`window.html`文件

   在文件中找到，下面那段代码

   ```html
   <script src="./app/window/frame.js" defer="defer"></script>
   ```

   在这段代码后面加上，

   ```html
   <script src="./plugins/image/upload.js" defer="defer"></script>
   ```

3. 进入`/Typora/resources/app/plugins/image`文件夹下，（也就是刚复制过去的`plugins`文件中），找到**upload.js**。

   ![image-20200714095510779](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/14/095511-45972.png)

4. 找到最后一行，会有个

   ```javascript
   $.image.init({})
   ```

   在里面添加参数信息。

   ```javascript
   $.image.init({
       // 只有下面这里需要设置，上面不需要进行设置
       target:'tencent',
       tencent : {
           // 必须参数，如果你有自己的腾讯云COS改成自己的配置
           // Bucket获取：对象存储->存储桶列表(存储桶名称就是Bucket)
           Bucket: '<此处填写你的存储桶名>',
           // SecretId获取：访问控制->用户->用户列表->用户详情->API密钥
           SecretId: '<此处填写你的SecretId>',  
           // SecretKey获取：访问控制->用户->用户列表->用户详情->API密钥
           SecretKey: '<此处填写你的SecretKey>',
           // Region获取：对象存储->存储桶列表(所属地域中的英文就是Region)
           Region: '<此处填写你的所属区域>',
           // Folder获取：对象存储->存储桶列表->存储桶文件夹
           Folder: '<此处填写你创建的文件夹名>',
       },
   });
   ```

   **注意，填好参数后，把注释删掉！！！**我是这么写的

   ```javascript
   $.image.init({
       target:'tencent',
       tencent : {
           Bucket: 'mdpic-xxxxxxx',
           SecretId: 'xxxxxxxxxxxxxxxxxxxxxxx',  
           SecretKey: 'xxxxxxxxxxxxxxxxxxxxxx',
           Region: 'ap-shanghai',
           Folder: 'night',
       },
   });
   ```

   需要的参数上面敲黑板提到过。

   

## 插件可能出现的问题

**问题1：注意，如果将Typora软件更新的话，该插件需要重新配置。**

 **问题2：关于插件可能出现重复上传图片的问题，Typora可以这样设置：**

![image-20200714100612426](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/14/100615-37227.png)

![image-20200714100824307](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/14/100826-14568.png)

**问题3：如果发现不能发现插件不能正常运行，可以打开Typora软件中的开发者工具调试一下，看看问题出在哪，或者去作者的gittub中的issue里看看别人有没有相同的问题。**

![image-20200714100357537](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/14/100358-63050.png)

# 参考链接

[利用腾讯云COS作为图床为Markdown文档提供图片链接 | 腾讯云大学](https://cloud.tencent.com/edu/learning/course-1825-20823)

[typora-plugins-win-img | github](https://github.com/Thobian/typora-plugins-win-img)

---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。