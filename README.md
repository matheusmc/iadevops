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

De forma resumida, o desenvolverdor faz a o CI/CD na versão web do [GitLab](https://gitlab.com/) que através da configuração do Gitlab Runner executa um container fazendo a implantação da aplicação na máquina de destino. A aplicação desse teste está rodando na porta 80 e pode se acessada através do link: http://site.matheusmc.com.br. Além da aplícação de teste, nessa máquina estão rodando as aplicações de monitoramento ([prometheus](http://18.229.156.110:9090/), [grafana](http://18.229.156.110:3000/) user: admin - pass: giropops, [cadvasor](http://18.229.156.110:8080/), [node exporte](http://18.229.156.110:9100/)). A figura abaixo representa a estrutura lógica da infra utilizada:

![Image](https://i.ibb.co/GvbCKpf/aws.png)



## Provisonando AWS com Terraform

No diretório **terraform_aws** estão os códigos em Terraform que utilizei para montar o cenário na AWS, me baseei no repósitório https://github.com/brokedba/terraform-examples para cosntuir o meu projeto. Com o código foi criada na AWS as seguintes configurações:

* Região: sa-east-1 
* Uma instância do tipo t2.medium
* VPC subnet 192.168.0.0/16
* Subnet:192.168.0.0/24
* Security group ports: 80, 443, 22, 9090 ...
* route53 zone: matheusmc.com.br

Na sequância, trexos dos códigos utilizados.

* Iniciando instância - compute.tf

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

* Criação de VPC - vpc.tf

```terraform
resource "aws_vpc" "terra_vpc" {
    cidr_block                       = var.vpc_cidr
    tags                             = {
        "Name" = var.vpc_name
    }
}

```

* Zona DNS - vpc.tf

```terraform
resource "aws_route53_zone" "primary" {
  name = "matheusmc.com.br"
}
```

* Para executar a implantação do cógido do Terraform, utilizei os seguintes comandos:

```
$ terraform init #para inicializar um diretório de trabalho contendo arquivos de configuração do Terraform
$ terraform plan #para criar um plano de execução. 
$ terraform apply #para aplicar as mudanças 
$ terraform destroy -target aws_instance.terra_inst #para desfazer uma configuração específica
```

## Gerenciamento de Configuração com o Ansible

No diretório **ansible** estão os códigos que utilizei para configurar a instância EC2, basicamente foi realizada a instalação da engine Docker (docker-compose) e para registrar a instância EC2 no GitLab Runner, segue trechos dos códigos dos scripts Ansible:

* arquivo **hosts**

```ansible
#endereço da instância na AWS
[app]
18.229.156.110 ansible_user=ubuntu ansible_ssh_private_key_file="devops.pem"
}
```
* arquivo **main.yml** - para chamar os outros playbooks em sequência:

```ansible
#arquivo para acionar playbooks em sequência.
#Autor: Matheus Medeiros
- name: Install_Docker
  import_playbook: provi_docker.yml

- name: Register_GitLab_runner_app
  import_playbook: gitlab-runner-app.yml
```

* no arquivo **app.yml** é o local onde cofiguro o tokem de registro do GitLab Runner:

```ansible
#fonte: https://github.com/riemers/ansible-gitlab-runner
gitlab_runner_coordinator_url: https://gitlab.com
gitlab_runner_registration_token: 'yourToken'
gitlab_runner_runners:
```

* O Arquivo **provi_docker.yml** é o responsável pela instalação da engine docker e docker compose.

* Para executar a implantação do cógido do Ansible, utilizei o seguinte comando:

```
$ ansible-playbook  main.yml -i hosts
```

## Configuração de CI/CD no GitLab


![Image](https://i.ibb.co/9hDbHhB/deploy.png)
![Image](https://i.ibb.co/6bzRjLG/pipelines.png)
![Image](https://i.ibb.co/6rjccXc/projeto.png)


Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)





























