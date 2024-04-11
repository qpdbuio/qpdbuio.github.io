+++
title = 'Jenkins Pipeline 构建spring boot工程'
+++

### 说明
这是一个spring boot项目, 通过Jenkins实现自动打包自动部署的构建过程.
- 准备参数化构建
- 首先checkout获取git仓库代码
- 使用maven编译打包工程project_name.tar.gz
- 通过ssh上传project_name.tar.gz和docker-compose.yaml
- 远程ssh使用docker-compose up启动docker

### Jenkins 构建参数

{{< table >}}
| 参数名称         | 默认值               | 描述                   |
| ---------------- | -------------------- | ---------------------- |
| ServerAPI        | 127.0.0.1            | API服务地址            |
| PortAPI          | 8181                 | API服务端口            |
| Server           | 127.0.0.1            | 前端IP地址             |
| Port             | 8080                 | 前端端口               |
{{< /table >}}

### Jenkins 脚本
{{< code >}}
pipeline{
    agent any
    environment  {
        server = "project_name-$Server"
    }
    stages {
        stage('checkout') {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/${branch_name}']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CleanBeforeCheckout'],[$class: 'RelativeTargetDirectory', relativeTargetDir: 'project_name']], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'cfbd1efc-e3de-46f1-926c-5ca9a86d00a8', url: 'https://github.com/project_name.git']]])
            }
        }
        stage('install') {
            steps{
                sh "node -v"
                sh "npm -v"
                dir("project_name") {
                    sh "pwd"
                    sh """sed -i "s/^mq.autoprice.queue.*/#/g" application-dev1.properties"""
                    sh """echo >> application-dev1.properties"""
                    sh """echo mq.message.queue\\=mq.queue.message.${Version} >> application-dev1.properties"""
                }
                dir("project_name/dist/") {
                    sh "tar -zcvf project_name.tar.gz *"
                }
            }
        }
        stage('build') {
            steps{
                sh "mvn -f ./project/pom.xml -s /opt/maven/conf/settings.xml -gs /opt/maven/conf/settings.xml clean install -P dev1"
            }
        }
        stage('push') {
            steps{
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: "$server", 
                            transfers: [
                                sshTransfer(cleanRemote: true, excludes: '', execCommand: '''
mkdir /opt/war/${JOB_NAME}/bin/
mkdir /opt/war/${JOB_NAME}/logs/
cat > /opt/war/${JOB_NAME}/docker-compose.yaml << EOF
version: "3"
services: 
  tomcat:
    image: "tomcat:8.5.40-jre8-alpine"
    container_name: "tomcat_${JOB_NAME}"
    restart: "always"
    environment:
      - TZ=Asia/Shanghai
      - DUBBO_IP_TO_REGISTRY=${Server}
      - DUBBO_PORT_TO_REGISTRY=${Port}
      - DUBBO_PORT_TO_BIND=${Port}
    command: 
      - /bin/bash
      - -c
      - |
        sh bin/catalina.sh run
    ports: 
      - "${Port}:8080"
    volumes: 
      - "/etc/timezone:/etc/timezone:ro"
      - "/etc/localtime:/etc/localtime:ro"
      - "/usr/share/fonts/:/usr/share/fonts/"
      - "/opt/war/${JOB_NAME}/bin/setenv.sh:/usr/local/tomcat/bin/setenv.sh:ro"
      - "/opt/war/${JOB_NAME}/logs/:/usr/local/logs"
      - "/opt/war/${JOB_NAME}/target/:/usr/local/tomcat/webapps"
EOF
cat > /opt/war/${JOB_NAME}/bin/setenv.sh << EOF
JAVA_OPTS="-server -Xms500m -Xmx500m -Xmn200m"
EOF
mv /opt/war/${JOB_NAME}/target/project_name-*.war /opt/war/${JOB_NAME}/target/project_name.war
docker-compose -f /opt/war/${JOB_NAME}/docker-compose.yaml down
docker-compose -f /opt/war/${JOB_NAME}/docker-compose.yaml up -d
''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/${JOB_NAME}/www/', remoteDirectorySDF: false, removePrefix: 'project_name/dist/', sourceFiles: 'project_name/dist/project_name.tar.gz')
                            ], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false
                        )
                    ]
                )
            }
        }
    }
}
{{< /code >}}