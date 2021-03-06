我们前面说了很多命令式的操作文章，并且也简单的了解了下DockerFile文件，并且也尝试的去写一个适合我们的文件，自己也去操作。但是会感觉还有很多自己没有学习到的内容。其中就有我们今天所说的容器与容器之间的交互应该怎么做。这就是我们想使用的互联机制
# 互联机制
互联机制是一个让多个容器之间进行交互的方式。他会使其他容器快速的根据名字链接到我们的源容器中，并且不需要指定IP地址。
既然我们说到容器名字，那么我们首先要学会的是建立容器的名字。
## 自定义容器命名
自定义容器名字，固然就是我们在使用容器的时候给命名好，每个容器都应该有自己的专属名字，这样才能正确的区分哪个容器是哪个。在这里我们使用的是--name 参数来命名的。
```
docker run -d -P --name  test-mysql  mysql:5.7
```
我们在这里把要创建的容器命名为test-mysql.这就是使用的名字。当然名字具有唯一性，后面在创建同样名字的容器就会出现错误，除非我们使用docker rm 删除该容器。
##  容器互联
我们在使用容器互联 ，在这里需要使用一个新的连接参数 --link ,该参数可以让容器之间安全的进行交互。
1. 我们首先创建一个容器redis
```
docker run -d --name test-redis   -p 6379:6379 redis 
```
2. 创建mongodb的容器，将其连接到我们的redis容器当中
```
docker run -d --name  test-mongodb  --link test-redis:redis mongo 
```
3. 建立连接后我们可以去容器中/etc/hosts 文件中查看是否增加上了
![容器命令](https://upload-images.jianshu.io/upload_images/4237685-120d682e1fc89b9c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在图片中我们可以十分的看到增加了redis 的链接代表我们已经互联上了
4. -- link  test-redis：redis   格式<name or id>:alias  其中 name是容器的名字或者使用id来操作也是可以的 alias代表源容器在link下的别名。

## 总结
这就是我们今天所要说的网络互联机制，--link 帮我们让容器之间可以十分愉快的进行网络之间传输。当然我们也可以使用端口映射的方式来进行网络的链接，但是暴露端口可能会有意外的危险。那么我们可以使用--link来进行网络的链接。后面我们会介绍一些其他的手段进行网络操作。例如网络的功能的虚拟化操作等。
