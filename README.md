
# 🐳 Desafio DIO - Cluster de Microsserviços com Docker

Este repositório contém a resolução do desafio de projeto da **Digital Innovation One (DIO)** sobre Docker e orquestração de containers.

O objetivo foi refatorar uma aplicação baseada no cenário de Toshiro Shibakita, transformando ela em uma arquitetura de microsserviços escalável e independente da infraestrutura local.

## 🚀 Melhorias

Diferente do projeto original, que possuía IPs fixos e dependência de máquina específica, esta versão traz as seguintes mudanças:

-   **Docker Compose:** Orquestração completa dos serviços (Banco, Backend e Proxy) com um único comando.
    
-   **Service Discovery:** Uso do DNS interno do Docker. Os containers se comunicam pelos nomes dos serviços (`db`, `backend`), eliminando a necessidade de IPs fixos.
    
-   **Load Balancer Dinâmico:** Configuração do Nginx como Proxy Reverso para distribuir a carga entre múltiplas réplicas da aplicação PHP.
    
-   **Variáveis de Ambiente:** Credenciais e configurações sensíveis abstraídas, seguindo as boas práticas do _Twelve-Factor App_.
    
-   **Persistência de Dados:** Uso de Volumes Docker para garantir que os dados do MySQL não se percam ao reiniciar os containers.
    

## 🛠️ Tecnologias Usadas

-   **Docker & Docker Compose**
    
-   **PHP 7.4** (Backend com extensão MySQLi)
    
-   **Nginx** (Proxy Reverso e Load Balancer)
    
-   **MySQL 5.7** (Banco de Dados)
    

## 📂 Estrutura do Projeto

```
.
├── docker-compose.yml   
├── mysql/
│   └── init.sql         
├── php/
│   ├── Dockerfile       
│   └── index.php        
└── proxy/
    ├── Dockerfile       
    └── nginx.conf       

```

## 👣 Como Executar

### Pré-requisitos

-   Docker e Docker Compose instalados.
    

### Passo a Passo

1.  **Clone o repositório:**
    
    
    ```bash
    git clone https://github.com/rafaelmsantos28/DIO-Docker-Challenge.git
    cd DIO-Docker-Challenge
    
    ```
    
2.  **Suba o ambiente:**

    ```bash
    docker-compose up -d --build
    
    ```
    
3.  **Escalando a aplicação (Opcional):** Para testar o Load Balancer, suba 3 instâncias do backend PHP:

    ```bash
    docker-compose up -d --scale backend=3
    
    ```
    
4.  **Acesse a aplicação:** Abra o navegador em: [http://localhost:4500](https://www.google.com/search?q=http://localhost:4500)
    
    Ao atualizar a página (F5), você verá o **Host ID** mudar, indicando que o Nginx está distribuindo as requisições entre os containers criados.
    

## 🧪 Testando

O script `index.php` realiza uma inserção no banco de dados a cada carregamento e exibe o ID do container que processou a requisição:

> "New record created successfully by HOST: [ID_DO_CONTAINER]"

Isso comprova que a aplicação está escrevendo no banco MySQL compartilhado, independentemente de qual réplica do container PHP está respondendo.

![3 containers rodando](https://imgur.com/4FMkOZb.jpg "3 containers rodando")

![Distribuição de carga nos containers](https://imgur.com/xuqJYMt.jpg "Distribuição de carga nos containers")

----------

**Desenvolvido como parte do Bootcamp da DIO.**