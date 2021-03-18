# Teste Técnico - vaga DevOps 

A presente documentação tem a finalidade de rigistrar os passos execeutados para a conclusão do desafio proposto pela equipe de recrutamento do Instituto Atlântico, onde o desafio foi assim descrito:

- Simple website/app using Docker;
- CI/CD (your choice);
- Provisioning the infrastructure you needed to accomplish this challenge, using your
preferred tool. (terraform, cloud formation or ansible);
- Deploy that website/app in this infrastructure;
- Presenting the monitoring/status of this application (tools of your choice);
- Write documentation of this website/app clearly and objectively;

## Explicando a infraestrutura utilizada

Para cumprir os passos solicitatos pelo desafio escolhi utilizar as seguintes ferramentas:
* Terraform: para provisionar a instancia EC2, VPC com sua sub-rede, Gateway, Security Group, entre outros.
* Ansible: para o gerenciamento de configuração da instância;
* [GitLab](https://gitlab.com/): utilizei a versão do site oficial como ferramenta para o CI/CD juntamente com o GitLab Runner para fazer o deploy direto na instancia EC2;
* Docker: para subir a aplicação WEB, no caso foi uma imágem de WORDPRESS;
* Monitoramento: stack de monitoramento com Prometheus, Grafana, CADVASOR e NODE EXPORT;

A figura abaixo representa a estrutura lógica da infra utilizada:

![Image](https://i.ibb.co/GvbCKpf/aws.png)



## Provisonando AWS com Terraform

No diretório **terraform_aws** estão os códigos em Terraform que utilizei para montar o cenário na AWS, me baseei no repósitório https://github.com/brokedba/terraform-examples para cosntuir o meu projeto. Com o código foi criada na AWS as seguintes configurações:

* Região: sa-east-1 
* Uma instância do tipo t2.medium
* VPC subnet 192.168.0.0/16
* Subnet:192.168.0.0/24
* ecurity group ports: 80, 443, 22, 9090 ...
* route53 zone: matheusmc.com.br

Na sequância, trexos dos códigos utilizados.

Iniciando instância - compute.tf

```terraform
resource "aws_instance" "terra_inst" {
    count         = var.instance_count
    ami                          = var.instance_ami_id["UBUNTU"]
    availability_zone            = data.aws_availability_zones.ad.names[0]
    #cpu_core_count               = 1
    #cpu_threads_per_core         = 1
    disable_api_termination      = false
    ebs_optimized                = false
    get_password_data            = false
    hibernation                  = false
    instance_type                = var.instance_type
   # private_ip                   = var.private_ip
    associate_public_ip_address  = var.map_public_ip_on_launch
    key_name                     = var.key_name
    #key_name = var.key_name
    monitoring                   = false
    secondary_private_ips        = []
    security_groups              = []
    source_dest_check            = true
    subnet_id                    = aws_subnet.terra_sub.id
    user_data                    = filebase64(var.user_data)
    #user_data = filebase64("${path.module}/example.sh") 
    # user_data                   = "${file(var.user_data)}"
    # user_data_base64            = var.user_data_base64
    
    }
```

Criação de VPC - vpc.tf

```terraform
resource "aws_vpc" "terra_vpc" {
    cidr_block                       = var.vpc_cidr
    tags                             = {
        "Name" = var.vpc_name
    }
}

```

Zona DNS - vpc.tf

```terraform
resource "aws_route53_zone" "primary" {
  name = "matheusmc.com.br"
}
```

Para executar a implantação do cógido do Terraform, utilizei os seguintes comandos:

```
**$ terraform init** #para inicializar um diretório de trabalho contendo arquivos de configuração do Terraform
$ terraform plan #para criar um plano de execução. 
$ terraform apply #para aplicar as mudanças 
$ terraform destroy -target aws_instance.terra_inst #para desfazer uma configuração específica
```

## Gerenciamento de Configuração com o Ansible

Utilizei o Ansible para gerenciar a configuração da instância que foi configurada 


Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)
