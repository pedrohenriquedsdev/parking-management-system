# Checklist de qualidade do TDD

Aplique antes de salvar. Corrija o documento, não apenas registre o problema.

> **TDD = Technical Design Document.** O checklist avalia decisão técnica, coerência de design e prontidão para implementação — não a implementação em si.

## Completude

- [ ] O resumo executivo técnico explica problema, solução, componentes afetados e recorte.
- [ ] Objetivos técnicos têm ID e forma de verificação.
- [ ] O recorte está explícito e o que está fora também.
- [ ] Restrições, premissas e dependências estão registradas com impacto.
- [ ] Há visão de contexto, containers e componentes (C4) ou justificativa para não haver.
- [ ] Todos os componentes têm responsabilidade, tecnologia, interfaces e dono.
- [ ] O modelo de dados cobre entidades, relacionamentos, tipos, restrições, índices, migração e retenção.
- [ ] Os contratos de API cobrem sucesso, erro, permissão, paginação e idempotência.
- [ ] Fluxos principais, alternativos e de erro estão descritos.
- [ ] Aspectos transversais (segurança, privacidade, observabilidade, desempenho, resiliência, concorrência) foram tratados.
- [ ] A estratégia de testes cobre níveis, componentes e cenários não funcionais.
- [ ] Implantação, configuração, backup e rollback estão descritos.
- [ ] Riscos e trade-offs têm probabilidade, impacto, mitigação e dono.
- [ ] Seções não aplicáveis estão marcadas como tal, com justificativa.

## Entrevista e ambiguidades

- [ ] A entrevista de descoberta foi realizada (ou a contingência sem a ferramenta foi registrada).
- [ ] Cada ambiguidade tem ID, tipo, impacto, origem e ação.
- [ ] Toda ambiguidade **Bloqueante** ou **Alta** foi perguntada com o sistema de perguntas do ambiente ou confirmada como decisão provisória na validação final.
- [ ] Nenhuma ambiguidade **Bloqueante** permanece `Em aberto`.
- [ ] O Registro de ambiguidades está coerente com as premissas e perguntas em aberto.
- [ ] A validação final foi feita com a ferramenta `question` e o usuário autorizou a finalização.

## Decisões técnicas

- [ ] Toda decisão material tem um registro `DEC-xxx`.
- [ ] Cada `DEC` tem contexto, decisão, alternativas, justificativa, consequências, dono e status.
- [ ] As alternativas descartadas explicam o motivo do descarte.
- [ ] O índice de decisões está atualizado e coerente com o detalhamento.
- [ ] Nenhuma seção contradiz uma decisão do registro.
- [ ] Decisões `Proposta` estão claramente marcadas e confirmadas na validação final ou registradas como pendência.
- [ ] Decisões substituídas mantêm o histórico e apontam para a substituta.
- [ ] Escolhas locais e reversíveis não foram infladas em ADR desnecessário.

## Arquitetura e componentes

- [ ] O estilo arquitetural está justificado contra pelo menos uma alternativa.
- [ ] As fronteiras entre componentes são claras.
- [ ] Dependências permitidas e proibidas estão explícitas.
- [ ] Não há ciclo de dependência entre componentes.
- [ ] O design evita acoplamento desnecessário a fornecedor ou framework.
- [ ] Diagramas são versionáveis (Mermaid) ou descritos em texto.

## Diagramas

- [ ] Relacionamentos de dados têm `erDiagram` ou justificativa.
- [ ] Fluxos principais e de erro têm `sequenceDiagram` ou `flowchart` quando agregam clareza.
- [ ] Máquinas de estado têm `stateDiagram-v2` quando existe ciclo de vida.
- [ ] Arquitetura e implantação têm diagrama de contexto/containers quando há mais de um serviço.
- [ ] Cada diagrama tem título, rótulos claros e um único assunto.
- [ ] Os nomes e IDs do diagrama são iguais aos do texto (`C-001`, `API-001`, `DEC-001`).
- [ ] O diagrama não contradiz o texto, o modelo de dados ou as decisões.
- [ ] Não há dados sensíveis, placas reais, segredos ou credenciais nos diagramas.
- [ ] Os blocos Mermaid renderizam sem erro.
- [ ] O catálogo de diagramas está preenchido e coerente com o documento.

## Dados e contratos

- [ ] Entidades e relacionamentos cobrem o domínio do recorte.
- [ ] Campos sensíveis têm tratamento de segurança e retenção.
- [ ] Restrições de integridade e índices estão justificados.
- [ ] A estratégia de migração permite convivência e reversão.
- [ ] Contratos de API são consistentes entre endpoints.
- [ ] Erros têm código, significado e ação esperada.
- [ ] Autenticação e autorização aparecem em cada operação protegida.
- [ ] Operações repetíveis têm idempotência definida.
- [ ] Limites, timeouts e paginação estão definidos.

## Segurança e privacidade

- [ ] Autenticação, sessão e autorização estão definidas.
- [ ] Segredos não ficam no repositório e têm estratégia de rotação.
- [ ] Dados pessoais foram identificados, com base legal e minimização.
- [ ] Retenção e expurgo estão definidos e verificáveis.
- [ ] Acesso a dados sensíveis é auditável.
- [ ] Ameaças relevantes foram consideradas (OWASP, abuso de fluxo, elevação de privilégio).
- [ ] Dados sensíveis não são registrados em log.

## Operação e confiabilidade

- [ ] Ambientes, pipeline e critérios de promoção estão definidos.
- [ ] Observabilidade cobre logs, métricas, traces e alertas.
- [ ] Há meta de disponibilidade e tratamento de indisponibilidade.
- [ ] Backup, RPO, RTO e teste de restauração estão definidos.
- [ ] Rollout, migração e rollback têm ordem e critério de acionamento.
- [ ] Capacidade e limites de infraestrutura estão dimensionados ou marcados como pendência.
- [ ] Existe plano de contingência para falha de dependência crítica.

## Testes e qualidade

- [ ] A estratégia cobre unitário, integração, contrato, E2E e não funcional.
- [ ] Cada componente crítico tem cenário de teste associado.
- [ ] Cenários de erro, concorrência e idempotência estão cobertos.
- [ ] Metas de cobertura têm justificativa ou estão marcadas como "a definir".
- [ ] Testes de carga, segurança e acessibilidade estão previstos quando aplicáveis.
- [ ] Dados de teste e isolamento estão definidos.

## Rastreabilidade e consistência

- [ ] Todo requisito do PRD no recorte aparece na matriz de rastreabilidade.
- [ ] Cada requisito aponta para componente, contrato/dado, teste e decisão.
- [ ] Cada decisão `DEC` está ligada a um requisito ou objetivo técnico.
- [ ] IDs são únicos e seguem o padrão (`OBJ`, `RT`, `PT`, `C`, `API`, `E`, `INT`, `DEC`, `R`, `Q`, `AMB`).
- [ ] Toda referência cruzada aponta para um ID existente.
- [ ] O mesmo conceito tem o mesmo nome em todo o documento.
- [ ] O histórico de versões reflete o estado atual.

## Qualidade editorial

- [ ] O documento está no idioma correto e com ortografia consistente.
- [ ] Não há placeholders vazios (`<...>`, `TBD`, `TODO`) sem tratamento.
- [ ] Termos vagos ("rápido", "escalável", "seguro") foram substituídos por número, condição ou mecanismo.
- [ ] Trechos de código, payloads e DDL estão marcados como ilustrativos.
- [ ] O documento pode ser lido por engenharia, produto e operação sem contexto adicional.
- [ ] Nenhum fato, tecnologia, número ou decisão foi inventado sem marcação de premissa.

## Sinais de alerta

Corrija antes de salvar se encontrar:

- arquitetura escolhida sem alternativa registrada;
- decisão tomada apenas no texto corrido, fora do registro `DEC`;
- contrato de API sem tratamento de erro ou permissão;
- modelo de dados sem migração ou retenção;
- fluxo sem cenário de falha ou concorrência;
- meta de teste ou cobertura inventada;
- requisito do PRD sem componente ou teste correspondente;
- decisão `Proposta` apresentada como `Aceita`;
- tecnologia citada sem evidência ou decisão;
- diagrama que contradiz o texto, o modelo de dados ou as decisões;
- diagrama sobrecarregado, sem título ou com dados sensíveis;
- relacionamento, fluxo ou estado complexo explicado só em texto corrido.

## Definição de pronto

O TDD está pronto quando:

1. todas as seções do template foram preenchidas ou justificadamente marcadas como não aplicáveis;
2. toda decisão material está no registro ADR, com alternativas e consequências;
3. todo requisito do PRD no recorte tem rastreabilidade completa;
4. dados, contratos, fluxos e erros estão coerentes entre si;
5. relacionamentos, fluxos e estados têm diagrama Mermaid ou justificativa registrada;
6. segurança, privacidade, observabilidade, backup e rollback foram tratados;
7. a estratégia de testes cobre os riscos principais;
8. as perguntas em aberto estão registradas e priorizadas;
9. toda ambiguidade foi classificada e nenhuma **Bloqueante** ficou sem resposta ou decisão confirmada;
10. a validação final foi respondida com autorização para salvar;
11. o checklist acima foi aplicado sem pendências bloqueantes.
