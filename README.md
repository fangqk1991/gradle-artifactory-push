# gradle-artifactory-push
这是一个针对 Artifactory 模块发布的脚本，示例工程 [artifactory-example](https://github.com/fangqk1991/artifactory-example)

## 使用
#### 1. 下载安装 [Artifactory](https://bintray.com/jfrog/artifactory/jfrog-artifactory-oss-zip/)

```
mv artifactory-oss-5.10.3/ /usr/local/

## 建立启动文件
echo 'cd /usr/local/artifactory-oss-5.10.3/bin/; ./artifactory.sh $1' > /usr/local/bin/artifactory
chmod +x /usr/local/bin/artifactory

## 启动服务
artifactory start
```

访问 `http://localhost:8081`，根据指引进行 admin 密码设置，Create Gradle Repository。

#### 2. 编辑工程根目录 `gradle.properties`
加入以下变量（请根据实际配置赋值）

```
arti_maven_path=http://localhost:8081/artifactory
arti_repo_key=gradle-release-local
arti_username=admin
arti_password=123456
```

#### 3. 编辑根目录 `build.gradle`
加入相关依赖

```
buildscript {
    dependencies {
        ...
        ///////////////// 添加 /////////////////
        classpath "org.jfrog.buildinfo:build-info-extractor-gradle:4.7.1"
        ///////////////// /// /////////////////
    }
}
```

#### 4. 编辑每个子模块工程 `build.gradle`
```
...
...

///////////////// 添加 /////////////////
ext {
    arti_group_id = 'com.example'
    arti_id = 'MODULE_NAME'
    arti_version = android.defaultConfig.versionName
}

apply from: 'https://raw.githubusercontent.com/fangqk1991/gradle-artifactory-push/master/my-arti.gradle'
///////////////// /// /////////////////
```

#### 5. 发布
```
./gradlew assembleRelease
./gradlew artifactoryPublish
```