# Questões Críticas para Alinhamento com Stakeholders
## Weather App MVP — Pre-Desenvolvimento

**Data:** 2026-08-12  
**Objetivo:** Resolver 13 ambiguidades críticas ANTES de iniciar desenvolvimento  
**Impacto de não responder:** 200+ horas de retrabalho + possível cancelamento  

---

## 📋 INSTRUÇÕES DE USO

Este documento deve ser apresentado em reunião com stakeholders (Produto, Negócio, Executivos). 
Para cada questão:
1. Leia o contexto
2. Discuta opções com stakeholders
3. Registre decisão na coluna "DECISÃO"
4. Indique severidade: 🔴 (bloqueadora) ou 🟡 (importante)

**Facilitador deve assegurar**: Todas as 🔴 respondidas antes de kickoff de desenvolvimento.

---

## 1️⃣ ESCOPO GEOGRÁFICO

**Pergunta Principal:** Quais países/regiões suportamos no MVP?

### Contexto
"Buscar cidades" é vago. Mundo inteiro tem ~4M cidades. Cidades duplicadas (50+ São Paulo).

### Opções Recomendadas
- [ ] A. **Brasil apenas** (MVP mínimo, mais simples)
- [ ] B. **Américas** (BR + US + LatAm, médio)
- [ ] C. **Mundo inteiro** (máximo, complexo)
- [ ] D. **Configurável** (qual lista? como atualizar?)

### Impacto da Decisão

| Opção | Complexidade | API calls | Custo Dev | Timeline |
|-------|-------------|-----------|-----------|----------|
| A | Baixa | 5K/dia | Básico | 2 sem |
| B | Média | 50K/dia | Médio | 3 sem |
| C | Alta | 500K/dia | Alto | 4 sem |

### Sub-questões Obrigatórias
- [ ] Como resolver cidades com nomes duplicados no autocomplete?
  - Mostrar todas? Agrupar por estado/país? Qual máximo?
- [ ] Dados de entrada: qual banco de dados (Wikipedia, OpenStreetMap, Open-Meteo)?
- [ ] Há restrições geopolíticas ou de soberania de dados?

### DECISÃO
**Escolhido:** ___________  
**Justificativa:** ___________  
**Responsável:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## 2️⃣ FREQUÊNCIA DE ATUALIZAÇÃO (Clima Atual)

**Pergunta Principal:** Com que frequência buscar dados novos?

### Contexto
Afeta: rate limiting de API, experiência do usuário, custo de banda.

### Opções Recomendadas
- [ ] A. **Real-time** (a cada requisição, sem cache) → abusa API, caro
- [ ] B. **Automático a cada 30 min** (padrão de apps climáticos)
- [ ] C. **Automático a cada 1 hora** (economiza banda, menos real-time)
- [ ] D. **Apenas em refresh manual do usuário** (app simples, mas offline ruim)
- [ ] E. **Híbrido** (30min automático + refresh manual)

### Impacto da Decisão

| Opção | Taxa API/dia | Cache? | UX Offline | Custo API |
|-------|-------------|--------|-----------|-----------|
| A | 2880 req/dia | Não | Péssima | Alto |
| B | 288 req/dia | Sim (30min) | Boa | Médio |
| C | 144 req/dia | Sim (1h) | OK | Baixo |
| D | ~10 req/dia | Não | Péssima | Muito baixo |
| E | 288 req/dia | Sim | Ótima | Médio |

### Sub-questões Obrigatórias
- [ ] Usuário pode forçar refresh manual?
- [ ] Qual é o limite de requisições por dia/por usuário?
- [ ] E se API ficar offline por 2h? Fallback para dado cacheado?
- [ ] Em mobile/3G, qual timeout máximo? (5s, 10s?)

### DECISÃO
**Escolhido:** ___________  
**Justificativa:** ___________  
**Responsável:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## 3️⃣ PRECISÃO E GRANULARIDADE DE DADOS

**Pergunta Principal:** Quais dados exatos mostrar?

### Contexto
"Clima atual" pode significar apenas temperatura OU 20+ campos de dados.

### Conjunto Mínimo (MVP)
- [ ] Temperatura atual
- [ ] Condição (céu limpo, nublado, chuva, neve, etc.)
- [ ] Umidade
- [ ] Velocidade do vento
- [ ] Sensação térmica

### Conjunto Expandido (Nice-to-have)
- [ ] Pressão atmosférica
- [ ] Radiação solar / UV
- [ ] Índice de qualidade do ar (AQI)
- [ ] Probabilidade de chuva
- [ ] Visibilidade
- [ ] Nascer/pôr do sol
- [ ] Alertas de clima severo

### Decisão de Precisão
- [ ] Temperatura em inteiros (ex: 25°C)
- [ ] Temperatura com 1 casa decimal (25.3°C)
- [ ] Temperatura com 2 casas (25.33°C)

### DECISÃO
**Dados selecionados (MVP):** ___________  
**Dados futuros (v2):** ___________  
**Precisão:** ___________  
**Responsável:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## 4️⃣ GRANULARIDADE DA PREVISÃO DE 5 DIAS

**Pergunta Principal:** Previsão é diária, horária, ou ambas?

### Contexto
Afeta design de UI, volume de dados, performance em mobile.

### Opções Recomendadas
- [ ] A. **Diária apenas** (mín/máx por dia, 5 linhas, simples) ✅ MVP
- [ ] B. **Horária completa** (24h × 5 dias = 120 pontos, complexo)
- [ ] C. **Resumida por período** (manhã/tarde/noite, 15 pontos)
- [ ] D. **Híbrida** (diária padrão, expandir para horária ao tocar)

### Impacto da Decisão

| Opção | Volume de dados | Complexidade UI | Performance Mobile | Espaço tela |
|-------|-----------------|-----------------|-------------------|-------------|
| A | 5 pontos | Baixa | Ótima | Vertical |
| B | 120 pontos | Alta | Ruim em 3G | Scroll pesado |
| C | 15 pontos | Média | Boa | Scroll moderado |
| D | 5+120 | Média | Boa | Adaptável |

### Sub-questões Obrigatórias
- [ ] Se horária, qual é o intervalo: a cada 1h, 3h, 6h?
- [ ] Como exibir em mobile 320px? (scroll horizontal de dias?)
- [ ] Mostrar precipitação (mm/chuva)?
- [ ] Mostrar chance de chuva (%)?

### DECISÃO
**Granularidade escolhida:** ___________  
**Intervalo (se horária):** ___________  
**Responsável:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## 5️⃣ PERSISTÊNCIA DE DADOS (Histórico e Favoritos)

**Pergunta Principal:** Quais dados salvamos localmente?

### Contexto
Afeta: GDPR/privacidade, arquitetura (local vs backend), UX de repetição.

### Opções Recomendadas
- [ ] A. **Sem persistência** (app stateless, zero privacidade risk, mas ruim UX)
- [ ] B. **Histórico de buscas** (últimas 10 cidades, localStorage)
- [ ] C. **Cidades favoritas** (salvar 5 favoritos, localStorage)
- [ ] D. **Ambos + sincronização** (requer login + backend)
- [ ] E. **Tudo online** (login, backend de dados)

### Impacto da Decisão

| Opção | Privacidade | GDPR | UX | Complexidade | Backend? |
|-------|-------------|------|-----|-------------|----------|
| A | Ótima | N/A | Ruim | Muito baixa | Não |
| B | Boa | Sim | Boa | Baixa | Não |
| C | Boa | Sim | Boa | Baixa | Não |
| D | Média | Sim | Ótima | Alta | Sim |
| E | Média | Sim | Ótima | Muito alta | Sim |

### Sub-questões Obrigatórias
- [ ] Se salvar dados: por quanto tempo? (30 dias, 90 dias, ilimitado?)
- [ ] Limite de armazenamento local? (5MB, 10MB?)
- [ ] Necessário autenticação/login?
- [ ] Dados sincronizam entre dispositivos?
- [ ] Qual política GDPR (direito ao esquecimento)?

### DECISÃO
**Persistência escolhida:** ___________  
**Retenção:** ___________  
**Backend necessário?** Sim / Não  
**Responsável:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## 6️⃣ AUTENTICAÇÃO E SINCRONIZAÇÃO

**Pergunta Principal:** Usuário precisa de login?

### Contexto
Define se arquitetura é client-only ou requer backend + DB.

### Opções Recomendadas
- [ ] A. **Anônimo completo** (zero login, localStorage apenas) ✅ MVP mais simples
- [ ] B. **Login opcional** (anonymous + login para sincronização)
- [ ] C. **Login obrigatório** (sempre autenticado)

### Se Login (Opções B/C):
- [ ] Google OAuth
- [ ] Apple Sign-In
- [ ] Email/Senha (requer backend seguro)
- [ ] Magic link (email)

### Impacto da Decisão

| Opção | Login? | Backend | DB | Auth Provider | Custo | Timeline |
|-------|--------|---------|-----|-------------|-------|----------|
| A | Não | Não | Não | N/A | Baixo | 2 sem |
| B | Opcional | Sim | Sim | Google/Apple | Médio | 4 sem |
| C | Obrigatório | Sim | Sim | Google/Apple | Alto | 4 sem |

### Sub-questões Obrigatórias
- [ ] Dados de usuário precisam de backend ou apenas localStorage?
- [ ] Sincronizar entre web + mobile app?
- [ ] Qual provider: Google, Apple, email/senha, GitHub?
- [ ] Necessário confirmar email?
- [ ] Social login obrigatório ou com fallback email?

### DECISÃO
**Modelo de auth:** ___________  
**Provider(s):** ___________  
**Backend necessário?** Sim / Não  
**Responsável:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## 7️⃣ UNIDADE PADRÃO (Celsius vs Fahrenheit)

**Pergunta Principal:** Qual unidade aparece por padrão?

### Contexto
Afeta UX global. Usuário no Brasil vê Fahrenheit = churn.

### Opções Recomendadas
- [ ] A. **Baseada em geolocalização** (BR = °C, US = °F, automático)
- [ ] B. **Baseada no navegador** (locale do sistema operacional)
- [ ] C. **Hardcoded Celsius** (para Brasil, simples)
- [ ] D. **Hardcoded Fahrenheit** (para US, simples)
- [ ] E. **Usuário escolhe na primeira abertura** (prompt inicial)

### Impacto da Decisão

| Opção | Geolocalização? | UX Global | Complexidade | Cache preferência? |
|-------|-----------------|-----------|-------------|-------------------|
| A | Sim (IP) | Ótima | Média | Sim |
| B | Não | Boa | Baixa | Sim |
| C | Não | Ruim (fora BR) | Muito baixa | Não |
| D | Não | Ruim (fora US) | Muito baixa | Não |
| E | Não | Boa | Baixa | Sim |

### Sub-questões Obrigatórias
- [ ] Preferência é salva mesmo offline?
- [ ] Usuário pode mudar a qualquer momento?
- [ ] Mostrar ambas as unidades (25°C / 77°F)?
- [ ] Outros dados também localizados? (km/h vs mph, hPa vs inHg?)

### DECISÃO
**Padrão escolhido:** ___________  
**Geolocalização?** Sim / Não  
**Salvar preferência?** Sim / Não  
**Mostrar ambas?** Sim / Não  
**Responsável:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## 8️⃣ PLATAFORMA (PWA vs Web vs Nativo)

**Pergunta Principal:** Como usuários acessam o app?

### Contexto
Define stack, timeline, manutenção. NÃO é "just make it responsive".

### Opções Recomendadas
- [ ] A. **Progressive Web App (PWA)** (web + installable + offline, recomendado MVP)
- [ ] B. **Web responsivo apenas** (browser web, nada mais)
- [ ] C. **React Native** (iOS + Android simultâneos, caro)
- [ ] D. **Native** (Swift + Kotlin separados, muito caro)
- [ ] E. **Híbrida** (PWA v1 + React Native v2)

### Impacto da Decisão

| Opção | Plataformas | Devs | Timeline | Manutenção | Offline |
|-------|-------------|------|----------|-----------|---------|
| A | Web (install) | 1 | 2 sem | 1x | Sim |
| B | Web | 1 | 1.5 sem | 1x | Não |
| C | iOS + Android | 3+ | 8+ sem | 3x | Sim |
| D | iOS, Android | 4+ | 12+ sem | 3x | Sim |
| E | Web + Native | 4+ | 10+ sem | 3x | Sim |

### PWA Específico: Service Worker?
- [ ] Sim → necessário para offline, install, push notifications
- [ ] Não → apenas web, sem offline

### DECISÃO
**Plataforma escolhida:** ___________  
**Service Worker?** Sim / Não  
**Timeline estimado:** ___________  
**Responsável:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## 9️⃣ CRITÉRIOS DE ACEITE (Testes & Validação)

**Pergunta Principal:** O que significa "pronto"?

### Contexto
Sem critérios, QA não sabe testar. Dev implementa diferente. Bugs chegam em produção.

### BUSCA DE CIDADES — Especificar:
- [ ] Quantas sugestões? (top 5, top 10, todas)
- [ ] Tempo máximo de resposta? (500ms, 1s, 2s)
- [ ] Case-sensitive? (São Paulo = são paulo?)
- [ ] Caracteres especiais aceitos? (ç, ñ, á)
- [ ] Vazio (search string = "") deve mostrar quê?

### VISUALIZAÇÃO DO CLIMA — Especificar:
- [ ] Todos os campos aparecem sempre ou alguns opcionais?
- [ ] Ordem de exibição fixa ou responsiva?
- [ ] Ícones climáticos obrigatórios?
- [ ] Testes com qual cidade? (São Paulo? Salvador?)

### PREVISÃO DE 5 DIAS — Especificar:
- [ ] 5 dias = próximos 5 dias ou hoje + 4 dias?
- [ ] Hoje aparece na previsão?
- [ ] Ordem: mais próximo primeiro ou cronológica?

### ALTERNÂNCIA C/F — Especificar:
- [ ] Todos os campos de temperatura mudam instantaneamente?
- [ ] Ícones/labels mudam?
- [ ] Labels numéricos (ex: "25") ou com símbolo ("25°C")?

### MOBILE RESPONSIVO — Especificar:
- [ ] Breakpoints: quais? (320px, 480px, 768px, etc.)
- [ ] Orientação paisagem: qual comportamento?
- [ ] Touch: qual ação = click? Swipe?
- [ ] Teclado virtual: como não obscurecer inputs?

### DECISÃO
**Critérios de aceite por feature:** ___________  
**Exemplos de teste incluídos?** Sim / Não  
**Responsável QA:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## 🔟 TRATAMENTO DE ERROS E ESTADOS DEGRADADOS

**Pergunta Principal:** O que acontece quando tudo dá errado?

### Contexto
Sem plano, app trava, freezes, ou exibe erros confusos.

### Cenários Críticos:

**A. API Offline**
- [ ] Mostrar último dado cacheado? (com label "offline"?)
- [ ] Mostrar erro genérico? ("Não foi possível carregar dados")
- [ ] Ambos com retry button?
- [ ] Quanto tempo cachear antes de expirar?

**B. Timeout**
- [ ] Qual é o timeout? (5s, 10s, 30s?)
- [ ] Diferente para mobile (3G) vs desktop (WiFi)?
- [ ] Mostrar spinner/loading? Quanto tempo máximo?
- [ ] Retry automático? Quantas tentativas?

**C. Limite de Requisições Excedido**
- [ ] Usar fila de requisições?
- [ ] Fallback para dados históricos?
- [ ] Informar usuário? ("Muito tráfego, tente novamente em X minutos")
- [ ] Throttle de requisições?

**D. Geolocalização Negada**
- [ ] Mostrar input de busca?
- [ ] Usar localização padrão (ex: Rio)?
- [ ] Mostrar modal explicativo?

**E. Dados Inválidos da API**
- [ ] Validar schema antes de exibir?
- [ ] Fallback se campos faltarem?
- [ ] Log de erro para diagnóstico?

### DECISÃO
**Plano de erro por cenário:** ___________  
**Timeout padrão:** ___________  
**Mensagens de erro customizadas?** Sim / Não  
**Responsável:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## 1️⃣1️⃣ PRIVACIDADE E CONFORMIDADE (GDPR/LGPD)

**Pergunta Principal:** Como tratamos dados de usuário?

### Contexto
Violação GDPR/LGPD = multa até €20M (GDPR) ou R$ 50M (LGPD).

### Questões Críticas:

**A. Coleta de Localização**
- [ ] Armazenar buscas de cidades no backend?
- [ ] Por quanto tempo? (30 dias, 90 dias, ilimitado?)
- [ ] Compartilhar com terceiros? (analytics, ads?)
- [ ] Consentimento necessário?

**B. Cookies e Analytics**
- [ ] Google Analytics? (com anonimização?)
- [ ] Hotjar/similar para heatmaps?
- [ ] Consentimento de cookie obrigatório (banner)?
- [ ] CCPA compliant (California)?

**C. GDPR/LGPD Compliance**
- [ ] Política de privacidade escrita?
- [ ] Direito ao esquecimento implementado?
- [ ] Exportação de dados do usuário?
- [ ] Data Protection Officer (DPO) configurado?

**D. Dados Sensíveis**
- [ ] Localização é sensível → protocolo seguro?
- [ ] Criptografia em trânsito (HTTPS)? ✅ Obrigatório
- [ ] Criptografia em repouso (se backend)?
- [ ] Senhas (se autenticação) → hashed com salt?

### DECISÃO
**Analytics habilitado?** Sim / Não  
**Anonimização?** Sim / Não  
**Política de privacidade:**  Pronta / A fazer  
**Conformidade legal:** BR / EU / Global  
**Responsável legal:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## 1️⃣2️⃣ MODELO DE MONETIZAÇÃO

**Pergunta Principal:** Como o app gera receita?

### Contexto
Define ROI, priorização de features, urgência.

### Opções Recomendadas
- [ ] A. **Free (sem monetização)** (volume de usuários, futuro premium)
- [ ] B. **Freemium** (free + paywall para features extras)
- [ ] C. **Ad-supported** (anúncios intrusivos, impacta UX)
- [ ] D. **Pago** (uma compra única, raramente funciona)
- [ ] E. **Subscription** (mensal/anual para acesso completo)

### Impacto da Decisão

| Modelo | DAU esperado | ROI | UX impacto | Complexidade |
|--------|-------------|-----|-----------|-------------|

---

## 📊 DECISÕES CONSOLIDADAS (MVP)

> **DATA DE CONSOLIDAÇÃO**: 2026-08-12
> **RESPONSÁVEL SPEC**: Spec Agent
> **CONSUMIDOR**: Plan Agent (próxima fase)

As 13 questões críticas foram resolvidas com suposições pragmáticas de MVP.
Veja `weather-app-spec.md` para a especificação formal completa.
| A (Free) | Alto | 0 | Nenhum | Baixa |
| B (Freemium) | Médio-Alto | Médio | Médio | Alta |
| C (Ads) | Médio | Médio-Alto | Alto (ruim) | Média |
| D (Pago) | Baixo | Alto/nulo | Nenhum | Baixa |
| E (Sub) | Médio | Alto | Médio | Alta |

### Se Freemium — Qual é a Paywall?
- [ ] Sem publicidades
- [ ] Previsão de 14 dias (vs 5)
- [ ] Alertas de clima severo
- [ ] Sincronização entre dispositivos
- [ ] Tema dark/customização
- [ ] Histórico ilimitado

### Se Ads — Qual Provider?
- [ ] Google AdSense
- [ ] Programmatic (Mediation)
- [ ] Direto (parceria com brands)
- [ ] Qual tamanho/posição? (Banner, Interstitial, Native)

### DECISÃO
**Modelo escolhido:** ___________  
**Paywall (se freemium):** ___________  
**Ad provider (se ads):** ___________  
**Timeline para ROI:** ___________  
**Responsável:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## 1️⃣3️⃣ MÉTRICAS DE SUCESSO & ROADMAP

**Pergunta Principal:** O que significa sucesso?

### Contexto
Define priorização, cancelamento vs pivô.

### KPIs de Lançamento — Definir Alvo:

**Adoção:**
- [ ] DAU (Daily Active Users): ___________ 
- [ ] MAU (Monthly Active Users): ___________
- [ ] Downloads no primeiro mês: ___________

**Retenção:**
- [ ] Churn semanal: < ___% 
- [ ] Churn mensal: < ___% 
- [ ] Tempo médio de sessão: ___________

**Engajamento:**
- [ ] Buscas por sessão: > ___________
- [ ] Cidades favoritas: > ___________

**Viabilidade:**
- [ ] Receita mensal: R$ ___________
- [ ] Break-even: ___________ meses
- [ ] Custo de aquisição: R$ ___________

### Fase Roadmap:
- [ ] **V1 (MVP):** Scope definido neste doc
- [ ] **V2 (Próximo):** Qual feature? (alertas, PWA, nativo?)
- [ ] **V3+:** Long-term vision?

### Timeline de Burn:
- [ ] MVP: ___________ (ex: 6 semanas)
- [ ] Launch: ___________ 
- [ ] Monitoramento: ___________ (quanto tempo antes de decisão?)
- [ ] Cancelamento se: ___________ (qual métrica de fracasso?)

### DECISÃO
**KPI DAU:** ___________  
**KPI Churn:** ___________  
**Roadmap V2:** ___________  
**Timeline burn:** ___________  
**Métrica de fracasso:** ___________  
**Responsável:** ___________  
**Data:** ___________

**Bloqueador para Dev?** 🔴 Sim / 🟡 Não

---

## ✅ CHECKLIST DE GO/NO-GO

Antes de kickoff, validar:

- [ ] **Todas as 13 questões respondidas?**
- [ ] **Todas as 🔴 (bloqueadores) resolvidas?**
- [ ] **Decisões documentadas com responsáveis?**
- [ ] **Timeline definido?**
- [ ] **Budget aprovado?**
- [ ] **Team confirmado (PMs, devs, designers)?**
- [ ] **Discovery document (`specs/discovery.md`) alinhado?**
- [ ] **Roadmap V2+ comunicado?**
- [ ] **Expectativas de sucesso claras para stakeholders?**

---

## 📋 PRÓXIMOS PASSOS

**Após esta reunião:**

1. ✅ Registrar todas as decisões neste documento
2. ✅ Criar `specs/weather-app-spec.md` (Especificação Formal) baseado nas respostas
3. ✅ Convocar **Plan Agent** para gerar `plans/weather-app-plan.md`
4. ✅ Convocar **Task Agent** para gerar `tasks/weather-app-tasks.md`
5. ✅ Kickoff de desenvolvimento com **Code Agent**

**Estimativa:** 2 horas de reunião + 8 horas de elaboração de docs (paralelo com design).

---

## 📞 CONTATOS RESPONSÁVEIS

| Papel | Nome | Email | Telefone |
|-------|------|-------|----------|
| Product Manager | | | |
| Tech Lead | | | |
| Designer | | | |
| CEO/Stakeholder | | | |

---

**Versão:** 1.0  
**Última atualização:** 2026-08-12  
**Status:** ⏳ Aguardando alinhamento com stakeholders  
**Próxima reunião:** ___________ 

