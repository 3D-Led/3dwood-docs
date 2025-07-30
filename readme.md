# 3D Wood - Documentação

Projeto de Catálogo Digital para produtos em madeira com vitrine pública e área de administração restrita a usuários autenticados.
***

### Sumário
1. [Resumo do Projeto](#1-resumo-do-projeto)
2. [Arquitetura do Sistema](#2-arquitetura-do-sistema)
3. [Tecnologias Utilizadas](#3-tecnologias-utilizadas)
4. [Fluxo de Funcionamento](#4-fluxo-de-funcionamento)
5. [Modelagem de Dados](#5-modelagem-de-dados)
6. [API - Endpoints](#6-api---endpoints)
7. [Autenticação](#7-autenticação)
8. [Upload de Imagens (Cloudinary)](#8-upload-de-imagens-cloudinary)
9. [Hospedagem e Deploy](#9-hospedagem-e-deploy)
10. [Instruções de Instalação](#10-instruções-de-instalação)
11. [Diagramas](#11-diagramas)
12. [Checklist de Segurança](#12-checklist-de-segurança)
***

### 1. Resumo do Projeto

O 3D Wood é um site de catálogo digital para produtos artesanais de madeira, onde:
* Visitantes podem visualizar os produtos em uma vitrine leve e responsiva.
* Administradores podem cadastrar, editar e remover produtos por um painel restrito com login.
* As imagens dos produtos são armazenadas e otimizadas via Cloudinary.
***
### 2. Arquitetura do Sistema

**Frontend:** Next.js + Tailwind CSSBackend:</br> 
**Backend:** Java + Spring Boot (API REST)</br>
**Banco de dados:** PostgreSQL</br>
**Armazenamento de imagens:** Cloudinary</br>
**Hospedagem:** Vercel (frontend) + Render (backend)

```
Usuário / Admin
     ↓
[Next.js Frontend]  ↔  [Spring Boot API]  ↔  PostgreSQL
                                    ↓
                                Cloudinary
```
***
### 3. Tecnologias Utilizadas

|Camada        |Tecnologia          |
|:---          |:---                |
|Frontend      |Next.js, Tailwind   |
|Backend       |Java 17, Spring Boot|
|Banco de Dados|PostgreSQL          |
|Autenticação  |JWT                 |
|Armazenamento |Cloudinary          |
|Hospedagem    |Vercel / Render     |
***
### 4. Fluxo de Funcionamento

**Usuário visitante:**

* Acessa o site

* Visualiza os produtos e imagens otimizadas

**Administrador:**

* Faz login

* Acessa painel admin

* Cadastra produtos com várias fotos

* Backend envia imagens para o Cloudinary

* URLs das imagens ficam salvas no banco

* Frontend consome a API e exibe os produtos
***
### 5. Modelagem de Dados

**Produto**
``` java
class Produto {
    Long id;
    String nome;
    String descricao;
    List<String> imagens; // URLs do Cloudinary
    List<Categorias> categorias;
    LocalDateTime criadoEm;
}
```
**Categoria**
``` java
class Categoria {
    Long id;
    String nome;
    List<Produto> produtos;
}
```
**Usuario**
``` java
class Usuario {
    Long id;
    String email;
    String senhaHash;
    String role; // ADMIN
}
```
***
### 6. API - Endpoints

| Método | Rota               | Descrição              | Autenticado? |
| :---   | :---               | :---                   | :---         |
| GET    | /api/products      | Listar produtos        | Não          |
| GET    | /api/products/{id} | Detalhes do produto    | Não          |
| POST   | /api/products      | Criar novo produto     | Sim (admin)  |
| PUT    | /api/products/{id} | Atualizar produto      | Sim (admin)  |
| DELETE | /api/products/{id} | Deletar produto        | Sim (admin)  | 
| POST   | /api/auth/login    | Login e gera token JWT | Não          |
*** 
### 7. Autenticação

* JWT Token é gerado no login
* Frontend armazena o token (localStorage ou cookie)
* Todas as requisições protegidas verificam o token no cabeçalho `Authorization`
***
### 8. Upload de Imagens (Cloudinary)

1. Backend envia imagem usando SDK Java:
``` java
    Map uploadResult = cloudinary.uploader().upload(file.getBytes(), ObjectUtils.emptyMap());
    String imageUrl = uploadResult.get("secure_url").toString();
```
2. Salva URL no banco
3. Frontend exibe imagem otimizada
```html
    <img src="https://res.cloudinary.com/SEU_CLOUD_NAME/image/upload/w_300/moveis.jpg" />
```
***
### 9. Hospedagem e Deploy
|Camada    |Plataforma  |Observações                 |
|:---      |:---        |:---                        |
| Frontend | Vercel     | Integra com GitHub         |
| Backend  | Render     | Suporte a Java/Spring Boot |
| Imagens  | Cloudinary | CDN automática             |

### 10. Instruções de Instalação

**Backend:**
```bash
    git clone https://github.com/seuusuario/3dwood-backend.git
    cd 3dwood-backend

    # Configurar application.properties
    spring.datasource.url=...
    cloudinary.api_key=...

    ./mvnw spring-boot:run
```
**Frontend:**
```bash
    git clone https://github.com/seuusuario/3dwood-frontend.git
    cd 3dwood-frontend

    npm install
    npm run dev
```
***
### 11. Diagramas
1. `arquitetura.png` – Arquitetura da aplicação
2. `classes-backend.png` – Diagrama de classes (Produto, Usuario)
3. `banco-erd.png` – Modelo relacional do banco
4. `sequencia-upload.png` – Diagrama de sequência do upload de produto   
Todos estão na pasta `/diagrams.`
***
### 12. Checklist de Segurança

_Desenvolvidor por: João Victor Nascimento_