# Spark on K8s



#### 下载相对应的Spark 版本

[Spark2.4.7](https://github.com/apache/spark/releases/tag/v2.4.7 "Spark2.4.7" )



#### 创建Docker镜像

```shell
# 解压并 进入spark 目录下
tar -zcvf spark-2.4.7-bin-hadoop2.7.tar.gz spark-2.4.7-bin-hadoop2.7
cd spark-2.4.7-bin-hadoop2.7

# 创建Docker镜像 输出镜像地址 =  k8s库地址:镜像标签
./bin/docker-image-tool.sh -r k8s库地址 -t 镜像标签 build 

# 推送进行到k8s地址
./bin/docker-image-tool.sh -r k8s库地址 -t 镜像标签 push
# 参数
  -f file               要为基于JVM的作业生成的Dockerfile。默认情况下，生成Spark附带的Dockerfile。
  -p file               要为Pypark作业生成的Dockerfile。构建Python依赖关系并附带Spark。
  -R file               要为SparkR作业生成的Dockerfile。建立R依赖关系并与Spark一起发布。
  -r repo               存储库地址。
  -t tag                标记以应用于生成的图像，或标识要推送的图像。
  -m                    使用 minikube 的Docker守护程序。
  -n                    生成docker映像 --无缓存
  -b arg                添加参数 每个参数用-b分开


```



#### 执行Spark命令

```shell
./bin/spark-submit \
--master k8s://https://XXX.XXX.XXX.XXX:6443 \ # k8s地址
--deploy-mode cluster \                       # 集群模式
--name spark-pi \                             # 容器名称
--class org.apache.spark.examples.SparkPi \   # main
--conf spark.executor.instances=1 \           # 开启容器并行数
--conf spark.kubernetes.authenticate.driver.serviceAccountName=spark \
--conf spark.kubernetes.authenticate.submission.caCertFile=/opt/spark/cert/kubernetes.pem \ #k8s秘钥
--conf spark.kubernetes.container.image=镜像地址 \
local:///opt/spark/examples/jars/spark-examples_2.11-2.4.7.jar # 执行jar包


```



#### Spark 访问Hive 添加hive-site.xml

```shell
# 添加


```









