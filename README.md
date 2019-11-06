
## SpringBoot + jib 构建镜像Demo
使用[jib]("https://github.com/GoogleContainerTools/jib")快速发布镜像到阿里云

#### 环境
- Java 8
- Ubuntu 19.10
- Docker 18.06.1-ce

#### 配置远程仓库
```xml
 <build>
        <plugins>
            <plugin>
                <groupId>com.google.cloud.tools</groupId>
                <artifactId>jib-maven-plugin</artifactId>
                <version>1.7.0</version>
                <configuration>
                    <from>
                        <!--base image -->
                        <image>openjdk:alpine</image>
                    </from>
                    <to>
                        <!--目标镜像registry地址,:前面是发布镜像的名称，:后面是发布的版本号 控制台 https://cr.console.aliyun.com/cn-hangzhou/instances/repositories-->
                        <image>registry.cn-hangzhou.aliyuncs.com/wangs/library:${project.version}</image>
                        <!-- 阿里云镜像账号 阿里云控制台->容器镜像服务->默认实例->访问凭证->设置固定密码 -->
                        <auth>
                            <username>xxx</username>
                            <password>xxx</password>
                        </auth>
                        <tags>
                            <tag>tag</tag>
                            <tag>latest</tag>
                        </tags>
                    </to>
                    <allowInsecureRegistries>true</allowInsecureRegistries>
                </configuration>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>build</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```
#### 构建并推送到远程仓库
```shell
mvn clean package
```
```shell
....
[INFO] Executing tasks:
[INFO] [===========================   ] 90.0% complete
[INFO] > launching layer pushers
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  21.849 s
[INFO] Finished at: 2019-11-01T16:26:59+08:00
[INFO] ------------------------------------------------------------------------

```
#### 拉取镜像
```shell
sudo docker pull registry.cn-hangzhou.aliyuncs.com/wangs/library
```

#### 测试
```shell
sudo docker run --name jib-demo -p 8080:8080 registry.cn-hangzhou.aliyuncs.com/wangs/library
```
