# Meu projeto de prática: AWS Cloud Practitioner Essentials

Repositório com os projetos práticos que estou fazendo durante o curso **AWS Cloud Practitioner Essentials**.

## Sobre mim

Estudante de Ciência da Computação (Mackenzie), buscando migrar para a área de tecnologia e programação.

## Projetos

- ✅ Feito **Projeto 1: Site estático** (S3) - [ver online](http://murilo-site-aws-2026.s3-website-us-east-1.amazonaws.com)
- ✅ Feito **Projeto 2: Banco NoSQL** (DynamoDB) - consultas Query x Scan
- 🔜 **Projeto 3: Rede na nuvem** (VPC, EC2, Security Groups) - a fazer
  
## Como está organizado

Cada projeto terá uma pasta com o código e um diagrama da arquitetura.

## Aprendizados

### Projeto 1: Site estático no Amazon S3

**O que fiz:** depois de alguns módulos do curso, configurei um alerta de gasto (AWS Budgets) e hospedei uma página HTML em um bucket do S3, com o código versionado aqui no GitHub.

**O que aprendi na prática**

- **O S3 vem fechado por padrão.** Então, para o site abrir, precisei desligar o *Block Public Access* do bucket e também escrever uma *bucket policy*. Foram duas camadas de proteção que tive de abrir de propósito.

- **Menor privilégio.** Na policy usei `Effect: Allow`, `Principal: "*"`, `Action: s3:GetObject` e `Resource: arn:aws:s3:::meu-bucket/*`. Ou seja, qualquer pessoa pode *ler* os arquivos, mas ninguém pode alterar ou apagar. Também descobri que o `/*` no final do ARN é o que faz a regra valer para os arquivos dentro do bucket.

- **Dois endereços diferentes.** O link do objeto e o endpoint do *Static website hosting* não são a mesma coisa. Só o segundo trata o `index.html` como página inicial, e ele funciona apenas em HTTP (por isso o navegador mostra "Inseguro"). Para ter HTTPS, o próximo passo é usar o CloudFront.

- **Região importa.** O console mudou sozinho para N. Virginia quando entrei nas páginas de Billing, e recursos ficam presos à região onde foram criados. Passei a conferir a região antes de criar qualquer coisa.

- **Responsabilidade compartilhada.** A AWS cuida da infraestrutura, mas decidir o que fica público é responsabilidade minha. Isso era só um conceito no curso, e aqui eu vivi na prática.

- **Segurança desde o início.** Ativei MFA, criei um alerta de gasto e um `.gitignore` para nunca subir chaves ou arquivos `.pem` por engano.

**Um erro que me ensinou algo:** no meu primeiro README usei `/` e `//` no lugar de `#` para os títulos, e a tabela quebrou. Aprendi como o Markdown funciona e que vale conferir o *Preview* antes de salvar.

**Como estudei:** fui seguindo o curso do Skill Builder e praticando em paralelo. Usei IA como guia para organizar os passos, mas executei cada etapa por conta própria e fui perguntando o porquê das coisas, como a versão `2012-10-17` da policy.
<img width="1363" height="694" alt="image" src="https://github.com/user-attachments/assets/c277e4bd-aba2-47aa-bdcd-f5daa0da2709" />

#### Versionamento no S3

- O versionamento guarda o histórico por nome de arquivo. Os arquivos que já existiam antes de ligar o recurso ficam com o ID `null`.
- Apagar uma versão específica é permanente. Apagar o objeto sem escolher versão só cria um *delete marker*, e dá para recuperar.
- Criei uma regra que apaga versões antigas depois de 30 dias.
- Backup em outro lugar: como o `index.html` original também está no GitHub, eu sabia que não perderia nada se errasse.

<img width="1360" height="347" alt="s3-versoes-index" src="https://github.com/user-attachments/assets/5fb72bbc-f854-4a2e-97dd-d527423cd2cd" />

### Projeto 2: Banco NoSQL com Amazon DynamoDB

**O que fiz:** criei uma tabela `Pedidos` com Partition key `Cliente` e Sort key `DataPedido`, cadastrei 4 pedidos pelo console e comparei consultas Query e Scan.

<img width="1344" height="610" alt="dynamodb-scan-4-itens" src="https://github.com/user-attachments/assets/14d4ad5d-367e-43e2-b1d9-7d2a0e45ae4d" />

**O que aprendi na prática**

- **Partition key e sort key.** A partition key agrupa os itens (todos os pedidos de uma cliente) e a sort key ordena dentro do grupo e permite buscar por faixa (`Between`, `Begins with`).
- **Query lê só o necessário.** Ao consultar os pedidos da Ana, o DynamoDB leu 3 itens e devolveu 3 (100% de eficiência).
<img width="1155" height="622" alt="dynamodb-query-ana-descendente" src="https://github.com/user-attachments/assets/57527e09-b6e6-4725-a621-671346a1b096" />
- **Scan com filtro lê tudo.** Para achar pedidos com valor acima de 100, ele leu os 4 itens da tabela para devolver 2 (50% de eficiência). Em uma tabela grande isso seria lento e caro.
<img width="1104" height="610" alt="dynamodb-scan-filtro" src="https://github.com/user-attachments/assets/8169eb28-7a5f-4c87-a1d6-3d156e4dfa1d" />
- **Sem esquema fixo.** Só um dos pedidos tinha o atributo `Observacao`, e os outros itens funcionaram normalmente sem alterar a tabela, diferente de uma tabela SQL.
- **Custo.** Usei o modo on-demand, que não cobra quando a tabela está parada, e acompanho o gasto pelo AWS Budgets. A tabela pode ser apagada a qualquer momento para zerar o recurso.

## Para Contato
[LinkedIn](https://www.linkedin.com/in/murilo-fidelis-55986b306/)
