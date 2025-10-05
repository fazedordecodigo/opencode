-----
applyTo: '*'
description: 'As instruções mais abrangentes, práticas e criadas por engenheiros para otimização de desempenho para todas as linguagens, frameworks e stacks. Cobre as melhores práticas de frontend, backend e banco de dados com orientações práticas, checklists baseados em cenários, solução de problemas e dicas profissionais.'
-----

# Melhores Práticas de Otimização de Desempenho

## Introdução

Desempenho não é apenas uma palavra da moda — é a diferença entre um produto que as pessoas amam e um que elas abandonam. Eu vi em primeira mão como um aplicativo lento pode frustrar usuários, aumentar as contas da nuvem e até mesmo perder clientes. Este guia é uma coleção viva das práticas de desempenho mais eficazes e do mundo real que usei e revisei, cobrindo as camadas de frontend, backend e banco de dados, bem como tópicos avançados. Use-o como referência, checklist e fonte de inspiração para construir software rápido, eficiente e escalável.

-----

## Princípios Gerais

  - **Meça Primeiro, Otimize Depois:** Sempre faça o profiling e meça antes de otimizar. Use benchmarks, profilers e ferramentas de monitoramento para identificar gargalos reais. Adivinhar é o inimigo do desempenho.
      - *Dica Pro:* Use ferramentas como Chrome DevTools, Lighthouse, New Relic, Datadog, Py-Spy ou os profilers nativos da sua linguagem.
  - **Otimize para o Caso Comum:** Foque em otimizar os caminhos de código que são executados com mais frequência. Não perca tempo com casos de borda raros, a menos que sejam críticos.
  - **Evite Otimização Prematura:** Escreva código claro e de fácil manutenção primeiro; otimize apenas quando necessário. A otimização prematura pode tornar o código mais difícil de ler e manter.
  - **Minimize o Uso de Recursos:** Use memória, CPU, rede e disco de forma eficiente. Sempre pergunte: "Isso pode ser feito com menos?"
  - **Prefira a Simplicidade:** Algoritmos e estruturas de dados simples são muitas vezes mais rápidos e fáceis de otimizar. Não complique a engenharia.
  - **Documente Suposições de Desempenho:** Comente claramente qualquer código que seja crítico para o desempenho ou que tenha otimizações não óbvias. Futuros mantenedores (incluindo você) agradecerão.
  - **Entenda a Plataforma:** Conheça as características de desempenho da sua linguagem, framework e runtime. O que é rápido em Python pode ser lento em JavaScript, e vice-versa.
  - **Automatize Testes de Desempenho:** Integre testes de desempenho e benchmarks em seu pipeline de CI/CD. Detecte regressões cedo.
  - **Defina Orçamentos de Desempenho:** Defina limites aceitáveis para tempo de carregamento, uso de memória, latência de API, etc. Garanta-os com verificações automatizadas.

-----

## Desempenho do Frontend

### Renderização e DOM

  - **Minimize Manipulações do DOM:** Agrupe atualizações sempre que possível. Mudanças frequentes no DOM são custosas.
      - *Anti-padrão:* Atualizar o DOM em um loop. Em vez disso, construa um fragmento de documento e anexe-o uma única vez.
  - **Frameworks de DOM Virtual:** Use React, Vue ou similares de forma eficiente — evite re-renderizações desnecessárias.
      - *Exemplo em React:* Use `React.memo`, `useMemo` e `useCallback` para prevenir renderizações desnecessárias.
  - **Chaves (`keys`) em Listas:** Sempre use chaves estáveis em listas para ajudar na diferenciação do DOM virtual. Evite usar índices de array como chaves, a menos que a lista seja estática.
  - **Evite Estilos Inline:** Estilos inline podem desencadear *layout thrashing* (reflows excessivos). Prefira classes CSS.
  - **Animações CSS:** Use transições/animações CSS em vez de JavaScript para efeitos mais suaves e acelerados por GPU.
  - **Adie a Renderização Não Crítica:** Use `requestIdleCallback` ou similar para adiar trabalho até que o navegador esteja ocioso.

### Otimização de Ativos (Assets)

  - **Compressão de Imagens:** Use ferramentas como ImageOptim, Squoosh ou TinyPNG. Prefira formatos modernos (WebP, AVIF) para entrega na web.
  - **SVGs para Ícones:** SVGs escalam bem e são frequentemente menores que PNGs para gráficos simples.
  - **Minificação e Bundling:** Use Webpack, Rollup ou esbuild para agrupar e minificar JS/CSS. Habilite o *tree-shaking* para remover código não utilizado.
  - **Cabeçalhos de Cache:** Defina cabeçalhos de cache de longa duração para ativos estáticos. Use *cache busting* para atualizações.
  - **Lazy Loading:** Use `loading="lazy"` para imagens e importações dinâmicas para módulos/componentes JS.
  - **Otimização de Fontes:** Use apenas os conjuntos de caracteres que você precisa. Crie subconjuntos de fontes e use `font-display: swap`.

### Otimização de Rede

  - **Reduza Requisições HTTP:** Combine arquivos, use sprites de imagem e inclua CSS crítico inline.
  - **HTTP/2 e HTTP/3:** Habilite esses protocolos para multiplexação e menor latência.
  - **Cache do Lado do Cliente:** Use Service Workers, IndexedDB e localStorage para visitas offline e repetidas.
  - **CDNs:** Sirva ativos estáticos de uma CDN próxima aos seus usuários. Use múltiplas CDNs para redundância.
  - **Scripts `defer`/`async`:** Use `defer` ou `async` para JS não crítico para evitar o bloqueio da renderização.
  - **Preload e Prefetch:** Use `<link rel="preload">` e `<link rel="prefetch">` para recursos críticos.

### Desempenho de JavaScript

  - **Evite Bloquear a Thread Principal:** Transfira computação pesada para Web Workers.
  - **Debounce/Throttle em Eventos:** Para eventos de scroll, resize e input, use debounce/throttle para limitar a frequência dos handlers.
  - **Vazamentos de Memória:** Limpe event listeners, intervalos e referências do DOM. Use as ferramentas de desenvolvedor do navegador para verificar nós desconectados.
  - **Estruturas de Dados Eficientes:** Use Maps/Sets para buscas, TypedArrays para dados numéricos.
  - **Evite Variáveis Globais:** Variáveis globais podem causar vazamentos de memória e desempenho imprevisível.
  - **Evite Clonagem Profunda de Objetos:** Use cópias rasas ou bibliotecas como `cloneDeep` do lodash apenas quando necessário.

### Acessibilidade e Desempenho

  - **Componentes Acessíveis:** Garanta que as atualizações ARIA não sejam excessivas. Use HTML semântico tanto para acessibilidade quanto para desempenho.
  - **Desempenho de Leitores de Tela:** Evite atualizações rápidas do DOM que podem sobrecarregar tecnologias assistivas.

### Dicas Específicas de Frameworks

#### React

  - Use `React.memo`, `useMemo` e `useCallback` para evitar renderizações desnecessárias.
  - Divida componentes grandes e use code-splitting (`React.lazy`, `Suspense`).
  - Evite funções anônimas no `render`; elas criam novas referências a cada renderização.
  - Use `ErrorBoundary` para capturar e tratar erros de forma elegante.
  - Faça o profiling com o React DevTools Profiler.

#### Angular

  - Use a detecção de mudanças `OnPush` para componentes que não precisam de atualizações frequentes.
  - Evite expressões complexas nos templates; mova a lógica para a classe do componente.
  - Use `trackBy` no `ngFor` para uma renderização eficiente de listas.
  - Carregue módulos e componentes de forma tardia (lazy load) com o Angular Router.
  - Faça o profiling com o Angular DevTools.

#### Vue

  - Use propriedades computadas (`computed properties`) em vez de métodos nos templates para cache.
  - Use `v-show` vs `v-if` apropriadamente (`v-show` é melhor para alternar a visibilidade com frequência).
  - Carregue componentes e rotas de forma tardia (lazy load) com o Vue Router.
  - Faça o profiling com o Vue Devtools.

### Armadilhas Comuns do Frontend

  - Carregar grandes bundles de JS no carregamento inicial da página.
  - Não comprimir imagens ou usar formatos desatualizados.
  - Falhar em limpar event listeners, causando vazamentos de memória.
  - Usar excessivamente bibliotecas de terceiros para tarefas simples.
  - Ignorar o desempenho em dispositivos móveis (teste em dispositivos reais\!).

### Solução de Problemas no Frontend

  - Use a aba *Performance* do Chrome DevTools para gravar e analisar frames lentos.
  - Use o Lighthouse para auditar o desempenho e obter sugestões práticas.
  - Use o WebPageTest para testes de carregamento em condições reais.
  - Monitore as Core Web Vitals (LCP, FID, CLS) para métricas centradas no usuário.

-----

## Desempenho do Backend

### Otimização de Algoritmos e Estruturas de Dados

  - **Escolha a Estrutura de Dados Certa:** Arrays para acesso sequencial, hash maps para buscas rápidas, árvores para dados hierárquicos, etc.
  - **Algoritmos Eficientes:** Use busca binária, quicksort ou algoritmos baseados em hash quando apropriado.
  - **Evite O(n²) ou Pior:** Analise laços aninhados e chamadas recursivas. Refatore para reduzir a complexidade.
  - **Processamento em Lote (Batch):** Processe dados em lotes para reduzir a sobrecarga (ex: inserções em massa no banco de dados).
  - **Streaming:** Use APIs de streaming para grandes conjuntos de dados para evitar carregar tudo na memória.

### Concorrência e Paralelismo

  - **E/S (I/O) Assíncrona:** Use async/await, callbacks ou event loops para evitar o bloqueio de threads.
  - **Pools de Threads/Workers:** Use pools para gerenciar a concorrência e evitar a exaustão de recursos.
  - **Evite Condições de Corrida (Race Conditions):** Use locks, semáforos ou operações atômicas quando necessário.
  - **Operações em Massa (Bulk):** Agrupe chamadas de rede/banco de dados para reduzir as viagens de ida e volta.
  - **Backpressure:** Implemente *backpressure* em filas e pipelines para evitar sobrecarga.

### Caching

  - **Cache de Computações Custosas:** Use caches em memória (Redis, Memcached) para dados "quentes" (acessados com frequência).
  - **Invalidação de Cache:** Use invalidação baseada em tempo (TTL), eventos ou manual. Cache obsoleto é pior do que nenhum cache.
  - **Cache Distribuído:** Para configurações com múltiplos servidores, use caches distribuídos e esteja ciente dos problemas de consistência.
  - **Proteção contra Estouro de Cache (Cache Stampede):** Use locks ou coalescência de requisições para prevenir o problema do "efeito manada" (*thundering herd*).
  - **Não Armazene Tudo em Cache:** Alguns dados são muito voláteis ou sensíveis para serem cacheados.

### API e Rede

  - **Minimize os Payloads:** Use JSON, comprima respostas (gzip, Brotli) e evite enviar dados desnecessários.
  - **Paginação:** Sempre pagine grandes conjuntos de resultados. Use cursores para dados em tempo real.
  - **Limitação de Taxa (Rate Limiting):** Proteja as APIs contra abuso e sobrecarga.
  - **Pooling de Conexões:** Reutilize conexões para bancos de dados e serviços externos.
  - **Escolha do Protocolo:** Use HTTP/2, gRPC ou WebSockets para comunicação de alta vazão e baixa latência.

### Logging e Monitoramento

  - **Minimize o Logging em Caminhos Críticos (Hot Paths):** Logging excessivo pode tornar o código crítico mais lento.
  - **Logging Estruturado:** Use logs em JSON ou chave-valor para facilitar a análise e o parsing.
  - **Monitore Tudo:** Latência, vazão, taxas de erro, uso de recursos. Use Prometheus, Grafana, Datadog ou similares.
  - **Alertas:** Configure alertas para regressões de desempenho e exaustão de recursos.

### Dicas Específicas de Linguagem/Framework

#### Node.js

  - Use APIs assíncronas; evite bloquear o event loop (ex: nunca use `fs.readFileSync` em produção).
  - Use clustering ou worker threads para tarefas que consomem muita CPU.
  - Limite as conexões abertas concorrentes para evitar a exaustão de recursos.
  - Use streams para processamento de grandes arquivos ou dados de rede.
  - Faça o profiling com `clinic.js`, `node --inspect` ou Chrome DevTools.

#### Python

  - Use estruturas de dados nativas (`dict`, `set`, `deque`) para velocidade.
  - Faça o profiling com `cProfile`, `line_profiler` ou `Py-Spy`.
  - Use `multiprocessing` ou `asyncio` para paralelismo.
  - Evite gargalos do GIL em código que consome muita CPU; use extensões C ou subprocessos.
  - Use `lru_cache` para memoização.

#### Java

  - Use coleções eficientes (`ArrayList`, `HashMap`, etc.).
  - Faça o profiling com VisualVM, JProfiler ou YourKit.
  - Use pools de threads (`Executors`) para concorrência.
  - Ajuste as opções da JVM para heap e coleta de lixo (`-Xmx`, `-Xms`, `-XX:+UseG1GC`).
  - Use `CompletableFuture` para programação assíncrona.

#### .NET

  - Use `async/await` para operações ligadas a E/S (I/O-bound).
  - Use `Span<T>` e `Memory<T>` para acesso eficiente à memória.
  - Faça o profiling com dotTrace, Visual Studio Profiler ou PerfView.
  - Faça pooling de objetos e conexões quando apropriado.
  - Use `IAsyncEnumerable<T>` para streaming de dados.

### Armadilhas Comuns do Backend

  - E/S síncrona/bloqueante em servidores web.
  - Não usar pooling de conexões para bancos de dados.
  - Cachear em excesso ou cachear dados sensíveis/voláteis.
  - Ignorar o tratamento de erros em código assíncrono.
  - Não monitorar ou alertar sobre regressões de desempenho.

### Solução de Problemas no Backend

  - Use *flame graphs* para visualizar o uso da CPU.
  - Use rastreamento distribuído (OpenTelemetry, Jaeger, Zipkin) para rastrear a latência de requisições entre serviços.
  - Use *heap dumps* e profilers de memória para encontrar vazamentos.
  - Registre (log) consultas lentas e chamadas de API para análise.

-----

## Desempenho de Banco de Dados

### Otimização de Consultas

  - **Índices:** Use índices em colunas que são frequentemente consultadas, filtradas ou usadas em joins. Monitore o uso dos índices e remova os não utilizados.
  - **Evite `SELECT *`:** Selecione apenas as colunas que você precisa. Reduz o uso de E/S e memória.
  - **Consultas Parametrizadas:** Previnem injeção de SQL e melhoram o cache de planos de consulta.
  - **Planos de Consulta:** Analise e otimize os planos de execução de consultas. Use `EXPLAIN` em bancos de dados SQL.
  - **Evite Consultas N+1:** Use joins ou consultas em lote para evitar consultas repetidas em loops.
  - **Limite os Conjuntos de Resultados:** Use `LIMIT`/`OFFSET` ou cursores para tabelas grandes.

### Design de Schema

  - **Normalização:** Normalize para reduzir redundância, mas desnormalize para cargas de trabalho de leitura intensiva, se necessário.
  - **Tipos de Dados:** Use os tipos de dados mais eficientes e defina restrições apropriadas.
  - **Particionamento:** Particione tabelas grandes para escalabilidade e gerenciamento.
  - **Arquivamento:** Arquive ou limpe regularmente dados antigos para manter as tabelas pequenas e rápidas.
  - **Chaves Estrangeiras:** Use-as para integridade dos dados, mas esteja ciente dos trade-offs de desempenho em cenários de alta escrita.

### Transações

  - **Transações Curtas:** Mantenha as transações o mais curtas possível para reduzir a contenção de locks.
  - **Níveis de Isolamento:** Use o nível de isolamento mais baixo que atenda às suas necessidades de consistência.
  - **Evite Transações de Longa Duração:** Elas podem bloquear outras operações e aumentar os deadlocks.

### Caching e Replicação

  - **Réplicas de Leitura:** Use para escalar cargas de trabalho de leitura intensiva. Monitore o atraso da replicação (replication lag).
  - **Cache de Resultados de Consultas:** Use Redis ou Memcached para consultas acessadas com frequência.
  - **Write-Through/Write-Behind:** Escolha a estratégia correta para suas necessidades de consistência.
  - **Sharding:** Distribua os dados entre múltiplos servidores para escalabilidade.

### Bancos de Dados NoSQL

  - **Projete para Padrões de Acesso:** Modele seus dados para as consultas que você precisa.
  - **Evite Partições Quentes (Hot Partitions):** Distribua as leituras/escritas uniformemente.
  - **Crescimento Ilimitado:** Fique atento a arrays ou documentos que crescem sem limites.
  - **Sharding e Replicação:** Use para escalabilidade e disponibilidade.
  - **Modelos de Consistência:** Entenda a consistência eventual vs. forte e escolha apropriadamente.

### Armadilhas Comuns de Banco de Dados

  - Índices ausentes ou não utilizados.
  - `SELECT *` em consultas de produção.
  - Não monitorar consultas lentas.
  - Ignorar o atraso da replicação.
  - Não arquivar dados antigos.

### Solução de Problemas em Banco de Dados

  - Use logs de consultas lentas para identificar gargalos.
  - Use `EXPLAIN` para analisar os planos de consulta.
  - Monitore as taxas de acerto/erro do cache (hit/miss ratios).
  - Use ferramentas de monitoramento específicas do banco de dados (pg\_stat\_statements, MySQL Performance Schema).

-----

## Checklist de Revisão de Código para Desempenho

  - [ ] Existem ineficiências algorítmicas óbvias (O(n²) ou pior)?
  - [ ] As estruturas de dados são apropriadas para seu uso?
  - [ ] Existem computações desnecessárias ou trabalho repetido?
  - [ ] O cache é usado quando apropriado e a invalidação é tratada corretamente?
  - [ ] As consultas ao banco de dados são otimizadas, indexadas e livres de problemas N+1?
  - [ ] Payloads grandes são paginados, transmitidos (streamed) ou divididos em pedaços (chunked)?
  - [ ] Existem vazamentos de memória ou uso ilimitado de recursos?
  - [ ] As requisições de rede são minimizadas, agrupadas e tentadas novamente em caso de falha?
  - [ ] Os ativos (assets) são otimizados, comprimidos e servidos de forma eficiente?
  - [ ] Existem operações de bloqueio em caminhos críticos (hot paths)?
  - [ ] O logging em caminhos críticos é minimizado e estruturado?
  - [ ] Os caminhos de código críticos para o desempenho são documentados и testados?
  - [ ] Existem testes automatizados ou benchmarks para código sensível ao desempenho?
  - [ ] Existem alertas para regressões de desempenho?
  - [ ] Existem anti-padrões (ex: `SELECT *`, E/S bloqueante, variáveis globais)?

-----

## Tópicos Avançados

### Profiling e Benchmarking

  - **Profilers:** Use profilers específicos da linguagem (Chrome DevTools, Py-Spy, VisualVM, dotTrace, etc.) para identificar gargalos.
  - **Microbenchmarks:** Escreva microbenchmarks para caminhos de código críticos. Use `benchmark.js`, `pytest-benchmark` ou JMH para Java.
  - **Teste A/B:** Meça o impacto real das otimizações com lançamentos A/B ou canário.
  - **Teste de Desempenho Contínuo:** Integre testes de desempenho no CI/CD. Use ferramentas como k6, Gatling ou Locust.

### Gerenciamento de Memória

  - **Limpeza de Recursos:** Sempre libere recursos (arquivos, sockets, conexões de BD) prontamente.
  - **Pooling de Objetos:** Use para objetos criados/destruídos com frequência (ex: conexões de BD, threads).
  - **Monitoramento do Heap:** Monitore o uso do heap e a coleta de lixo (GC). Ajuste as configurações do GC para sua carga de trabalho.
  - **Vazamentos de Memória:** Use ferramentas de detecção de vazamentos (Valgrind, LeakCanary, Chrome DevTools).

### Escalabilidade

  - **Escalabilidade Horizontal:** Projete serviços sem estado (stateless), use sharding/particionamento e balanceadores de carga.
  - **Auto-Escalonamento:** Use grupos de auto-escalonamento da nuvem e defina limiares sensatos.
  - **Análise de Gargalos:** Identifique e resolva pontos únicos de falha.
  - **Sistemas Distribuídos:** Use operações idempotentes, novas tentativas e *circuit breakers*.

### Segurança e Desempenho

  - **Criptografia Eficiente:** Use bibliotecas criptográficas aceleradas por hardware e bem mantidas.
  - **Validação:** Valide entradas de forma eficiente; evite regex em caminhos críticos.
  - **Limitação de Taxa (Rate Limiting):** Proteja contra DoS sem prejudicar usuários legítimos.

### Desempenho Móvel

  - **Tempo de Inicialização:** Carregue recursos de forma tardia (lazy load), adie trabalhos pesados e minimize o tamanho do bundle inicial.
  - **Otimização de Imagens/Ativos:** Use imagens responsivas e comprima ativos para a largura de banda móvel.
  - **Armazenamento Eficiente:** Use SQLite, Realm ou armazenamento otimizado para a plataforma.
  - **Profiling:** Use Android Profiler, Instruments (iOS) ou Firebase Performance Monitoring.

### Nuvem e Serverless

  - **Cold Starts:** Minimize dependências e mantenha as funções "aquecidas".
  - **Alocação de Recursos:** Ajuste memória/CPU para funções serverless.
  - **Serviços Gerenciados:** Use cache, filas e bancos de dados gerenciados para escalabilidade.
  - **Otimização de Custos:** Monitore e otimize o custo da nuvem como uma métrica de desempenho.

-----

## Exemplos Práticos

### Exemplo 1: Debouncing de Entrada do Usuário em JavaScript

```javascript
// RUIM: Dispara uma chamada de API a cada tecla pressionada
input.addEventListener('input', (e) => {
  fetch(`/search?q=${e.target.value}`);
});

// BOM: Faz debounce das chamadas de API
let timeout;
input.addEventListener('input', (e) => {
  clearTimeout(timeout);
  timeout = setTimeout(() => {
    fetch(`/search?q=${e.target.value}`);
  }, 300);
});
```

### Exemplo 2: Consulta SQL Eficiente

```sql
-- RUIM: Seleciona todas as colunas e não usa um índice
SELECT * FROM users WHERE email = 'user@example.com';

-- BOM: Seleciona apenas as colunas necessárias e usa um índice
SELECT id, name FROM users WHERE email = 'user@example.com';
```

### Exemplo 3: Cache de Computação Custosa em Python

```python
# RUIM: Recomputa o resultado toda vez
result = expensive_function(x)

# BOM: Armazena o resultado em cache
from functools import lru_cache

@lru_cache(maxsize=128)
def expensive_function(x):
    ...
result = expensive_function(x)
```

### Exemplo 4: Lazy Loading de Imagens em HTML

```html
<img src="large-image.jpg" />

<img src="large-image.jpg" loading="lazy" />
```

### Exemplo 5: E/S Assíncrona em Node.js

```javascript
// RUIM: Leitura de arquivo bloqueante
const data = fs.readFileSync('file.txt');

// BOM: Leitura de arquivo não bloqueante
fs.readFile('file.txt', (err, data) => {
  if (err) throw err;
  // processa os dados
});
```

### Exemplo 6: Profiling de uma Função Python

```python
import cProfile
import pstats

def slow_function():
    ...

cProfile.run('slow_function()', 'profile.stats')
p = pstats.Stats('profile.stats')
p.sort_stats('cumulative').print_stats(10)
```

### Exemplo 7: Usando Redis para Cache em Node.js

```javascript
const redis = require('redis');
const client = redis.createClient();

function getCachedData(key, fetchFunction) {
  return new Promise((resolve, reject) => {
    client.get(key, (err, data) => {
      if (data) return resolve(JSON.parse(data));
      fetchFunction().then(result => {
        client.setex(key, 3600, JSON.stringify(result));
        resolve(result);
      });
    });
  });
}
```

-----

## Referências e Leituras Adicionais

  - [Google Web Fundamentals: Desempenho](https://web.dev/performance/)
  - [MDN Web Docs: Desempenho](https://developer.mozilla.org/pt-BR/docs/Web/Performance)
  - [OWASP: Teste de Desempenho](https://owasp.org/www-project-performance-testing/)
  - [Microsoft: Melhores Práticas de Desempenho](https://www.google.com/search?q=https://learn.microsoft.com/pt-br/azure/architecture/best-practices/performance)
  - [Otimização de Desempenho do PostgreSQL](https://wiki.postgresql.org/wiki/Performance_Optimization)
  - [Ajuste de Desempenho do MySQL](https://dev.mysql.com/doc/refman/8.0/en/optimization.html)
  - [Melhores Práticas de Desempenho do Node.js](https://www.google.com/search?q=https://nodejs.org/pt-br/docs/guides/simple-profiling/)
  - [Dicas de Desempenho em Python](https://docs.python.org/3/library/profile.html)
  - [Ajuste de Desempenho em Java](https://www.oracle.com/java/technologies/javase/performance.html)
  - [Guia de Desempenho do .NET](https://www.google.com/search?q=https://learn.microsoft.com/pt-br/dotnet/standard/performance/)
  - [WebPageTest](https://www.webpagetest.org/)
  - [Lighthouse](https://developers.google.com/web/tools/lighthouse)
  - [Prometheus](https://prometheus.io/)
  - [Grafana](https://grafana.com/)
  - [Teste de Carga com k6](https://k6.io/)
  - [Gatling](https://gatling.io/)
  - [Locust](https://locust.io/)
  - [OpenTelemetry](https://opentelemetry.io/)
  - [Jaeger](https://www.jaegertracing.io/)
  - [Zipkin](https://zipkin.io/)

-----

## Conclusão

A otimização de desempenho é um processo contínuo. Sempre meça, analise (profile) e itere. Use estas melhores práticas, checklists e dicas de solução de problemas para guiar seu desenvolvimento e revisões de código para um software de alto desempenho, escalável e eficiente. Se você tiver novas dicas ou lições aprendidas, adicione-as aqui — vamos manter este guia crescendo\!

<!-- Fim das instruções de Otimização de Desempenho -->