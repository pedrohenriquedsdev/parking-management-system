# Checklist de qualidade do PRD

Aplique antes de salvar. Corrija o documento, não apenas registre o problema.

## Completude

- [ ] O resumo executivo explica problema, proposta, público e benefício em linguagem acessível.
- [ ] O problema está descrito com evidência ou com a ausência de evidência declarada.
- [ ] Objetivos de negócio e de produto estão separados e mensuráveis.
- [ ] Não-objetivos e itens fora do escopo estão explícitos.
- [ ] Personas e jornadas cobrem quem usa e quem é impactado.
- [ ] Todo requisito funcional tem ID, descrição, prioridade e critério de aceitação.
- [ ] Regras de negócio estão identificadas e referenciadas pelos requisitos.
- [ ] Requisitos não funcionais relevantes têm meta mensurável e forma de verificação.
- [ ] Fluxos alternativos, erros e estados vazios foram considerados.
- [ ] Riscos têm probabilidade, impacto, mitigação e responsável.
- [ ] Premissas estão registradas com o impacto caso sejam falsas.
- [ ] Perguntas em aberto têm dono ou justificativa para não ter.
- [ ] Seções não aplicáveis estão marcadas como tal, com justificativa.

## Consistência

- [ ] IDs são únicos e seguem o padrão (`OE`, `M`, `P`, `J`, `RF`, `RN`, `RNF`, `PR`, `R`, `Q`).
- [ ] Toda referência cruzada aponta para um ID existente.
- [ ] A mesma capacidade tem o mesmo nome em todo o documento.
- [ ] Prioridades MoSCoW são coerentes com o escopo do MVP.
- [ ] Não há contradição entre escopo, requisitos, não-objetivos e cronograma.
- [ ] O histórico de versões reflete o estado atual.

## Testabilidade

- [ ] Todo requisito `Must` e `Should` tem critério de aceitação verificável.
- [ ] Critérios de aceitação usam `Dado/Quando/Então` e descrevem resultado observável.
- [ ] Requisitos não funcionais têm números, limites ou condições claras.
- [ ] Métricas têm baseline, meta, prazo e fonte — ou "a definir" explícito.
- [ ] Não há requisito vago ("rápido", "intuitivo", "amigável") sem critério objetivo.

## Rastreabilidade

- [ ] Cada objetivo tem ao menos um requisito que o endereça.
- [ ] Cada requisito está ligado a um objetivo ou jornada.
- [ ] Cada métrica está ligada a um objetivo.
- [ ] A matriz de rastreabilidade está preenchida e consistente.

## Riscos e conformidade

- [ ] Dados pessoais foram identificados e há tratamento de LGPD (base legal, retenção, consentimento, direitos do titular) ou justificativa de não aplicabilidade.
- [ ] Requisitos de segurança e controle de acesso estão explícitos quando há dados sensíveis.
- [ ] Acessibilidade foi considerada para interfaces (meta verificável, ex.: WCAG 2.2 AA).
- [ ] Dependências externas e seus riscos estão listados.
- [ ] O plano de lançamento cobre rollback ou desativação, quando aplicável.

## Qualidade editorial

- [ ] O documento está no idioma correto e com ortografia consistente.
- [ ] Não há placeholders vazios (`<...>`, `TBD`, `TODO`) sem tratamento.
- [ ] O texto é product-level: não prescreve arquitetura, bibliotecas ou esquema físico sem necessidade.
- [ ] O documento pode ser lido por alguém de produto, engenharia e negócio sem contexto adicional.
- [ ] Nenhum fato, número, pesquisa ou decisão foi inventado.

## Definição de pronto

O PRD está pronto quando:

1. todas as seções do template foram preenchidas ou justificadamente marcadas como não aplicáveis;
2. todos os requisitos `Must` têm critério de aceitação;
3. existe ao menos uma métrica de sucesso por objetivo;
4. as perguntas em aberto estão registradas e priorizadas;
5. o checklist acima foi aplicado sem pendências bloqueantes.
