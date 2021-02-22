# Terraform

## 1. Terraform 安装

### 1.1 登录Terraform官网, 选择对应的安装包下载

```sh
Terraform程序包就是一个二进制文件, 无需编译等额外操作
这里演示在AWS Linux2 上安装和使用Terraform
```

https://www.terraform.io/downloads.html

```bash
wget https://releases.hashicorp.com/terraform/0.14.7/terraform_0.14.7_linux_amd64.zip
```

### 1.2 解压Terraform安装包到PATH路径

```bash
unzip terraform_0.14.7_linux_amd64.zip
mv terraform /usr/sbin
```

### 1.3 验证Terraform安装成功

```bash
[root@ip-172-31-44-104 ~]# terraform --version
Terraform v0.14.7	
```

### 1.4 将AWS-IAM用户秘钥添加到环境变量

```bash
vim /etc/profile.d/terraform_key.sh
export AWS_ACCESS_KEY_ID="KEY_ID"
export AWS_SECRET_ACCESS_KEY="ACCESS_KEY"
```

## 2. Terraform-部署单个服务器

### 2.1 创建项目目录

```bash
mkdir Terraform_Up_Running # Terraform总目录
cd Terraform_Up_Running
```

```bash
# 将项目目录初始化为git仓库, 后期可以将代码传到github

[root@ip-172-31-44-104 ~]# vim .gitignore

*.terraform
*.tfstate
*.tfstate.backup	
*.lock.hcl

[root@ip-172-31-44-104 Terraform_Up_Running]# git init
Initialized empty Git repository in /root/Terraform_Up_Running/.git/
[root@ip-172-31-44-104 Terraform_Up_Running]# git add .gitignore
[root@ip-172-31-44-104 Terraform_Up_Running]# git commit -m 'Add .gitignore file'
```

```bash
mkdir chapter2_ex1  # 本案例所在的目录
cd chapter2_ex1

[root@ip-172-31-44-104 Terraform_Up_Running]# git add chapter2_ex1/
[root@ip-172-31-44-104 Terraform_Up_Running]# git commit -m 'Add project chapter2_ex1'
```



### 2.2 配置要使用的运营商和需要部署的服务器信息

```bash
[root@ip-172-31-44-104 chapter2_ex1]# vim main.tf 

provider "aws" {
        region = "us-east-2"
}
resource "aws_instance" "example"{
        ami = "ami-0c55b159cbfafe1f0"
        instance_type = "t2.micro"
}
```

### 2.3 初始化项目

```bash
terraform init
```

### 2.4 执行预部署

```bash
terraform plan
```

```bash
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.example will be created
  + resource "aws_instance" "example" {
      + ami                          = "ami-0c55b159cbfafe1f0"
      + arn                          = (known after apply)
      + associate_public_ip_address  = (known after apply)
      + availability_zone            = (known after apply)
      + cpu_core_count               = (known after apply)
      + cpu_threads_per_core         = (known after apply)
      + get_password_data            = false
      + host_id                      = (known after apply)
      + id                           = (known after apply)
      + instance_state               = (known after apply)
      + instance_type                = "t2.micro"
      + ipv6_address_count           = (known after apply)
      + ipv6_addresses               = (known after apply)
      + key_name                     = (known after apply)
      + outpost_arn                  = (known after apply)
      + password_data                = (known after apply)
      + placement_group              = (known after apply)
      + primary_network_interface_id = (known after apply)
      + private_dns                  = (known after apply)
      + private_ip                   = (known after apply)
      + public_dns                   = (known after apply)
      + public_ip                    = (known after apply)
      + secondary_private_ips        = (known after apply)
      + security_groups              = (known after apply)
      + source_dest_check            = true
      + subnet_id                    = (known after apply)
      + tenancy                      = (known after apply)
      + vpc_security_group_ids       = (known after apply)

      + ebs_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + snapshot_id           = (known after apply)
          + tags                  = (known after apply)
          + throughput            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }

      + enclave_options {
          + enabled = (known after apply)
        }

      + ephemeral_block_device {
          + device_name  = (known after apply)
          + no_device    = (known after apply)
          + virtual_name = (known after apply)
        }

      + metadata_options {
          + http_endpoint               = (known after apply)
          + http_put_response_hop_limit = (known after apply)
          + http_tokens                 = (known after apply)
        }

      + network_interface {
          + delete_on_termination = (known after apply)
          + device_index          = (known after apply)
          + network_interface_id  = (known after apply)
        }

      + root_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + tags                  = (known after apply)
          + throughput            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.

```

### 2.5 部署实例

```bash
[root@ip-172-31-44-104 chapter2_ex1]# terraform apply

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.example will be created
  + resource "aws_instance" "example" {
      + ami                          = "ami-0c55b159cbfafe1f0"
      + arn                          = (known after apply)
      + associate_public_ip_address  = (known after apply)
      + availability_zone            = (known after apply)
      + cpu_core_count               = (known after apply)
      + cpu_threads_per_core         = (known after apply)
      + get_password_data            = false
      + host_id                      = (known after apply)
      + id                           = (known after apply)
      + instance_state               = (known after apply)
      + instance_type                = "t2.micro"
      + ipv6_address_count           = (known after apply)
      + ipv6_addresses               = (known after apply)
      + key_name                     = (known after apply)
      + outpost_arn                  = (known after apply)
      + password_data                = (known after apply)
      + placement_group              = (known after apply)
      + primary_network_interface_id = (known after apply)
      + private_dns                  = (known after apply)
      + private_ip                   = (known after apply)
      + public_dns                   = (known after apply)
      + public_ip                    = (known after apply)
      + secondary_private_ips        = (known after apply)
      + security_groups              = (known after apply)
      + source_dest_check            = true
      + subnet_id                    = (known after apply)
      + tenancy                      = (known after apply)
      + vpc_security_group_ids       = (known after apply)

      + ebs_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + snapshot_id           = (known after apply)
          + tags                  = (known after apply)
          + throughput            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }

      + enclave_options {
          + enabled = (known after apply)
        }

      + ephemeral_block_device {
          + device_name  = (known after apply)
          + no_device    = (known after apply)
          + virtual_name = (known after apply)
        }

      + metadata_options {
          + http_endpoint               = (known after apply)
          + http_put_response_hop_limit = (known after apply)
          + http_tokens                 = (known after apply)
        }

      + network_interface {
          + delete_on_termination = (known after apply)
          + device_index          = (known after apply)
          + network_interface_id  = (known after apply)
        }

      + root_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + tags                  = (known after apply)
          + throughput            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes  # 手动输入yes, 确认执行

aws_instance.example: Creating...
aws_instance.example: Still creating... [10s elapsed]
aws_instance.example: Still creating... [20s elapsed]
aws_instance.example: Creation complete after 28s [id=i-0e02af805861e558a] # 部署的实例ID

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.  # 部署成功

```

![image-20210223004942074](C:\Users\David\AppData\Roaming\Typora\typora-user-images\image-20210223004942074.png)

### 2.6 给部署的实例添加Name

```
# Terraform会跟踪已经为这组配置文件创建过的所有资源, 所以其知道该EC2实例已经存在(当运行apply命令时, Terraform显示输出Refreshing state ...),同时列出了当前部署的内容与Terraform代码中的内容之间的区别, 这是与过程式语言相比, 声明式语言的优势之一
过程式语言关注的是最后想实现的状态, 而非完成这个目标的具体步骤
```

```bash
[root@ip-172-31-44-104 chapter2_ex1]# vim main.tf 

provider "aws" {
        region = "us-east-2"
}
resource "aws_instance" "example"{
        ami = "ami-0c55b159cbfafe1f0"
        instance_type = "t2.micro"

        tags = { 
             Name = "terraform-example"     # 给实例添加tags标签, 起个别名
        }
}
```

```bash
[root@ip-172-31-44-104 chapter2_ex1]# terraform apply
aws_instance.example: Refreshing state... [id=i-0e02af805861e558a]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  # aws_instance.example will be updated in-place
  ~ resource "aws_instance" "example" {     # ~ 表示要被修改的配置项
        id                           = "i-0e02af805861e558a"
      ~ tags                         = {
          + "Name" = "terraform-example"    # + 表示要添加的配置项
        }
        # (26 unchanged attributes hidden)




        # (4 unchanged blocks hidden)
    }

Plan: 0 to add, 1 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes     # 手动输入yes确认

aws_instance.example: Modifying... [id=i-0e02af805861e558a]
aws_instance.example: Modifications complete after 5s [id=i-0e02af805861e558a]

Apply complete! Resources: 0 added, 1 changed, 0 destroyed.
```

![image-20210223005831206](C:\Users\David\AppData\Roaming\Typora\typora-user-images\image-20210223005831206.png)

### 2.7 将Terraform代码上传到Github

```bash
[root@ip-172-31-44-104 Terraform_Up_Running]# git add .
[root@ip-172-31-44-104 Terraform_Up_Running]# git commit -m 'Upload chapter2_ex1'
```

```bash
[root@ip-172-31-44-104 Terraform_Up_Running]# git remote add origin git@github.com:ChickenWinner2019/Terraform_Up_Running.git
[root@ip-172-31-44-104 Terraform_Up_Running]# git branch -M main
[root@ip-172-31-44-104 Terraform_Up_Running]# git push -u origin main
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (7/7), 703 bytes | 703.00 KiB/s, done.
Total 7 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To github.com:ChickenWinner2019/Terraform_Up_Running.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

