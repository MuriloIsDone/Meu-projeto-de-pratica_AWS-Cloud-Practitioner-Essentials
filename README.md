# Meu projeto de prática: AWS Cloud Practitioner Essentials

Repositório com os projetos práticos que estou fazendo durante o curso **AWS Cloud Practitioner Essentials**.

## Sobre mim

Estudante de Ciência da Computação (Mackenzie), buscando migrar para a área de tecnologia e programação.

## Projetos

| # | Projeto | Serviços AWS | Status |
| 1 | Site estático (veja online: (http://murilo-site-aws-2026.s3-website-us-east-1.amazonaws.com)) | S3 | Concluído  |
| 2 | Rede na nuvem (VPC + servidor web) | VPC, EC2, Security Groups | A fazer |

## Como está organizado

Cada projeto terá uma pasta com o código e um diagrama da arquitetura.

## Aprendizados:

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


**Próximos passos:** VPC com sub-redes pública e privada, Security Groups e uma instância EC2 (Módulo 5), e depois CloudFront para HTTPS.

## Para Contato
[LinkedIn](https://www.linkedin.com/in/murilo-fidelis-55986b306/)
