---
name: generate-prd
description: Gera um Documento de Requisitos de Produto (PRD) completo, estruturado e acionável a partir de um prompt inicial. Use quando o usuário pedir para criar, escrever ou gerar um PRD, documento de requisitos, especificação de produto, ou quando executar /generate-prd.
---

# Gerar PRD

Transforma um prompt inicial em um PRD completo, verificável e pronto para revisão com stakeholders.

## Entradas

- **Prompt inicial (obrigatório):** descrição do produto, funcionalidade ou iniciativa.
- **Opções reconhecidas no prompt** (todas opcionais):
  - `--out <caminho>`: arquivo de saída. Padrão: `docs/prd/<slug>.md`.
  - `--lang <idioma>`: idioma do documento. Padrão: o mesmo do prompt inicial.
  - `--quick`: não fazer perguntas; preencher lacunas com premissas explícitas.
  - `--mvp`: limitar o escopo funcional ao mínimo viável.

Se o prompt não trouxer informação suficiente, siga o fluxo de esclarecimento abaixo. Nunca invente fatos, números, pesquisas ou decisões de negócio.

## Princípios

1. **Não inventar.** Dados ausentes viram premissa explícita ou pergunta em aberto, nunca afirmação.
2. **Product-level, não técnico.** Descreva comportamento e resultados esperados. Arquitetura, bibliotecas e esquema de banco só entram quando forem restrição de produto.
3. **Testável.** Todo requisito funcional tem critério de aceitação verificável; todo requisito não funcional tem meta mensurável.
4. **Rastreável.** Objetivo → requisito → critério de aceitação → métrica. Sem requisito órfão.
5. **Escopo explícito.** O que está fora é tão importante quanto o que está dentro.
6. **Linguagem do usuário.** Escreva no idioma do prompt inicial, salvo `--lang`.

## Fluxo

### 1. Interpretar o prompt inicial

Extraia, mesmo que implicitamente:

- problema ou oportunidade;
- produto/funcionalidade e resultado esperado;
- público-alvo;
- escopo percebido e não-objetivos;
- restrições (prazo, orçamento, compliance, integrações);
- métricas de sucesso, se houver.

Se o prompt já responder tudo com clareza, pule direto para o passo 4.

### 2. Esclarecer o que for material

Pergunte **somente** o que muda escopo, comportamento observável, critérios de aceitação ou viabilidade. No máximo 5 perguntas, agrupadas e objetivas, cobrindo o que faltar:

1. Qual problema/objetivo de negócio o produto resolve e por que agora?
2. Quem são os usuários e qual a jornada principal?
3. O que é obrigatório no MVP e o que está explicitamente fora?
4. Como o sucesso será medido (métrica, baseline e meta)?
5. Quais restrições existem (prazo, orçamento, integrações, segurança, LGPD)?

Use a ferramenta de perguntas quando disponível; caso contrário, pergunte em texto. Se o usuário responder `--quick`, "siga com o que tem" ou equivalente, **não pergunte**: prossiga com premissas e registre cada uma na seção "Premissas".

### 3. Inspecionar o contexto do projeto (somente leitura)

Antes de redigir, leia o que existir e for relevante:

- `README.md`, `docs/`, `openspec/specs/`, `openspec/changes/`;
- configurações e código que revelem domínio, entidades, integrações e restrições;
- documentos de entrevista, pesquisa ou requisitos já existentes.

Use os achados para fundamentar o PRD e para não contradizer decisões já registradas. Não altere código nem artefatos existentes. Se o projeto não tiver código, baseie-se nos documentos disponíveis e declare a limitação.

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
- o documento está no idioma correto e sem placeholders vazios.

### 7. Salvar e entregar

Grave o arquivo no caminho definido. Na resposta final, informe de forma concisa:

- caminho do PRD criado;
- resumo em 2–3 frases do produto e do escopo do MVP;
- as 3 perguntas em aberto mais críticas;
- sugestão de próximo passo: revisar com stakeholders e, se o projeto usa OpenSpec, rodar `/opsx-propose` com o PRD como insumo.

Não implemente nada, não crie código e não inicie o fluxo de proposta sem pedido explícito do usuário.

## Casos especiais

| Situação | Ação |
| --- | --- |
| Prompt vago ("quero um app") | Fazer as perguntas do passo 2; não gerar PRD genérico. |
| Prompt muito extenso | Extrair requisitos, agrupar por tema e confirmar o recorte antes de escrever. |
| Pedido de PRD + implementação | Entregar apenas o PRD e avisar que a implementação exige um novo pedido. |
| Produto com dados pessoais | Incluir seção de privacidade/LGPD com base legal, retenção, consentimento e direitos do titular (ou marcar `a definir`). |
| Documento já existente | Perguntar antes de sobrescrever; preservar histórico de versão no cabeçalho. |
| Usuário pede em outro idioma | Respeitar `--lang` ou o idioma explícito do pedido. |

## Arquivos de apoio

- `references/prd-template.md` — estrutura obrigatória do documento.
- `references/review-checklist.md` — checklist de qualidade e definição de pronto.
