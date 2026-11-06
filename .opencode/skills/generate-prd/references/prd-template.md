# Template de PRD

> **Como usar:** este é o esqueleto obrigatório do documento. Substitua todo texto entre `<...>` pelo conteúdo real. Remova as citações de orientação (`>`) antes de salvar. Não deixe placeholders vazios: use `Não aplicável` com justificativa ou registre uma pergunta em aberto.

---

# <Nome do Produto ou Iniciativa> — PRD

> **Versão:** <0.1> · **Status:** <Rascunho | Em revisão | Aprovado> · **Data:** <AAAA-MM-DD>
> **Autor:** <nome> · **Revisores/Stakeholders:** <nomes e papéis>
> **Histórico de versões:** <0.1 — criação; 0.2 — ...>

## 1. Resumo executivo

<3 a 5 parágrafos curtos: qual problema existe, qual é a proposta, para quem, qual o benefício esperado e o que está no MVP. Deve ser compreensível por alguém de fora do time.>

## 2. Contexto e problema

### 2.1 Situação atual

<Como o problema é resolvido hoje, incluindo processos manuais, ferramentas e limitações.>

### 2.2 Problema

<Descrição objetiva do problema, quem é afetado e com que frequência.>

### 2.3 Evidências

<Pesquisas, entrevistas, dados, tickets ou observações que sustentam o problema. Cite a fonte. Se não houver evidência, declare explicitamente.>

### 2.4 Custo de não fazer

<Impacto de manter o status quo: financeiro, operacional, risco, experiência do usuário.>

## 3. Objetivos e métricas de sucesso

### 3.1 Objetivo geral

<Resultado de negócio/produto que a iniciativa deve produzir.>

### 3.2 Objetivos específicos

| ID | Objetivo | Como será verificado |
| --- | --- | --- |
| OE-001 | <objetivo mensurável> | <métrica ou evidência> |

### 3.3 Métricas de sucesso

| ID | Métrica | Baseline | Meta | Prazo | Fonte do dado |
| --- | --- | --- | --- | --- | --- |
| M-001 | <métrica> | <valor ou "a definir"> | <valor> | <data> | <sistema/processo> |

### 3.4 Não-objetivos

<O que esta iniciativa deliberadamente não pretende resolver.>

## 4. Público e personas

### 4.1 Personas

| ID | Persona | Perfil | Objetivos | Dores |
| --- | --- | --- | --- | --- |
| P-001 | <nome/papel> | <contexto, frequência de uso> | <o que quer alcançar> | <o que a impede hoje> |

### 4.2 Jornadas principais

| ID | Jornada | Persona | Passos | Resultado esperado |
| --- | --- | --- | --- | --- |
| J-001 | <nome> | P-001 | <1 → 2 → 3> | <resultado> |

### 4.3 Necessidades e dores priorizadas

<Necessidades mais relevantes, com origem na evidência da seção 2.>

## 5. Escopo

### 5.1 Dentro do escopo (MVP)

- <capacidade incluída no MVP>

### 5.2 Fora do escopo

- <capacidade explicitamente excluída e motivo>

### 5.3 Fases futuras

| Fase | Conteúdo | Gatilho para iniciar |
| --- | --- | --- |
| <2> | <capacidades> | <condição> |

## 6. Requisitos funcionais

### 6.1 Visão geral

| ID | Requisito | Prioridade | Objetivo relacionado |
| --- | --- | --- | --- |
| RF-001 | <título curto> | Must | OE-001 |

### 6.2 Detalhamento

#### RF-001 — <título>

- **Descrição:** <comportamento esperado do ponto de vista do usuário.>
- **Prioridade:** <Must | Should | Could | Won't>
- **Persona/jornada:** <P-001 / J-001>
- **Regras de negócio:** <RN-001>
- **Dependências:** <RF-002, integração X, ou "nenhuma">
- **Critérios de aceitação:**

```gherkin
Dado <contexto>
Quando <ação>
Então <resultado observável>
```

<Repita o bloco para cada requisito funcional.>

## 7. Regras de negócio

| ID | Regra | Condição | Exceções | Requisitos impactados |
| --- | --- | --- | --- | --- |
| RN-001 | <regra objetiva> | <quando se aplica> | <casos especiais> | RF-001 |

## 8. Requisitos não funcionais

| ID | Categoria | Requisito | Meta mensurável | Verificação |
| --- | --- | --- | --- | --- |
| RNF-001 | Desempenho | <comportamento> | <ex.: p95 < 300 ms> | <teste, monitoramento> |
| RNF-002 | Disponibilidade | <comportamento> | <ex.: 99,9% mensal> | <SLA, monitoramento> |
| RNF-003 | Segurança | <comportamento> | <controle verificável> | <teste, auditoria> |
| RNF-004 | Privacidade/LGPD | <comportamento> | <base legal, retenção> | <revisão jurídica> |
| RNF-005 | Acessibilidade | <comportamento> | <ex.: WCAG 2.2 AA> | <auditoria> |
| RNF-006 | Usabilidade | <comportamento> | <ex.: conclusão da tarefa > 90%> | <teste com usuários> |
| RNF-007 | Escalabilidade | <comportamento> | <ex.: 10 mil usuários simultâneos> | <teste de carga> |
| RNF-008 | Observabilidade | <comportamento> | <logs, métricas, alertas> | <revisão operacional> |
| RNF-009 | Compatibilidade | <comportamento> | <navegadores, dispositivos> | <matriz de testes> |

<Remova linhas não aplicáveis ou marque "Não aplicável" com justificativa.>

## 9. Experiência e fluxos

### 9.1 Fluxo principal

<Passo a passo do caminho feliz, por persona/jornada.>

### 9.2 Fluxos alternativos e de erro

<O que acontece em falha de validação, indisponibilidade, falta de permissão, timeout, duplicidade etc.>

### 9.3 Estados da interface

<Estados vazio, carregando, sucesso, erro e sem permissão, quando aplicável.>

### 9.4 Conteúdo e tom

<Mensagens, nomenclatura, tom de voz e requisitos de localização, quando aplicável.>

## 10. Modelo de dados conceitual

<Entidades principais, atributos relevantes e relacionamentos em nível de produto. Não prescrever esquema físico de banco.>

| Entidade | Descrição | Atributos-chave | Relacionamentos |
| --- | --- | --- | --- |
| <Entidade> | <o que representa> | <campos relevantes> | <1:N com ...> |

## 11. Integrações e dependências

| Integração/Dependência | Tipo | Dono | O que fornece/consome | Risco |
| --- | --- | --- | --- | --- |
| <sistema> | <interna/externa> | <time> | <dados/serviço> | <alto/médio/baixo> |

## 12. Premissas, restrições e dependências

### 12.1 Premissas

| ID | Premissa | Ambiguidade de origem | Impacto se falsa | Confirmada? |
| --- | --- | --- | --- | --- |
| PR-001 | <premissa adotada> | <AMB-001> | <consequência> | <sim/não — data> |

### 12.2 Restrições

- <prazo, orçamento, tecnologia imposta, contrato, regulação>

### 12.3 Dependências

- <time, aprovação, integração ou entrega externa necessária>

## 13. Riscos e mitigações

| ID | Risco | Probabilidade | Impacto | Mitigação | Responsável |
| --- | --- | --- | --- | --- | --- |
| R-001 | <risco> | <alta/média/baixa> | <alto/médio/baixo> | <ação> | <nome ou "a definir"> |

## 14. Cronograma e marcos

| Marco | Escopo | Data alvo | Critério de conclusão |
| --- | --- | --- | --- |
| <M1> | <MVP> | <data> | <critério> |

<Se não houver cronograma definido, escreva "Não definido" e registre a pergunta em aberto.>

## 15. Plano de lançamento

<Estratégia de disponibilização, público piloto, comunicação, treinamento, suporte e rollback. Marque "Não aplicável" quando não fizer sentido.>

## 16. Perguntas em aberto

| ID | Pergunta | Por que importa | Dono | Prazo |
| --- | --- | --- | --- | --- |
| Q-001 | <pergunta> | <impacto> | <nome ou "a definir"> | <data ou "a definir"> |

## 17. Decisões registradas

| Data | Decisão | Justificativa | Fonte |
| --- | --- | --- | --- |
| <AAAA-MM-DD> | <decisão> | <motivo> | <reunião, prompt, documento> |

## 18. Aprovações

| Papel | Nome | Status | Data |
| --- | --- | --- | --- |
| Product Owner | <nome> | <pendente/aprovado> | <data> |
| Tech Lead | <nome> | <pendente/aprovado> | <data> |
| Stakeholder de negócio | <nome> | <pendente/aprovado> | <data> |

## Apêndices

### A. Glossário

| Termo | Definição |
| --- | --- |
| <termo> | <definição> |

### B. Referências

- <documento, pesquisa, link interno>

### C. Matriz de rastreabilidade

| Objetivo | Requisitos | Métrica |
| --- | --- | --- |
| OE-001 | RF-001, RF-002 | M-001 |

### D. Registro de ambiguidades

Toda ambiguidade classificada durante a entrevista, inclusive as assumidas, deve aparecer aqui.

| ID | Tipo | Impacto | Origem | Ação | Pergunta feita | Resposta/Premissa | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AMB-001 | <Escopo/Comportamento/Regra de negócio/Dados/Métrica/Restrição/Integração/Termo> | <Bloqueante/Alto/Médio/Baixo> | <Prompt/Documento/Código/Inferência> | <Perguntar/Assumir/Registrar em aberto> | <pergunta> | <resposta ou premissa> | <Resolvida/Premissa confirmada/Em aberto> |

> Nenhuma ambiguidade com impacto **Bloqueante** pode permanecer `Em aberto` na versão final.
