声明
本系统的主要用来统计实例的信息,提供面板查询实例的基础配置

以下为 【免责条款】

本仓库发布的项目中涉及的任何脚本，仅用于测试和学习研究，禁止用于商业用途，不能保证其合法性，准确性，完整性和有效性，请根据情况自行判断.

所有使用者在使用项目的任何部分时，需先遵守法律法规。对于一切使用不当所造成的后果，需自行承担。对任何脚本问题概不负责，包括但不限于由任何脚本错误导致的任何损失或损害.

如果任何单位或个人认为该项目可能涉嫌侵犯其权利，则应及时通知并提供身份证明，所有权证明，我们将在收到认证文件后删除相关文件.

任何以任何方式查看此项目的人或直接或间接使用该项目的任何脚本的使用者都应仔细阅读此声明。本人保留随时更改或补充此免责声明的权利。一旦使用并复制了任何相关脚本或本项目的规则，则视为您已接受此免责声明.

您必须在下载后的24小时内从计算机或手机中完全删除以上内容.

您使用或者复制了本仓库且本人制作的任何脚本，则视为已接受此声明，请仔细阅读

        

一:环境说明: 需要提前安装jdk8+版本

    2.1: Debian/Ubuntu
    
        sudo apt update
        sudo apt install default-jdk
        
    2.2: CentOS/RHEL (Red Hat Enterprise Linux)

        CentOS 7:
            sudo yum install java-1.8.0-openjdk-devel
            
        CentOS 8 及之后版本（使用 dnf）：
            sudo dnf install java-11-openjdk-devel

    

三:部署说明:
   一: 脚本部署

      3.1:登录linux服务器,切换到root用户下.
    
      3.2:下载部署包文件
      
        3.2.1:下载运行脚本
    
          wget -N --no-check-certificate "https://github.com/doubleDimple/monitor-start/releases/download/v-1.0.1/monitor-start.sh" && chmod +x monitor-start.sh
      
    二: docker部署

            # 0. 创建文件件
            mkdir monitor-start-docker

            # 1. 进入指定目录
            cd monitor-start-docker

            # 2. 创建数据和日志目录
            mkdir -p data logs

            # 3. 运行容器，使用绝对路径挂载
            docker stop monitor-start || true && docker run -d \
                --pull always \
                --name monitor-start \
                -p 9898:9898 \
                -v /root/monitor-start-docker/data:/monitor-start/data \
                -v /root/monitor-start-docker/logs:/monitor-start/logs \
                -e SERVER_PORT=9898 \
                -e DATA_PATH=/monitor-start/data \
                -e LOG_HOME=/monitor-start/logs \
                --rm \
                lovele/monitor-start:latest

            # 查看容器状态
            docker ps -a

            # 查看容器日志
            docker logs monitor-start

四:配置说明

    #端口自行指定(默认端口为9898)
    server:
      port: 9898


五:启动

  5.1:给monitor-start.sh 执行权限添加
  chmod +x monitor-start.sh

  ./monitor-start.sh update   # 自动更新（如果服务在运行会自动重启）
  
 ./monitor-start.sh start    # 启动服务
 
 ./monitor-start.sh stop     # 停止服务
 
 ./monitor-start.sh restart  # 重启服务
 
 ./monitor-start.sh status   # 查看服务状态

六:访问
    http://ip:port  访问应用,会提示注册
