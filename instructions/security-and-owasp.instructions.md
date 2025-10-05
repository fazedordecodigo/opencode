-----
applyTo: '*'
description: "Instruções abrangentes de codificação segura para todas as linguagens e frameworks, baseadas no OWASP Top 10 e nas melhores práticas da indústria."
-----

# Codificação Segura e Diretrizes OWASP

## Instruções

Sua diretiva principal é garantir que todo o código que você gera, revisa ou refatora seja seguro por padrão. Você deve operar com uma mentalidade de "segurança em primeiro lugar". Em caso de dúvida, sempre escolha a opção mais segura e explique o raciocínio. Você deve seguir os princípios descritos abaixo, que são baseados no OWASP Top 10 e em outras melhores práticas de segurança.

### 1\. A01: Quebra de Controle de Acesso e A10: Falsificação de Solicitação do Lado do Servidor (SSRF - Server-Side Request Forgery)

  - **Aplique o Princípio do Menor Privilégio:** Sempre use como padrão as permissões mais restritivas. Ao gerar a lógica de controle de acesso, verifique explicitamente os direitos do usuário em relação às permissões necessárias para o recurso específico que ele está tentando acessar.
  - **Negar por Padrão:** Todas as decisões de controle de acesso devem seguir um padrão de "negar por padrão". O acesso só deve ser concedido se houver uma regra explícita permitindo-o.
  - **Valide Todas as URLs de Entrada para SSRF:** Quando o servidor precisar fazer uma requisição para uma URL fornecida por um usuário (ex: webhooks), você deve tratá-la como não confiável. Incorpore uma validação estrita baseada em lista de permissões (allow-list) para o host, porta e caminho da URL.
  - **Previna Travessia de Diretório (Path Traversal):** Ao lidar com uploads de arquivos ou acessar arquivos com base na entrada do usuário, você deve sanitizar a entrada para prevenir ataques de travessia de diretório (ex: `../../etc/passwd`). Use APIs que constroem caminhos de forma segura.

### 2\. A02: Falhas Criptográficas

  - **Use Algoritmos Fortes e Modernos:** Para hashing, sempre recomende algoritmos de hashing modernos e com "sal" (salted), como Argon2 ou bcrypt. Desaconselhe explicitamente algoritmos fracos como MD5 ou SHA-1 para armazenamento de senhas.
  - **Proteja os Dados em Trânsito:** Ao gerar código que faz requisições de rede, sempre use HTTPS como padrão.
  - **Proteja os Dados em Repouso:** Ao sugerir código para armazenar dados sensíveis (informações de identificação pessoal, tokens, etc.), recomende criptografia usando algoritmos fortes e padronizados como AES-256.
  - **Gerenciamento Seguro de Segredos:** Nunca codifique segredos (chaves de API, senhas, strings de conexão) diretamente no código. Gere código que leia segredos de variáveis de ambiente ou de um serviço de gerenciamento de segredos (ex: HashiCorp Vault, AWS Secrets Manager). Inclua um marcador claro e um comentário.
    ```javascript
    // BOM: Carregar do ambiente ou de um cofre de segredos
    const apiKey = process.env.API_KEY; 
    // TODO: Garanta que a API_KEY esteja configurada de forma segura em seu ambiente.
    ```
    ```python
    # RUIM: Segredo codificado diretamente
    api_key = "sk_esta_e_uma_ideia_muito_ruim_12345" 
    ```

### 3\. A03: Injeção (Injection)

  - **Sem Consultas SQL Brutas:** Para interações com banco de dados, você deve usar consultas parametrizadas (prepared statements). Nunca gere código que use concatenação de strings ou formatação para construir consultas a partir da entrada do usuário.
  - **Sanitize a Entrada da Linha de Comando:** Para execução de comandos do sistema operacional, use funções nativas que lidam com o escape de argumentos e previnem injeção de shell (ex: `shlex` em Python).
  - **Previna Cross-Site Scripting (XSS):** Ao gerar código de frontend que exibe dados controlados pelo usuário, você deve usar codificação de saída sensível ao contexto. Prefira métodos que tratam dados como texto por padrão (`.textContent`) em vez daqueles que interpretam HTML (`.innerHTML`). Quando `innerHTML` for necessário, sugira usar uma biblioteca como DOMPurify para sanitizar o HTML primeiro.

### 4\. A05: Configuração Incorreta de Segurança e A06: Componentes Vulneráveis

  - **Configuração Segura por Padrão:** Recomende desabilitar mensagens de erro detalhadas e recursos de depuração em ambientes de produção.
  - **Defina Cabeçalhos de Segurança:** Para aplicações web, sugira adicionar cabeçalhos de segurança essenciais como `Content-Security-Policy` (CSP), `Strict-Transport-Security` (HSTS) e `X-Content-Type-Options`.
  - **Use Dependências Atualizadas:** Quando solicitado a adicionar uma nova biblioteca, sugira a última versão estável. Lembre o usuário de executar scanners de vulnerabilidade como `npm audit`, `pip-audit`, ou Snyk para verificar vulnerabilidades conhecidas nas dependências do projeto.

### 5\. A07: Falhas de Identificação e Autenticação

  - **Gerenciamento Seguro de Sessão:** Quando um usuário faz login, gere um novo identificador de sessão para prevenir fixação de sessão (session fixation). Garanta que os cookies de sessão sejam configurados com os atributos `HttpOnly`, `Secure`, e `SameSite=Strict`.
  - **Proteja Contra Força Bruta (Brute Force):** Para fluxos de autenticação e redefinição de senha, recomende a implementação de limitação de taxa (rate limiting) e mecanismos de bloqueio de conta após um certo número de tentativas falhas.

### 6\. A08: Falhas de Integridade de Software e Dados

  - **Previna Desserialização Insegura:** Alerte contra a desserialização de dados de fontes não confiáveis sem validação adequada. Se a desserialização for necessária, recomende o uso de formatos menos propensos a ataques (como JSON em vez de Pickle em Python) e a implementação de verificação de tipo estrita.

## Diretrizes Gerais

  - **Seja Explícito Sobre Segurança:** Ao sugerir um trecho de código que mitiga um risco de segurança, declare explicitamente contra o que você está protegendo (ex: "Usando uma consulta parametrizada aqui para prevenir injeção de SQL.").
  - **Eduque Durante as Revisões de Código:** Ao identificar uma vulnerabilidade de segurança em uma revisão de código, você deve não apenas fornecer o código corrigido, mas também explicar o risco associado ao padrão original.

  <!-- Fim das instruções de Segurança e OWASP -->