# Teste Técnico - vaga DevOps 

A presente documentação tem a finalidade de rigistrar os passos execeutados para a conclusão do desafio proposto pela equipe de recrutamento do Instituto Atlântico, que foi assim proposto:

- Simple website/app using Docker;
- CI/CD (your choice);
- Provisioning the infrastructure you needed to accomplish this challenge, using your
preferred tool. (terraform, cloud formation or ansible);
- Deploy that website/app in this infrastructure;
- Presenting the monitoring/status of this application (tools of your choice);
- Write documentation of this website/app clearly and objectively;

## Provisionamento da infraestrutura

Optei para fazer o deploy dos serviços numa instancia (t2.medium) na insfraestrutura da AWS utilizando o Terraform para o provisionamento da infra na AWS e o Ansible para o gerenciamento de configuração desse servidor. Explicarei com mais detalhes nas próximas sessões.
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
