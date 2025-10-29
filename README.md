# cloudformationdio
Experiências e aprendizados sobre implementação de infraestrutura automatizada com AWS CloudFormation e Terraform.

Este repositório documenta minha jornada de aprendizado e experimentação na **implementação de infraestrutura automatizada** na AWS utilizando **CloudFormation** e **Terraform**.

O objetivo é criar um material de apoio para estudos, referência e futuras implementações.

---

## Conteúdo

- `cloudformation/` — Estudos, exemplos e projetos práticos com AWS CloudFormation  
- `terraform/` — Estudos, exemplos e projetos práticos com HashiCorp Terraform  
- `insights/` — Comparações, melhores práticas e lições aprendidas  

---

##  Tecnologias abordadas
- **AWS CloudFormation** — Gerenciamento de infraestrutura como código nativa da AWS  
- **Terraform** — Ferramenta multi-cloud para provisionamento de recursos  

---

##  Objetivos de aprendizado
- Entender a filosofia de **Infraestrutura como Código (IaC)**  
- Criar e gerenciar pilhas de infraestrutura com CloudFormation  
- Utilizar Terraform para provisionar recursos AWS de forma modular e reutilizável  
- Comparar CloudFormation e Terraform em termos de sintaxe, controle e manutenção  


markdown
Copiar código
# CloudFormation — Notas e Aprendizados

## O que é o AWS CloudFormation?
O CloudFormation é o serviço da AWS para modelar, provisionar e gerenciar recursos da AWS usando arquivos declarativos (YAML/JSON).

### Estrutura básica de um template
Um template CloudFormation normalmente possui:

AWSTemplateFormatVersion: '2010-09-09'
Description: Exemplo de criação de um bucket S3
Resources:
  MeuBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: meu-bucket-exemplo

### Criando uma stack

aws cloudformation create-stack \
  --stack-name exemplo-s3 \
  --template-body file://simple-s3-bucket.yaml
  
### Atualizando uma stack

aws cloudformation update-stack \
  --stack-name exemplo-s3 \
  --template-body file://simple-s3-bucket.yaml
  
### Boas práticas
Sempre versionar templates no Git.
Usar Parameters e Outputs para flexibilidade.
Validar templates com: aws cloudformation validate-template --template-body file://template.yaml

## Terraform — Notas e Aprendizados

## O que é o Terraform?
O Terraform é uma ferramenta da HashiCorp para provisionamento de infraestrutura em múltiplos provedores (AWS, Azure, GCP, etc).

### Estrutura básica de uma stack
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "meu_bucket" {
  bucket = "meu-bucket-exemplo"
  acl    = "private"
}

output "bucket_name" {
  value = aws_s3_bucket.meu_bucket.bucket
}

## Passos para criar uma stack

terraform init
terraform plan
terraform apply
Modularização

Recomenda-se dividir a infraestrutura em módulos reutilizáveis, por exemplo:

modules/
└── s3/
    ├── main.tf
    ├── variables.tf
    └── outputs.tf

## Boas práticas
Utilizar remote backend (S3 + DynamoDB) para armazenar o estado.
Implementar naming conventions consistentes.
Usar workspaces para ambientes (dev, staging, prod).

# Comparação: CloudFormation vs Terraform

| Aspecto                  | CloudFormation                         | Terraform                            |
|---------------------------|----------------------------------------|--------------------------------------|
| Linguagem                 | YAML/JSON                              | HCL (HashiCorp Configuration Language) |
| Multi-cloud               | Somente AWS                            | Multi-cloud (AWS, Azure, GCP, etc.)  |
| Estado                    | Gerenciado automaticamente pela AWS    | Requer backend para estado remoto    |
| Modularização             | Limitada                               | Forte suporte a módulos              |
| Curva de aprendizado       | Mais simples para quem já usa AWS      | Mais flexível, porém mais complexa   |

### Conclusão
- **CloudFormation**: Ideal para ambientes 100% AWS e integração nativa.  
- **Terraform**: Melhor para cenários híbridos/multi-cloud e maior reuso.

# Melhores Práticas IaC

1. **Versionamento no Git** — Toda modificação de infraestrutura deve passar por versionamento.
2. **Automatização CI/CD** — Use GitHub Actions ou AWS CodePipeline para automatizar o deploy.
3. **Validação e linting** — Ferramentas como `cfn-lint` e `terraform validate`.
4. **Documentação** — Comente e descreva recursos e variáveis.
5. **Modularização** — Evite templates monolíticos; crie componentes independentes.
6. **Controle de acesso** — Proteja o estado (no Terraform) e use roles adequadas na AWS.

