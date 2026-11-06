---
name: generate-prd
description: Gera um Documento de Requisitos de Produto (PRD) completo, estruturado e acionável a partir de um prompt inicial, conduzindo uma entrevista estruturada e classificando ambiguidades com o sistema de perguntas do ambiente antes de finalizar. Use quando o usuário pedir para criar, escrever ou gerar um PRD, documento de requisitos, especificação de produto, ou quando executar /generate-prd.
---

# Gerar PRD

Transforma um prompt inicial em um PRD completo, verificável e pronto para revisão com stakeholders.

## Entradas

- **Prompt inicial (obrigatório):** descrição do produto, funcionalidade ou iniciativa.
- **Opções reconhecidas no prompt** (todas opcionais):
  - `--out <caminho>`: arquivo de saída. Padrão: `docs/prd/<slug>.md`.
  - `--lang <idioma>`: idioma do documento. Padrão: o mesmo do prompt inicial.
  - `--quick`: encurtar a entrevista de descoberta; a classificação de ambiguidades e a confirmação final das premissas continuam obrigatórias.
  - `--mvp`: limitar o escopo funcional ao mínimo viável.

A entrevista é parte obrigatória do fluxo. O prompt inicial nunca é suficiente por si só: mesmo que pareça completo, as ambiguidades devem ser classificadas e, quando materiais, resolvidas com o usuário antes de o artefato ser finalizado.

## Princípios

1. **Não inventar.** Dados ausentes viram premissa explícita ou pergunta em aberto, nunca afirmação.
2. **Product-level, não técnico.** Descreva comportamento e resultados esperados. Arquitetura, bibliotecas e esquema de banco só entram quando forem restrição de produto.
3. **Testável.** Todo requisito funcional tem critério de aceitação verificável; todo requisito não funcional tem meta mensurável.
4. **Rastreável.** Objetivo → requisito → critério de aceitação → métrica. Sem requisito órfão.
5. **Escopo explícito.** O que está fora é tão importante quanto o que está dentro.
6. **Linguagem do usuário.** Escreva no idioma do prompt inicial, salvo `--lang`.
7. **Entrevista explícita.** Não deduza silenciosamente o que pode ser perguntado. Toda decisão material é confirmada com o usuário.
8. **Ambiguidade classificada.** Nenhuma ambiguidade é resolvida "no achismo": ela recebe tipo, impacto e ação, e as materiais são levadas ao sistema de perguntas do ambiente.

## Entrevista obrigatória

A geração do PRD inclui uma entrevista estruturada com o usuário. Ela não é opcional e acontece em dois momentos:

1. **Descoberta** — antes de redigir, para fechar problema, público, escopo, sucesso e restrições.
2. **Validação final** — depois do rascunho e antes de salvar, para confirmar escopo, prioridades, premissas e pendências.

### Regras da entrevista

- Use **sempre** a ferramenta de perguntas do ambiente (`question`) em cada rodada. Não substitua a entrevista por uma lista de perguntas em texto livre quando a ferramenta estiver disponível.
- Faça uma decisão por pergunta, no máximo 4 perguntas por chamada e `header` com até 30 caracteres.
- Ofereça de 2 a 5 opções concretas; a recomendada vem primeiro com `(Recomendado)` no rótulo e uma `description` que explica a consequência de cada escolha.
- Não inclua opção "outro"/"digitar": o ambiente já adiciona a resposta livre automaticamente. Não inclua opção para pular a entrevista.
- Não pergunte o que já foi respondido no prompt, em documentos do projeto ou em respostas anteriores.
- Se a ferramenta não estiver disponível, faça a mesma entrevista em texto, deixe claro que é uma contingência e registre a limitação no PRD.

### Classificação de ambiguidades

Antes de perguntar, assumir ou registrar uma pendência, classifique cada ambiguidade encontrada no prompt, nos documentos ou no código:

| Campo | Valores |
| --- | --- |
| **ID** | `AMB-001`, `AMB-002`… |
| **Tipo** | Escopo, Comportamento, Regra de negócio, Dados, Métrica, Restrição, Integração, Termo |
| **Impacto** | Bloqueante, Alto, Médio, Baixo |
| **Origem** | Prompt, Documento, Código, Inferência |
| **Ação** | Perguntar, Assumir, Registrar em aberto |

Critérios de impacto:

- **Bloqueante:** impede definir escopo, comportamento observável ou critério de aceitação. Ex.: não se sabe se o pagamento é por hora ou por diária.
- **Alto:** muda prioridade, métrica de sucesso, regra de negócio ou conformidade. Ex.: não se sabe quem pode cancelar uma reserva.
- **Médio:** afeta detalhe de experiência ou operação, mas há alternativa razoável. Ex.: ordem dos campos de um formulário.
- **Baixo:** preferência editorial ou detalhe reversível. Ex.: variação de nomenclatura.

Regras de decisão:

- **Bloqueante ou Alto → obrigatoriamente `Perguntar`** com a ferramenta `question` antes de finalizar o artefato.
- **Médio →** perguntar se a resposta for barata; caso contrário, assumir com premissa explícita.
- **Baixo →** assumir e registrar, sem interromper o usuário.
- Toda ambiguidade entra no apêndice **Registro de ambiguidades** do PRD com ID, tipo, impacto, pergunta feita, resposta e status (`Resolvida`, `Premissa confirmada`, `Em aberto`).
- Se o usuário não responder ou pedir para seguir (`--quick`, "siga com o que tem"), as ambiguidades Bloqueantes e Altas viram premissas explícitas e precisam ser **confirmadas em bloco na validação final**; elas não podem simplesmente desaparecer.
- **O artefato não pode ser finalizado enquanto houver ambiguidade Bloqueante sem resposta ou sem premissa confirmada pelo usuário.**

## Fluxo

### 1. Interpretar o prompt inicial

Extraia, mesmo que implicitamente:

- problema ou oportunidade;
- produto/funcionalidade e resultado esperado;
- público-alvo;
- escopo percebido e não-objetivos;
- restrições (prazo, orçamento, compliance, integrações);
- métricas de sucesso, se houver.

Se o prompt já responder tudo com clareza, pule a rodada de descoberta, mas ainda classifique as ambiguidades e conduza a validação final. Nunca invente fatos, números, pesquisas ou decisões de negócio.

### 2. Inspecionar o contexto do projeto (somente leitura)

Antes de redigir e de montar a entrevista, leia o que existir e for relevante:

- `README.md`, `docs/`, `openspec/specs/`, `openspec/changes/`;
- configurações e código que revelem domínio, entidades, integrações e restrições;
- documentos de entrevista, pesquisa ou requisitos já existentes.

Use os achados para fundamentar o PRD, reduzir perguntas redundantes e não contradizer decisões já registradas. Não altere código nem artefatos existentes. Se o projeto não tiver código, baseie-se nos documentos disponíveis e declare a limitação.

### 3. Conduzir a entrevista de descoberta e classificar ambiguidades

Siga a seção **Entrevista obrigatória** e o roteiro de `references/interview-guide.md`.

a. **Levante as ambiguidades** do prompt, dos documentos e do código, com IDs `AMB-001`….
b. **Classifique cada uma** por tipo e impacto usando a tabela acima.
c. **Pergunte** todas as Bloqueantes e Altas com a ferramenta `question`, agrupadas por tema (máximo 4 por chamada). Cubra, no mínimo, o que faltar sobre:
   - problema/objetivo de negócio e por que agora;
   - usuários e jornada principal;
   - escopo obrigatório do MVP e não-objetivos;
   - métricas de sucesso (baseline e meta);
   - restrições (prazo, orçamento, integrações, segurança, LGPD).
d. **Incorpore as respostas** ao PRD e atualize o status da ambiguidade. Se a resposta abrir uma nova ambiguidade material, classifique e pergunte novamente.
e. **Assuma apenas o que for Médio/Baixo**, registrando a premissa e o impacto caso esteja errada. Premissa não é resposta: ela precisa constar no registro e na seção de premissas.

### 4. Definir slug e arquivo de saída

- Gere um slug kebab-case em ASCII a partir do nome do produto (ex.: "Gestão de Estacionamento" → `gestao-de-estacionamento`).
- Use `--out` quando informado (relativo ao diretório atual). Caso contrário, `docs/prd/<slug>.md`.
- Crie os diretórios necessários.
- Se o arquivo já existir, **não sobrescreva sem confirmação**: pergunte se deve substituir, criar `<slug>-v2.md` ou atualizar a versão existente.

### 5. Redigir o PRD

Leia `references/prd-template.md` e use a estrutura dele como esqueleto obrigatório. Você pode:

- adicionar seções específicas do domínio (ex.: "Regras de tarifação", "Requisitos regulatórios");
- marcar seções não aplicáveis como `Não aplicável` com justificativa curta — nunca deixe placeholders vazios;
- condensar seções em produtos muito pequenos, registrando o motivo.

Regras de conteúdo:

- **Requisitos funcionais:** IDs `RF-001`, `RF-002`… com título, descrição, prioridade MoSCoW (`Must`/`Should`/`Could`/`Won't`), regras associadas, critérios de aceitação em Gherkin (`Dado/Quando/Então`) e dependências.
- **Regras de negócio:** IDs `RN-001`…, cada uma referenciada pelos requisitos que a utilizam.
- **Requisitos não funcionais:** IDs `RNF-001`…, por categoria, com meta mensurável e forma de verificação (ex.: "p95 < 300 ms em 500 usuários simultâneos").
- **Métricas:** IDs `M-001`…, com baseline, meta, prazo e fonte do dado. Sem baseline, escreva `baseline a definir` — não estime sem base.
- **Personas e jornadas:** IDs `P-001`/`J-001`, com objetivos, dores e passos da jornada.
- **Riscos:** probabilidade, impacto, mitigação e responsável (ou `a definir`).
- **Premissas:** o que foi assumido para escrever o documento e o impacto se estiver errado.
- **Perguntas em aberto:** IDs `Q-001`…, com dono e prazo, quando conhecidos.
- **Ambiguidades:** preencha o apêndice **Registro de ambiguidades** com `AMB-001`…, tipo, impacto, origem, ação, pergunta feita, resposta e status. Nenhuma ambiguidade Bloqueante pode ficar sem resolução ou premissa confirmada.
- **Decisões:** registre data, decisão e justificativa quando o prompt ou o projeto já tiverem definido algo.

Priorização:

- Sem indicação do usuário, use `Must` para o MVP, `Should` para a próxima fase e `Could` para melhorias.
- Com `--mvp`, tudo que não for indispensável ao fluxo principal vira `Should`/`Won't` na seção de escopo futuro.

### 6. Autorrevisar antes de salvar

Leia `references/review-checklist.md` e aplique cada item. Corrija o documento antes de gravá-lo. Verificações mínimas:

- todos os IDs são únicos e as referências cruzadas existem;
- todo `Must` tem critério de aceitação;
- nenhuma métrica ou fato foi inventado;
- não há contradição entre escopo, requisitos e não-objetivos;
- o documento está no idioma correto e sem placeholders vazios;
- toda ambiguidade foi classificada e as Bloqueantes/Altas foram perguntadas ou confirmadas como premissa;
- o Registro de ambiguidades está preenchido e coerente com as seções de premissas e perguntas em aberto.

### 7. Validar o rascunho com o usuário

Antes de salvar, apresente um resumo curto do rascunho e use a ferramenta `question` para uma última rodada de confirmação. Essa rodada é obrigatória mesmo com `--quick` e mesmo quando não há perguntas em aberto.

Pergunte, no máximo em duas chamadas:

1. **Escopo e prioridades** — o MVP e os não-objetivos estão corretos? As prioridades `Must`/`Should` refletem a realidade?
2. **Premissas e pendências** — as premissas assumidas podem ser confirmadas? Alguma pendência impede a aprovação?

Opções típicas da rodada final: `Aprovar e salvar` (Recomendado), `Ajustar escopo`, `Revisar premissas`, `Responder pendências`.
Se existir ambiguidade **Bloqueante** sem resposta ou sem premissa confirmada, **não ofereça** `Aprovar e salvar`; ofereça apenas as opções que resolvem a pendência.

Incorpore os ajustes e repita a validação se o escopo ou as premissas mudarem materialmente.

### 8. Salvar e entregar

Grave o arquivo no caminho definido. Na resposta final, informe de forma concisa:

- caminho do PRD criado;
- resumo em 2–3 frases do produto e do escopo do MVP;
- quantas ambiguidades foram resolvidas, confirmadas como premissa ou permanecem em aberto;
- as 3 perguntas em aberto mais críticas;
- sugestão de próximo passo: revisar com stakeholders e, se o projeto usa OpenSpec, rodar `/opsx-propose` com o PRD como insumo.

Não implemente nada, não crie código e não inicie o fluxo de proposta sem pedido explícito do usuário.

## Casos especiais

| Situação | Ação |
| --- | --- |
| Prompt vago ("quero um app") | Conduzir a entrevista de descoberta completa; não gerar PRD genérico. |
| Prompt muito extenso | Extrair requisitos, agrupar por tema, classificar ambiguidades e confirmar o recorte antes de escrever. |
| Pedido de PRD + implementação | Entregar apenas o PRD e avisar que a implementação exige um novo pedido. |
| Produto com dados pessoais | Incluir seção de privacidade/LGPD com base legal, retenção, consentimento e direitos do titular (ou marcar `a definir`). |
| Documento já existente | Perguntar antes de sobrescrever; preservar histórico de versão no cabeçalho. |
| Usuário pede em outro idioma | Respeitar `--lang` ou o idioma explícito do pedido. |
| Usuário pede `--quick` | Encurtar a descoberta, mas classificar ambiguidades e fazer a validação final das premissas com `question`. |
| Ambiguidade descoberta durante a redação | Interromper a redação, classificar e, se Bloqueante/Alta, perguntar antes de continuar. |

## Arquivos de apoio

- `references/prd-template.md` — estrutura obrigatória do documento.
- `references/interview-guide.md` — roteiro da entrevista, classificação de ambiguidades e padrões de pergunta.
- `references/review-checklist.md` — checklist de qualidade e definição de pronto.
