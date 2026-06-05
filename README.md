# 🌐 Infraestrutura como Código (IaC) na AWS: Migração para Arquitetura com Serviços Gerenciados (RDS)

Este repositório contém o projeto prático desenvolvido para a disciplina de **COMPUTAÇÃO EM NUVEM (2026.1 - T02)**, onde foi realizada a modernização e migração de uma infraestrutura de rede e sistemas baseada em instâncias EC2 tradicionais para uma arquitetura escalável utilizando serviços gerenciados da **AWS (Amazon Relational Database Service - RDS)**.

A automação foi construída utilizando o **Terraform** para o provisionamento dos recursos de nuvem e o **Ansible** para a gerência de configuração e deploy de serviços.

---

## 🏗️ Cenário de Migração e Arquitetura

O objetivo principal do projeto foi desacoplar o banco de dados da camada de computação ec2 tradicional, mitigando problemas de gerência manual de SO, espaço em disco e backups.

* **Abordagem Antiga:** Duas instâncias EC2 rodando em conjunto, onde o PostgreSQL era instalado e configurado manualmente pelo Ansible dentro de um sistema operacional Ubuntu bruto.
* **Nova Abordagem (Este Projeto):** Utilização do **Terraform** para provisionar diretamente o motor do **PostgreSQL 16** através do **AWS RDS**, isolado em um grupo de subnets privado (`aws_db_subnet_group`). O **Ansible** passou a gerenciar exclusivamente o servidor Web (NGINX), instalando apenas o `postgresql-client` para efetuar a comunicação segura de rede.

---

## 🛠️ Tecnologias e Ferramentas Utilizadas

* **Provedor de Nuvem:** AWS (Amazon Web Services) via Learner Lab Ambiente Acadêmico.
* **Orquestração & Provisionamento:** Terraform (HCL).
* **Gerenciamento de Configuração:** Ansible (YAML) com utilização de **Inventário Dinâmico** via plugin `amazon.aws.aws_ec2`.
* **Serviços de Infraestrutura:** AWS EC2 (Nginx), AWS RDS (PostgreSQL 16), VPC, Subnets, Internet Gateway e Security Groups.

---

## 📂 Estrutura do Repositório

```text
├── ansible/
│   ├── group_vars/
│   │   └── aws_ec2         # Variáveis de conexão SSH globais
│   ├── roles/
│   │   └── web/            # Role responsável pela automação do servidor web
│   │       └── tasks/
│   │           └── main.yml # Instalação do Nginx e postgresql-client
│   ├── aws_ec2.yaml        # Configuração do Inventário Dinâmico AWS
│   └── playbook.yml        # Playbook principal do Ansible
├── terraform/
│   ├── main.tf             # Definição da EC2, RDS e Subnet Groups
│   ├── provider.tf         # Configurações do Provedor AWS e Região
│   ├── securitygroup.tf    # Regras de Ingress/Egress (Portas 22, 80 e 5432)
│   └── variables.tf        # Declaração de variáveis dinâmicas e sensíveis
└── .gitignore              # Proteção contra envio de arquivos de estado e senhas
