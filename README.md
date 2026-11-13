# ☁️ Meu projeto de prática: AWS Cloud Practitioner Essentials

Repositório com os projetos práticos que estou fazendo durante o curso **AWS Cloud Practitioner Essentials** (AWS Skill Builder). Cada projeto aplica na console o que o curso explica na teoria.

> **Status:** curso em andamento. Não tenho a certificação AWS Certified Cloud Practitioner. Este repositório mostra o que aprendi na prática.

## 👋 Sobre mim

Estudante de Ciência da Computação (Mackenzie), buscando migrar para a área de tecnologia e programação.

## 🗂️ Projetos

- ✅ **Projeto 1: Site estático** (Amazon S3) · [ver online](http://murilo-site-aws-2026.s3-website-us-east-1.amazonaws.com)
- ✅ **Projeto 2: Banco NoSQL** (Amazon DynamoDB) · consultas Query x Scan
- ✅ **Projeto 3: Rede na nuvem** (VPC, EC2, Security Groups) · subnet pública e privada, Internet Gateway e servidor web

Os projetos foram feitos pelo console da AWS; o código do site está na pasta `site/`.

---

## 📚 Aprendizados

### Projeto 1: Site estático no Amazon S3

**O que fiz:** depois de alguns módulos do curso, configurei um alerta de gasto (AWS Budgets) e hospedei uma página HTML em um bucket do S3, com o código versionado aqui no GitHub.

<p align="center">
  <img src="https://github.com/user-attachments/assets/c277e4bd-aba2-47aa-bdcd-f5daa0da2709" width="650" alt="Site Olá, nuvem! rodando no endpoint do S3">
</p>

**O que aprendi na prática**

- **O S3 vem fechado por padrão.** Para o site abrir, precisei desligar o *Block Public Access* do bucket e também escrever uma *bucket policy*. Foram duas camadas de proteção que tive de abrir de propósito.
- **Menor privilégio.** Na policy usei `Effect: Allow`, `Principal: "*"`, `Action: s3:GetObject` e `Resource: arn:aws:s3:::meu-bucket/*`. Ou seja, qualquer pessoa pode *ler* os arquivos, mas ninguém pode alterar ou apagar. O `/*` no final do ARN é o que faz a regra valer para os arquivos dentro do bucket.
- **Dois endereços diferentes.** O link do objeto e o endpoint do *Static website hosting* não são a mesma coisa. Só o segundo trata o `index.html` como página inicial, e ele funciona apenas em HTTP (por isso o navegador mostra "Inseguro"). Para ter HTTPS, o próximo passo é usar o CloudFront.
- **Região importa.** O console mudou sozinho para N. Virginia quando entrei nas páginas de Billing, e recursos ficam presos à região onde foram criados. Passei a conferir a região antes de criar qualquer coisa.
- **Responsabilidade compartilhada.** A AWS cuida da infraestrutura, mas decidir o que fica público é responsabilidade minha. No curso era só um conceito, aqui eu vivi na prática.
- **Segurança desde o início.** Ativei MFA, criei um alerta de gasto e um `.gitignore` para nunca subir chaves ou arquivos `.pem` por engano.

**Um erro que me ensinou algo:** no meu primeiro README usei `/` e `//` no lugar de `#` para os títulos, e a tabela quebrou. Aprendi como o Markdown funciona e que vale conferir o *Preview* antes de salvar.

**Como estudei:** fui seguindo o curso do Skill Builder e praticando em paralelo. Usei IA como guia para organizar os passos, mas executei cada etapa por conta própria e fui perguntando o porquê das coisas, como a versão `2012-10-17` da policy.

#### Versionamento no S3

- O versionamento guarda o histórico por nome de arquivo. Os arquivos que já existiam antes de ligar o recurso ficam com o ID `null`.
- Apagar uma versão específica é permanente. Apagar o objeto sem escolher versão só cria um *delete marker*, e dá para recuperar.
- Criei uma regra de ciclo de vida que apaga versões antigas depois de 30 dias, para não acumular custo.
- Backup em outro lugar: como o `index.html` original também está no GitHub, eu sabia que não perderia nada se errasse.

<p align="center">
  <img src="https://github.com/user-attachments/assets/5fb72bbc-f854-4a2e-97dd-d527423cd2cd" width="650" alt="Lista de objetos do S3 mostrando duas versões do index.html">
</p>

---

### Projeto 2: Banco NoSQL com Amazon DynamoDB

**O que fiz:** criei uma tabela `Pedidos` com Partition key `Cliente` e Sort key `DataPedido`, cadastrei 4 pedidos pelo console e comparei consultas Query e Scan.

<p align="center">
  <img src="https://github.com/user-attachments/assets/14d4ad5d-367e-43e2-b1d9-7d2a0e45ae4d" width="650" alt="Tabela Pedidos no DynamoDB com 4 itens">
</p>

**O que aprendi na prática**

- **Partition key e sort key.** A partition key agrupa os itens (todos os pedidos de uma cliente) e a sort key ordena dentro do grupo e permite buscar por faixa (`Between`, `Begins with`).
- **Query lê só o necessário.** Ao consultar os pedidos da Ana, o DynamoDB leu 3 itens e devolveu 3 (100% de eficiência).
- **Scan com filtro lê tudo.** Para achar pedidos com valor acima de 100, ele leu os 4 itens da tabela para devolver 2 (50% de eficiência). Em uma tabela grande isso seria lento e caro.
- **Sem esquema fixo.** Só um dos pedidos tinha o atributo `Observacao`, e os outros itens funcionaram normalmente sem alterar a tabela, diferente de uma tabela SQL.
- **Custo.** Usei o modo on-demand, que não cobra quando a tabela está parada, e acompanho o gasto pelo AWS Budgets. A tabela pode ser apagada a qualquer momento para zerar o recurso.

**Resultado: Query x Scan**

<p align="center">
  <img src="https://github.com/user-attachments/assets/6af966bd-f7b9-458d-a13a-1019486115d3" width="48%" alt="Query dos pedidos da Ana: 3 itens lidos, 3 devolvidos">
  <img src="https://github.com/user-attachments/assets/8169eb28-7a5f-4c87-a1d6-3d156e4dfa1d" width="48%" alt="Scan com filtro Valor maior que 100: 4 itens lidos, 2 devolvidos">
</p>

---

### Projeto 3: Rede na nuvem com Amazon VPC e EC2

**O que fiz:** montei uma rede do zero, com uma VPC (`10.0.0.0/16`), uma subnet pública e uma privada, um Internet Gateway, uma route table e um security group. Depois subi uma instância EC2 na subnet pública, com um servidor Apache instalado por *user data*, e abri a página pela internet. Ao final, terminei a instância e apaguei toda a rede.

<p align="center">
  <img src="https://github.com/user-attachments/assets/78083d6d-447f-4ea1-a6a0-4c5e1bf2c045" width="800" alt="Resource map da VPC: subnet pública ligada à route table e ao Internet Gateway, subnet privada na route table principal">
</p>

**O que aprendi na prática**

- **O que torna uma subnet pública não é o nome.** É a route table: a `subnet-publica` tinha uma rota `0.0.0.0/0` para o Internet Gateway, e a `subnet-privada` ficou na route table principal, só com a rota local.
- **O Internet Gateway sozinho não basta.** Para a página abrir foram necessárias quatro peças juntas: o IGW anexado à VPC, a rota `0.0.0.0/0` para ele, um IP público na instância e uma regra do security group liberando a porta 80.
- **Security group é stateful.** Liberei só a entrada (HTTP, porta 80). A resposta volta sem regra extra, e não abri SSH porque não precisava entrar na máquina.
- **Uma rota para um recurso apagado vira *Blackhole*.** Depois de apagar o IGW, a rota `0.0.0.0/0` da route table ficou nesse estado.
- **User data roda uma vez, na primeira inicialização.** Um erro de digitação no script só aparece depois que a instância sobe, então vale conferir o texto antes de lançar.
- **Ordem de limpeza importa.** A AWS não apaga um recurso enquanto outro depende dele: instância → Internet Gateway → route table → subnets → security group → VPC.

<p align="center">
  <img src="https://github.com/user-attachments/assets/64180a97-7418-487e-897d-ee7d7ce80ead" width="650" alt="Página servida pela instância EC2 na subnet pública">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/SEU-CODIGO-AQUI" width="650" alt="Instância servidor-web em Running com 3 de 3 checks aprovados">
</p>

**Erros que me ensinaram algo**

- Ao lançar a instância, deixei a opção de criar um security group novo com um nome que já existia na VPC, e a AWS recusou o lançamento. Aprendi a escolher o grupo existente. O grupo novo também vem com SSH aberto para o mundo por padrão, o que eu não queria.
- Sobrou um `DD` no final do caminho do arquivo no script, o que criaria um arquivo `index.htmlDD` em vez do `index.html`. Percebi ao revisar o formulário, depois da primeira falha de lançamento.
- Parei a instância em vez de terminar. Parar pausa a computação, mas o disco continua cobrando. Terminar elimina o custo.

**Custo:** usei `t3.micro` (cerca de US$ 0,01 por hora) por poucas horas, com os créditos do plano, e apaguei tudo depois, sem deixar recurso ligado.

---

## 📬 Contato

[LinkedIn](https://www.linkedin.com/in/murilo-fidelis-55986b306/)
