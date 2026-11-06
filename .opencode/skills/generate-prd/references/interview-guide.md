# Guia da entrevista e classificação de ambiguidades

Este guia operacionaliza a seção **Entrevista obrigatória** da skill. Use-o junto com `prd-template.md` e `review-checklist.md`.

## 1. Quando a entrevista acontece

| Momento | Objetivo | Ferramenta |
| --- | --- | --- |
| **Descoberta** | Fechar problema, público, escopo, métricas e restrições antes de redigir | `question` |
| **Validação final** | Confirmar rascunho, premissas e pendências antes de salvar | `question` |

Ambas são obrigatórias. Mesmo que o prompt pareça completo, classifique as ambiguidades e faça a validação final.

## 2. Classificação de ambiguidades

Registre cada ambiguidade com ID `AMB-001`… e classifique:

| Campo | Valores | Pergunta de apoio |
| --- | --- | --- |
| Tipo | Escopo, Comportamento, Regra de negócio, Dados, Métrica, Restrição, Integração, Termo | O que exatamente está indefinido? |
| Impacto | Bloqueante, Alto, Médio, Baixo | Isso impede fechar escopo, comportamento ou critério de aceitação? |
| Origem | Prompt, Documento, Código, Inferência | De onde veio a lacuna? |
| Ação | Perguntar, Assumir, Registrar em aberto | O que faremos com ela? |

### Matriz de decisão

| Impacto | Critério | Ação |
| --- | --- | --- |
| **Bloqueante** | Impede definir escopo, comportamento observável ou critério de aceitação | `Perguntar` antes de finalizar. Sem resposta, vira premissa a confirmar na validação final. |
| **Alto** | Muda prioridade, métrica, regra de negócio ou conformidade | `Perguntar` antes de finalizar. |
| **Médio** | Afeta detalhe de experiência/operação com alternativa razoável | Perguntar se for barato; senão `Assumir` e registrar. |
| **Baixo** | Preferência editorial ou detalhe reversível | `Assumir` e registrar. |

### Exemplos

| ID | Tipo | Impacto | Situação | Ação |
| --- | --- | --- | --- | --- |
| AMB-001 | Regra de negócio | Bloqueante | Não se sabe se a cobrança é por hora ou por diária | Perguntar |
| AMB-002 | Escopo | Alto | Não se sabe se o app precisa de relatórios no MVP | Perguntar |
| AMB-003 | Comportamento | Médio | Não se sabe se o cancelamento exige justificativa | Perguntar ou assumir |
| AMB-004 | Termo | Baixo | "Ticket" e "comprovante" usados como sinônimos | Assumir e padronizar |
| AMB-005 | Integração | Alto | Não se sabe se há integração com meio de pagamento existente | Perguntar |

### Registro no PRD

Toda ambiguidade aparece no apêndice **Registro de ambiguidades**, mesmo as assumidas:

| ID | Tipo | Impacto | Origem | Ação | Pergunta feita | Resposta/Premissa | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AMB-001 | Regra de negócio | Bloqueante | Prompt | Perguntar | Cobrança por hora ou diária? | Por hora, fração de 15 min | Resolvida |
| AMB-004 | Termo | Baixo | Prompt | Assumir | — | Usar "ticket" como termo único | Premissa confirmada |

Status permitidos: `Resolvida`, `Premissa confirmada`, `Em aberto`.

## 3. Roteiro de descoberta

Pergunte apenas o que faltar. Agrupe por tema e faça no máximo 4 perguntas por chamada.

### Rodada A — Problema e objetivo

1. Qual problema de negócio o produto resolve?
2. Por que resolver agora?
3. Qual resultado mensurável se espera?
4. O que acontece se nada for feito?

Opções úteis: reduzir custo operacional, aumentar receita, reduzir tempo de atendimento, cumprir exigência regulatória, melhorar satisfação.

### Rodada B — Público e jornada

1. Quem são os usuários primários e secundários?
2. Qual é a jornada principal, do início ao fim?
3. Quem opera, quem aprova e quem é impactado sem usar o sistema?
4. Quais são as maiores dores atuais?

### Rodada C — Escopo e prioridades

1. Quais capacidades são indispensáveis no MVP?
2. O que está explicitamente fora do escopo?
3. Qual é a prioridade relativa entre as capacidades?
4. Existe prazo, orçamento ou dependência externa que condicione o escopo?

Opções úteis: fluxo principal apenas, fluxo + relatórios, fluxo + integrações, escopo completo.

### Rodada D — Sucesso e restrições

1. Como o sucesso será medido? Qual baseline e meta?
2. Quais restrições técnicas, regulatórias ou contratuais existem?
3. Há dados pessoais, pagamentos ou requisitos de auditoria?
4. Quais integrações são obrigatórias?

### Temas condicionais

Pergunte somente quando o domínio indicar:

- **LGPD/privacidade:** dados pessoais, base legal, retenção, consentimento, direitos do titular.
- **Pagamentos:** meios, estorno, conciliação, emissão fiscal.
- **Multi-tenant/permissões:** papéis, isolamento de dados, aprovações.
- **Offline/mobilidade:** operação sem conexão, sincronização, dispositivos.
- **Migração:** dados legados, convivência com sistema atual, corte.
- **Acessibilidade:** meta WCAG, dispositivos de entrada, idioma.
- **Operação e suporte:** SLA, canal de suporte, treinamento, rollback.

## 4. Como usar a ferramenta `question`

Regras:

- Um `header` curto (até 30 caracteres) e uma pergunta objetiva.
- De 2 a 5 opções concretas; a recomendada primeiro com `(Recomendado)`.
- Cada opção tem uma `description` com a consequência da escolha.
- Não crie opção "outro", "digitar" ou "pular": o ambiente adiciona resposta livre automaticamente.
- Não repita perguntas já respondidas.
- Se a resposta abrir outra ambiguidade material, classifique e pergunte de novo.

Exemplo de chamada:

```json
{
  "questions": [
    {
      "header": "Escopo do MVP",
      "question": "Quais capacidades devem entrar no MVP?",
      "options": [
        {
          "label": "Fluxo principal (Recomendado)",
          "description": "Apenas o caminho essencial: entrada, cálculo e fechamento."
        },
        {
          "label": "Fluxo + relatórios",
          "description": "Inclui relatórios operacionais básicos no lançamento."
        },
        {
          "label": "Fluxo + integrações",
          "description": "Inclui pagamento e integração com sistemas existentes."
        }
      ]
    },
    {
      "header": "Métrica de sucesso",
      "question": "Qual métrica melhor representa o sucesso no primeiro trimestre?",
      "options": [
        {
          "label": "Tempo de atendimento",
          "description": "Reduzir o tempo médio de entrada e saída de veículos."
        },
        {
          "label": "Receita por vaga",
          "description": "Aumentar a receita média por vaga ocupada."
        },
        {
          "label": "Satisfação do cliente",
          "description": "Elevar a nota de satisfação após o atendimento."
        }
      ]
    }
  ]
}
```

## 5. Validação final

Antes de salvar, apresente um resumo de 5 a 10 linhas e faça uma última rodada com `question`:

1. **Escopo e prioridades** — MVP, não-objetivos e prioridades `Must`/`Should` estão corretos?
2. **Premissas e pendências** — as premissas podem ser confirmadas? Alguma pendência bloqueia a aprovação?

Opções:

- `Aprovar e salvar` (Recomendado)
- `Ajustar escopo`
- `Revisar premissas`
- `Responder pendências`

Regras:

- Se houver ambiguidade **Bloqueante** sem resposta ou sem premissa confirmada, não ofereça `Aprovar e salvar`.
- Se o usuário pedir ajustes que mudem escopo ou premissas, atualize o PRD e repita a validação.
- Só grave o arquivo depois de uma resposta que autorize a finalização.

## 6. Exemplo de ciclo completo

1. Prompt: "Quero um sistema de estacionamento".
2. Ambiguidades: cobrança (Bloqueante), público (Alto), relatórios no MVP (Alto), nomenclatura (Baixo).
3. Rodada A/B: perguntar público e jornada.
4. Rodada C: perguntar escopo e cobrança.
5. Baixo: assumir "ticket" e registrar.
6. Redigir o PRD e preencher o Registro de ambiguidades.
7. Validação final: apresentar resumo e confirmar premissas e pendências.
8. Salvar somente após `Aprovar e salvar`.
