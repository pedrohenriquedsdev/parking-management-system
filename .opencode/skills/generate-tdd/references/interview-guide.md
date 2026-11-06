# Guia da entrevista técnica e registro de decisões

Este guia operacionaliza a seção **Entrevista obrigatória** da skill `generate-tdd`. Use-o junto com `tdd-template.md` e `review-checklist.md`.

> **TDD = Technical Design Document.** O foco é decisão técnica, não teste de software — embora a estratégia de testes seja uma das dimensões do documento.

## 1. Quando a entrevista acontece

| Momento | Objetivo | Ferramenta |
| --- | --- | --- |
| **Descoberta** | Fechar recorte, arquitetura, dados, contratos, segurança, operação e qualidade antes de redigir | `question` |
| **Validação final** | Confirmar arquitetura, decisões `DEC`, premissas e pendências antes de salvar | `question` |

Ambas são obrigatórias. Mesmo que o prompt ou o PRD pareçam completos, classifique as ambiguidades técnicas e faça a validação final.

## 2. Classificação de ambiguidades técnicas

Registre cada ambiguidade com ID `AMB-001`… e classifique:

| Campo | Valores | Pergunta de apoio |
| --- | --- | --- |
| Tipo | Arquitetura, Modelo de dados, Contrato de API, Integração, Segurança, Privacidade, Desempenho, Operação, Observabilidade, Testes, Tecnologia, Escopo técnico, Termo | O que exatamente está indefinido? |
| Impacto | Bloqueante, Alto, Médio, Baixo | Isso impede fechar arquitetura, contrato, dado ou critério de aceitação técnica? |
| Origem | Prompt, PRD, Documento, Código, Inferência | De onde veio a lacuna? |
| Ação | Perguntar, Decidir, Registrar em aberto | O que faremos com ela? |

### Matriz de decisão

| Impacto | Critério | Ação |
| --- | --- | --- |
| **Bloqueante** | Impede definir arquitetura, modelo de dados, contrato de API, implantação ou critério de aceitação técnica | `Perguntar` antes de finalizar. Sem resposta, vira decisão provisória a confirmar na validação final. |
| **Alto** | Muda decisão central, segurança, conformidade, desempenho ou custo relevante | `Perguntar` antes de finalizar. |
| **Médio** | Afeta detalhe de implementação com alternativa razoável | Perguntar se for barato; senão `Decidir` com justificativa. |
| **Baixo** | Preferência editorial ou detalhe reversível | `Decidir` e registrar. |

### Exemplos

| ID | Tipo | Impacto | Situação | Ação |
| --- | --- | --- | --- | --- |
| AMB-001 | Arquitetura | Bloqueante | Não se sabe se o cálculo de diárias é no cliente ou no servidor | Perguntar |
| AMB-002 | Modelo de dados | Bloqueante | Não se sabe como uma vaga é associada a um veículo ao longo do tempo | Perguntar |
| AMB-003 | Contrato de API | Alto | Não se sabe se a API é REST ou GraphQL | Perguntar |
| AMB-004 | Segurança | Alto | Não se sabe como o token é armazenado no frontend | Perguntar |
| AMB-005 | Operação | Alto | Não se sabe onde as fotos serão armazenadas | Perguntar |
| AMB-006 | Observabilidade | Médio | Formato de log estruturado | Decidir |
| AMB-007 | Termo | Baixo | "Permanência" e "estadia" usados como sinônimos | Decidir e padronizar |
| AMB-008 | Testes | Médio | Ferramenta de teste de integração | Decidir |

### Registro no TDD

Toda ambiguidade aparece no apêndice **Registro de ambiguidades**, mesmo as decididas:

| ID | Tipo | Impacto | Origem | Ação | Pergunta feita | Resposta/Decisão | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AMB-001 | Arquitetura | Bloqueante | PRD | Perguntar | O cálculo ocorre no cliente ou no servidor? | No servidor, como fonte única | Resolvida |
| AMB-006 | Observabilidade | Médio | Inferência | Decidir | — | Logs JSON com trace id | Decisão confirmada |

Status permitidos: `Resolvida`, `Decisão confirmada`, `Em aberto`.

## 3. Como registrar decisões (ADR)

Toda decisão material vira um `DEC-xxx` no capítulo 12 do TDD. O registro é a **fonte única da verdade**: o restante do documento referencia o `DEC` e não o contradiz.

### Quando criar um `DEC`

Crie quando a escolha for difícil de reverter ou afetar mais de um componente:

- estilo arquitetural, fronteiras e comunicação entre componentes;
- banco, persistência, ORM e migração;
- estilo de API, versionamento e contrato;
- autenticação, autorização e sessão;
- armazenamento de arquivos e retenção;
- processamento assíncrono, filas e consistência;
- cache, idempotência e concorrência;
- observabilidade, deploy, ambientes e rollback;
- bibliotecas/frameworks com custo de troca alto;
- estratégia e metas de teste;
- integrações externas.

Não crie `DEC` para escolhas locais e reversíveis: elas vão na seção correspondente.

### Estrutura mínima

| Campo | Conteúdo |
| --- | --- |
| **ID** | `DEC-001`… |
| **Status** | `Proposta`, `Aceita`, `Substituída`, `Rejeitada` |
| **Data** | `AAAA-MM-DD` |
| **Dono** | Nome ou `a definir` |
| **Contexto** | Forças, restrições e problema que exigem a decisão |
| **Decisão** | O que foi decidido, em uma frase |
| **Alternativas** | Pelo menos duas, com prós, contras e motivo do descarte |
| **Justificativa** | Por que esta opção venceu |
| **Consequências** | Impactos positivos, negativos, custos e dívidas |
| **Requisitos impactados** | `PRD:RF-001`, `PRD:RNF-001` |
| **Riscos associados** | `R-001` |
| **Substituição** | Qual `DEC` substitui ou é substituída |

### Regras de status

- `Aceita` só quando o usuário confirmou na entrevista ou existe autoridade documentada.
- `Proposta` quando o agente decidiu por conta própria ou a confirmação ficou pendente.
- `Substituída` mantém o histórico e aponta para a decisão nova.
- `Rejeitada` registra a alternativa descartada e o motivo, para evitar retrabalho.

## 4. Roteiro de descoberta

Pergunte apenas o que faltar. Agrupe por tema e faça no máximo 4 perguntas por chamada.

### Rodada A — Recorte e restrições

1. Qual parte do PRD ou do sistema este TDD cobre?
2. Existe stack, infraestrutura ou fornecedor obrigatório?
3. O que já existe hoje (código, banco, APIs, integrações)?
4. Há prazo, equipe, orçamento ou restrição regulatória que condicione o design?

Opções úteis: recorte mínimo do fluxo, fluxo completo, plataforma inteira; stack fixa, stack preferencial, livre.

### Rodada B — Arquitetura e componentes

1. Qual estilo arquitetural faz sentido (monólito modular, camadas, microserviços, serverless)?
2. Como frontend e backend se comunicam (REST, GraphQL, RPC)?
3. O que precisa ser síncrono e o que pode ser assíncrono?
4. Existe mais de uma unidade, tenant ou canal de acesso?

Opções úteis: monólito modular, microsserviços, serverless; REST versionada, GraphQL, gRPC.

### Rodada C — Dados e integrações

1. Qual banco de dados e mecanismo de persistência?
2. Como será o versionamento e a migração de schema?
3. Onde ficam arquivos e fotos? Qual o ciclo de vida?
4. Quais integrações externas existem e qual o contrato?

Opções úteis: relacional, documento, chave-valor; migrations versionadas, expand/contract; storage object, banco, filesystem.

### Rodada D — Segurança, operação e qualidade

1. Como será a autenticação e a autorização?
2. Como segredos e dados sensíveis serão protegidos?
3. Quais sinais de observabilidade e alertas são necessários?
4. Como será o CI/CD, o ambiente e o rollback?
5. Qual a estratégia e a meta de testes?

Opções úteis: sessão por cookie, JWT, provedor externo; RBAC, ABAC; cobertura por risco, meta percentual.

### Temas condicionais

Pergunte somente quando o domínio indicar:

- **Pagamentos:** idempotência, conciliação, estorno, arredondamento, moeda.
- **LGPD/privacidade:** base legal, minimização, retenção, expurgo, direitos do titular.
- **Multi-tenant:** isolamento, chaves, permissões, migração entre tenants.
- **Offline/mobilidade:** sincronização, conflito, armazenamento local.
- **Migração de legado:** convivência, corte, rollback, reconciliação.
- **Relatórios/BI:** modelo de leitura, agregação, volume, atualização.
- **Alta escala:** cache, filas, sharding, limites, custo.
- **Acessibilidade:** meta WCAG, navegação por teclado, leitor de tela.
- **Integrações:** contrato, versionamento, timeout, retry, fallback.

## 5. Como usar a ferramenta `question`

Regras:

- Um `header` curto (até 30 caracteres) e uma pergunta objetiva.
- De 2 a 5 opções concretas; a recomendada primeiro com `(Recomendado)`.
- Cada opção tem uma `description` com a consequência técnica da escolha.
- Não crie opção "outro", "digitar" ou "pular": o ambiente adiciona resposta livre automaticamente.
- Não repita perguntas já respondidas no prompt, no PRD, no código ou em respostas anteriores.
- Se a resposta abrir outra ambiguidade material, classifique e pergunte de novo.

Exemplo de chamada:

```json
{
  "questions": [
    {
      "header": "Estilo arquitetural",
      "question": "Qual estilo arquitetural deve orientar este recorte?",
      "options": [
        {
          "label": "Monólito modular (Recomendado)",
          "description": "Um deploy único com módulos bem separados; menor custo operacional e evolução gradual."
        },
        {
          "label": "Microsserviços",
          "description": "Serviços independentes; maior isolamento, porém mais infraestrutura e complexidade."
        },
        {
          "label": "Serverless",
          "description": "Funções gerenciadas por evento; bom para carga intermitente, com limites de execução."
        }
      ]
    },
    {
      "header": "Contrato de API",
      "question": "Como o frontend deve consumir o backend?",
      "options": [
        {
          "label": "REST versionada (Recomendado)",
          "description": "Contratos simples, cacheáveis e fáceis de documentar com OpenAPI."
        },
        {
          "label": "GraphQL",
          "description": "Consultas flexíveis pelo cliente, com maior custo de controle e cache."
        },
        {
          "label": "gRPC",
          "description": "Alto desempenho e contrato binário, com menor suporte nativo no navegador."
        }
      ]
    }
  ]
}
```

## 6. Validação final

Antes de salvar, apresente um resumo de 5 a 10 linhas e faça uma última rodada com `question`:

1. **Arquitetura e decisões** — a arquitetura proposta e os `DEC` estão corretos? Alguma decisão precisa ser revista?
2. **Riscos e pendências** — as premissas e decisões provisórias podem ser confirmadas? Alguma pendência bloqueia a aprovação?

Opções:

- `Aprovar e salvar` (Recomendado)
- `Revisar arquitetura`
- `Revisar decisões`
- `Responder pendências`

Regras:

- Se houver ambiguidade **Bloqueante** sem resposta ou sem decisão provisória confirmada, não ofereça `Aprovar e salvar`.
- Se o usuário pedir ajustes que mudem arquitetura ou decisões, atualize o TDD e repita a validação.
- Só grave o arquivo depois de uma resposta que autorize a finalização.

## 7. Exemplo de ciclo completo

1. Prompt: "Crie o TDD do módulo de check-in a partir do PRD".
2. Contexto lido: PRD, README e código existente (.NET + SQL Server + Angular).
3. Ambiguidades: cálculo no cliente/servidor (Bloqueante), banco (Alto), storage de fotos (Alto), logs (Médio), nomenclatura (Baixo).
4. Rodada B: perguntar estilo arquitetural e contrato de API.
5. Rodada C: perguntar banco, migração e storage.
6. Médio/Baixo: decidir logs estruturados e padronizar "permanência"; registrar.
7. Redigir o TDD, criar `DEC-001`… e preencher o Registro de ambiguidades.
8. Validação final: resumo + confirmação de arquitetura, `DEC` e premissas.
9. Salvar somente após `Aprovar e salvar`.
