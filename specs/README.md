# Especificações (`specs/`)

Esta pasta guarda os artefatos de **especificação** produzidos no fluxo Spec-Driven Development.

## Arquivos

### 📄 [discovery.md](discovery.md) — Especificação Formal v1.0

**Status**: ✅ Completa e pronta para Plan Agent

Este arquivo contém a **especificação formal completa do Weather App** (v1.0 - MVP):

- **Visão Geral**: Objetivo, público-alvo, proposta de valor
- **Histórias de Usuário (HU1-HU7)**: Busca, clima atual, previsão 5d, C/F toggle, favoritos, histórico, offline
- **Critérios de Aceite**: Verificáveis, automatizáveis por teste
- **Modelo de Dados**: TypeScript interfaces normalizadas
- **Especificação de API (Open-Meteo)**: Endpoints, requests, responses, error handling
- **Requisitos Não-Funcionais**: Performance, acessibilidade, segurança, SEO, privacidade
- **Fluxos de Interação**: Happy path, offline, erros, mobile
- **Critérios de Aceite Globais**: Testes (80% coverage), E2E, lint, build, a11y
- **Suposições & Restrições**: MVP com 13 decisões respondidas
- **Glossário**: Termos técnicos do projeto

### 📋 [stakeholder-alignment.md](stakeholder-alignment.md)

Documento de decisão com 13 questões críticas e suposições de MVP. Respondidas e consolidadas em `discovery.md`.

---

## Pipeline SDD

```
Discovery (análise)
    ↓
Spec (discovery.md) ← VOCÊ ESTÁ AQUI
    ↓
Plan (plans/weather-app-plan.md)
    ↓
Tasks (tasks/weather-app-tasks.md)
    ↓
Code (src/)
    ↓
Test (tests/)
```

---

## Como Usar Esta Spec

### Para Desenvolvedores

1. Leia [discovery.md](discovery.md) **seção 2 (Histórias de Usuário)**
2. Consulte a **seção 3 (Modelo de Dados)** para tipos TypeScript
3. Use **seção 4 (API Open-Meteo)** como contrato de integração
4. Valide contra **seção 7 (Critérios de Aceite Globais)**

### Para QA/Testes

1. Extraia os critérios de aceite de cada HU (seção 2)
2. Use exemplos concretos (dados de teste)
3. Implemente testes E2E baseados em **seção 6 (Fluxos)**

### Para Product/Stakeholders

1. Leia **seção 1 (Visão Geral)** para alinhamento de escopo
2. Valide **suposições & restrições (seção 8)** com a estratégia
3. Monitore KPIs: DAU 10K, churn < 5%/semana

---

## Princípios

> **NÃO COMECE PELO CÓDIGO.**  
> Sempre: **Spec → Plan → Tasks → Code.**

- Spec clara = fewer questions during dev
- Spec testável = QA sabe o que validar
- Spec completa = zero ambiguidades
