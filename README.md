# Teste Técnico - vaga DevOps 

A presente documentação tem a finalidade de rigistrar os passos execeutados para a conclusão do desafio proposto pela equipe de recrutamento do Instituto Atlântico, que foi assim proposto:

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
* Docker: para subier a aplicação WEB, no caso foi uma imágem de WORDPRESS;
* Monitoramento: stack de monitoramento com Prometheus, Grafana, CADVASOR e NODE EXPORT;

![Image](https://i.ibb.co/GvbCKpf/aws.png)


```bash
pip install foobar
```

## Usage

```python
import foobar

foobar.pluralize('word') # returns 'words'
foobar.pluralize('goose') # returns 'geese'
foobar.singularize('phenomena') # returns 'phenomenon'
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)
