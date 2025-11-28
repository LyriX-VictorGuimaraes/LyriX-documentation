# LyriX Docs

## 🎵 Sobre o Projeto
O **LyriX** é um aplicativo Android nativo desenvolvido em Kotlin que atua como um "companheiro musical". Ele permite que o usuário utilize sua conta do Spotify para buscar músicas e artistas, salvando suas preferências (favoritos e histórico) em um backend próprio e seguro.

## 🛠️ Stack Tecnológica

### Backend (Servidor)
- **Linguagem:** Kotlin
- **Framework:** Spring Boot 3.x (JPA/Hibernate, Web)
- **Banco de Dados:** MySQL 8 (Rodando via Docker)
- **API Externa:** Spotify Web API

### Frontend (Mobile)
- **Plataforma:** Android Nativo (Views/XML)
- **Linguagem:** Kotlin
- **Autenticação:** Spotify Android Auth SDK (OAuth 2.0)
- **Comunicação:** Retrofit 2
