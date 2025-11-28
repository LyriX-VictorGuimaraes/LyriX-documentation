# Backend — Repositório e Execução

O backend do LyriX é uma API RESTful construída com **Kotlin** e **Spring Boot**, responsável pela persistência de dados do usuário, favoritos e histórico.

## Estrutura do Repositório

A organização do projeto segue o padrão de camadas (Layered Architecture) do Spring Boot:

```text
lyrix-api/
├── src/
│   ├── main/
│   │   ├── kotlin/com/app/lyrix/
│   │   │   ├── controller/   # Camada Web (Endpoints REST)
│   │   │   ├── model/        # Camada de Domínio (Entidades JPA)
│   │   │   ├── repository/   # Camada de Dados (Interfaces Spring Data)
│   │   │   └── LyrixApplication.kt # Ponto de entrada (Main)
│   │   └── resources/
│   │       └── application.properties # Configurações (Banco, Porta, Hibernate)
├── build.gradle.kts          # Gerenciamento de dependências (Kotlin DSL)
├── settings.gradle.kts       # Configurações do projeto Gradle
└── docker-compose.yml        # (Opcional) Orquestração do banco de dados
````

-----

## Como Executar Localmente

### Pré-requisitos

  * **Java JDK 17** ou superior instalado.
  * **Docker** rodando com o container MySQL na porta **3307**.

### 1\. Configurar Variáveis (`application.properties`)

Certifique-se de que o arquivo `src/main/resources/application.properties` está apontando para a porta correta do Docker e utilizando a estratégia de nomenclatura correta para evitar erros de case-sensitivity no Linux:

```properties
server.port=8080
spring.datasource.url=jdbc:mysql://localhost:3307/lyrix
spring.datasource.username=admin
spring.datasource.password=admin
spring.jpa.hibernate.ddl-auto=update
# Linha crítica para compatibilidade com Docker/Linux:
spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
```

### 2\. Rodar a Aplicação

No terminal, dentro da pasta raiz do projeto backend:

**Windows:**

```powershell
./gradlew.bat bootRun
```

**Linux / Mac:**

```bash
./gradlew bootRun
```

Aguarde até ver a mensagem: `Tomcat started on port 8080 (http)`.

-----

## Endpoints Disponíveis

A API roda base em `http://localhost:8080`. Abaixo os principais recursos:

### Usuários (`/usuarios`)

| Método | Rota | Descrição | Body (JSON) |
| :--- | :--- | :--- | :--- |
| `POST` | `/usuarios/cadastrar` | Cria um novo usuário local. | `{ "nome": "Victor", "username": "user1" }` |
| `POST` | `/usuarios/login-spotify` | **Login Social:** Busca usuário pelo ID do Spotify ou cria se não existir. | `{ "nome": "Victor", "username": "spotify_id_123" }` |

### Favoritos (`/favoritos`)

*Endpoint principal de integração. Salva música/artista automaticamente se não existirem.*

| Método | Rota | Descrição | Body (JSON) |
| :--- | :--- | :--- | :--- |
| `POST` | `/favoritos` | Favorita uma música. | *Ver exemplo abaixo* |
| `GET` | `/favoritos/usuario/{id}` | Lista todas as músicas favoritas de um usuário. | - |
| `DELETE`| `/favoritos/{id}` | Remove um favorito. | - |

**Exemplo de Body para Favoritar:**

```json
{
  "usuarioId": 1,
  "tituloMusica": "Tempo Perdido",
  "nomeArtista": "Legião Urbana"
}
```

### Músicas (`/musicas`)

*Acesso direto ao catálogo salvo no banco local.*

| Método | Rota | Descrição |
| :--- | :--- | :--- |
| `GET` | `/musicas/buscar?termo=X` | Busca músicas no banco local por título, artista ou trecho de letra. |
| `GET` | `/musicas` | Lista todas as músicas cadastradas. |

### Histórico (`/historico`)

| Método | Rota | Descrição | Body (JSON) |
| :--- | :--- | :--- | :--- |
| `POST` | `/historico` | Registra um termo pesquisado. | `{ "usuarioId": 1, "texto": "Skank" }` |
| `GET` | `/historico/usuario/{id}` | Recupera buscas recentes do usuário. | - |
