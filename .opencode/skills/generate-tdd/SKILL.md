---
name: generate-tdd
description: Gera um Documento Técnico de Design (TDD — Technical Design Document) detalhado, estruturado e verificável a partir de um PRD, prompt técnico ou recorte de sistema, conduzindo uma entrevista estruturada, classificando ambiguidades técnicas e centralizando as decisões em um registro ADR. Use quando o usuário pedir para criar, escrever ou gerar um TDD, documento técnico de design, design doc, documento de arquitetura, especificação técnica, ADR consolidado, ou quando executar /generate-tdd.
---

# Gerar TDD (Documento Técnico de Design)

> **TDD = Technical Design Document.** Neste comando, TDD **não** significa Test-Driven Development.

Transforma um PRD, um prompt técnico ou um recorte de sistema em um Documento Técnico de Design detalhado, verificável e pronto para revisão de engenharia.

## Entradas

- **Prompt inicial (obrigatório):** funcionalidade, sistema ou recorte técnico a ser projetado.
- **PRD de referência (opcional, recomendado):** `--prd <caminho>`. Se omitido, procure o PRD mais recente em `docs/prd/` e confirme com o usuário antes de usá-lo.
- **Opções reconhecidas no prompt** (todas opcionais):
  - `--out <caminho>`: arquivo de saída. Padrão: `docs/tdd/<slug>.md`.
  - `--lang <idioma>`: idioma do documento. Padrão: o mesmo do prompt/PRD.
  - `--scope <texto>`: recorte explícito do PRD (ex.: "somente check-in e retirada").
  - `--quick`: encurtar a entrevista de descoberta; a classificação de ambiguidades e a confirmação final das decisões continuam obrigatórias.

A entrevista é parte obrigatória do fluxo. O prompt inicial nunca é suficiente por si só: mesmo que pareça completo, as ambiguidades técnicas devem ser classificadas e, quando materiais, resolvidas com o usuário antes de o artefato ser finalizado.

## Princípios

1. **Fundamentado, não inventado.** Toda afirmação sobre o sistema atual deve ter evidência (código, documento, configuração). O que não existe vira premissa, decisão provisória ou pergunta em aberto.
2. **Decisão centralizada.** Toda escolha técnica material vira um registro `DEC-001`… com contexto, alternativas, decisão, justificativa, consequências e dono. O registro de decisões é a fonte única da verdade do documento.
3. **Alternativas explícitas.** Nenhuma decisão relevante é apresentada como única opção. Registre o que foi descartado e por quê.
4. **Rastreável.** Requisito do PRD → componente → contrato ou dado → teste → decisão. Sem requisito órfão e sem decisão órfã.
5. **Técnico e verificável.** Descreva contratos, estados, erros, limites e critérios de aceitação técnica. Evite "rápido", "escalável" ou "seguro" sem número, condição ou mecanismo.
6. **Operável.** Projete também implantação, configuração, observabilidade, backup, migração e rollback — não apenas o caminho feliz.
7. **Segurança e privacidade por padrão.** Autenticação, autorização, segredos, dados pessoais e retenção são parte do design, não um apêndice.
8. **Entrevista explícita.** Não deduza silenciosamente o que pode ser perguntado. Toda decisão material é confirmada com o usuário.
9. **Ambiguidade classificada.** Nenhuma ambiguidade é resolvida "no achismo": ela recebe tipo, impacto e ação, e as materiais são levadas ao sistema de perguntas do ambiente.
10. **Não implementar.** Entregue apenas o documento. Trechos ilustrativos (assinaturas, payloads, DDL, pseudocódigo) são permitidos quando marcados como ilustrativos; não crie código, migrations ou configuração.
11. **Diagramas quando explicam.** Relacionamentos, fluxos, estados, arquitetura e implantação devem ser explicados também por diagramas Mermaid versionáveis. Um diagrama bom reduz ambiguidade; não é decoração.

## Entrevista obrigatória

A geração do TDD inclui uma entrevista estruturada com o usuário. Ela não é opcional e acontece em dois momentos:

1. **Descoberta** — antes de redigir, para fechar escopo técnico, arquitetura, dados, integrações, segurança, operação e qualidade.
2. **Validação final** — depois do rascunho e antes de salvar, para confirmar arquitetura, decisões, premissas e pendências.

### Regras da entrevista

- Use **sempre** a ferramenta de perguntas do ambiente (`question`) em cada rodada. Não substitua a entrevista por uma lista de perguntas em texto livre quando a ferramenta estiver disponível.
- Faça uma decisão por pergunta, no máximo 4 perguntas por chamada e `header` com até 30 caracteres.
- Ofereça de 2 a 5 opções concretas; a recomendada vem primeiro com `(Recomendado)` no rótulo e uma `description` que explica a consequência técnica de cada escolha.
- Não inclua opção "outro"/"digitar": o ambiente já adiciona a resposta livre automaticamente. Não inclua opção para pular a entrevista.
- Não pergunte o que já foi respondido no prompt, no PRD, em documentos do projeto, no código ou em respostas anteriores.
- Se a ferramenta não estiver disponível, faça a mesma entrevista em texto, deixe claro que é uma contingência e registre a limitação no TDD.

### Classificação de ambiguidades técnicas

Antes de perguntar, decidir ou registrar uma pendência, classifique cada ambiguidade encontrada no prompt, no PRD, nos documentos ou no código:

| Campo | Valores |
| --- | --- |
| **ID** | `AMB-001`, `AMB-002`… (locais ao TDD; use `PRD:AMB-xxx` para as do PRD) |
| **Tipo** | Arquitetura, Modelo de dados, Contrato de API, Integração, Segurança, Privacidade, Desempenho, Operação, Observabilidade, Testes, Tecnologia, Escopo técnico, Termo |
| **Impacto** | Bloqueante, Alto, Médio, Baixo |
| **Origem** | Prompt, PRD, Documento, Código, Inferência |
| **Ação** | Perguntar, Decidir, Registrar em aberto |

Critérios de impacto:

- **Bloqueante:** impede definir arquitetura, modelo de dados, contrato de API, modelo de implantação ou critério de aceitação técnica. Ex.: não se sabe se o cálculo acontece no cliente ou no servidor.
- **Alto:** muda uma decisão técnica central, segurança, conformidade, desempenho ou custo relevante. Ex.: não se sabe onde as fotos serão armazenadas.
- **Médio:** afeta detalhe de implementação com alternativa razoável. Ex.: formato de paginação de uma lista interna.
- **Baixo:** preferência editorial ou detalhe reversível. Ex.: convenção de nome de tabela.

Regras de decisão:

- **Bloqueante ou Alto → obrigatoriamente `Perguntar`** com a ferramenta `question` antes de finalizar o artefato.
- **Médio →** perguntar se a resposta for barata; caso contrário, `Decidir` com justificativa e registrar.
- **Baixo →** decidir e registrar, sem interromper o usuário.
- Toda ambiguidade entra no apêndice **Registro de ambiguidades** do TDD com ID, tipo, impacto, origem, pergunta feita, resposta/decisão e status (`Resolvida`, `Decisão confirmada`, `Em aberto`).
- Se o usuário não responder ou pedir para seguir (`--quick`, "siga com o que tem"), as ambiguidades Bloqueantes e Altas viram **decisões provisórias** (`Status: Proposta`) e precisam ser **confirmadas em bloco na validação final**; elas não podem simplesmente desaparecer.
- **O artefato não pode ser finalizado enquanto houver ambiguidade Bloqueante sem resposta ou sem decisão provisória confirmada pelo usuário.**

### O que exige um registro de decisão (ADR)

Registre `DEC-xxx` quando a escolha for difícil de reverter ou afetar mais de um componente. Exemplos:

- estilo arquitetural e fronteiras entre componentes;
- banco de dados, mecanismo de persistência e estratégia de migração;
- estilo de API, versionamento e contrato entre frontend e backend;
- autenticação, autorização e gestão de sessão;
- armazenamento de arquivos e retenção;
- processamento assíncrono, filas e consistência;
- cache, idempotência e controle de concorrência;
- observabilidade, deploy, ambientes e rollback;
- bibliotecas ou frameworks com custo de troca alto;
- estratégia de testes e metas de cobertura;
- integrações externas e contratos de terceiros.

Escolhas locais e reversíveis (nome de variável, organização interna de um arquivo, componente visual isolado) são documentadas na seção correspondente, sem ADR.

## Fluxo

### 1. Interpretar o prompt inicial

Extraia, mesmo que implicitamente:

- qual sistema, funcionalidade ou recorte será projetado;
- qual problema técnico ou requisito do PRD precisa ser atendido;
- quais restrições já existem (stack, infraestrutura, prazo, equipe, compliance);
- quais decisões já estão tomadas e por quem;
- o que está claramente fora do recorte.

Se o prompt já responder tudo com clareza, pule a rodada de descoberta, mas ainda classifique as ambiguidades e conduza a validação final. Nunca invente fatos, números, tecnologias ou decisões de arquitetura.

### 2. Inspecionar o contexto do projeto (somente leitura)

Antes de redigir e de montar a entrevista, leia o que existir e for relevante:

- PRD indicado ou o mais recente em `docs/prd/`;
- `README.md`, `docs/`, `openspec/specs/`, `openspec/changes/`;
- arquivos de solução/projeto, dependências, configuração e estrutura de pastas;
- modelos, esquemas, migrations, contratos de API e testes existentes;
- pipeline de CI/CD, containers, infraestrutura como código e configuração de ambientes.

Use os achados para fundamentar o TDD, reduzir perguntas redundantes e não contradizer decisões já registradas. Não altere código nem artefatos existentes. Se o projeto for greenfield ou não tiver código, baseie-se nos documentos disponíveis e declare a limitação no documento.

### 3. Montar o inventário técnico

Antes da entrevista, registre internamente:

| Item | Estado | Evidência | Implicação |
| --- | --- | --- | --- |
| Stack e versões | existe/previsto/desconhecido | arquivo ou documento | restrição ou decisão a tomar |
| Componentes existentes | … | … | reutilizar, estender ou criar |
| Dados e migrations | … | … | compatibilidade e migração |
| APIs e integrações | … | … | contrato e compatibilidade |
| Testes e CI/CD | … | … | qualidade e entrega |
| Segurança e observabilidade | … | … | lacunas a decidir |

Esse inventário evita perguntar o que o repositório já responde e revela decisões que precisam ser tomadas.

### 4. Conduzir a entrevista de descoberta e classificar ambiguidades

Siga a seção **Entrevista obrigatória** e o roteiro de `references/interview-guide.md`.

a. **Levante as ambiguidades** do prompt, do PRD, dos documentos e do código, com IDs `AMB-001`….
b. **Classifique cada uma** por tipo e impacto usando a tabela acima.
c. **Pergunte** todas as Bloqueantes e Altas com a ferramenta `question`, agrupadas por tema (máximo 4 por chamada). Cubra, no mínimo, o que faltar sobre:
   - recorte do PRD e critérios de aceitação técnica;
   - arquitetura e fronteiras entre componentes;
   - persistência, modelo de dados e migração;
   - contratos de API e integrações;
   - autenticação, autorização e proteção de dados;
   - desempenho, disponibilidade e escala;
   - observabilidade, deploy, backup e rollback;
   - estratégia de testes e metas de qualidade.
d. **Incorpore as respostas** ao TDD, crie os registros `DEC-xxx` correspondentes e atualize o status da ambiguidade. Se a resposta abrir uma nova ambiguidade material, classifique e pergunte novamente.
e. **Decida apenas o que for Médio/Baixo**, registrando a justificativa, as alternativas descartadas e o impacto caso a decisão esteja errada.

### 5. Definir slug e arquivo de saída

- Gere um slug kebab-case em ASCII a partir do nome do sistema ou da funcionalidade (ex.: "Módulo de Check-in" → `modulo-de-check-in`).
- Use `--out` quando informado (relativo ao diretório atual). Caso contrário, `docs/tdd/<slug>.md`.
- Crie os diretórios necessários.
- Se o arquivo já existir, **não sobrescreva sem confirmação**: pergunte se deve substituir, criar `<slug>-v2.md` ou atualizar a versão existente preservando o histórico.

### 6. Projetar a solução

Leia `references/tdd-template.md` e use a estrutura dele como esqueleto obrigatório. Aprofunde cada dimensão:

- **Arquitetura e componentes:** estilo arquitetural, fronteiras, responsabilidades, dependências permitidas e proibidas. Use diagramas C4 em Mermaid (contexto, containers e, quando útil, componentes).
- **Modelo de dados:** entidades, relacionamentos, esquema lógico com tipos e restrições, índices, integridade, migrations, retenção e expurgo. Use `erDiagram` em Mermaid.
- **Contratos de API:** princípios, endpoints, payloads de exemplo, modelo de erros, paginação, filtros, idempotência, versionamento, autenticação e limites.
- **Fluxos:** caminho feliz, alternativas, erros, timeouts, concorrência e máquinas de estado. Use `sequenceDiagram` e `stateDiagram-v2` em Mermaid.
- **Aspectos transversais:** segurança, privacidade/LGPD, observabilidade, desempenho, resiliência, concorrência, configuração, feature flags, localização e acessibilidade.
- **Testes:** pirâmide, matriz por componente, dados de teste, metas de cobertura e testes não funcionais (carga, segurança, acessibilidade, contrato).
- **Implantação e operação:** ambientes, pipeline, infraestrutura, segredos, backup, recuperação, rollout, migração e rollback.
- **Decisões:** um registro ADR por decisão material, com contexto, alternativas, decisão, justificativa, consequências e dono.
- **Riscos técnicos:** probabilidade, impacto, mitigação, dono e decisão relacionada.
- **Plano de implementação:** fases, ordem, dependências e critério de conclusão técnico.

#### 6.1 Diagramas Mermaid (sugeridos em todo TDD)

Sugira e produza diagramas Mermaid sempre que eles explicarem melhor do que texto corrido. Todo TDD deve **avaliar** os diagramas abaixo; os que não fizerem sentido devem ser marcados como `Não aplicável` com justificativa curta.

| Pergunta que o diagrama responde | Tipo Mermaid | Seção sugerida |
| --- | --- | --- |
| Quem usa o sistema e com quais sistemas externos ele fala? | `flowchart` (C4 contexto) | 4.2 |
| Quais aplicações, bancos, filas e storages existem? | `flowchart` (C4 containers) | 4.3 |
| Como os componentes internos se relacionam? | `flowchart` (C4 componentes) | 4.4 |
| Como as entidades e tabelas se relacionam? | `erDiagram` | 6.1 |
| Como o domínio se organiza em classes/agregados? | `classDiagram` | 6.1 (opcional) |
| Como uma requisição percorre o sistema, passo a passo? | `sequenceDiagram` | 8.1 |
| Quais estados existem e quais transições são válidas? | `stateDiagram-v2` | 8.3 |
| Qual o caminho de decisão, erro ou compensação? | `flowchart` | 8.2 |
| Qual a ordem e as dependências das fases? | `gantt` ou `flowchart` | 14 |
| Como o ambiente é implantado e conectado? | `flowchart` | 11.3 |

Regras para diagramas:

- **Um assunto por diagrama.** Se passar de ~15 nós, divida em visões (contexto, fluxo, detalhe).
- **Título e rótulos claros**, em pt-BR, usando os mesmos nomes e IDs do texto (`C-001`, `API-001`, `DEC-001`).
- **Coerência obrigatória:** o diagrama não pode contradizer o texto, o modelo de dados ou o registro de decisões.
- **Sem dados sensíveis:** não inclua nomes de clientes, placas reais, segredos ou credenciais.
- **Versionável:** prefira Mermaid; se o ambiente não renderizar, mantenha o bloco e descreva o diagrama em texto.
- **Catálogo:** registre em `4.6` quais diagramas foram incluídos e quais foram dispensados.

Regras de conteúdo:

- **IDs:** `OBJ` (objetivos técnicos), `RT` (restrições), `PT` (premissas técnicas), `C` (componentes), `API` (endpoints), `E` (entidades), `INT` (integrações), `DEC` (decisões), `R` (riscos), `Q` (perguntas), `AMB` (ambiguidades). Referencie requisitos do PRD como `PRD:RF-001` / `PRD:RNF-001`.
- **Rastreabilidade:** todo requisito do PRD dentro do recorte deve aparecer na matriz com componente, contrato/dado, teste e decisão.
- **Diagramas:** siga o catálogo da seção 6.1. Todo diagrama deve ter título, rótulos claros e coerência com IDs e nomes do texto. Relacionamentos, fluxos e estados são os candidatos prioritários.
- **Trechos ilustrativos:** assinaturas, payloads, DDL e pseudocódigo são permitidos apenas como ilustração, marcados como tal. Não implemente.
- **Não aplicável:** seções que não fizerem sentido devem ser marcadas como `Não aplicável` com justificativa curta — nunca placeholders vazios.

### 7. Autorrevisar antes de salvar

Leia `references/review-checklist.md` e aplique cada item. Corrija o documento antes de gravá-lo. Verificações mínimas:

- todos os IDs são únicos e as referências cruzadas existem;
- toda decisão material está no registro ADR e nenhuma seção contradiz o registro;
- todo requisito do PRD no recorte tem tratamento e rastreabilidade;
- contratos, dados, fluxos e erros estão coerentes entre si;
- os diagramas Mermaid renderizam, têm título e são coerentes com o texto, os IDs e os nomes;
- segurança, privacidade, observabilidade, backup e rollback foram tratados;
- não há tecnologia, número ou fato inventado sem marcação de premissa;
- toda ambiguidade foi classificada e as Bloqueantes/Altas foram perguntadas ou confirmadas como decisão provisória;
- o Registro de ambiguidades está coerente com as premissas e perguntas em aberto.

### 8. Validar o rascunho com o usuário

Antes de salvar, apresente um resumo curto do rascunho e use a ferramenta `question` para uma última rodada de confirmação. Essa rodada é obrigatória mesmo com `--quick` e mesmo quando não há perguntas em aberto.

Pergunte, no máximo em duas chamadas:

1. **Arquitetura e decisões** — a arquitetura proposta e os registros `DEC` estão corretos? Alguma decisão precisa ser revista?
2. **Riscos e pendências** — as premissas e decisões provisórias podem ser confirmadas? Alguma pendência impede a aprovação?
3. **Diagramas** — os diagramas propostos cobrem relacionamentos, fluxos e estados? Falta algum que ajude a explicar o design?

Opções típicas da rodada final: `Aprovar e salvar` (Recomendado), `Revisar arquitetura`, `Revisar decisões`, `Responder pendências`.
Se existir ambiguidade **Bloqueante** sem resposta ou sem decisão provisória confirmada, **não ofereça** `Aprovar e salvar`; ofereça apenas as opções que resolvem a pendência.

Incorpore os ajustes e repita a validação se a arquitetura ou as decisões mudarem materialmente.

### 9. Salvar e entregar

Grave o arquivo no caminho definido. Na resposta final, informe de forma concisa:

- caminho do TDD criado;
- resumo em 2–3 frases da solução técnica e do recorte coberto;
- quantas decisões foram registradas (`Aceitas`, `Propostas`, `Substituídas`);
- quantas ambiguidades foram resolvidas, confirmadas ou permanecem em aberto;
- quais diagramas Mermaid foram incluídos e em quais seções;
- as 3 decisões ou perguntas em aberto mais críticas;
- sugestão de próximo passo: revisar com o time de engenharia e, se o projeto usa OpenSpec, usar o TDD como insumo de `/opsx-propose` ou `/opsx-apply`.

Não implemente nada, não crie código, migrations ou configuração e não inicie o fluxo de proposta sem pedido explícito do usuário.

## Casos especiais

| Situação | Ação |
| --- | --- |
| Prompt vago ("quero um design do sistema") | Conduzir a entrevista completa; não gerar TDD genérico. |
| PRD existe mas não foi indicado | Localizar o mais recente em `docs/prd/`, confirmar com o usuário e citá-lo como referência. |
| PRD e código divergem | Registrar a divergência, perguntar qual prevalece e criar uma decisão `DEC` sobre o alinhamento. |
| Projeto greenfield | Projetar a solução e declarar explicitamente que não há código de referência. |
| Projeto legado | Priorizar compatibilidade, migração incremental, convivência e rollback. |
| Pedido de TDD + implementação | Entregar apenas o TDD e avisar que a implementação exige um novo pedido. |
| Documento já existente | Perguntar antes de sobrescrever; preservar histórico de versão no cabeçalho. |
| Usuário pede `--quick` | Encurtar a descoberta, mas classificar ambiguidades e fazer a validação final com `question`. |
| Decisão tomada durante a redação | Registrar `DEC` na hora, atualizar a matriz e não deixar a escolha só no texto corrido. |
| Requisito do PRD sem solução técnica viável | Registrar o conflito, propor alternativa e abrir pergunta em aberto com dono. |
| Dados pessoais ou pagamentos | Incluir segurança, base legal, retenção, auditoria e tratamento de falha no design. |
| Múltiplas unidades ou multi-tenant | Decidir e registrar isolamento, chaves, permissões e migração. |

## Arquivos de apoio

- `references/tdd-template.md` — estrutura obrigatória do documento.
- `references/interview-guide.md` — roteiro da entrevista, classificação de ambiguidades e padrão de decisão.
- `references/review-checklist.md` — checklist de qualidade e definição de pronto técnico.
