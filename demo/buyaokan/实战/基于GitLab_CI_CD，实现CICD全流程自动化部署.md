![]()

Linux服务器安装：  
•Linux安装文档：https://docs.gitlab.com/runner/install/linux-repository.html  
curl-L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | sudo  
bash  
yum install gitlab-runner  
Docker服务器安装：  
•docker安装文档：https://docs.gitlab.com/runner/install/docker.html  
docker run-d--name gitlab-runner--restart always\  
-v /srv/gitlab-runner/config:/etc/gitlab-runner\  
-v /var/run/docker.sock:/var/run/docker.sock\  
gitlab/gitlab-runner:latest

