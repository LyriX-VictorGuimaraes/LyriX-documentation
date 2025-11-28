# Arquitetura do Projeto LyriX

## Introdução

A arquitetura do LyriX foi projetada para ser **híbrida e distribuída**. O sistema não depende exclusivamente de um banco de dados local para o conteúdo musical, mas sim da integração com serviços de terceiros (Spotify) para a descoberta de conteúdo, utilizando o backend proprietário apenas para personalização (favoritos e histórico).

Diferente de arquiteturas monolíticas tradicionais, o LyriX opera em um modelo onde o **Frontend (Android)** orquestra chamadas entre duas APIs distintas.

---

## Arquitetura Escolhida

O projeto adota a seguinte estrutura tecnológica:

- **Frontend:** Android Nativo com sistema de **Views (XML)**. A escolha por XML garante compatibilidade e facilidade de implementação para a estrutura atual de Activities.
- **Backend:** **Spring Boot** (Kotlin) com arquitetura em camadas (Controller-Service-Repository).
- **Banco de Dados:** **MySQL 8** rodando em container Docker.
- **Integração Externa:** **Spotify Web API** para busca de metadados e **Spotify Auth SDK** para autenticação OAuth 2.0.

---

## Estrutura do Aplicativo Android

O aplicativo segue uma arquitetura baseada em **Componentes de UI e Serviços de Rede**:

### 1. Camada de UI (User Interface)
- Responsável pela interação com o usuário.
- Implementada via **Activities** (`MainActivity`, `BuscaActivity`) e layouts **XML**.
- Gerencia o fluxo de navegação e exibição de dados (RecyclerViews).

### 2. Camada de Rede (Network Layer)
- Implementada com **Retrofit 2**.
- Possui dois clientes HTTP distintos:
    - `SpotifyApi`: Responsável por buscar músicas e capas de álbuns na nuvem.
    - `LyrixApi`: Responsável por salvar favoritos e histórico no servidor local (Spring Boot).

### 3. Camada de Modelo (Data Models)
- Classes de dados (Data Classes) que mapeiam as respostas JSON tanto do Spotify quanto da API LyriX.

---

## Arquitetura do Backend (Spring Boot)

O backend atua como uma **API RESTful** de persistência e personalização.

### Estrutura em Camadas

**1. Controllers (Camada Web)**
   - Expõe os endpoints REST (ex: `POST /favoritos`, `GET /musicas`).
   - Gerencia os códigos de resposta HTTP e validação básica de entrada (DTOs).

**2. Repositories (Camada de Dados)**
   - Utiliza **Spring Data JPA** para abstrair a comunicação com o banco.
   - Gerencia queries automáticas e personalizadas (JPQL).

**3. Database (Infraestrutura)**
   - **MySQL 8** containerizado.
   - Configurado para rodar na porta **3307** para evitar conflitos locais.
   - Estratégia de *Naming Strategy* física para compatibilidade com Linux (Case Sensitivity).

---

## Fluxo de Dados (Híbrido)

O diferencial da arquitetura do LyriX é o fluxo de dados dividido:

```mermaid
sequenceDiagram
    participant User as Android App
    participant Spotify as Spotify API
    participant Lyrix as Lyrix Backend (Spring)
    participant DB as MySQL

    Note over User, Spotify: Fluxo de Leitura (Busca)
    User->>Spotify: GET /search (Termo)
    Spotify-->>User: JSON (Lista de Músicas e Capas)
    
    Note over User, Lyrix: Fluxo de Escrita (Favoritar)
    User->>Lyrix: POST /favoritos (Dados da Música)
    Lyrix->>DB: Salva Artista e Música (se novo)
    Lyrix->>DB: Cria vínculo de Favorito
    DB-->>Lyrix: Confirmação
    Lyrix-->>User: 200 OK
