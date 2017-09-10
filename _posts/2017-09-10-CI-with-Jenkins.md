---
layout: post
title: 基于Jenkins的持续集成
date: 2017-09-10 12:17:00 +0800
categories:
- Tech
tags:
- Jenkins
- CI

---

## 基础环境

- CentOS 7
	- CPU: 1 Core
	- Memory: 1GB & 1GB Swap
- Docker 17.06.1-ce
	- Docker Image: [jenkinsci/blueocean](https://hub.docker.com/r/jenkinsci/blueocean/)

## 初始化Jenkins

- 安装Docker
	- [Get Docker CE for CentOS](https://docs.docker.com/engine/installation/linux/docker-ce/centos/)
- 下载***jenkinsci/blueocean***镜像
	- `sudo docker pull jenkinsci/blueocean`
- 启动Jenkins服务
	- `docker run -d -p 8080:8080 jenkinsci/blueocean`
- 通过<your IP>:8080进行访问，完成后续的初始化操作

## 基于Jenkins的持续集成

> "Blue Ocean puts Continuous Delivery in reach of any team without sacrificing the power and sophistication of Jenkins." - by *Jenkins*

持续集成的步骤：**Build -> Test -> Deploy**。

使用[Blue Ocean](https://jenkins.io/projects/blueocean/)实现，集成GitHub。

**示例项目**：[devops](https://github.com/xianyuxmu/devops)

- 请参考示例项目，在项目根目录下增加`Jenkinsfile`文件，并进行配置：
	- [Creating your first Pipeline](https://jenkins.io/doc/pipeline/tour/hello-world/)
- 新建`pipeline`:
	- [Creating Pipelines](https://jenkins.io/doc/book/blueocean/creating-pipelines/)

`Jenkinsfile`参考配置文件：

```
pipeline {
  agent {
    node {
      label 'master'
    }
    
  }
  stages {
    stage('Build') {
      steps {
        sh 'echo \'Bonjour monde! This is a Build\''
      }
    }
    stage('Tests') {
      steps {
        sh 'echo \'Testing Code Here\''
      }
    }
    stage('Deploy') {
      steps {
        sh 'echo \'This is a Deploy\''
      }
    }
  }
}
```

## 资源消耗

Jenkins在基本没使用的情况，内存消耗高达**48.9%**。

``` bash
  CPU[1.3%]
  Mem[808M/991M]
  Swp[872M/1024M]

  PID USER      PRI  NI  VIRT   RES   SHR S CPU% MEM%   TIME+  Command
  346 username   20   0 1956M  484M  1220 S  0.0 48.9  0:03.67 java -jar /usr/share/jenkins/jenkins.war
  347 username   20   0 1956M  484M  1220 S  0.0 48.9  0:15.71 java -jar /usr/share/jenkins/jenkins.war

```

## 注意点

使用Docker来安装Jinkins后不能使用`Docker Plugin`插件，因为Jenkins运行在容器内找不到Docker daemon。

也就是说，当你使用Docker运行Jenkins的时候，下面的配置会因为找不到`Docker daemon`而报错:

```
pipeline {
    agent { docker 'node:6.3' }
    stages {
        stage('build') {
            steps {
                sh 'npm --version'
            }
        }
    }
}
```

报错内容：

```
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Declarative: Agent Setup)
[Pipeline] sh
[xianyuxmu_devops_master-HVO6V4OAJNQ3CC3T6UDHMRW3QWE7B72GIPKMH4DMGX4JDV55UFAQ] Running shell script
+ docker pull node:6.3
Warning: failed to get default registry endpoint from daemon (Cannot connect to the Docker daemon. Is the docker daemon running on this host?). Using system default: https://index.docker.io/v1/
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
```

## 持续集成工具比较

| CI工具    | 简介                                                                                                           | When use it?                                                                          |
|-----------|----------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| CircleCI  | 就两个环境Ubuntu、MacOS，每月免费提供15000分钟无限用户、无限项目的单容器、单并发构建，第二个容器起每个50美金。 | 小公司、初创公司可以使用。                                                            |
| Travis CI | 功能强大、开源项目免费使用、私有项目收费是每月69美金。                                                         | 适合开源项目，开源项目永久免费。                                                      |
| Jenkins   | 开源免费，但是需要单独的服务器(配置双核、2G内存可搞定)，同时还有开发和维护成本。                               | 适合处于有一定经验的、需要降低CI成本的公司。初创公司同样适用，因为搭建Jenkins也不难。 |



## 相关文章

- [Getting Started with Blue Ocean](https://jenkins.io/doc/book/blueocean/getting-started/) - 安装教程、添加GitHub项目教程
- [Say Hello to the Blue Ocean Pipeline Editor](https://jenkins.io/blog/2017/02/15/pipeline-editor-preview/) - 使用Blue Ocean编辑`Jenkinsfile`
- [enkinsci/pipeline-examples](https://github.com/jenkinsci/pipeline-examples) - `pipeline示例`
- [Continuous Integration. CircleCI vs Travis CI vs Jenkins](https://hackernoon.com/continuous-integration-circleci-vs-travis-ci-vs-jenkins-41a1c2bd95f5)

----

Robin on 10 Sep. 2017 at Wangjing CBD