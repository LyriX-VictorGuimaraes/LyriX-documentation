# Visão Geral do Projeto LyriX

Este documento detalha o planejamento e o escopo de entrega do projeto LyriX, alinhado aos critérios de avaliação da disciplina (PC1, PC2 e PC3).

O **LyriX** é uma solução de software composta por uma aplicação móvel (Android Nativo) e uma API RESTful (Spring Boot), focado na integração com o Spotify e organização de biblioteca musical pessoal.

---

## Ponto de Controle 1 (PC1) - Concepção e Infraestrutura
**Valor Total:** 10 pontos
**Status:** Entregue/Implementado

Nesta etapa, o foco foi estruturar a base do projeto, definir requisitos e configurar o ambiente de desenvolvimento.

| Critério | Implementação no LyriX |
| :--- | :--- |
| **Diagrama UML** | Diagrama de Classes representando as entidades (`Usuario`, `Musica`, `Artista`, `Favorito`) e seus relacionamentos (JPA/Hibernate). |
| **Backlog** | User Story Mapping (USM) detalhando Épicos, Features e Histórias de Usuário (MVP definido). |
| **Protótipo de Alta Fidelidade** | Definição da identidade visual (Dark Mode, Roxo Elétrico) e fluxos de tela (Login, Busca, Favoritos). |
| **Docker** | Containerização do banco de dados MySQL rodando na porta 3307, isolado do ambiente local. |

---

## Ponto de Controle 2 (PC2) - Backend e Qualidade de Código
**Valor Total:** 20 pontos
**Status:** Em Desenvolvimento

Nesta etapa, o foco é a robustez do Backend, arquitetura e garantia de qualidade via testes e padronização.

| Critério | Planejamento/Implementação |
| :--- | :--- |
| **Arquitetura** | Arquitetura Cliente-Servidor. Backend seguindo padrão MVC/Layers (Controller ↔ Repository ↔ Model) com injeção de dependência. |
| **Clean Code** | Código Kotlin idiomático, uso de DTOs para transferência de dados e nomes semânticos de funções/variáveis. |
| **Teste Parametrizado** | |
| **Teste Integração** |  |
| **Banco de Dados** | MySQL 8 configurado |
| **Modelo Físico do Banco** | Schema relacional normalizado gerado via Hibernate (DDL Auto) com chaves estrangeiras e constraints. |
| **API em repositório separado** | Projeto dividido em dois repositórios (ou módulos distintos): `lyrix-api` (Backend) e `lyrix-app` (Mobile). |
| **Lint ou derivados** |  |

---

## Ponto de Controle 3 (PC3) - Frontend, Deploy e Testes E2E
**Valor Total:** 30 pontos
**Status:** Planejado

A etapa final foca na entrega da experiência do usuário completa, disponibilização do serviço na nuvem e testes de interface.

| Critério | Planejamento |
| :--- | :--- |
| **Front completo** |  |
| **Hospedagem** |  |
| **Teste automatizado** |  |

---
