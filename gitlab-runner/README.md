
## 注册
执行`docker exec -it oss-gitlab-runner gitlab-runner register`: 

```
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
${INTERNAL_GIT_SERVICE}/ci,Ex: http://local-git/ci
Please enter the gitlab-ci token for this runner:
xxx(从 gitlab runner 页面里找)
Please enter the gitlab-ci description for this runner:
[oss-gitlab-runner]:oss-gitlab-runner-${ip}
Please enter the gitlab-ci tags for this runner (comma separated):
local-runner
Whether to run untagged builds [true/false]:
[false]: true
Whether to lock Runner to current project [true/false]:
[false]:
Registering runner... succeeded                     runner=g7Q3K_A1
Please enter the executor: parallels, virtualbox, shell, ssh, docker+machine, docker-ssh+machine, kubernetes, docker, docker-ssh:
shell
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!

```

## 自定义配置文件
指定CONFIG_FILE环境变量
配置项[参考官方](https://docs.gitlab.com/runner/configuration/advanced-configuration.html)


## 注意
因为有从容器内部访问外部 ```docker service``` 的需求，需要修改宿主机的```/var/run/docker.sock```文件访问权限.

```
sudo chmod a+rw /var/run/docker.sock
```

## 关于Environment

- GIT_SERVICE_TOKEN：访问git服务上私有的项目需要用到用户的认证/授权token，需要在启动镜像的时候export指定，获取token方法：
  
  1. 方法1: 登录git服务, 例如gitlab: 进入 Profile Settings->Account->Private Token,获取token
  2. 方法2: 命令行, 例如gitlab: `curl --request POST "${GIT_SERVICE}/api/v3/session?login={邮箱}&password={密码}"`

- 获取token后，在启动镜像前执行`export GIT_SERVICE_TOKEN=<your_git_service_token>`
