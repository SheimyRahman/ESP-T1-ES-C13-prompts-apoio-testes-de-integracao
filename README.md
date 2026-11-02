# ESP-T1-ES-C13-prompts-apoio-testes-de-integra-o
Estudo e testes com prompts para apoio em tarefas de integracao

----

Repositório criado para um exercício acadêmico da Unidade III, capítulo 13, sobre o uso de inteligência artificial generativa no apoio a testes de integração.

O exercício explora como prompts com diferentes objetivos podem ajudar a derivar casos de teste para um fluxo de integração entre serviços REST. O cenário adotado usa um serviço de pedidos em Go, integrado a serviços de estoque e pagamento.

## Objetivos

- Criar cenários funcionais de integração a partir de um contrato REST e regras de negócio.
- Explorar falhas, timeouts, idempotência e compensação entre serviços.
- Comparar como a estrutura e as técnicas de prompting afetam a clareza e a diversidade dos casos gerados.
- Identificar lacunas de especificação que precisam de decisão antes da implementação dos testes.

## Conteúdo

- [`prompts-de-integracao/prompt_01`](prompts-de-integracao/prompt_01) — prompt estruturado zero-shot para cenários funcionais, com casos positivos e negativos derivados do contrato.
- [`prompts-de-integracao/prompt_02`](prompts-de-integracao/prompt_02) — prompt few-shot orientado a falhas, timeouts, compensação e idempotência.
- [`LICENSE`](LICENSE) — licença do repositório.

Cada arquivo de prompt registra o cenário e suas premissas, o texto usado, os casos gerados, uma avaliação (humana) breve, os ajustes aplicados após a primeira iteração e uma sugestão de implementação em Go.

## Cenário de integração

O serviço `orders` recebe `POST /v1/orders` e se comunica por REST com:

- `inventory`, para reservar itens;
- `payments`, para autorizar o pagamento.

O contrato considerado nos prompts descreve o fluxo de sucesso/caminho feliz e respostas para estoque insuficiente, pagamento recusado e falha de dependências. Também considera repetição idempotente. Essas regras são premissas do exercício criado e, portanto, não cobrem toda a complexidade, tampouco representam uma API de produção.

## Técnicas de prompting

**Prompt 1 — zero-shot estruturado:** define o papel de QA, o contexto da API, as regras de negócio, as restrições e o formato tabular esperado, sem incluir exemplos de resposta. O foco é gerar cenários funcionais claros e rastreáveis.

**Prompt 2 — few-shot para falhas:** inclui um exemplo do nível de detalhe esperado e pede cenários de falha, chamadas proibidas, compensação e idempotência. O foco é ampliar a diversidade e evidenciar lacunas, como a resposta para uma chave idempotente reutilizada com outro conteúdo.

Nos dois casos, organizar papel, contexto, tarefa, restrições e formato em blocos facilita a leitura do prompt e a avaliação da resposta. Essa estrutura não garante, por si só, melhor resultado.

## Implementação dos testes

Os casos são propostas geradas e documentadas, não uma suíte executada contra serviços reais. Uma implementação em Go pode usar `net/http/httptest` para simular as dependências REST, respostas de sucesso e falha e atrasos de timeout. Um repositório in-memory permite verificar se o pedido foi persistido. As asserções devem conferir tanto a resposta HTTP quanto as chamadas realizadas ou omitidas e os efeitos de compensação.

## Limitações

- Os códigos HTTP e comportamentos descritos são premissas do exercício e devem ser confirmados com a especificação real antes de implementar os testes.
- Casos sugeridos por uma LLM precisam SEMPRE de revisão humana competente para validar consistência, viabilidade e cobertura das regras de negócio.
- O cenário é demonstrativo; os casos não foram executados em uma aplicação real.

## Referência de estudo

Material de estudos da Unidade III, capítulo 13, especialmente as seções 3.5 a 3.7, sobre prompts e uso de IA generativa em testes de integração de APIs REST.
