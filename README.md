# go-Automatic-deployment
golang自动部署脚本

```

host="ip"
pem="id_rsa" 
user="user"

esac
echo $host
# 初始化swagger
swag init


projectpath="/project/"       #远程目录
projectname="project_name"
# 远程环境
export CGO_ENABLED=0
export GOOS=linux
export GOARCH=amd64
export GO111MODULE=on


echo "********** start build"
go build -o $projectname main.go
echo "********** start upload go-file"

scp -i $pem $projectname $user@$host:$projectpath/rebuild

echo "scp done!\n"
rm -rf $projectname

ssh -i $pem  $user@$host "cd $projectpath; mv rebuild $projectname;chmod 755 $projectname; sudo lsof -i :11818 | awk 'NR>1 {print \$2}' |xargs sudo kill -9;sleep 1;nohup ./$projectname > runoob.log 2>&1 &"

echo "success $host"
#改回本机环境变量go
export CGO_ENABLED=0
export GOOS=darwin
export GOARCH=amd64
```
