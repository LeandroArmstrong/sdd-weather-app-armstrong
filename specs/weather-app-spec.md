# Weather App — Especificação de Produto

**Versão**: 1.1
**Data**: 2026-08-12
**Status**: Pronta para planejamento técnico
**Entradas**: [specs/discovery.md](./discovery.md), [specs/personas.md](./personas.md), [specs/stakeholder-alignment.md](./stakeholder-alignment.md)
**Consumidor**: Plan Agent

---

## 1. Objetivo

O Weather App é uma PWA para consulta rápida do clima em cidades brasileiras. A aplicação deve permitir busca por cidade, visualização do clima atual, previsão de 5 dias e uso contínuo em cenários com conectividade instável.

O produto deve funcionar sem login, em português do Brasil, com persistência local e com comportamento degradado explícito quando houver falha na rede ou na API.

## 2. Escopo funcional

### 2.1 Requisitos funcionais

| ID | Requisito |
|---|---|
| RF01 | Buscar cidade brasileira por texto com autocomplete a partir de 2 caracteres. |
| RF02 | Exibir clima atual da cidade selecionada com temperatura, sensação térmica, umidade, vento e horários de sol. |
| RF03 | Exibir previsão diária para os próximos 5 dias em ordem cronológica. |
| RF04 | Alternar unidade de temperatura entre °C e °F com conversão imediata na interface. |
| RF05 | Salvar até 5 cidades em favoritos com persistência local. |
| RF06 | Manter histórico das últimas 10 buscas sem duplicatas, com remoção individual de item e limpeza total. |
| RF07 | Exibir dados em cache quando o app estiver offline, com indicador explícito de offline. |
| RF08 | Atualizar manualmente os dados da cidade selecionada. |
| RF09 | Permitir instalação como PWA com manifest e service worker. |
| RF10 | Persistir a preferência de unidade do usuário entre sessões. |

### 2.2 Fora de escopo do MVP

- Busca fora do Brasil.
- Login, contas e sincronização entre dispositivos.
- Previsão horária detalhada.
- Notificações push ou alertas automáticos.
- Backend próprio ou banco de dados remoto.
- Traduções para outros idiomas.
- Geolocalização automática (GPS) para detectar a cidade do usuário.
- Alternância de unidade para velocidade do vento (fixa em km/h).
- Previsão além de 5 dias ou dados históricos de clima.
- Temas alternáveis (claro/escuro); a UI é fixa em dark glassmorphism.
- Compartilhamento, exportação ou impressão de dados do clima.
- Reordenação manual ou edição/renomeação de favoritos.
- Analytics, telemetria ou cookies de rastreamento de uso.
- Suporte a navegadores sem Service Worker/PWA (comportamento apenas "best effort" como web comum).
- Alertas de clima severo (chuva forte, tempestade, etc.).

---

## 3. Regras de negócio

### 3.1 Busca de cidade

- A busca deve iniciar após o usuário digitar 2 ou mais caracteres.
- O sistema deve limitar as sugestões a cidades brasileiras relevantes e retornar no máximo 10 resultados.
- Quando houver cidades com o mesmo nome, a opção deve exibir o nome da cidade seguido do estado, por exemplo: "Rio de Janeiro (RJ)".
- A busca deve ignorar variações de caixa e acento no texto digitado.
- Entrada vazia ou composta apenas por espaços deve ser tratada como sem busca.
- Se a API de geocoding falhar, o app deve manter a última busca válida visível e apresentar a mensagem "Não foi possível buscar cidades agora. Tente novamente.".

### 3.2 Clima atual

- Ao selecionar uma cidade, o app deve carregar em paralelo o clima atual e a previsão de 5 dias.
- O clima atual deve incluir: temperatura, sensação térmica, descrição da condição, ícone, umidade, velocidade do vento, nascer e pôr do sol e timestamp da última atualização.
- A velocidade do vento é sempre exibida em km/h, como valor inteiro, independente da unidade de temperatura selecionada.
- Umidade e chance de chuva são exibidas como valores inteiros (sem casas decimais).
- O timestamp deve refletir a data e hora locais da cidade consultada, no formato relativo "Atualizado há X min" (ou "Atualizado agora" quando menor que 1 minuto).
- O nascer e o pôr do sol devem ser exibidos no formato 24h "HH:mm", no timezone da cidade.
- Toda requisição à API de clima deve respeitar timeout de 8 segundos; ao expirar, é tratada como falha e segue o fluxo de erro/cache já definido.
- Quando houver dados em cache válidos e a rede estiver indisponível, o app deve renderizar o cache e exibir estado de offline.
- Se não houver cache válido, o app deve exibir erro claro e permitir retry.

### 3.3 Previsão de 5 dias

- A previsão deve apresentar exatamente 5 registros: dia atual + 4 dias seguintes.
- Cada item deve conter data, dia da semana em pt-BR, temperatura mínima, máxima, descrição da condição e chance de chuva, quando disponível.
- O app deve manter o layout responsivo em mobile, sem quebra de conteúdo.

### 3.4 Unidade de temperatura

- A unidade padrão deve ser Celsius (°C).
- A troca de unidade deve refletir imediatamente em todas as temperaturas visíveis da interface.
- A conversão deve usar: F = (C × 9/5) + 32, arredondada para inteiro.
- A preferência deve ser persistida em localStorage.

### 3.5 Favoritos

- O app deve permitir salvar até 5 cidades como favoritas.
- Favoritos devem ser persistidos localmente.
- Favoritos são exibidos em ordem de adição, do mais recente para o mais antigo.
- Cada favorito deve ser acessível por clique direto.
- Quando o limite de 5 for alcançado, a ação de favoritar deve ser bloqueada e exibir a mensagem "Limite de 5 favoritos atingido. Remova um para adicionar outro.".
- A remoção de favorito deve atualizar a persistência imediatamente.

### 3.6 Histórico de busca

- O histórico deve armazenar até 10 cidades.
- Ao repetir uma cidade já existente, o item deve ser movido para o topo e não duplicado.
- A nova pesquisa deve remover o registro mais antigo quando o limite for atingido.
- O histórico deve ser persistido e renderizado com clique para reutilização.
- O usuário pode remover um item individual do histórico ou limpar o histórico inteiro mediante confirmação.

### 3.7 Offline e cache

- O app deve manter em cache os dados da cidade selecionada por até 30 minutos.
- Quando offline e com cache válido, o app deve exibir o ultimo estado salvo com identificação de idade do cache, no formato "Dados de há X min (offline)".
- Quando offline e sem cache válido, o app deve exibir erro e não falhar silenciosamente.
- Ao tentar atualizar em modo offline, o app deve exibir a mensagem "Sem conexão. Você verá os dados assim que a conexão for restabelecida.".
- A sincronização automática deve ocorrer quando a conexão estiver restabelecida.
- O debounce de 200ms e o cache de 30 minutos são suficientes para manter o volume de chamadas à API baixo, sem necessidade de limite adicional de requisições no MVP.

### 3.8 PWA

- O app deve ser instalável em navegadores compatíveis.
- O manifest deve incluir nome, ícones e cor de tema.
- O service worker deve registrar assets estáticos com cache apropriado.

---

## 4. Critérios de aceite

### RF01 — Busca por cidade

- [ ] Dado que o usuário digita 2 ou mais caracteres, quando a busca é disparada, então o app exibe até 10 sugestões relevantes.
- [ ] Dado que o usuário digita menos de 2 caracteres, quando o campo é atualizado, então nenhuma sugestão é exibida.
- [ ] Dado que existem cidades homônimas, quando as sugestões são exibidas, então cada item mostra o nome da cidade e o estado.
- [ ] Dado que o usuário seleciona uma sugestão com teclado, quando pressiona Enter, então a cidade é carregada e o campo de busca é limpo.
- [ ] Dado que o usuário digita com variações de caixa ou acentos, quando a busca é executada, então o resultado é o mesmo.
- [ ] Dado que a API de geocoding falha, quando o usuário tenta buscar uma cidade, então o app exibe mensagem de erro e mantém a última busca válida.

### RF02 — Clima atual

- [ ] Dado que uma cidade foi selecionada, quando os dados forem carregados, então o app exibe temperatura atual, sensação térmica, descrição, ícone, umidade, vento, nascer e pôr do sol e timestamp.
- [ ] Dado que há cache válido e o app está offline, quando a tela for carregada, então o app exibe o dado em cache com indicador de offline.
- [ ] Dado que não há cache válido, quando ocorre falha na API, então o app exibe a mensagem "Não foi possível carregar. Tente novamente.".
- [ ] Dado que o usuário clica em atualizar, quando há conexão, então a tela entra em estado de carregamento e os dados são recarregados.

### RF03 — Previsão de 5 dias

- [ ] Dado que a cidade foi selecionada, quando os dados forem carregados, então a previsão apresenta 5 registros em ordem cronológica.
- [ ] Dado que cada item da previsão é renderizado, quando a informação estiver disponível, então o app mostra data, dia da semana, mínima, máxima, ícone e chance de chuva.
- [ ] Dado que o app está em mobile, quando a previsão é exibida, então o layout permanece legível sem quebra visual.

### RF04 — Unidade de temperatura

- [ ] Dado que o usuário acessa a app pela primeira vez, quando a tela carrega, então a unidade padrão exibida é °C.
- [ ] Dado que o usuário alterna a unidade, quando a conversão é aplicada, então todas as temperaturas visíveis são convertidas imediatamente.
- [ ] Dado que a temperatura em Celsius é C, quando convertida para Fahrenheit, então o valor respeita a fórmula F = (C × 9/5) + 32 e é arredondado para inteiro.
- [ ] Dado que o usuário seleciona outra unidade, quando a página é recarregada, então a última escolha é restaurada do storage.

### RF05 — Favoritos

- [ ] Dado que o usuário clica em favoritar, quando a cidade ainda não estiver salva, então ela é adicionada e exibida na lista de favoritos.
- [ ] Dado que existem 5 favoritos, quando o usuário tenta adicionar outro, então a ação é bloqueada e o limite é informado visualmente.
- [ ] Dado que o usuário remove um favorito, quando a ação é concluída, então o item sai da lista e a persistência é atualizada.
- [ ] Dado que o usuário seleciona um favorito salvo, quando a cidade for carregada, então o clima correspondente é exibido imediatamente.

### RF06 — Histórico de buscas

- [ ] Dado que o usuário seleciona uma cidade, quando a busca é concluída, então a cidade entra no histórico como item mais recente.
- [ ] Dado que a cidade já existe no histórico, quando a mesma busca é repetida, então o item é movido para o topo sem duplicação.
- [ ] Dado que o histórico já tem 10 itens, quando uma nova busca é concluída, então o item mais antigo é removido.
- [ ] Dado que o usuário confirma a limpeza do histórico, quando a ação é executada, então todos os itens são removidos.
- [ ] Dado que o usuário remove um item individual do histórico, quando a ação é confirmada, então apenas aquele item é removido, sem afetar os demais.

### RF07 — Offline e cache

- [ ] Dado que há conexão instável ou inexistente e existe cache válido, quando a app for acessado, então é exibido o último dado salvo com indicador de offline e idade do cache.
- [ ] Dado que o usuário tenta atualizar em modo offline, quando clica em refresh, então o app mantém estado explícito de espera e não falha silenciosamente.
- [ ] Dado que a conexão volta, quando o app está em execução, então os dados são atualizados sem necessidade de interação do usuário.

### RF08 — Atualização manual

- [ ] Dado que o usuário clica em atualizar, quando há conexão, então a cidade selecionada é recarregada e a interface mostra o estado de carregamento.

### RF09 — PWA

- [ ] Dado que o app é acessado em browser compatível, quando o usuário navega pela aplicação, então a opção de instalação é exibida.
- [ ] Dado que a PWA foi instalada, quando o app é aberto, então ele inicia em modo standalone com ícone e splash screen próprios.

### RF10 — Persistência de unidade

- [ ] Dado que o usuário altera a unidade, quando a ação é concluída, então a preferência é salva no localStorage.
- [ ] Dado que há preferência salva, quando o app é recarregado, então a última unidade é reaproveitada.

---

## 5. Requisitos não funcionais

### 5.1 Performance

- Carregamento inicial: menor que 2s em rede estável.
- Autocomplete: resposta em até 500ms.
- Refresh manual: atualização em até 1s, excluindo latência da rede.
- Bundle final comprimido: menor que 200KB.
- LCP: menor que 2.5s.
- CLS: menor que 0.1.

### 5.2 Acessibilidade

- Conformidade com WCAG 2.1 AA.
- Contraste mínimo de 4.5:1 em textos normais.
- Navegação completa por teclado.
- Uso de labels e roles em elementos interativos.
- Suporte a zoom de 200% sem quebra funcional.

### 5.3 Segurança

- Comunicações via HTTPS.
- Sanitização de entradas do usuário.
- Sem dados sensíveis em localStorage.
- CSP configurado para restringir scripts e estilos.

### 5.4 Resiliência

- Cache local válido por até 30 minutos.
- Debounce de 200ms na busca por cidade.
- Estado de erro ou offline sempre visível ao usuário.

### 5.5 Responsividade

- Mobile: 320–480px
- Tablet: 481–1024px
- Desktop: 1025px+

### 5.6 PWA

- Manifest válido com nome, ícones e paleta do app.
- Service worker para ativos estáticos.
- Cache busting em cada nova publicação.

---

## 6. Casos de borda

- Busca sem resultado: mostrar mensagem "Nenhuma cidade encontrada".
- Cidades homônimas: exibir estado/UF para distinguir a opção correta.
- Campo com apenas espaços: tratar como vazio.
- API indisponível sem cache: exibir erro e permitir nova tentativa.
- Tentativa de 6º favorito: bloquear a ação e indicar o limite.
- Remoção de todos os favoritos: a seção deve deixar de ser renderizada.
- Conexão interrompida durante busca: cancelar ou tratar requisição pendente como offline.
- Diferença de timezone entre dispositivo e cidade: usar o timezone da cidade para horários de sol.
- Cache expirado em modo offline: continuar exibindo os dados, mas com idade correta do cache.
- Dados sem chance de chuva: omitir o campo em vez de mostrar valor inválido.

---

## 7. Premissas

- A API Open-Meteo está disponível gratuitamente e sem chave de autenticação.
- O MVP cobre somente cidades brasileiras.
- A previsão de 5 dias representa o dia atual + 4 dias seguintes.
- O app não exige autenticação nem sincronização entre dispositivos.
- Persistência é local por browser, usando armazenamento do cliente.
- O mapeamento dos weather codes do Open-Meteo para ícones/descrições em pt-BR é uma decisão de implementação, a ser definida pelo Plan Agent com base na tabela oficial de códigos WMO usada pelo Open-Meteo.

---

## 8. Riscos principais

| Risco | Impacto | Mitigação |
|---|---|---|
| Falha ou mudança de contrato da API | Alto | Cache local e isolamento do serviço de API |
| Limite de requisições excedido | Médio | Debounce, cache e uso controlado de dados |
| localStorage indisponível ou corrompido | Médio | Fallback para estado padrão |
| Cidade homônima escolhida errada | Médio | Exibir estado nas sugestões |
| Conexão lenta ou instável | Médio | Cache e feedback explícito de carregamento |
| Timezone inconsistente | Baixo | Usar timezone retornado pela API |

---

## 9. Definição de pronto

A feature está pronta para seguir para implementação quando:

- requisitos funcionais e critérios de aceite estão claros e verificáveis;
- regras de persistência, cache e offline estão definidas;
- os fluxos de erro e fallback estão documentados;
- a interface mínima para busca, clima atual, previsão e favoritos foi validada como consistente com o público alvo;
- os critérios de performance e acessibilidade atendem ao MVP.
