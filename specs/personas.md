# Personas — Weather App

**Objetivo:** Definir os usuários-alvo do MVP e suas necessidades específicas para priorizar features e validar design.

**Data:** 2026-08-12  
**Status:** Validado pela discovery

---

## 1️⃣ PERSONA: Ana Silva — Commuter Planejador

### 👤 Perfil Demográfico
- **Idade:** 28-40 anos
- **Profissão:** Executiva / Profissional de escritório
- **Localização:** Zona urbana (São Paulo, Rio, Brasília)
- **Renda:** Média-alta (R$ 6k-12k/mês)
- **Dispositivos:** Smartphone (primário) + Desktop (secundário)
- **Tech-savvy:** Alta (app power user)
- **Frequência de uso:** 2-3x por dia

### 🎯 Objetivo Principal
**"Sair de casa preparada para o dia, sem surpresas do clima."**

Ana acorda e quer saber rapidamente se leva guarda-chuva, jaqueta, ou protetor solar. Ela planeja sua semana considerando clima (reuniões outdoor? vai chover? preciso ar/aquecimento?). Tempo é crítico — tem 5 minutos antes de sair.

### 📱 Contexto de Uso

| Momento | Dispositivo | Ação | Duração | Frequência |
|---------|------------|------|---------|-----------|
| **Manhã (6h-8h)** | 📱 Mobile | Checar clima atual + próximas 2h | 30-60s | Diária |
| **Commute (8h-9h)** | 📱 Mobile | Validar se mudou (chuva chegou?) | 15-30s | Ocasional |
| **Trabalho (14h)** | 💻 Desktop | Planejar fim de semana | 2-3min | 1-2x por semana |
| **Noite (21h)** | 📱 Mobile | Checar amanhã antes de dormir | 30-60s | Diária |

### ✅ Necessidades Específicas

**Informação:**
- Temperatura atual (agora)
- Condição (céu limpo, nublado, chuva, etc.)
- Sensação térmica (wind chill)
- Previsão próximas 2h (vai chover antes de sair?)
- Previsão 5 dias (planejar semana)

**Qualidade:**
- ✅ Informação em < 2 segundos (pijama, pressa)
- ✅ Confiável (não pode errar — consequências sociais)
- ✅ Contexto (não só dado bruto — "Leve guarda-chuva")
- ✅ Notificação (alerta se clima mudar drasticamente)

**UX:**
- ✅ Mobile-first (99% uso em celular)
- ✅ Rápido carregamento (WiFi ou 4G, mas tolerância baixa)
- ✅ Interface clara (não quer aprender, quer resultado)

**Não quer:**
- ❌ Anúncios intrusivos (perde tempo)
- ❌ Dados muito técnicos (radiação UV, hPa desnecessários)
- ❌ Notificações excessivas (apenas mudanças relevantes)
- ❌ Histórico/gamificação (não é hobby, é ferramenta)

### 📊 Métrica de Sucesso (Ana)

| Métrica | Esperado | Impacto da Falha |
|---------|----------|------------------|
| **Tempo de decisão** | < 10 segundos | Abandona para rival mais rápido |
| **Acurácia** | > 90% de confiança | Saiu sem guarda-chuva, choveu = churn |
| **Retenção** | Abrir 5+ dias/semana | "É meu ritual matinal" |
| **Recomendação** | Compartilha com amigas | Marketing orgânico |
| **Notificação útil** | Evita ≥ 1 situação ruim/mês | "Aviso de chuva me salvou da reunião" |

### ⚠️ Critério de Abandono
- ❌ Lentidão > 3s de carregamento
- ❌ Notificação fake (aviso errado 2x seguidas)
- ❌ Muitos anúncios que atrapalham leitura rápida
- ❌ Sem previsão clara (só "pode chover" genérico)

### 💡 Implicações para Design
- Layout minimalista (Ana quer informação, não decoração)
- Botão gigante de refresh rápido
- Card destacado para "hoje"
- Ícones grandes e intuitivos
- Modo claro (Ana lê ao amanhecer, sem light bloat)
- Favoritar cidades (não quer digitar todos os dias)

---

## 2️⃣ PERSONA: Lucas Dias — Ativista Outdoor

### 👤 Perfil Demográfico
- **Idade:** 22-35 anos
- **Profissão:** Personal trainer / Guia turístico / Aventureiro / Ciclista
- **Localização:** Zona urbana + entorno (SG/MG/RJ/interior)
- **Renda:** Média (R$ 3k-8k/mês)
- **Dispositivos:** Smartphone (quase exclusivamente, 95%+ uso)
- **Tech-savvy:** Média (conhece alguns apps, quer robustez)
- **Frequência de uso:** 4-5x por dia, intenso

### 🎯 Objetivo Principal
**"Planejar atividades outdoor sem risco, com máximo conforto e performance."**

Lucas vai caminhar, fazer trail running, pedalar, surfar ou guiar turistas. Precisa dados exatos: temperatura, vento (velocidade + direção), UV (queimadura), chuva hora-a-hora, umidade. Uma previsão errada = cancelamento de atividade ou lesão.

### 📱 Contexto de Uso

| Momento | Dispositivo | Ação | Duração | Frequência |
|---------|------------|------|---------|-----------|
| **Noite antes (19h-21h)** | 📱 Mobile | Planejar rota com 5 dias antecedência | 5-10min | Quinzenal (fins de semana) |
| **Manhã cedo (5h-6h)** | 📱 Mobile | Check clima + 24h detalhado | 2-3min | Diária (treino) |
| **Durante atividade (7h-11h)** | 📱 Mobile | Verificar previsão hora-a-hora | 30-60s cada | A cada 2h (sem sinal) |
| **Pós-atividade (12h)** | 📱 Mobile | Registrar como clima afetou performance | 1-2min | Semanal |

### ✅ Necessidades Específicas

**Informação:**
- Previsão hora-a-hora (não diária)
- Velocidade do vento (km/h) + direção (N, NE, etc.)
- Índice UV (risco de queimadura)
- Temperatura exata (sensação térmica)
- Umidade relativa
- Probabilidade de chuva (%)
- Precipitação esperada (mm)
- Visibilidade

**Qualidade:**
- ✅ Granular: hora-a-hora (não resumida)
- ✅ Técnica: dados brutos, sem suavização
- ✅ Offline: funciona em trilha sem sinal
- ✅ Histórico: registrar + comentar clima vs performance
- ✅ Compartilhamento: enviar previsão para clientes/amigos

**UX:**
- ✅ Dados compactos mas legíveis (pequeno = precisa zoom)
- ✅ Gráficos simples (vento, chuva por hora)
- ✅ Modo escuro (uso ao amanhecer, noite)
- ✅ Responsivo em 3G (vai para floresta/montanha)

**Não quer:**
- ❌ UI complexa (precisa de rápido acesso em trilha)
- ❌ Informações desnecessárias (pólen, qualidade do ar = low priority)
- ❌ Publicidade (distrai durante atividade)
- ❌ Animações/transições (consome banda em 3G)

### 📊 Métrica de Sucesso (Lucas)

| Métrica | Esperado | Impacto da Falha |
|---------|----------|------------------|
| **Acurácia por hora** | > 85% | "Não confio — vou usar rival" |
| **Offline disponível** | Funciona sem conexão | Sem app em montanha = problema |
| **Dados técnicos** | Todos os 7 campos | Não consegue tomar decisão técnica |
| **Performance** | App responsivo em 3G | Trava durante atividade = churn |
| **Recomendações salvam** | Evita cancelamento | "Já saí preparado, não voltei" |
| **Detalhe por hora** | Não generaliza (ex: "pode chover") | Planejamento impreciso |
| **Histórico rastreável** | Correlação clima/performance | Melhora treino |

### ⚠️ Critério de Abandono
- ❌ Sem previsão horária (só diária = inútil para atividade)
- ❌ Sem offline (vai para trail, sem app = churn)
- ❌ Lento em 3G (carrega > 5s = frustrante)
- ❌ Sem histórico (não consegue analisar padrões)
- ❌ Dados imprecisos (hora errada = saiu no horário errado, choveu)

### 💡 Implicações para Design
- Gráficos hora-a-hora (vento, chuva, UV em timeline)
- Dados técnicos destacados (não escondidos)
- Modo offline robusto (cache agressivo)
- Histórico de buscas + comentários
- Compartilhar previsão (link ou SMS)
- Modo dark por padrão (uso ao amanhecer/noite)
- Sem publicidade (distrai)

---

## 3️⃣ PERSONA: Dona Maria — Casual Informada

### 👤 Perfil Demográfico
- **Idade:** 50-70 anos
- **Profissão:** Aposentada / Dona de casa
- **Localização:** Interior / Zona urbana, próximo a família
- **Renda:** Média-baixa (R$ 1.5k-3k/mês, aposentadoria)
- **Dispositivos:** Tablet (primário) + Smartphone (secundário, incômodo)
- **Tech-savvy:** Baixa-média (tem dificuldade, prefere simples)
- **Frequência de uso:** 1-2x por dia, casual

### 🎯 Objetivo Principal
**"Entender o clima para atividades do dia-a-dia, sem estresse ou confusão."**

Maria quer saber se vai chover para regar plantas, pendurar roupas, ir ao mercado ou visitar netos. Não é urgência profissional, é planejamento doméstico. Prefere interface grande, clara, sem jargão. Às vezes checa previsão para filhos/netos em outras cidades.

### 📱 Contexto de Uso

| Momento | Dispositivo | Ação | Duração | Frequência |
|---------|------------|------|---------|-----------|
| **Manhã (9h-10h)** | 📱 Tablet | Verificar clima de hoje | 1-2min | Diária |
| **Tarde (14h-16h)** | 📱 Tablet | Planejar tarefas domésticas | 1-2min | 3-4x por semana |
| **Noite (19h)** | 📱 Tablet | Conversar com família sobre clima | 2-3min | Ocasional |

### ✅ Necessidades Específicas

**Informação:**
- Temperatura (simples, sem detalhes)
- Condição (vai chover? vai fazer sol? está frio?)
- Recomendação em linguagem simples ("Leve guarda-chuva")
- Previsão 5 dias (planejar semana)
- Cidades de filhos/netos salvos como favoritos

**Qualidade:**
- ✅ Simples: sem jargão técnico (não entende "AQI", "hPa", "UV index")
- ✅ Grande: letras ≥ 18px, ícones gigantes
- ✅ Recomendações: "Leve guarda-chuva" em linguagem simples
- ✅ Acessibilidade: contraste alto, suporte a leitura de tela (WCAG AA)
- ✅ Confiável: sem atualizações que quebrem UI

**UX:**
- ✅ Tablet-friendly (Maria prefere tela grande)
- ✅ Sem tutorial necessário (interface auto-explicativa)
- ✅ Botões grandes (Maria tem artrite, precisão baixa)
- ✅ Sem scroll infinito (Maria quer ver tudo de uma vez)
- ✅ Modo claro (sempre, sem dark mode obrigatório)

**Não quer:**
- ❌ Publicidade confusa (clica acidental, se vê frustrada)
- ❌ Dark mode (prefere claro, dói o olho escuro)
- ❌ Muitos campos (informação = ruído)
- ❌ Notificações (asusta ou confunde)
- ❌ Gamificação (Maria não quer "pontos", quer clima)
- ❌ Termos técnicos (não entende, abandona)

### 📊 Métrica de Sucesso (Maria)

| Métrica | Esperado | Impacto da Falha |
|---------|----------|------------------|
| **Compreensão** | Entende 100% sem ajuda | Precisa ligar para filhos = incômodo |
| **Acessibilidade** | Interface AA (WCAG) | Não consegue ler com óculos antigos |
| **Confiabilidade** | Usa 3-4x por semana | "É meu habitual verificar aqui" |
| **Simplicidade** | Não precisa tutorial | Filhos não precisam ensinar novamente |
| **Satisfação** | Recomenda para amigas | "Falei com amigas no grupo de WhatsApp" |
| **Segurança** | Sem cliques acidentais | Não abre anúncios por engano |

### ⚠️ Critério de Abandono
- ❌ Interface muito pequena (não consegue ler mesmo com óculos)
- ❌ Muitos termos técnicos (não entende, frustra)
- ❌ Publicidade confusa (clica acidental, se sente enganada)
- ❌ App lento/travando em tablet antigo (Apple iPad 2012 precisa funcionar)
- ❌ Dark mode forçado (dói os olhos)
- ❌ Muitos campos (confunde com informação)

### 💡 Implicações para Design
- **Tipografia:** Mínimo 18px (corpo de texto), 24px para título
- **Ícones:** Grandes, bem conhecidos (sol, nuvem, chuva) — sem ambiguidade
- **Layout:** Vertical (sem scroll horizontal), 1 coluna
- **Cores:** Contraste alto (WCAG AAA), sem cinzas
- **Recomendações:** Linguagem simples ("Leve guarda-chuva" vs "AQI 65")
- **Favoritos:** Simples adicionar/remover (1 click)
- **Teste com Maria de verdade** (5+ mulheres 50-70 anos)

---

## 📊 MATRIZ COMPARATIVA

| Aspecto | Ana (Commuter) | Lucas (Outdoor) | Maria (Casual) |
|---------|-----------------|-----------------|-----------------|
| **Frequência** | 2-3x/dia | 4-5x/dia | 1-2x/dia |
| **Duração** | 30-60s | 2-5min | 1-3min |
| **Dispositivo** | 📱 Mobile | 📱 Mobile | 📱 Tablet |
| **Tipo de uso** | Rápido (hábito) | Profundo (técnico) | Casual (informação) |
| **Necessidade crítica** | Velocidade | Granularidade | Simplicidade |
| **Métrica sucesso** | Tempo decisão | Acurácia por hora | Compreensão |
| **Tech-savvy** | Alta | Média | Baixa |
| **Risco maior** | Lentidão | Sem offline | Interface complexa |
| **Engagement** | Alto | Muito alto | Médio |
| **Lifetime Value** | 1-2 anos | 5+ anos | 2-3 anos |
| **Churn by** | Rival mais rápido | Falta dados técnicos | Confusão UI |

---

## 🎯 IMPLICAÇÕES PARA O MVP

### MVP DEVE SATISFAZER:

**ANA (Commuter):**
- ✅ Carregamento rápido (< 2s)
- ✅ Layout mobile-first (responsivo 320px+)
- ✅ Destacar hoje + amanhã
- ✅ Recomendações contextuais ("Leve guarda-chuva")
- ✅ Busca + favoritos

**LUCAS (Outdoor):**
- ✅ Previsão de 5 dias (legível)
- ✅ Dados técnicos (vento, temperatura, umidade)
- ✅ Modo offline funcional
- ✅ Dados de qualidade > animações
- 🔲 Previsão hora-a-hora (V2)
- 🔲 Histórico detalhado (V2)

**MARIA (Casual):**
- ✅ Acessibilidade WCAG AA (mínimo)
- ✅ Letras grandes (18px+ corpo)
- ✅ Ícones gigantes e intuitivos
- ✅ Sem jargão técnico
- ✅ Favoritos salvos
- ✅ Modo claro obrigatório (v1)
- 🔲 Dark mode (V2, opcional)

### V2 PODE ADICIONAR:
- 📲 **Notificações** (Ana + Lucas: "Chuva em 1h")
- 🕐 **Previsão hora-a-hora** (Lucas: visibilidade vs Ana)
- 📊 **Histórico + gráficos** (Lucas: correlação clima/performance)
- 🌙 **Dark mode** (Lucas primeira, Maria opcional)
- 🔔 **Alertas severos** (Lucas: tornado, granizo)

### NUNCA DEVE FAZER:
- ❌ Dark mode obrigatório (Maria abandona)
- ❌ Publicidade intrusiva (todas abandonam)
- ❌ UI complexa com 50 campos (Maria não usa)
- ❌ Sem dados técnicos básicos (Lucas não usa)
- ❌ Termos técnicos sem tradução (Maria confunde)

---

## 🧪 VALIDAÇÃO DE PERSONAS

### Como validar:

1. **Entrevistas com 5-10 usuários por persona** (15-30min cada)
   - Ana: Executivos que abrem app ao acordar
   - Lucas: Athletes/outdoors do Strava/Facebook groups
   - Maria: Grupos de WhatsApp de amigas/aposentadas

2. **Teste de usabilidade (think-aloud)**
   - Ana: medir tempo de decisão (objetivo: < 10s)
   - Lucas: validar dados técnicos suficientes
   - Maria: sem tutorial, consegue usar?

3. **Pesquisa de motivação**
   - Por que abrem o app?
   - O que os frustra?
   - Qual rival usam?

4. **Métrica de sucesso real**
   - Ana: D7 retention (5+ uso semana)
   - Lucas: D30 engagement (4+ uso/dia)
   - Maria: NPS (vai recomendar?)

---

## 📋 PRÓXIMOS PASSOS

1. ✅ Compartilhar personas com design team
2. ✅ Criar **wireframes por persona** (Ana: speed, Lucas: data, Maria: a11y)
3. ✅ Priorizar features no backlog por persona satisfeita
4. ✅ **Testar com personas reais** (antes de dev)
5. ✅ Usar personas em daily standups ("Essa feature ajuda Ana?")

---

**Versão:** 1.0  
**Status:** Validado pela discovery  
**Próxima revisão:** Após testes com usuários reais
