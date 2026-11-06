---
description: Gera um Documento Técnico de Design (TDD) detalhado a partir de um PRD ou prompt técnico
---

Gere um Documento Técnico de Design (TDD — Technical Design Document) detalhado a partir da solicitação abaixo.

**Prompt inicial:** $ARGUMENTS

Carregue a skill `generate-tdd` com a ferramenta `skill` e siga o fluxo dela à risca. Não comece a redigir o documento antes de ler a skill e o template em `references/tdd-template.md`.

A entrevista é obrigatória: classifique as ambiguidades técnicas e use o sistema de perguntas do ambiente (`question`) antes de finalizar o artefato. Centralize toda decisão técnica relevante no registro de decisões (ADR) do documento. Quando ajudar a explicar relacionamentos, fluxos, estados ou implantação, sugira e inclua diagramas Mermaid versionáveis. Não salve o TDD sem a validação final do usuário.
