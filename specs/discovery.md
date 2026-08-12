# Weather App — Especificação Formal v1.0

**Data**: 2026-08-12  
**Versão**: 1.0 (MVP)  
**Status**: ✅ Pronta para Plan Agent  
**Consumidor Próximo**: Plan Agent (`plans/weather-app-plan.md`)

---

## 🎯 1. VISÃO GERAL

### Objetivo do Produto

Fornecer aos usuários brasileiros um **aplicativo web de previsão do tempo moderno, acessível e rápido** que permita:
- Consultar o clima atual de qualquer cidade brasileira
- Visualizar previsão de 5 dias com temperaturas mín/máx
- Alternar entre Celsius e Fahrenheit
- Usar offline com dados cacheados
- Acessar de qualquer dispositivo (desktop, tablet, mobile)

### Público-Alvo

1. **Usuários Primários**: Brasileiros urbanos (16-65 anos) que necessitam de informações meteorológicas rápidas
   - Planejadores diários (saída, roupa, transporte)
   - Viajantes consultando clima de destino
   - Curiosos sobre condições locais
   
2. **Usuários Secundários**:
   - Jornalistas/meteorologistas
   - Agricultores/profissionais ao ar livre

### Proposta de Valor

| Diferencial | Benefício |
|------------|-----------|
| **PWA installável** | Acessa como app nativo (home screen, offline) |
| **Sem login necessário** | Uso imediato, zero fricção |
| **Open-Meteo (free)** | Zero custo de API, dados precisos |
| **Dark glassmorphism UI** | Moderno, atraente, acessível |
| **Funciona offline** | Confiável mesmo sem internet |
| **Rápido** | < 2s carregamento, bundle < 200KB |

---

## 🧭 2. DECISÕES

### 2.1. Fonte de dados: Open-Meteo

**Decisão**: usar a API do Open-Meteo para geocodificação e previsão do tempo, sem necessidade de chave de API.

**Justificativa**: o Open-Meteo fornece dados meteorológicos confiáveis, é gratuito para uso em MVP e elimina a necessidade de autenticação ou infraestrutura de backend.

**O que resolve**: fecha a incerteza sobre a fonte de dados, reduz o custo e a complexidade de integração e garante que o produto funcione sem credenciais externas.

### 2.2. "5 dias" = hoje + 4 dias

**Decisão**: a seção de previsão de 5 dias cobrirá o dia atual e mais quatro dias seguintes, totalizando cinco registros na tela.

**Justificativa**: essa definição é clara para usuários e compatível com a expectativa de uma previsão de curto prazo sem ambiguidade sobre contagem de dias.

**O que resolve**: elimina a dúvida sobre se "5 dias" significa a partir de amanhã ou incluindo o dia atual, padronizando a renderização e os critérios de aceite.

### 2.3. Unidade padrão: Celsius

**Decisão**: a interface usa Celsius como unidade padrão, com possibilidade de alternar para Fahrenheit via toggle.

**Justificativa**: o público-alvo é brasileiro e a unidade mais comum no país será a default para reduzir fricção e melhorar a experiência inicial.

**O que resolve**: define a unidade inicial da aplicação, a conversão exigida, a persistência da preferência do usuário e a expectativa de comportamento da tela.

### 2.4. Sem autenticação e sem persistência de servidor

**Decisão**: o app não requer login e os dados persistentes serão mantidos no cliente (localStorage/cache local), sem backend próprio.

**Justificativa**: essa abordagem mantém o MVP simples, reduz custo operacional e é suficiente para cenários como favoritos, histórico e preferências de unidade sem infraestrutura de servidor.

**O que resolve**: fecha a necessidade de autenticação, de banco de dados do lado do servidor e de sincronização remota, definindo o modelo de persistência e a arquitetura do produto.

### 2.5. Idioma da UI: pt-BR

**Decisão**: toda a interface será em português do Brasil, incluindo labels, textos de ajuda, descrições de clima e textos de status.

**Justificativa**: o app tem foco em usuários brasileiros e a experiência fica mais natural, acessível e alinhada ao mercado local.

**O que resolve**: define o idioma do produto, remove ambiguidades sobre localização e linguagem da interface e orienta a tradução dos rótulos, mensagens e datas.

---

## 👥 3. HISTÓRIAS DE USUÁRIO E CRITÉRIOS DE ACEITE

### HU1: Buscar Cidades (Autocomplete)

**Narrativa**
> Como usuário, quero buscar uma cidade por nome para visualizar o clima dessa localidade.

**Critérios de Aceite**

- [ ] Campo de busca está visível e acessível na tela inicial
- [ ] Ao digitar 2+ caracteres, sistema exibe até 10 sugestões de cidades brasileiras
- [ ] Sugestões aparecem em < 500ms (performance)
- [ ] Sugestões são ordenadas por relevância (proximidade do texto digitado)
- [ ] Usuário pode navegar sugestões com teclado (↑↓ arrows, Enter para selecionar)
- [ ] Clicando em uma sugestão, a cidade fica selecionada como "localização atual"
- [ ] Campo de busca limpa após seleção
- [ ] Pesquisa é case-insensitive (São paulo = SÃO PAULO)
- [ ] Caracteres especiais são suportados (ç, ã, é, etc.)
- [ ] Vazio ou menos de 2 chars = sem sugestões (placeholder "Digite a cidade...")
- [ ] Cidades com nomes duplicados (ex: 3x "Rio") mostram estado em parênteses (Rio - SP)

**Exemplos**

```
Input: "são"
Output:
  - São Paulo (SP)
  - São Gonçalo (RJ)
  - São Bernardo do Campo (SP)
```

**Limite de Taxa de Requisição**
- Máx 1 requisição por 200ms (debounce)
- Máx 100 requisições/dia por usuário (fallback: sugestões em cache local)

---

### HU2: Visualizar Clima Atual

**Narrativa**
> Como usuário, quero ver o clima atual da cidade selecionada (temperatura, condição, umidade, vento) para me planejar.

**Critérios de Aceite**

- [ ] Após selecionar uma cidade, clima atual carrega em < 1s (após debounce)
- [ ] Card de clima atual exibe:
  - Temperatura (em C ou F, conforme preferência)
  - Ícone representativo (céu limpo, nublado, chuva, etc.)
  - Descrição da condição ("Céu limpo", "Nublado", "Chuva leve", etc.)
  - Umidade (%)
  - Velocidade do vento (km/h ou mph)
  - Sensação térmica (wind chill)
  - Nascer e pôr do sol (HH:mm em timezone local)
  - Cidade + Estado (ex: "São Paulo, SP")
  - Timestamp de última atualização (ex: "Atualizado há 2 min")
- [ ] Card está centralizado e visível em todas as resoluções (mobile 320px+, desktop)
- [ ] Ao clicar em ícone "refresh", dados se atualizam em < 1s
- [ ] Se API falha, mostra último dado em cache com label "Offline" (em destaque)
- [ ] Se nenhum dado anterior, erro genérico: "Não foi possível carregar. Tente novamente."

**Exemplos**

```
Cidade: São Paulo, SP
Temperatura: 25°C (sensação térmica: 23°C)
Umidade: 65%
Vento: 12 km/h
Nascer do sol: 06:30
Pôr do sol: 18:15
Condição: Céu limpo ☀️
Atualizado: agora
```

**Precisão de Dados**
- Temperatura: inteira (25°C, não 25.33°C)
- Vento: inteiro (12 km/h)
- Umidade: inteira (65%)

---

### HU3: Visualizar Previsão de 5 Dias

**Narrativa**
> Como usuário, quero ver a previsão de temperatura (mín/máx) para os próximos 5 dias para planejar minhas atividades.

**Critérios de Aceite**

- [ ] Previsão aparece abaixo do clima atual (seção separada com header "Próximos 5 dias")
- [ ] Exibe 5 cards/linhas, um por dia
- [ ] Cada card mostra:
  - Data (formato DD/MM)
  - Dia da semana (Sex, Sáb, Dom, etc. — em pt-BR)
  - Temperatura mínima (ex: 18°C)
  - Temperatura máxima (ex: 28°C)
  - Ícone da condição prevista
  - Descrição curta (ex: "Parcialmente nublado")
  - Chance de chuva (%) (se disponível na API)
- [ ] Dias são em ordem cronológica (mais próximo primeiro)
- [ ] Em mobile (< 768px), cards ocupam width 100% com scroll horizontal ou stack vertical
- [ ] Em desktop, cards ficam em linha (responsive grid)
- [ ] Ao tocar/clicar em um dia, expande para mostrar previsão horária (nice-to-have v2)

**Exemplos**

```
Próximos 5 dias

Qui 13/08        Sex 14/08        Sáb 15/08
18-28°C          20-30°C          19-27°C
Céu limpo ☀️      Parcial nublado  Chuva leve 🌧️
Chuva: 0%        Chuva: 10%       Chuva: 60%
```

---

### HU4: Alternar Celsius/Fahrenheit

**Narrativa**
> Como usuário, quero alternar entre Celsius e Fahrenheit para ver temperaturas na minha unidade preferida.

**Critérios de Aceite**

- [ ] Toggle ou botão "°C / °F" está visível no header
- [ ] Clicando, todas as temperaturas na página (clima atual + previsão) mudam instantaneamente
- [ ] Conversão é matemática correta: F = (C × 9/5) + 32
- [ ] Preferência é salva em localStorage (persiste após fecha/reabre)
- [ ] Sensação térmica também converte
- [ ] Labels adicionam o símbolo (°C ou °F) automaticamente
- [ ] Ao primeiro carregamento, padrão é °C (localization do Brasil)

**Exemplos**

```
Antes (C):  25°C → Sensação: 23°C
Depois (F): 77°F → Sensação: 73°F
```

---

### HU5: Salvar Cidades Favoritas

**Narrativa**
> Como usuário, quero salvar minhas cidades favoritas para acessá-las rapidamente sem ter que digitar a busca novamente.

**Critérios de Aceite**

- [ ] Botão "❤️ Favoritar" (ou ⭐) está no card de clima atual
- [ ] Ao clicar, cidade é salva em localStorage (lista de favoritos)
- [ ] Máximo 5 favoritos (após 5º, botão fica desabilitado com tooltip "Máximo 5 favoritos")
- [ ] Favoritos aparecem como pills/chips abaixo do campo de busca
- [ ] Clicando em um pill, carrega clima dessa cidade imediatamente
- [ ] Ícone "X" no pill remove o favorito
- [ ] Reordenação de favoritos: drag-and-drop (nice-to-have) ou ordem de adição
- [ ] Se remover todos os favoritos, seção desaparece
- [ ] Favoritos persistem após fechar/reabrir app

**Exemplos**

```
[♥ São Paulo] [♥ Rio de Janeiro] [♥ Brasília] [♥ Salvador]
```

---

### HU6: Histórico de Buscas

**Narrativa**
> Como usuário, quero ver meu histórico de cidades recentemente consultadas para acessá-las rapidamente.

**Critérios de Aceite**

- [ ] Histórico exibe até 10 últimas cidades buscadas (em ordem reversa: mais recente primeiro)
- [ ] Histórico aparece dropdown ao focar no campo de busca (vazio)
- [ ] Clicando em uma cidade do histórico, carrega clima imediatamente
- [ ] Botão "Limpar histórico" remove todos (com confirmação: "Tem certeza?")
- [ ] Histórico persiste em localStorage
- [ ] Após buscar mesma cidade 2x, não duplica (move para topo)
- [ ] Máximo 10 itens (nova busca remove a mais antiga)

---

### HU7: Suportar Acesso Offline (PWA + Service Worker)

**Narrativa**
> Como usuário, quero acessar minha última consulta de clima mesmo quando estou sem internet.

**Critérios de Aceite**

- [ ] Quando offline, app carrega última cidade consultada (do cache)
- [ ] Card de clima exibe label "📴 Offline — dados de XX min atrás"
- [ ] Botão "Refresh" mostra spinner + "Aguardando conexão..."
- [ ] Ao reconectar, atualiza automaticamente sem ação do usuário
- [ ] Service Worker está registrado e atualiza assets (cache busting)
- [ ] App é installável como PWA (chrome mobile: "Instalar app")
- [ ] Ícone do app no home screen leva à app
- [ ] Splash screen customizado ao abrir (nome + logo)
- [ ] Suporta modo full-screen (sem browser chrome)

**Limite de Cache**
- Dados climáticos cacheados por 30 minutos
- Assets (HTML, CSS, JS) cacheados por versão (cache busting)
- Favoritos e histórico em localStorage (ilimitado até 5MB)

---

## 📊 3. MODELO DE DADOS

### TypeScript Interfaces (Definições Globais)

```typescript
// Tipo: Localização de Cidade
interface City {
  id: string; // "city_3448439" (ID único de Open-Meteo)
  name: string; // "São Paulo"
  state: string; // "SP"
  country: string; // "BR"
  latitude: number; // -23.5505
  longitude: number; // -46.6333
  timezone: string; // "America/Sao_Paulo"
  population?: number; // 12.3M (opcional, para ranking em search)
}

// Tipo: Condição Climática
interface WeatherCondition {
  code: number; // WMO code (0=clear, 1=mainly clear, 80=rain showers, etc.)
  description: string; // "Céu limpo", "Nublado", "Chuva leve"
  icon: string; // "☀️", "☁️", "🌧️" (Unicode ou URL de ícone)
}

// Tipo: Clima Atual
interface CurrentWeather {
  cityId: string; // FK para City
  temperature: number; // 25 (inteiro em C ou F)
  feelsLike: number; // 23 (sensação térmica)
  humidity: number; // 65 (%)
  windSpeed: number; // 12 (km/h ou mph)
  condition: WeatherCondition; // {code, description, icon}
  pressure?: number; // 1013 (hPa, opcional)
  visibility?: number; // 10 (km, opcional)
  sunrise: string; // "06:30" (HH:mm em timezone local)
  sunset: string; // "18:15" (HH:mm em timezone local)
  timestamp: Date; // ISO timestamp de quando foi atualizado
  isCached: boolean; // true se veio de cache (offline)
  cacheAge?: number; // minutos desde update (ex: 2)
}

// Tipo: Previsão Diária
interface DailyForecast {
  date: string; // "2026-08-14" (YYYY-MM-DD)
  dayOfWeek: string; // "Sexta" (em pt-BR)
  tempMin: number; // 18 (inteiro)
  tempMax: number; // 28 (inteiro)
  condition: WeatherCondition;
  precipitation: number; // 2.5 (mm)
  precipitationProbability: number; // 60 (%)
  windSpeedMax: number; // 15 (km/h)
}

// Tipo: Previsão Completa (5 Dias)
interface Forecast {
  cityId: string; // FK para City
  days: DailyForecast[]; // Array de 5 elementos
  timestamp: Date; // Quando foi atualizado
}

// Tipo: Preferências do Usuário (localStorage)
interface UserPreferences {
  temperatureUnit: "C" | "F"; // Padrão: "C"
  language: "pt-BR" | "en-US"; // Padrão: "pt-BR"
  currentCityId?: string; // Última cidade visitada
  favorites: string[]; // Array de city IDs (máx 5)
  searchHistory: string[]; // Array de city IDs (máx 10)
}

// Tipo: Estado da App (Context)
interface AppState {
  selectedCity: City | null;
  currentWeather: CurrentWeather | null;
  forecast: Forecast | null;
  preferences: UserPreferences;
  isLoading: boolean;
  error: string | null; // Mensagem de erro (ou null se ok)
  isOnline: boolean; // Detectado via navigator.onLine
}
```

---

## 🔌 4. ESPECIFICAÇÃO DE API (Open-Meteo)

### Contexto

A aplicação consome dados de **Open-Meteo**, uma API **gratuita, open-source, sem chave de API**, focada em previsão meteorológica e dados geográficos.

**URL Base**: `https://api.open-meteo.com/v1`

---

### Endpoint 1: Buscar Cidades (Geocoding)

**GET** `/v1/geocoding?name={city}&count=10&language=pt`

**Response 200 OK**

```json
{
  "results": [
    {
      "id": 3448439,
      "name": "São Paulo",
      "latitude": -23.5505,
      "longitude": -46.6333,
      "admin1": "São Paulo",
      "country_code": "BR",
      "timezone": "America/Sao_Paulo",
      "population": 12394000
    }
  ]
}
```

---

### Endpoint 2: Clima Atual

**GET** `/v1/weather?latitude={lat}&longitude={lon}&current=temperature_2m,humidity,weather_code,wind_speed_10m&timezone=Auto&temperature_unit=celsius`

**Response 200 OK**

```json
{
  "latitude": -23.55,
  "longitude": -46.63,
  "timezone": "America/Sao_Paulo",
  "current": {
    "time": "2026-08-12T14:30",
    "temperature_2m": 25.0,
    "relative_humidity_2m": 65,
    "apparent_temperature": 23.2,
    "weather_code": 0,
    "wind_speed_10m": 12.5,
    "sunrise": "2026-08-12T06:30",
    "sunset": "2026-08-12T18:15"
  }
}
```

---

### Endpoint 3: Previsão de 5 Dias

**GET** `/v1/forecast?latitude={lat}&longitude={lon}&daily=temperature_2m_max,temperature_2m_min,weather_code&timezone=Auto&forecast_days=5`

**Response 200 OK**

```json
{
  "daily": {
    "time": ["2026-08-13", "2026-08-14", "2026-08-15", "2026-08-16", "2026-08-17"],
    "temperature_2m_max": [28, 30, 27, 25, 26],
    "temperature_2m_min": [18, 20, 19, 17, 18],
    "weather_code": [0, 1, 80, 3, 2]
  }
}
```

---

### WMO Weather Code Mapping

| Código | Condição | Ícone |
|--------|----------|-------|
| 0 | Céu limpo | ☀️ |
| 1-2 | Principalmente limpo | 🌤️ |
| 3 | Nublado | ☁️ |
| 45, 48 | Nevoeiro | 🌫️ |
| 51-67 | Chuva leve | 🌦️ |
| 71-77 | Neve | ❄️ |
| 80-82 | Chuva forte | 🌧️ |
| 95-99 | Tempestade | ⛈️ |

---

## ⚡ 5. REQUISITOS NÃO-FUNCIONAIS

### Performance

| Métrica | Target |
|---------|--------|
| LCP (Largest Contentful Paint) | < 2.5s |
| FID (First Input Delay) | < 100ms |
| CLS (Cumulative Layout Shift) | < 0.1 |
| Carregamento inicial | < 2s |
| Busca de cidades | < 500ms |
| Bundle gzip | < 200KB |

### Acessibilidade (WCAG 2.1 AA)

- [ ] Contraste: 4.5:1 (texto normal)
- [ ] Keyboard navigation: 100% funcional (Tab, Enter, Esc)
- [ ] ARIA labels: em todos os botões/inputs
- [ ] Screen reader compatible: sem erros
- [ ] Zoom até 200% sem horizontal scroll

### Segurança

- [ ] HTTPS obrigatório
- [ ] CSP (Content Security Policy) configurada
- [ ] Validação de input (sem XSS)
- [ ] localStorage sem dados sensíveis

### Responsividade

| Tamanho | Breakpoint |
|---------|-----------|
| Mobile | 320px - 480px |
| Tablet | 481px - 1024px |
| Desktop | 1025px+ |

### Internacionalização

- Suporte: PT-BR, EN-US (v1.0)
- Formatos: Data (DD/MM/YYYY), decimal (,), unidades (km/h)

### PWA

- [ ] Service Worker registrado
- [ ] Installable (manifest.json)
- [ ] Offline-first (cache-first strategy)
- [ ] Splash screen customizado

---

## 🔄 6. FLUXOS DE INTERAÇÃO

### Happy Path (Busca → Visualização)

```
1. Usuário abre app
2. App carrega última cidade (localStorage)
3. Busca clima atual + previsão de 5 dias (Open-Meteo) < 2s
4. Exibe clima + previsão
5. Usuário alterna C/F
6. Temperatura muda instantaneamente
7. Usuário favorita a cidade
8. Cidade salva em localStorage
```

### Fluxo Offline

```
1. Usuário online: consulta São Paulo
2. Dados cacheados por 30 min
3. Rede cai
4. Ao buscar Rio: fallback para cache
5. Label "📴 Offline" aparece
6. Botão "Retry" disponível
7. Ao reconectar: atualiza automaticamente
```

---

## ✅ 7. CRITÉRIOS DE ACEITE GLOBAIS

### Testes

- [ ] Cobertura ≥ 80% (branches)
- [ ] E2E: 3 cenários (happy path, offline, favoritos)
- [ ] Lighthouse: ≥ 90 (perf + a11y)

### Lint & Build

- [ ] `pnpm lint` zero warnings
- [ ] `pnpm build` sucesso, bundle < 200KB gzip
- [ ] TypeScript strict mode: zero erros

---

## 🎯 8. SUPOSIÇÕES & RESTRIÇÕES (MVP)

| Item | Decisão |
|------|---------|
| **Escopo Geográfico** | Brasil (todas as cidades) |
| **Frequência de Atualização** | 30 min automático + refresh manual |
| **Dados** | Temp, umidade, vento, nascer/pôr |
| **Previsão** | Diária (mín/máx) × 5 dias |
| **Persistência** | Histórico (10) + favoritos (5) localStorage |
| **Autenticação** | Anônimo (sem login) |
| **Unidade Padrão** | °C (geolocalização BR) |
| **Plataforma** | PWA (web + installable) |
| **Monetização** | Free (v2: freemium) |
| **Stack** | React + TypeScript + Tailwind + Vite |
| **Navegadores** | Chrome, Firefox, Safari, Edge (últimas 2 versões) |
| **Rate Limit** | Open-Meteo 10K req/dia (cache agressivo) |

---

## 📖 9. GLOSSÁRIO

| Termo | Definição |
|-------|-----------|
| **PWA** | Progressive Web App (app web installável) |
| **Service Worker** | Web Worker que cria cache offline |
| **WMO Code** | Padrão de códigos climáticos |
| **Debounce** | Delay antes de chamar API (200ms após última digitação) |
| **Rate Limit** | Limite de requisições (10K req/dia Open-Meteo) |
| **Cache Busting** | Versionamento de assets para força atualização |
| **GDPR** | Lei de privacidade (EU/BR) |
| **Timezone** | Fuso horário (ex: America/Sao_Paulo) |
| **WCAG** | Padrão de acessibilidade web |
| **ARIA** | Atributos para acessibilidade |

---

## 🚀 PRÓXIMOS PASSOS

Esta especificação está pronta para **Plan Agent** criar o plano técnico em `plans/weather-app-plan.md`.

**Validação Completa**
- ✅ Sem ambiguidades
- ✅ Decisões MVP definidas
- ✅ Exemplos concretos
- ✅ Critérios de aceite verificáveis
- ✅ Stack técnico documentado
- ✅ Fluxos de interação detalhados

---

**Produzido por**: Spec Agent (SDD)  
**Data**: 2026-08-12  
**Status**: ✅ Pronto para Plan Phase
