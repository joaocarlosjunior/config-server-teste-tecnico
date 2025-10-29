# ⚙️ Config Server - Email Service (Desafio Back-end Viasoft)

## 🧾 Descrição do Projeto

Este repositório contém o **servidor de configuração** utilizado pela aplicação
principal [📧 Email Service](https://github.com/joaocarlosjunior/email-service), desenvolvida para o **teste técnico de
Back-end Java Pleno da Viasoft**.

O servidor de configuração foi implementado com o **Spring Cloud Config Server**, permitindo **gerenciar dinamicamente
as configurações** da aplicação principal **sem a necessidade de reiniciar o serviço**.

Ele fornece, via HTTP, os arquivos de configuração do sistema, centralizando as definições em um único ponto.

---

## 🧠 Objetivo

O principal objetivo deste servidor é **permitir a troca dinâmica do serviço de envio de e-mails** entre **AWS** e **OCI**, que são as duas opções suportadas pela aplicação principal (`email-service`).

Com isso, é possível alternar o provedor de e-mail de forma centralizada e controlada, apenas atualizando este
repositório — sem precisar modificar ou reiniciar o back-end principal.

---

## 🏗️ Estrutura do Projeto

````
config-server-teste-tecnico
├── src
│ ├── main
│ │ └── com.joaocarlos.email_service
│ │      └── ConfigServerApplication.java # Classe principal
│ └── resources
│   ├── email-service
│   │    └── email-service.yml # Configuração remota consumida pela aplicação principal
│   └── application.yml # Configuração do servidor de configuração
````

---

## ⚙️ Configurações Principais

### 📄 `application.yml`

Define as propriedades do servidor de configuração e o repositório Git remoto onde as configurações estão armazenadas.

```yaml
server:
  port: 8888

spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/joaocarlosjunior/config-server-teste-tecnico.git
          search-paths:
            - /src/main/resources/email-service
```

Este servidor escuta por padrão na porta 8888 e fornece as configurações para a aplicação principal (email-service).

### 📄 `email-service.yml`

```yaml
mail:
  integracao: 'AWS'
```

- Valor padrão: `AWS`

- Valor alternativo possível: `OCI`

## 🔄 Como alterar o serviço de e-mail (AWS / OCI)

Para trocar o serviço de envio de e-mails utilizado pela aplicação principal:

1. Acesse o repositório deste servidor de configuração:

   👉 https://github.com/joaocarlosjunior/config-server-teste-tecnico
2. No arquivo src/main/resources/email-service.yml altere o valor da propriedade:
    ```yaml
   mail:
     integracao: 'OCI' # ou 'AWS'
   ```
3. Faça commit e push para o repositório remoto no GitHub.
4. Na aplicação principal (email-service), execute o comando/requisição de atualização:
   ```
   curl -X POST http://localhost:8080/actuator/refresh
   ```
   ou acesse o endpoint /actuator/refresh.
5. A aplicação principal passará a utilizar automaticamente o novo serviço (sem precisar ser reiniciada).

## 🚀 Executando o Servidor de Configuração

Requisitos:
- Java 17 

1. Clone o repositório e acesse a pasta:
   ```
   git clone https://github.com/joaocarlosjunior/config-server-teste-tecnico.git
   cd config-server-teste-tecnico
   ```
2. Execute o servidor:
- Linux:
   ```
   ./mvnw spring-boot:run
   ```
- Windows:
   ```
   mvnw.cmd spring-boot:run
   ```
O servidor será iniciado em: http://localhost:8888

## 👨‍💻 Autor

João Carlos Junior

Desenvolvedor Full-stack

📎 [GitHub](https://github.com/joaocarlosjunior)

📎 [Linkedin](https://www.linkedin.com/in/joaocarlosjr/)





