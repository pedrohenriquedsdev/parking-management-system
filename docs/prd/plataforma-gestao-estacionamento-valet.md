# Plataforma de Gestão de Estacionamento e Valet — PRD

> **Versão:** 0.1 · **Status:** Em revisão · **Data:** 2026-11-05
> **Autor:** Agente OpenCode, a partir da entrevista de Tiago Santini com Alexandre Rech
> **Revisores/Stakeholders:** Alexandre Rech (negócio), Tiago Santini (produto), Tech Lead e responsável de operações (a definir)
> **Histórico de versões:** 0.1 (2026-11-05) — criação a partir de `docs/entrevista.md` e validação do escopo com o usuário.

## 1. Resumo executivo

O valet opera hoje de forma manual: o cliente entrega o carro, recebe um ticket de papel e a equipe procura uma vaga. Na saída, o cálculo das diárias é feito à mão e o controle financeiro termina em planilhas. Esse modelo gera perda de tickets, demora para localizar veículos, ocupação de vagas desconhecida, divergências de cobrança e dificuldade de comprovar danos.

A proposta é uma plataforma web de gestão de estacionamento e valet, operada pela equipe, que centraliza o check-in digital, a ocupação das vagas, a fila de retirada, o cálculo automático de diárias, o registro de pagamentos, os relatórios e a auditoria das ações. O cliente continua sendo atendido no balcão no MVP; não haverá aplicativo ou autoatendimento do cliente nesta fase.

O MVP contempla a operação completa de uma única unidade: check-in com placa e código único, fotos de condição do veículo, vaga individual, painel de ocupação, retirada com status, cobrança em blocos de 24 horas, pagamentos registrados, relatórios financeiros e operacionais, três perfis de acesso e trilha de auditoria. Pagamentos são apenas registrados, sem integração com maquininha, Pix ou gateway.

O benefício esperado é reduzir erros e tempo de espera, dar transparência à cobrança, evitar perdas financeiras, criar evidências para disputas de danos e permitir que a operação cresça sem depender de controles paralelos. As metas quantitativas de tempo de retirada e divergência financeira ainda precisam de baseline e serão definidas com o negócio.

## 2. Contexto e problema

### 2.1 Situação atual

O fluxo atual é inteiramente manual:

1. O cliente chega e um funcionário recebe o veículo.
2. A equipe anota placa, modelo e horário de entrada em um controle interno.
3. Um ticket numerado de papel é entregue ao cliente.
4. Um funcionário procura uma vaga e estaciona o veículo.
5. Na saída, o cliente apresenta o ticket; a equipe localiza o carro, calcula o valor pelas diárias e cobra.
6. O caixa é fechado manualmente e os números são consolidados em planilhas.

Não há visão atualizada de vagas livres, fila formal de retirada, histórico padronizado de danos nem controle de acesso por função.

### 2.2 Problema

A operação depende de memória, comunicação verbal e registros dispersos. Isso produz:

- demora para localizar veículos e atender pedidos de retirada;
- ocupação de vagas desconhecida e conflitos entre funcionários;
- perda de tickets e dificuldade de confirmar a propriedade do veículo;
- cálculo de diárias inconsistente entre operadores;
- divergências entre valor cobrado, valor pago e valor efetivamente recebido;
- dificuldade de comprovar danos ocorridos durante a permanência;
- acesso irrestrito a informações financeiras e operacionais;
- ausência de dados confiáveis para decisões de gestão.

### 2.3 Evidências

A principal evidência é a entrevista de levantamento de requisitos registrada em `docs/entrevista.md`, conduzida por Tiago Santini com Alexandre Rech. A entrevista relata perda de tickets, demora na entrega, divergências de cobrança, reclamações de danos e controle financeiro manual.

O próprio documento declara que se trata de uma **entrevista fictícia para fins de levantamento de requisitos**. Portanto, as necessidades aqui registradas são um ponto de partida e devem ser validadas com a operação real antes do desenvolvimento. Não há, nesta fase, dados quantitativos de volume, tempo de atendimento, receita ou reclamações.

### 2.4 Custo de não fazer

Manter o processo manual significa continuar exposto a:

- perda de receita por cobrança incorreta ou não registrada;
- custos operacionais maiores com retrabalho e tempo de procura;
- insatisfação de clientes por espera e falta de transparência;
- prejuízo em disputas de danos por falta de evidência;
- risco de acesso indevido a dados financeiros;
- limitação de crescimento, porque cada nova unidade ou volume exige mais controle manual.

## 3. Objetivos e métricas de sucesso

### 3.1 Objetivo geral

Centralizar a operação de valet em uma plataforma digital única, reduzindo a dependência de papel e planilhas e fornecendo dados confiáveis para a gestão do negócio.

### 3.2 Objetivos específicos

| ID | Objetivo | Como será verificado |
| --- | --- | --- |
| OE-001 | Digitalizar o check-in e eliminar a dependência do ticket de papel | Percentual de permanências com registro digital completo (M-002) |
| OE-002 | Dar visibilidade atualizada da ocupação e da localização dos veículos | Acurácia da ocupação em auditorias (M-004) |
| OE-003 | Padronizar a cobrança e reduzir divergências financeiras | Taxa de divergência de cobrança (M-003) |
| OE-004 | Organizar a fila de retirada e reduzir o tempo de entrega | Tempo médio de retirada (M-001) |
| OE-005 | Gerar relatórios confiáveis para decisão diária, semanal e mensal | Fechamentos gerados pelo sistema (M-005) |
| OE-006 | Restringir acesso por função e manter histórico auditável | Cobertura de auditoria das ações críticas (M-006) |

### 3.3 Métricas de sucesso

| ID | Métrica | Baseline | Meta | Prazo | Fonte do dado |
| --- | --- | --- | --- | --- | --- |
| M-001 | Tempo médio de retirada (solicitação → entrega) | Não medido hoje | A definir com o negócio | A definir | Módulo de solicitações de retirada |
| M-002 | Permanências com registro digital completo (placa, código, vaga e condição) | 0% (processo manual) | 100% até 30 dias após o go-live | 30 dias após go-live | Sistema |
| M-003 | Taxa de divergência de cobrança (permanências com valor cobrado diferente do pago) | Não medido hoje | A definir com o negócio | A definir | Relatório de divergências |
| M-004 | Acurácia da ocupação (contagem física versus sistema) | Não medido hoje | ≥ 98% em auditorias mensais | A partir do 2º mês | Auditoria de ocupação |
| M-005 | Fechamentos diários gerados pelo sistema, sem planilha | 0% | 100% em 60 dias | 60 dias após go-live | Relatórios financeiros |
| M-006 | Cobertura de auditoria das ações críticas | 0% | 100% das ações críticas registradas | 30 dias após go-live | Trilha de auditoria |

> As metas de M-002, M-004, M-005 e M-006 são requisitos de processo. M-001 e M-003 dependem de medição e devem ser definidas com o negócio (Q-001). Nenhum baseline foi estimado sem base.

### 3.4 Não-objetivos

- Substituir o atendimento humano no balcão ou criar autoatendimento do cliente.
- Processar pagamentos ou integrar maquininha, Pix ou gateway.
- Gerenciar múltiplas unidades ou franquias nesta fase.
- Reconhecer placas automaticamente por câmera (LPR/ANPR).
- Emitir nota fiscal ou integrar com sistema fiscal.
- Operar sem conexão com a internet.
- Oferecer aplicativo nativo para funcionários ou clientes.
- Criar programa de fidelidade, planos mensais ou descontos promocionais.
- Fazer análise preditiva, BI avançado ou inteligência artificial.
- Integrar-se a sistemas externos de terceiros.

## 4. Público e personas

### 4.1 Personas

| ID | Persona | Perfil | Objetivos | Dores |
| --- | --- | --- | --- | --- |
| P-001 | Administrador (gestor) | Acompanha operação, finanças, tarifas e usuários; usa o sistema diariamente para gestão | Controlar receita, reduzir erros, decidir com dados confiáveis, auditar ocorrências | Planilhas, divergências de caixa, falta de visão da operação, dificuldade de comprovar danos |
| P-002 | Manobrista | Recebe, estaciona e busca veículos; alta frequência de uso operacional | Fazer check-in rápido, encontrar vagas e veículos sem perder tempo, registrar condição | Procura manual por carros, confusão de vagas, pedidos verbais esquecidos |
| P-003 | Caixa/Atendente | Atende o cliente, calcula e cobra, registra pagamentos e resolve ticket perdido | Calcular e explicar o valor com segurança, registrar pagamentos corretamente, evitar divergências | Cálculo manual, regras inconsistentes, retrabalho com tickets perdidos |
| P-004 | Cliente do valet | Não opera o sistema; é atendido no balcão | Entregar e retirar o carro com rapidez, entender o preço, ter o veículo preservado | Espera, perda de ticket, cobrança duvidosa, danos não comprovados |

### 4.2 Jornadas principais

| ID | Jornada | Persona | Passos | Resultado esperado |
| --- | --- | --- | --- | --- |
| J-001 | Check-in do veículo | P-002, P-003 | Receber o carro → registrar placa e modelo → registrar condição e fotos → associar vaga → gerar código → entregar o código ao cliente | Permanência aberta, vaga ocupada e veículo localizável |
| J-002 | Retirada do veículo | P-003, P-002, P-004 | Cliente informa código ou placa → localizar veículo → criar solicitação → manobrista busca → confirmar entrega → liberar vaga | Veículo entregue com tempo registrado e vaga disponível |
| J-003 | Cobrança e pagamento | P-003 | Consultar permanência → conferir tempo, diárias e total → registrar pagamento → emitir comprovante → atualizar status financeiro | Cobrança padronizada e pagamento rastreável |
| J-004 | Gestão e auditoria | P-001 | Acompanhar painel → consultar relatórios → analisar divergências → auditar ocorrências → ajustar tarifa e usuários | Decisão baseada em dados e operação controlada |

### 4.3 Necessidades e dores priorizadas

1. **Localizar veículos rapidamente** por placa ou código, sem depender de papel.
2. **Saber a ocupação real** das vagas e onde cada veículo está.
3. **Organizar a fila de retirada** para não esquecer pedidos e reduzir a espera.
4. **Calcular a cobrança de forma uniforme**, com detalhamento compreensível ao cliente.
5. **Registrar e conciliar pagamentos**, identificando pendências e divergências.
6. **Comprovar a condição do veículo** na entrada e na saída.
7. **Restringir o acesso** conforme a função e manter histórico de quem fez cada ação.
8. **Ter relatórios confiáveis** por dia, semana e mês para planejar o negócio.

## 5. Escopo

### 5.1 Dentro do escopo (MVP)

- Autenticação de usuários e controle de acesso por perfil.
- Cadastro e configuração de vagas, com status livre, ocupada e bloqueada.
- Check-in digital com placa, modelo, cor opcional, horário automático, nome e telefone opcionais do cliente, observações e código único.
- Registro de condição do veículo com observações e até 5 fotos por evento (entrada e saída).
- Associação de uma vaga individual a cada veículo.
- Painel de ocupação em tempo real e consulta de veículo por placa ou código.
- Fila de solicitações de retirada com status: aguardando, em busca, entregue e cancelada.
- Cálculo automático de diárias em blocos de 24 horas, com tolerância de 15 minutos.
- Tarifa única configurável em BRL, com histórico de vigência.
- Registro de pagamentos em dinheiro, débito, crédito e Pix, com múltiplos pagamentos e estornos.
- Controle de saldo e status financeiro: pendente, parcial e pago.
- Comprovante não fiscal opcional.
- Dashboard operacional e relatórios financeiros e de ocupação por período.
- Trilha de auditoria das ações críticas e histórico do veículo.
- Expurgo e anonimização de dados pessoais e fotos após 12 meses.

### 5.2 Fora do escopo

- **Aplicativo ou autoatendimento do cliente:** o MVP é operado somente pela equipe. O cliente interage no balcão.
- **Processamento de pagamentos:** o sistema registra a transação, mas não processa cartão, Pix ou dinheiro. Não há integração com maquininha ou gateway.
- **Múltiplas unidades:** o MVP atende uma única unidade; expansão fica para fase futura.
- **Reconhecimento de placa por câmera:** a placa é digitada manualmente.
- **Emissão fiscal:** não há nota fiscal, NF-e ou integração contábil.
- **Operação offline:** o sistema exige conexão com a internet.
- **Aplicativo nativo:** a interface é web responsiva, acessada por navegador.
- **Planos mensais, fidelidade e descontos:** não fazem parte do MVP.
- **Integrações externas:** não há integração obrigatória com sistemas de terceiros.
- **Sessão formal de caixa:** o MVP gera relatórios diários, mas não controla abertura e fechamento de caixa com conferência de gaveta.
- **Notificações automáticas ao cliente:** SMS, e-mail e WhatsApp ficam para fase futura.

### 5.3 Fases futuras

| Fase | Conteúdo | Gatilho para iniciar |
| --- | --- | --- |
| 2 | Autoatendimento do cliente para consultar status e solicitar retirada | MVP estável e demanda validada |
| 2 | Integração com Pix e/ou maquininha para cobrança automática | Volume que justifique conciliação automática |
| 3 | Gestão de múltiplas unidades | Abertura da segunda unidade |
| 3 | Reconhecimento de placa por câmera | Redução de erros de digitação e ganho de tempo |
| 3 | Emissão fiscal e integração contábil | Exigência legal ou contábil |
| 3 | Notificações ao cliente e pesquisa de satisfação | Necessidade de comunicação proativa |
| 3 | BI avançado e análise preditiva | Volume de dados suficiente para análise |

## 6. Requisitos funcionais

### 6.1 Visão geral

| ID | Requisito | Prioridade | Objetivo relacionado |
| --- | --- | --- | --- |
| RF-001 | Autenticação de usuários | Must | OE-006 |
| RF-002 | Perfis e permissões de acesso | Must | OE-006 |
| RF-003 | Gestão de usuários | Must | OE-006 |
| RF-004 | Registrar entrada, gerar código e atribuir vaga | Must | OE-001, OE-002 |
| RF-005 | Registrar condição do veículo e fotos | Must | OE-001 |
| RF-006 | Consultar veículo e histórico | Must | OE-001, OE-002 |
| RF-007 | Corrigir ou cancelar check-in | Must | OE-001 |
| RF-008 | Painel de ocupação | Must | OE-002 |
| RF-009 | Cadastrar e configurar vagas | Must | OE-002 |
| RF-010 | Transferir veículo entre vagas | Should | OE-002 |
| RF-011 | Solicitar retirada | Must | OE-004 |
| RF-012 | Fila e status de retirada | Must | OE-004 |
| RF-013 | Confirmar entrega e liberar vaga | Must | OE-002, OE-004 |
| RF-014 | Calcular diárias automaticamente | Must | OE-003 |
| RF-015 | Configurar tarifa | Must | OE-003 |
| RF-016 | Registrar pagamentos e estornos | Must | OE-003 |
| RF-017 | Calcular saldo e status financeiro | Must | OE-003 |
| RF-018 | Emitir comprovante não fiscal | Should | OE-003 |
| RF-019 | Dashboard operacional | Must | OE-004, OE-005 |
| RF-020 | Relatório financeiro por período | Must | OE-005 |
| RF-021 | Relatório de divergências | Must | OE-003, OE-005 |
| RF-022 | Relatório de ocupação e horários de pico | Must | OE-002, OE-005 |
| RF-023 | Exportar relatórios | Should | OE-005 |
| RF-024 | Trilha de auditoria | Must | OE-006 |
| RF-025 | Expurgo e anonimização de dados | Must | OE-006 |

### 6.2 Detalhamento

#### RF-001 — Autenticação de usuários

- **Descrição:** permitir que usuários ativos acessem o sistema com e-mail e senha, mantenham sessão enquanto trabalham e encerrem a sessão.
- **Prioridade:** Must
- **Persona/jornada:** todas as personas internas
- **Regras de negócio:** RN-015
- **Dependências:** nenhuma
- **Critérios de aceitação:**

```gherkin
Dado um usuário ativo com credenciais válidas
Quando informar e-mail e senha corretos
Então o sistema libera o acesso e exibe o painel conforme o perfil do usuário

Dado um usuário com senha incorreta
Quando tentar entrar
Então o sistema nega o acesso com mensagem genérica e registra a tentativa

Dado um usuário inativo
Quando tentar entrar
Então o sistema nega o acesso

Dado um usuário autenticado e inativo por 30 minutos
Quando tentar continuar a operação
Então o sistema encerra a sessão e exige novo login
```

#### RF-002 — Perfis e permissões de acesso

- **Descrição:** aplicar o modelo de três perfis (Administrador, Manobrista e Caixa), com permissões distintas e bloqueio de acesso a funções não autorizadas.
- **Prioridade:** Must
- **Persona/jornada:** P-001, P-002, P-003
- **Regras de negócio:** RN-015
- **Dependências:** RF-001
- **Critérios de aceitação:**

```gherkin
Dado um usuário com perfil Manobrista
Quando tentar abrir relatórios financeiros, tarifas ou gestão de usuários
Então o sistema nega o acesso e registra a tentativa

Dado um usuário com perfil Caixa
Quando tentar gerenciar usuários ou alterar tarifas
Então o sistema nega o acesso

Dado um usuário com perfil Administrador
Quando acessar qualquer função
Então o sistema permite a operação conforme a matriz de permissões
```

#### RF-003 — Gestão de usuários

- **Descrição:** permitir que o Administrador crie, edite, ative, desative e redefina a senha de usuários, atribuindo um perfil.
- **Prioridade:** Must
- **Persona/jornada:** P-001
- **Regras de negócio:** RN-015
- **Dependências:** RF-001, RF-002
- **Critérios de aceitação:**

```gherkin
Dado um Administrador autenticado
Quando cadastrar um usuário com nome, e-mail, perfil e senha temporária
Então o usuário fica ativo e pode acessar o sistema

Dado um e-mail já cadastrado
Quando o Administrador tentar criar outro usuário com o mesmo e-mail
Então o sistema bloqueia o cadastro e informa a duplicidade

Dado um usuário ativo
Quando o Administrador o desativar
Então o usuário perde o acesso imediatamente

Dado um usuário com perfil alterado
Quando ele iniciar uma nova sessão
Então o sistema aplica as permissões do novo perfil
```

#### RF-004 — Registrar entrada, gerar código e atribuir vaga

- **Descrição:** registrar a entrada do veículo com placa, modelo, cor opcional, dados opcionais do cliente e observações; gerar um código único de retirada; associar uma vaga livre e marcar a vaga como ocupada.
- **Prioridade:** Must
- **Persona/jornada:** P-002, P-003 / J-001
- **Regras de negócio:** RN-001, RN-002, RN-003, RN-004, RN-019, RN-020, RN-024
- **Dependências:** RF-002, RF-009
- **Critérios de aceitação:**

```gherkin
Dado um veículo sem permanência ativa e uma vaga livre
Quando o operador informar placa e modelo, selecionar a vaga e confirmar
Então o sistema cria a permanência com código único, registra horário e usuário e marca a vaga como ocupada

Dado um veículo com permanência ativa
Quando o operador tentar registrar nova entrada para a mesma placa
Então o sistema bloqueia a operação e exibe a permanência existente

Dado uma vaga já ocupada
Quando o operador tentar associá-la a outro veículo
Então o sistema impede a associação e sugere uma vaga livre

Dado que não há vaga livre
Quando o operador tentar concluir o check-in
Então o sistema bloqueia a operação e orienta a liberar ou desbloquear uma vaga

Dado um check-in concluído
Quando o operador consultar a placa ou o código gerado
Então o sistema encontra a permanência com vaga, horário e status
```

#### RF-005 — Registrar condição do veículo e fotos

- **Descrição:** registrar observações e até 5 fotos da condição do veículo no check-in e na saída, com data, hora e usuário responsável, formando evidência para contestação de danos.
- **Prioridade:** Must
- **Persona/jornada:** P-002 / J-001, J-002
- **Regras de negócio:** RN-019
- **Dependências:** RF-004
- **Critérios de aceitação:**

```gherkin
Dado um check-in em andamento
Quando o operador anexar fotos e observações
Então o sistema vincula as evidências à permanência com data, hora e usuário

Dado um veículo sem avarias aparentes
Quando o operador não anexar fotos
Então o sistema permite concluir o check-in, registrando que não houve evidência fotográfica

Dado um veículo com avaria registrada na entrada
Quando o operador consultar o histórico
Então o sistema exibe as fotos e observações daquele check-in

Dado um check-out em andamento
Quando o operador registrar a condição de saída
Então o sistema mantém as evidências de entrada e saída lado a lado no histórico
```

#### RF-006 — Consultar veículo e histórico

- **Descrição:** localizar permanências por placa completa, placa parcial (mínimo de 3 caracteres) ou código, exibindo status, vaga, tempo de permanência, condição e linha do tempo de eventos.
- **Prioridade:** Must
- **Persona/jornada:** P-001, P-002, P-003 / J-001, J-002, J-003
- **Regras de negócio:** RN-001, RN-015, RN-023
- **Dependências:** RF-004, RF-005, RF-011, RF-013
- **Critérios de aceitação:**

```gherkin
Dado um veículo no pátio
Quando o operador buscar pela placa completa
Então o sistema exibe a permanência, a vaga, o horário de entrada e o status

Dado que o operador informa ao menos 3 caracteres da placa
Quando executar a busca
Então o sistema lista as permanências compatíveis

Dado o código de retirada de uma permanência
Quando o operador buscar pelo código
Então o sistema exibe a permanência correspondente

Dado um usuário sem permissão financeira
Quando consultar a permanência
Então o sistema oculta valores, pagamentos e dados financeiros

Dado uma permanência com eventos
Quando o operador abrir o histórico
Então o sistema exibe entrada, fotos, pedidos de retirada, entrega, pagamentos e auditoria em ordem cronológica
```

#### RF-007 — Corrigir ou cancelar check-in

- **Descrição:** permitir a correção de dados do check-in e o cancelamento de uma permanência aberta, sempre com motivo e registro de auditoria.
- **Prioridade:** Must
- **Persona/jornada:** P-001, P-003 / J-001
- **Regras de negócio:** RN-003, RN-013, RN-014, RN-016
- **Dependências:** RF-004
- **Critérios de aceitação:**

```gherkin
Dado um check-in sem pagamento
Quando o Administrador cancelar a permanência com motivo
Então o sistema libera a vaga, encerra a permanência e registra a ação na auditoria

Dado um check-in com pagamento registrado
Quando o Administrador tentar cancelar
Então o sistema bloqueia o cancelamento e exige o estorno prévio

Dado um check-in com placa ou vaga incorreta
Quando o operador autorizado corrigir os dados
Então o sistema valida as regras, atualiza a permanência e registra os valores anteriores e novos
```

#### RF-008 — Painel de ocupação

- **Descrição:** exibir totais e a situação individual das vagas (livre, ocupada, bloqueada), com a placa do veículo quando ocupada, atualizando após cada operação.
- **Prioridade:** Must
- **Persona/jornada:** P-001, P-002, P-003 / J-004
- **Regras de negócio:** RN-020
- **Dependências:** RF-004, RF-009, RF-013
- **Critérios de aceitação:**

```gherkin
Dado que existem vagas livres, ocupadas e bloqueadas
Quando o painel for aberto
Então o sistema exibe os totais por status e a lista de vagas com a placa quando ocupada

Dado um check-in concluído
Quando o painel for atualizado
Então a vaga associada aparece como ocupada em até 5 segundos

Dado uma entrega confirmada
Quando o painel for atualizado
Então a vaga aparece como livre em até 5 segundos

Dado uma vaga bloqueada
Quando o painel calcular as vagas disponíveis
Então a vaga bloqueada não é contada como livre
```

#### RF-009 — Cadastrar e configurar vagas

- **Descrição:** permitir que o Administrador cadastre vagas com código e área, edite dados e altere o status entre ativa e bloqueada.
- **Prioridade:** Must
- **Persona/jornada:** P-001 / J-004
- **Regras de negócio:** RN-004, RN-020
- **Dependências:** RF-002
- **Critérios de aceitação:**

```gherkin
Dado um Administrador autenticado
Quando cadastrar uma vaga com código único e área
Então a vaga fica disponível como livre

Dado uma vaga ocupada
Quando o Administrador tentar excluí-la
Então o sistema bloqueia a exclusão e orienta a liberar o veículo primeiro

Dado uma vaga livre
Quando o Administrador a bloquear
Então ela deixa de contar como disponível e não pode receber veículos
```

#### RF-010 — Transferir veículo entre vagas

- **Descrição:** mover uma permanência ativa para outra vaga livre, registrando vaga de origem, vaga de destino, usuário e horário.
- **Prioridade:** Should
- **Persona/jornada:** P-002, P-001 / J-002
- **Regras de negócio:** RN-004, RN-013
- **Dependências:** RF-004, RF-008
- **Critérios de aceitação:**

```gherkin
Dado um veículo ocupando uma vaga e outra vaga livre
Quando o operador transferir o veículo
Então o sistema libera a origem, ocupa o destino e registra a movimentação

Dado um destino já ocupado
Quando o operador tentar transferir
Então o sistema bloqueia a operação

Dado um veículo com transferências registradas
Quando o histórico for consultado
Então o sistema exibe a sequência de vagas e responsáveis
```

#### RF-011 — Solicitar retirada

- **Descrição:** criar uma solicitação de retirada a partir do código ou da placa, registrando horário, usuário e veículo, com status inicial "aguardando".
- **Prioridade:** Must
- **Persona/jornada:** P-003, P-002 / J-002
- **Regras de negócio:** RN-012
- **Dependências:** RF-004, RF-006
- **Critérios de aceitação:**

```gherkin
Dado um veículo no pátio
Quando o operador solicitar a retirada pelo código
Então o sistema cria a solicitação com status "aguardando" e registra horário e usuário

Dado que o cliente não tem o código
Quando o operador localizar o veículo pela placa e solicitar a retirada
Então o sistema cria a solicitação normalmente

Dado um veículo com solicitação ativa
Quando o operador tentar criar outra solicitação
Então o sistema bloqueia a duplicidade e exibe a solicitação existente

Dado um código ou placa sem permanência ativa
Quando o operador solicitar a retirada
Então o sistema informa que o veículo não foi encontrado
```

#### RF-012 — Fila e status de retirada

- **Descrição:** exibir a fila de solicitações ordenada por horário, com tempo de espera e status "aguardando", "em busca", "entregue" ou "cancelada"; permitir atualização de status e cancelamento com motivo.
- **Prioridade:** Must
- **Persona/jornada:** P-002, P-003 / J-002
- **Regras de negócio:** RN-012, RN-016
- **Dependências:** RF-011
- **Critérios de aceitação:**

```gherkin
Dado que existem solicitações pendentes
Quando a fila for aberta
Então o sistema exibe as solicitações mais antigas primeiro, com o tempo de espera

Dado uma solicitação "aguardando"
Quando o manobrista iniciar a busca
Então o status muda para "em busca" e o horário é registrado

Dado uma solicitação "em busca"
Quando a entrega for confirmada
Então o status muda para "entregue" e o horário é registrado

Dado uma solicitação ativa
Quando o operador autorizado cancelar com motivo
Então o status muda para "cancelada" e a ação é auditada

Dado uma solicitação "entregue"
Quando o operador tentar voltar o status para "aguardando"
Então o sistema bloqueia a transição inválida
```

#### RF-013 — Confirmar entrega e liberar vaga

- **Descrição:** confirmar a entrega do veículo, registrar horário e responsável, encerrar a permanência, calcular o valor final e liberar a vaga.
- **Prioridade:** Must
- **Persona/jornada:** P-002, P-003 / J-002, J-003
- **Regras de negócio:** RN-009, RN-010, RN-013
- **Dependências:** RF-012, RF-014, RF-017
- **Critérios de aceitação:**

```gherkin
Dado um veículo com solicitação "em busca"
Quando o operador confirmar a entrega
Então o sistema registra a saída, encerra a permanência, libera a vaga e exibe o valor final

Dado um veículo com saldo pendente
Quando o operador tentar confirmar a entrega
Então o sistema alerta sobre o saldo e exige autorização do Administrador

Dado que o Administrador autorize a entrega com pendência
Quando confirmar com justificativa
Então o sistema conclui a entrega e registra a autorização na auditoria

Dado um veículo entregue
Quando o painel de ocupação for consultado
Então a vaga aparece como livre
```

#### RF-014 — Calcular diárias automaticamente

- **Descrição:** calcular o valor da permanência em blocos de 24 horas desde a entrada, com tolerância de 15 minutos por bloco, tarifa registrada no check-in e detalhamento de tempo, diárias e total.
- **Prioridade:** Must
- **Persona/jornada:** P-003 / J-003
- **Regras de negócio:** RN-005, RN-006, RN-007
- **Dependências:** RF-004, RF-015
- **Critérios de aceitação:**

```gherkin
Dado um veículo com 10 horas de permanência
Quando o valor for calculado
Então o sistema cobra 1 diária

Dado um veículo com 24 horas e 15 minutos de permanência
Quando o valor for calculado
Então o sistema cobra 1 diária

Dado um veículo com 24 horas e 16 minutos de permanência
Quando o valor for calculado
Então o sistema cobra 2 diárias

Dado um veículo com 49 horas de permanência
Quando o valor for calculado
Então o sistema cobra 3 diárias

Dado que a tarifa foi alterada após o check-in
Quando o valor da permanência aberta for calculado
Então o sistema mantém a tarifa registrada no check-in

Dado uma permanência em cobrança
Quando o operador abrir o detalhamento
Então o sistema exibe tempo de permanência, número de diárias, valor unitário e total
```

#### RF-015 — Configurar tarifa

- **Descrição:** permitir que o Administrador defina a tarifa única de diária em BRL, com data de vigência e histórico, sem alterar permanências já abertas.
- **Prioridade:** Must
- **Persona/jornada:** P-001 / J-004
- **Regras de negócio:** RN-006, RN-007
- **Dependências:** RF-002
- **Critérios de aceitação:**

```gherkin
Dado um Administrador autenticado
Quando cadastrar uma tarifa com valor e início de vigência
Então a tarifa passa a valer para novos check-ins a partir da data informada

Dado uma tarifa vigente
Quando o Administrador cadastrar uma nova tarifa
Então o histórico mantém a tarifa anterior e a nova vigência

Dado uma permanência aberta com tarifa registrada
Quando a tarifa for alterada
Então o valor da permanência não muda

Dado uma tentativa de alterar a tarifa de uma permanência já encerrada
Quando o Administrador tentar editar
Então o sistema bloqueia a alteração retroativa
```

#### RF-016 — Registrar pagamentos e estornos

- **Descrição:** registrar um ou mais pagamentos por permanência, com valor, forma (dinheiro, débito, crédito ou Pix), data, hora e usuário; permitir estorno pelo Administrador com motivo.
- **Prioridade:** Must
- **Persona/jornada:** P-003 / J-003
- **Regras de negócio:** RN-008, RN-009, RN-022, RN-025
- **Dependências:** RF-014, RF-017
- **Critérios de aceitação:**

```gherkin
Dado uma permanência com saldo devedor
Quando o Caixa registrar um pagamento em dinheiro
Então o sistema registra valor, forma, data, hora e usuário e atualiza o saldo

Dado uma permanência com saldo parcial
Quando o Caixa registrar um segundo pagamento
Então o sistema soma os pagamentos e atualiza o status financeiro

Dado um pagamento superior ao saldo devedor
Quando o Caixa tentar registrar
Então o sistema bloqueia a operação e informa o valor máximo permitido

Dado um pagamento confirmado
Quando o Administrador solicitar estorno com motivo
Então o sistema registra o estorno, mantém o histórico e recalcula o saldo
```

#### RF-017 — Calcular saldo e status financeiro

- **Descrição:** calcular total cobrado, total pago e saldo pendente de cada permanência, classificando o status como pendente, parcial ou pago.
- **Prioridade:** Must
- **Persona/jornada:** P-003, P-001 / J-003
- **Regras de negócio:** RN-009, RN-010
- **Dependências:** RF-014, RF-016
- **Critérios de aceitação:**

```gherkin
Dado uma permanência sem pagamento
Quando o sistema calcular o saldo
Então o status é "pendente" e o saldo é igual ao total

Dado uma permanência com pagamento menor que o total
Quando o sistema calcular o saldo
Então o status é "parcial" e o saldo é a diferença

Dado uma permanência com pagamento igual ao total
Quando o sistema calcular o saldo
Então o status é "pago" e o saldo é zero

Dado uma permanência com saldo pendente
Quando o operador tentar entregar o veículo
Então o sistema exibe o alerta e aplica a regra de autorização
```

#### RF-018 — Emitir comprovante não fiscal

- **Descrição:** gerar um comprovante simples, impresso ou em PDF, com código, placa, período, número de diárias, valor total, pagamentos e status, identificado como não fiscal.
- **Prioridade:** Should
- **Persona/jornada:** P-003 / J-003
- **Regras de negócio:** RN-021
- **Dependências:** RF-017
- **Critérios de aceitação:**

```gherkin
Dado uma permanência encerrada ou em cobrança
Quando o Caixa emitir o comprovante
Então o documento contém código, placa, período, diárias, total, pagamentos e status

Dado um comprovante emitido
Quando o documento for exibido
Então ele informa claramente que não é documento fiscal

Dado um comprovante em PDF
Quando o Caixa baixar o arquivo
Então o sistema gera o PDF com os mesmos dados da tela
```

#### RF-019 — Dashboard operacional

- **Descrição:** exibir indicadores do dia: vagas livres, ocupadas e bloqueadas, veículos no pátio, solicitações na fila, receita recebida, valores pendentes e ocupação percentual.
- **Prioridade:** Must
- **Persona/jornada:** P-001, P-003 / J-004
- **Regras de negócio:** RN-015, RN-018, RN-020
- **Dependências:** RF-008, RF-012, RF-017
- **Critérios de aceitação:**

```gherkin
Dado que o dia tem operações registradas
Quando o Administrador abrir o dashboard
Então o sistema exibe os indicadores atualizados do dia

Dado um usuário Manobrista
Quando abrir o dashboard
Então o sistema oculta receita e valores pendentes

Dado uma nova entrega ou pagamento
Quando o dashboard for atualizado
Então os indicadores refletem a operação em até 5 segundos

Dado um dia sem movimento
Quando o dashboard for aberto
Então o sistema exibe os indicadores zerados e um estado vazio compreensível
```

#### RF-020 — Relatório financeiro por período

- **Descrição:** gerar relatórios diários, semanais e mensais com receita recebida, valor cobrado, valores pendentes, veículos atendidos, ticket médio e totais por forma de pagamento.
- **Prioridade:** Must
- **Persona/jornada:** P-001, P-003 / J-004
- **Regras de negócio:** RN-018
- **Dependências:** RF-016, RF-017
- **Critérios de aceitação:**

```gherkin
Dado um período selecionado
Quando o Administrador gerar o relatório financeiro
Então o sistema exibe receita recebida, cobrada, pendente, veículos atendidos e ticket médio

Dado um período com pagamentos em formas diferentes
Quando o relatório for gerado
Então o sistema totaliza por dinheiro, débito, crédito e Pix

Dado um período sem movimento
Quando o relatório for gerado
Então o sistema exibe totais zerados e estado vazio

Dado um usuário sem permissão financeira
Quando tentar acessar o relatório
Então o sistema nega o acesso
```

#### RF-021 — Relatório de divergências

- **Descrição:** listar permanências com valor cobrado diferente do pago, pagamentos parciais, pendentes e estornados, com o valor da diferença e filtros por período.
- **Prioridade:** Must
- **Persona/jornada:** P-001, P-003 / J-004
- **Regras de negócio:** RN-009, RN-022
- **Dependências:** RF-017
- **Critérios de aceitação:**

```gherkin
Dado que existem permanências com saldo pendente
Quando o relatório de divergências for gerado
Então o sistema lista cada permanência, o valor cobrado, o pago, a diferença e o status

Dado um período com estornos
Quando o relatório for gerado
Então os estornos aparecem com motivo e impacto no saldo

Dado um filtro por período
Quando o relatório for gerado
Então o sistema considera apenas as permanências do intervalo

Dado que não há divergências
Quando o relatório for gerado
Então o sistema exibe estado vazio com total zero
```

#### RF-022 — Relatório de ocupação e horários de pico

- **Descrição:** apresentar a ocupação ao longo do tempo, horários de maior entrada e saída, tempo médio de permanência e giro das vagas.
- **Prioridade:** Must
- **Persona/jornada:** P-001 / J-004
- **Regras de negócio:** RN-018, RN-020
- **Dependências:** RF-004, RF-013
- **Critérios de aceitação:**

```gherkin
Dado um período selecionado
Quando o Administrador gerar o relatório de ocupação
Então o sistema exibe a ocupação ao longo do tempo e o percentual médio

Dado um período com entradas e saídas
Quando o relatório for gerado
Então o sistema identifica os horários de pico de entrada e saída

Dado um período com permanências encerradas
Quando o relatório for gerado
Então o sistema calcula o tempo médio de permanência e o giro das vagas

Dado um usuário sem permissão
Quando tentar acessar o relatório
Então o sistema nega o acesso
```

#### RF-023 — Exportar relatórios

- **Descrição:** exportar os relatórios financeiros, de divergências e de ocupação em CSV, respeitando os filtros aplicados.
- **Prioridade:** Should
- **Persona/jornada:** P-001, P-003 / J-004
- **Regras de negócio:** RN-015, RN-018
- **Dependências:** RF-020, RF-021, RF-022
- **Critérios de aceitação:**

```gherkin
Dado um relatório com filtro de período
Quando o usuário exportar em CSV
Então o arquivo contém apenas os dados do filtro e os mesmos totais da tela

Dado um usuário sem permissão financeira
Quando tentar exportar relatório financeiro
Então o sistema nega a exportação

Dado uma exportação concluída
Quando o arquivo for aberto
Então os valores e rótulos estão legíveis e separados corretamente
```

#### RF-024 — Trilha de auditoria

- **Descrição:** registrar de forma append-only as ações críticas do sistema, com usuário, data, hora, ação, entidade afetada e valores anterior e novo quando aplicável.
- **Prioridade:** Must
- **Persona/jornada:** P-001 / J-004
- **Regras de negócio:** RN-016
- **Dependências:** RF-001, RF-004, RF-007, RF-012, RF-013, RF-016
- **Critérios de aceitação:**

```gherkin
Dado um check-in, edição, cancelamento, retirada, entrega, pagamento, estorno, alteração de tarifa ou de usuário
Quando a ação for concluída
Então o sistema registra usuário, data, hora, ação, entidade e valores alterados

Dado um registro de auditoria
Quando o Administrador tentar editá-lo ou excluí-lo
Então o sistema bloqueia a operação

Dado um filtro por período, usuário, veículo ou tipo de ação
Quando o Administrador consultar a auditoria
Então o sistema retorna os registros correspondentes em ordem cronológica

Dado um usuário sem permissão de auditoria
Quando tentar acessar a trilha
Então o sistema nega o acesso
```

#### RF-025 — Expurgo e anonimização de dados

- **Descrição:** após 12 meses do encerramento da permanência, remover ou anonimizar fotos, nome e telefone do cliente, preservando registros financeiros pelo prazo legal aplicável e mantendo evidência da execução.
- **Prioridade:** Must
- **Persona/jornada:** P-001 / J-004
- **Regras de negócio:** RN-017
- **Dependências:** RF-005, RF-024
- **Critérios de aceitação:**

```gherkin
Dado uma permanência encerrada há mais de 12 meses
Quando o expurgo for executado
Então fotos, nome e telefone são removidos ou anonimizados

Dado um registro financeiro vinculado à permanência
Quando o expurgo for executado
Então os dados financeiros são preservados pelo prazo legal aplicável

Dado uma solicitação de titular de dados
Quando o Administrador executar o atendimento
Então o sistema permite localizar, corrigir, exportar ou anonimizar os dados cabíveis

Dado um expurgo concluído
Quando a auditoria for consultada
Então o sistema exibe a execução, a data e a quantidade de registros tratados
```

## 7. Regras de negócio

| ID | Regra | Condição | Exceções | Requisitos impactados |
| --- | --- | --- | --- | --- |
| RN-001 | A placa é normalizada em maiúsculas, sem espaços ou hífens, e aceita os formatos antigo (AAA-9999) e Mercosul (AAA9A99) | Todo cadastro e busca de veículo | Nenhuma | RF-004, RF-006 |
| RN-002 | O código de retirada tem 6 caracteres alfanuméricos, sem caracteres ambíguos (I, O, 0, 1), não diferencia maiúsculas de minúsculas e é único entre permanências ativas | Geração de código no check-in | Nenhuma | RF-004, RF-006, RF-011 |
| RN-003 | Um veículo identificado pela placa só pode ter uma permanência ativa por vez | Novo check-in | Nenhuma | RF-004, RF-007 |
| RN-004 | Uma vaga só pode estar associada a um veículo por vez | Check-in ou transferência | Nenhuma | RF-004, RF-009, RF-010 |
| RN-005 | A diária é um bloco de 24 horas contado da entrada; cada bloco admite 15 minutos de tolerância antes de iniciar o próximo; a fração inicial conta como diária cheia | Cálculo de permanência | Nenhuma | RF-014 |
| RN-006 | A tarifa vigente no check-in é registrada na permanência e aplicada no cálculo final; alterações posteriores não afetam permanências abertas | Check-in e cobrança | Nenhuma | RF-014, RF-015 |
| RN-007 | A tarifa é única, em BRL, configurável pelo Administrador, com data de início de vigência e histórico | Configuração de tarifa | Nenhuma | RF-014, RF-015 |
| RN-008 | As formas de pagamento aceitas são dinheiro, débito, crédito e Pix; uma permanência pode ter múltiplos pagamentos | Registro de pagamento | Nenhuma | RF-016 |
| RN-009 | O status financeiro é pendente quando o pago é zero, parcial quando o pago é maior que zero e menor que o total, e pago quando o pago é igual ao total | Cálculo de saldo | Nenhuma | RF-016, RF-017, RF-021 |
| RN-010 | A entrega com saldo pendente gera alerta e exige autorização do Administrador com justificativa registrada na auditoria | Confirmação de entrega | Nenhuma | RF-013, RF-017 |
| RN-011 | Em caso de ticket perdido, somente o Administrador pode liberar a saída, após confirmar a identidade do cliente; a ocorrência é auditada e não há taxa automática | Ticket perdido | Taxa específica, se aprovada no futuro | RF-007, RF-013, RF-024 |
| RN-012 | Cada veículo pode ter uma solicitação de retirada ativa; o fluxo é aguardando → em busca → entregue, com cancelamento permitido mediante motivo | Fila de retirada | Nenhuma | RF-011, RF-012 |
| RN-013 | A vaga é liberada somente na confirmação de entrega ou no cancelamento do check-in | Liberação de vaga | Nenhuma | RF-007, RF-010, RF-013 |
| RN-014 | O cancelamento de check-in só é permitido sem pagamento registrado; exige motivo, libera a vaga e é auditado | Cancelamento de permanência | Com pagamento, exige estorno prévio | RF-007 |
| RN-015 | Permissões: Administrador acessa tudo; Manobrista opera check-in, condição, vagas, consulta operacional, retirada e entrega, sem dados financeiros, tarifas, usuários ou auditoria; Caixa opera check-in, consulta, cobrança, pagamentos, comprovante e relatórios financeiros, sem usuários, tarifas ou auditoria | Controle de acesso | Nenhuma | RF-001, RF-002, RF-003, RF-006, RF-019, RF-020, RF-023, RF-024 |
| RN-016 | A trilha de auditoria é append-only, sem edição ou exclusão, e registra usuário, data, hora, ação, entidade e valores alterados | Auditoria | Nenhuma | RF-024 |
| RN-017 | Fotos, nome e telefone são mantidos por 12 meses após o encerramento da permanência e depois removidos ou anonimizados; a placa vinculada a registro financeiro e os demais dados financeiros seguem o prazo legal aplicável, com acesso restrito | Retenção e expurgo | Prazo fiscal definido com contabilidade | RF-025 |
| RN-018 | Relatórios usam o fuso America/Sao_Paulo, dia civil e valores em BRL | Relatórios e dashboard | Nenhuma | RF-019, RF-020, RF-022, RF-023 |
| RN-019 | O check-in exige placa e modelo; cor, nome e telefone do cliente e observações são opcionais; são permitidas até 5 fotos por evento de entrada ou saída | Check-in e condição | Nenhuma | RF-004, RF-005 |
| RN-020 | A ocupação considera apenas vagas ativas; vagas bloqueadas não contam como livres | Painel e relatórios | Nenhuma | RF-008, RF-009, RF-019, RF-022 |
| RN-021 | O comprovante é não fiscal e deve informar essa condição | Emissão de comprovante | Nenhuma | RF-018 |
| RN-022 | O estorno é exclusivo do Administrador, exige motivo, não apaga o pagamento original e gera um registro de estorno | Estorno | Nenhuma | RF-016, RF-021 |
| RN-023 | A busca por placa parcial exige no mínimo 3 caracteres | Consulta de veículo | Nenhuma | RF-006 |
| RN-024 | Quando não há vaga livre, o check-in é bloqueado e a operação deve liberar ou desbloquear uma vaga | Check-in sem vaga | Nenhuma | RF-004, RF-009 |
| RN-025 | O sistema não aceita pagamento superior ao saldo devedor; o operador deve corrigir o valor informado | Registro de pagamento | Nenhuma | RF-016 |

## 8. Requisitos não funcionais

| ID | Categoria | Requisito | Meta mensurável | Verificação |
| --- | --- | --- | --- | --- |
| RNF-001 | Desempenho | Consultas e operações precisam responder rápido para não travar o atendimento | Busca por placa ou código com p95 ≤ 1 s; telas operacionais com p95 ≤ 2 s; painel atualizado ≤ 5 s após a ação | Teste de carga e monitoramento |
| RNF-002 | Disponibilidade | O sistema deve estar disponível durante a operação | 99,5% ao mês, com janela de manutenção comunicada | Monitoramento de disponibilidade e SLA |
| RNF-003 | Segurança | Proteger autenticação, sessão e dados contra acesso indevido | Senhas com hash forte, sessão de 30 min, menor privilégio, TLS e controles do OWASP Top 10 | Revisão de segurança e testes |
| RNF-004 | Privacidade/LGPD | Tratar dados pessoais com base legal documentada, minimização e direitos do titular | Base legal registrada; retenção de 12 meses para fotos, nome e telefone; placa e dados financeiros pelo prazo legal aplicável, com acesso restrito; criptografia em trânsito e em repouso; atendimento a acesso, correção e anonimização | Revisão jurídica e auditoria de dados |
| RNF-005 | Acessibilidade | As telas devem ser utilizáveis por pessoas com deficiência | WCAG 2.2 nível AA nas telas operacionais | Auditoria de acessibilidade |
| RNF-006 | Usabilidade | A operação deve ser rápida para usuários treinados | Check-in ≤ 60 s; solicitação de retirada ≤ 15 s; erro operacional < 2% em teste com 5 usuários | Teste de usabilidade |
| RNF-007 | Escalabilidade | Suportar o volume da unidade sem degradação | 1.000 veículos ativos, 30 usuários simultâneos e 500 check-ins por dia (premissa a validar) | Teste de carga |
| RNF-008 | Observabilidade | Permitir detectar e investigar falhas | Logs estruturados, métricas de erro 5xx e alertas de indisponibilidade; retenção de logs por 90 dias | Monitoramento e revisão operacional |
| RNF-009 | Compatibilidade | Funcionar nos dispositivos usados pela operação | Últimas 2 versões de Chrome, Edge, Firefox e Safari; desktop e tablet; layout responsivo | Matriz de testes |
| RNF-010 | Backup e recuperação | Proteger os dados contra perda | Backup diário; RPO ≤ 24 h; RTO ≤ 8 h; teste de restauração trimestral | Teste de restauração |
| RNF-011 | Concorrência | Evitar duplicidade em acessos simultâneos | Dupla atribuição de vaga e permanência duplicada bloqueadas por restrição de integridade | Teste de concorrência |
| RNF-012 | Localização | Interface adequada à operação brasileira | pt-BR, BRL e America/Sao_Paulo | Revisão de interface |

> As metas de RNF são propostas iniciais de engenharia e operação. Volume, disponibilidade e desempenho devem ser confirmados com o negócio (Q-008).

## 9. Experiência e fluxos

### 9.1 Fluxo principal

1. O manobrista recebe o veículo e abre o check-in.
2. Informa placa e modelo; o sistema valida e verifica se não há permanência ativa.
3. Registra observações e fotos da condição de entrada.
4. Seleciona uma vaga livre; o sistema marca a vaga como ocupada.
5. O sistema gera o código único e o operador entrega ao cliente.
6. Quando o cliente pede o carro, o caixa localiza por código ou placa.
7. O sistema cria a solicitação de retirada com status "aguardando".
8. O manobrista inicia a busca; o status muda para "em busca".
9. O veículo é entregue; o operador confirma a entrega e o sistema registra o horário.
10. O sistema calcula as diárias, mostra o detalhamento e o saldo.
11. O caixa registra o pagamento e, se aplicável, emite o comprovante.
12. A vaga é liberada e os indicadores e relatórios são atualizados.

### 9.2 Fluxos alternativos e de erro

| Situação | Comportamento esperado |
| --- | --- |
| Placa já possui permanência ativa | Bloquear o novo check-in e exibir a permanência existente |
| Vaga escolhida está ocupada | Bloquear a associação e sugerir vaga livre |
| Não há vaga livre | Bloquear o check-in e orientar a liberar ou desbloquear uma vaga |
| Cliente perdeu o ticket | Administrador confirma a identidade, libera a saída e registra a ocorrência |
| Pagamento superior ao saldo | Bloquear e informar o valor máximo permitido |
| Entrega com saldo pendente | Alertar e exigir autorização do Administrador com justificativa |
| Check-in cancelado sem pagamento | Liberar a vaga, encerrar a permanência e auditar |
| Check-in cancelado com pagamento | Bloquear até o estorno ser registrado |
| Solicitação de retirada duplicada | Bloquear e exibir a solicitação ativa |
| Transição inválida de status | Bloquear e manter o status atual |
| Dois operadores atualizam o mesmo veículo | Aplicar controle de concorrência e informar que o registro foi alterado |
| Sistema indisponível | Usar o procedimento manual de contingência e registrar os atendimentos para lançamento posterior |
| Cliente não tem código nem placa | Solicitar identificação alternativa ao Administrador; não liberar sem confirmação |
| Divergência de dano | Consultar fotos e observações de entrada e saída; registrar a ocorrência na auditoria |

### 9.3 Estados da interface

- **Vazio:** sem veículos no pátio, sem solicitações na fila, sem resultados de busca ou sem divergências.
- **Carregando:** indicador visual durante consultas e geração de relatórios.
- **Sucesso:** confirmação clara da ação com código, vaga ou valor quando aplicável.
- **Erro:** mensagem objetiva com a causa e o próximo passo.
- **Sem permissão:** informar que a função não está disponível para o perfil.
- **Sem vaga livre:** destacar a ocupação e orientar a liberar ou desbloquear uma vaga.
- **Concorrência:** informar que o registro foi atualizado por outro usuário e solicitar recarga.

### 9.4 Conteúdo e tom

- Idioma pt-BR, valores em BRL e horários em America/Sao_Paulo.
- Mensagens curtas, diretas e orientadas à ação.
- Não usar termos técnicos internos na interface operacional.
- Código de retirada sempre em maiúsculas e com caracteres não ambíguos.
- Mensagens de erro devem explicar o que aconteceu e o que fazer, sem culpar o operador.
- Exemplos: "Vaga ocupada. Selecione outra vaga."; "Saldo pendente de R$ 45,00. A entrega exige autorização do administrador."; "Código não encontrado. Confira o código ou busque pela placa."

## 10. Modelo de dados conceitual

| Entidade | Descrição | Atributos-chave | Relacionamentos |
| --- | --- | --- | --- |
| Unidade | Estacionamento atendido pelo sistema | id, nome, endereço, fuso, moeda | 1:N com Vaga, Permanência e Usuário |
| Usuário | Pessoa que opera o sistema | id, nome, e-mail, senha protegida, perfil, status, criado_em | 1:N com Permanência, Pagamento e Auditoria |
| Perfil | Papel de acesso | id, nome (Administrador, Manobrista, Caixa) | 1:N com Usuário |
| Vaga | Posição física de estacionamento | id, unidade_id, código, área, status (livre, ocupada, bloqueada) | 1:N com Permanência |
| Veículo | Veículo recebido | id, placa, modelo, cor | 1:N com Permanência |
| Cliente | Pessoa que deixa o veículo (opcional) | id, nome, telefone | 1:N com Veículo |
| Permanência | Estadia do veículo no pátio | id, unidade_id, veículo_id, vaga_id, código, entrada_em, saída_em, status, tarifa_id, valor_total, observações | 1:N com Foto, Solicitação e Pagamento |
| Foto de condição | Evidência fotográfica de entrada ou saída | id, permanência_id, tipo, arquivo, tirada_em, usuário_id | N:1 com Permanência |
| Solicitação de retirada | Pedido de retirada do veículo | id, permanência_id, solicitada_em, status, iniciada_em, entregue_em, cancelada_em, motivo, usuário_id | N:1 com Permanência |
| Tarifa | Valor da diária com vigência | id, valor_diaria, vigência_inicio, vigência_fim, criado_por, criado_em | 1:N com Permanência |
| Pagamento | Valor recebido ou estornado | id, permanência_id, valor, forma, pago_em, usuário_id, status, motivo_estorno | N:1 com Permanência |
| Registro de auditoria | Histórico imutável de ações críticas | id, timestamp, usuário_id, ação, entidade, entidade_id, dados_anteriores, dados_novos, ip | N:1 com Usuário |
| Configuração | Parâmetros operacionais | id, chave, valor | N:1 com Unidade |

## 11. Integrações e dependências

O MVP não possui integração externa obrigatória. Pagamentos são registrados manualmente e não há comunicação com maquininha, Pix, gateway, sistema fiscal ou câmera de placas.

| Integração/Dependência | Tipo | Dono | O que fornece/consome | Risco |
| --- | --- | --- | --- | --- |
| Armazenamento de fotos | Interna | Infraestrutura | Guarda as evidências de condição do veículo | Médio |
| Banco de dados relacional | Interna | Infraestrutura | Persiste operação, finanças e auditoria | Alto |
| Backup e restauração | Interna | Infraestrutura | Protege os dados contra perda | Alto |
| Serviço de e-mail | Interna | Infraestrutura | Envio de credenciais e comunicações administrativas, se adotado | Baixo |
| Maquininha/Pix | Futura | Financeiro | Processamento automático de pagamentos | Médio |
| Sistema fiscal | Futura | Financeiro/contábil | Emissão de documento fiscal | Alto |
| Câmera/LPR | Futura | Operações | Leitura automática de placas | Médio |

## 12. Premissas, restrições e dependências

### 12.1 Premissas

| ID | Premissa | Ambiguidade de origem | Impacto se falsa | Confirmada? |
| --- | --- | --- | --- | --- |
| PR-001 | O MVP atende uma única unidade | AMB-011 | Exige isolamento por unidade e revisão de escopo, prazos e permissões | Sim — 2026-11-05 |
| PR-002 | A interface é web, operada somente pela equipe | AMB-013 | Exige autoatendimento, app ou totem, ampliando o escopo | Sim — 2026-11-05 |
| PR-003 | O sistema apenas registra pagamentos; não processa transações | AMB-004 | Exige integração com maquininha, Pix ou gateway e conciliação | Sim — 2026-11-05 |
| PR-004 | A tarifa aplicada é a vigente no check-in | AMB-015 | Muda o valor final e pode gerar disputa com o cliente | Sim — 2026-11-05 |
| PR-005 | A entrega com saldo pendente exige autorização do Administrador | AMB-016 | Muda o controle financeiro e o fluxo de entrega | Sim — 2026-11-05 |
| PR-006 | Ticket perdido não gera taxa automática; somente o Administrador libera | AMB-008 | Exige regra de cobrança adicional e revisão do fluxo | Sim — 2026-11-05 |
| PR-007 | Não há emissão fiscal no MVP | AMB-012 | Exige integração fiscal e novo escopo | Sim — 2026-11-05 |
| PR-008 | O sistema não opera offline | AMB-017 | Exige sincronização, resolução de conflitos e novo escopo | Sim — 2026-11-05 |
| PR-009 | A placa é digitada manualmente, sem LPR | AMB-018 | Exige integração com câmeras e tratamento de erros de leitura | Sim — 2026-11-05 |
| PR-010 | Fotos, nome e telefone são retidos por 12 meses; financeiro segue prazo fiscal | AMB-010, AMB-024 | Exige revisão jurídica e mudança no expurgo | Sim — 2026-11-05 |
| PR-011 | Nome e telefone do cliente são opcionais | AMB-019 | Pode dificultar contato em casos de dano ou cobrança | Sim — 2026-11-05 |
| PR-012 | São permitidas até 5 fotos por evento de entrada ou saída | AMB-021 | Exige ajuste de armazenamento e da interface | Sim — 2026-11-05 |
| PR-013 | Relatórios usam America/Sao_Paulo e BRL | AMB-020 | Exige suporte a múltiplos fusos ou moedas | Sim — 2026-11-05 |
| PR-014 | Sem vaga livre, o check-in é bloqueado | AMB-027 | Exige regra de exceção e controle de overbooking | Sim — 2026-11-05 |
| PR-015 | Não há sessão formal de caixa; há relatórios diários | AMB-028 | Exige abertura/fechamento de caixa e conferência de gaveta | Sim — 2026-11-05 |
| PR-016 | A exportação do MVP é em CSV; PDF é opcional | AMB-029 | Exige geração de PDF e layout adicional | Sim — 2026-11-05 |
| PR-017 | O projeto usa o stack já sinalizado no README (.NET, SQL Server e Angular) | AMB-030 | Exige revisão de arquitetura e treinamento da equipe | Sim — 2026-11-05 |
| PR-018 | O cancelamento de retirada é permitido com motivo e auditoria | AMB-031 | Exige manter solicitações canceladas e tratar reabertura | Sim — 2026-11-05 |
| PR-019 | Pagamento acima do saldo é bloqueado | AMB-032 | Exige tratamento de troco, crédito ou ajuste manual | Sim — 2026-11-05 |
| PR-020 | Os estados de retirada são aguardando, em busca, entregue e cancelada | AMB-006 | Exige novos estados e transições | Sim — 2026-11-05 |
| PR-021 | As metas de RNF são propostas iniciais; as metas de M-001 e M-003 serão definidas com o negócio | AMB-025, AMB-026 | Pode exigir redimensionamento de infraestrutura e revisão das metas | Sim — 2026-11-05 |

### 12.2 Restrições

- O projeto já sinaliza o stack .NET, SQL Server, ASP.NET Core Identity, Angular, Tailwind CSS e Lucide Icons no `README.md`. O PRD não prescreve arquitetura, mas essa escolha é uma restrição do projeto.
- O MVP atende uma única unidade e uma única tabela de tarifa.
- A operação exige conexão com a internet; não há modo offline.
- A solução deve atender à LGPD e às obrigações fiscais aplicáveis.
- Prazo, orçamento e equipe de desenvolvimento não estão definidos.
- Não há integração fiscal, de pagamento ou de reconhecimento de placa no MVP.

### 12.3 Dependências

- Validação do PRD por Alexandre Rech e pela operação real.
- Definição dos valores de tarifa e da data de vigência (Q-002).
- Revisão jurídica da base legal, retenção e direitos do titular (Q-003).
- Definição de volume esperado e metas de desempenho (Q-008).
- Infraestrutura de hospedagem, banco de dados, armazenamento de fotos e backup.
- Treinamento dos perfis Administrador, Manobrista e Caixa.
- Procedimento manual de contingência para indisponibilidade.

## 13. Riscos e mitigações

| ID | Risco | Probabilidade | Impacto | Mitigação | Responsável |
| --- | --- | --- | --- | --- | --- |
| R-001 | Escopo do MVP crescer e atrasar a entrega | Alta | Alto | Congelar o escopo aprovado, registrar mudanças e priorizar o fluxo operacional | Produto |
| R-002 | Resistência da equipe e baixa adoção | Alta | Alto | Envolver a operação, treinar, pilotar e ajustar a interface | Operações |
| R-003 | Erros de digitação de placa ou dados incompletos | Média | Médio | Validação de formato, normalização, busca parcial e conferência no check-in | Produto/Engenharia |
| R-004 | Divergência na cobrança por dúvida sobre a regra de diárias | Alta | Alto | Exibir detalhamento, publicar a regra e manter histórico de tarifa | Produto/Financeiro |
| R-005 | Ocupação divergente por movimentação não registrada | Alta | Alto | Tornar a atualização obrigatória, auditar contagens e revisar o fluxo | Operações |
| R-006 | Vazamento ou acesso indevido a dados pessoais e financeiros | Média | Alto | RBAC, menor privilégio, criptografia, auditoria e revisão de segurança | Engenharia/Segurança |
| R-007 | Não conformidade com LGPD | Média | Alto | Revisão jurídica, minimização, retenção definida e atendimento ao titular | Jurídico/Produto |
| R-008 | Indisponibilidade do sistema no horário de pico | Média | Alto | Monitoramento, backup, plano de contingência manual e SLA | Infraestrutura |
| R-009 | Concorrência gerar vaga ou permanência duplicada | Média | Médio | Restrições de integridade, controle de concorrência e mensagens claras | Engenharia |
| R-010 | Fotos de danos com baixa qualidade ou sem contexto | Média | Médio | Checklist de fotos, carimbo de data/hora e orientação de captura | Operações |
| R-011 | Tarifa alterada durante permanência gerar disputa | Média | Médio | Registrar a tarifa no check-in e exibir o histórico | Produto/Financeiro |
| R-012 | Perda de dados por falha de backup | Baixa | Alto | Backup diário, teste de restauração e retenção adequada | Infraestrutura |
| R-013 | Dependência de uma única pessoa para validar regras de negócio | Média | Médio | Documentar decisões, envolver mais stakeholders e registrar premissas | Produto |

## 14. Cronograma e marcos

| Marco | Escopo | Data alvo | Critério de conclusão |
| --- | --- | --- | --- |
| M1 — MVP | Operação completa descrita na seção 5.1 | A definir | Requisitos Must aceitos, piloto concluído e treinamento realizado |
| M2 — Estabilização | Correções do piloto e ajustes de usabilidade | A definir | Métricas de adoção e erros dentro da meta |
| M3 — Fase 2 | Autoatendimento ou integração de pagamento, conforme prioridade | A definir | Escopo priorizado e aprovado |

> Cronograma não definido na entrevista. As datas devem ser preenchidas após estimativa de esforço e priorização (Q-015).

## 15. Plano de lançamento

- **Piloto:** uma unidade, com acompanhamento diário nas duas primeiras semanas.
- **Operação paralela:** manter o processo manual como contingência durante o período de estabilização, sem duplicar a cobrança oficial.
- **Treinamento:** sessões por perfil, com foco em check-in, retirada, cobrança e tratamento de exceções.
- **Comunicação:** avisar clientes e equipe sobre a mudança e o novo comprovante.
- **Go-live:** iniciar em horário de menor movimento e com suporte presencial.
- **Rollback:** se houver falha crítica, voltar ao processo manual e registrar os atendimentos para lançamento posterior.
- **Critérios de sucesso do piloto:** check-ins digitais completos, fila de retirada funcionando, cobrança sem divergência sistemática e equipe treinada.
- **Responsáveis:** Produto, Operações e Infraestrutura (nomes a definir).

## 16. Perguntas em aberto

| ID | Pergunta | Por que importa | Dono | Prazo |
| --- | --- | --- | --- | --- |
| Q-001 | Qual é o baseline e a meta de tempo médio de retirada e de divergência de cobrança? | Sem baseline não é possível medir o sucesso de M-001 e M-003 | Alexandre Rech | A definir |
| Q-002 | Quais são os valores da tarifa e a data de vigência inicial? | O cálculo depende do valor configurado | Financeiro | A definir |
| Q-003 | Qual é o prazo legal de retenção dos registros financeiros? | Pode conflitar com a retenção de 12 meses de dados pessoais | Jurídico/Contabilidade | A definir |
| Q-004 | Qual é o horário de funcionamento e o corte do dia para relatórios? | Afeta fechamento diário, ocupação e horários de pico | Operações | A definir |
| Q-005 | A entrega com saldo pendente deve ser bloqueada ou apenas alertada? | A premissa atual exige autorização do Administrador | Alexandre Rech | A definir |
| Q-006 | O cliente precisa de ticket ou comprovante impresso no MVP? | Afeta impressão, papel e fluxo de balcão | Operações | A definir |
| Q-007 | Qual é o limite de fotos, tamanho e tempo de armazenamento? | Afeta custo de armazenamento e evidência de danos | Operações/Infraestrutura | A definir |
| Q-008 | Qual é o volume esperado de veículos por dia e de usuários simultâneos? | Valida as metas de escalabilidade e desempenho | Alexandre Rech | A definir |
| Q-009 | Nome e telefone do cliente devem ser obrigatórios? | Afeta contato em casos de dano, cobrança e LGPD | Operações/Jurídico | A definir |
| Q-010 | Haverá emissão fiscal no futuro próximo? | Pode antecipar integração e mudar o comprovante | Financeiro | A definir |
| Q-011 | Qual é o gatilho para a segunda unidade? | Define quando o modelo multiunidade entra no roadmap | Alexandre Rech | A definir |
| Q-012 | O negócio precisa de sessão formal de caixa com conferência de gaveta? | Pode adicionar controle de abertura/fechamento e sangria | Financeiro | A definir |
| Q-013 | O que fazer quando não há vaga livre e o cliente já chegou? | A premissa atual bloqueia o check-in; a operação pode exigir exceção | Operações | A definir |
| Q-014 | Quais notificações ao cliente são desejadas? | Define se SMS, e-mail ou WhatsApp entram na fase 2 | Operações | A definir |
| Q-015 | Qual é o prazo e o orçamento do MVP? | Necessário para cronograma e priorização | Alexandre Rech | A definir |

## 17. Decisões registradas

| Data | Decisão | Justificativa | Fonte |
| --- | --- | --- | --- |
| 2026-11-05 | MVP com operação completa: check-in, vagas, retirada, cobrança, relatórios, perfis e auditoria | A entrevista trata esses pontos como necessidades centrais | Entrevista + validação |
| 2026-11-05 | Diária em blocos de 24 h desde a entrada, com tolerância de 15 min | Padroniza o cálculo e reduz divergência entre operadores | Entrevista + validação |
| 2026-11-05 | Identificação por placa + código único de 6 caracteres | Permite localizar o veículo sem depender do ticket de papel | Entrevista + validação |
| 2026-11-05 | Pagamentos apenas registrados, sem gateway | O MVP precisa controlar o que foi pago, não processar a transação | Validação |
| 2026-11-05 | Perfis Administrador, Manobrista e Caixa | Separa operação, cobrança e gestão | Entrevista + validação |
| 2026-11-05 | Vaga individual por veículo | Resolve a dificuldade de localizar o carro e saber a ocupação | Entrevista + validação |
| 2026-11-05 | Ticket perdido: somente Administrador libera, sem taxa automática | Equilibra segurança e simplicidade; taxa não foi definida | Entrevista + validação |
| 2026-11-05 | Retenção de fotos e dados pessoais por 12 meses | Reduz exposição e atende à necessidade de contestação | Validação |
| 2026-11-05 | Interface somente para a equipe, em uma unidade | Mantém o MVP focado na operação interna | Entrevista + validação |
| 2026-11-05 | Tarifa única em BRL, com histórico de vigência | Simplifica o MVP e permite reajuste controlado | Validação |
| 2026-11-05 | Métrica principal: tempo médio de retirada | É a dor mais diretamente ligada à experiência do cliente | Entrevista + validação |
| 2026-11-05 | Tarifa registrada no check-in e aplicada no cálculo final | Evita alteração retroativa e disputa | Validação |
| 2026-11-05 | Entrega com saldo pendente exige autorização do Administrador | Preserva o controle financeiro sem bloquear a operação | Validação |
| 2026-11-05 | Sem emissão fiscal, LPR, offline ou app do cliente no MVP | Mantém o escopo viável e focado no fluxo operacional | Validação |

## 18. Aprovações

| Papel | Nome | Status | Data |
| --- | --- | --- | --- |
| Product Owner | A definir | Pendente | — |
| Stakeholder de negócio | Alexandre Rech | Pendente | — |
| Tech Lead | A definir | Pendente | — |
| Operações | A definir | Pendente | — |

## Apêndices

### A. Glossário

| Termo | Definição |
| --- | --- |
| Valet | Serviço em que a equipe recebe, estaciona e devolve o veículo ao cliente |
| Check-in | Registro de entrada do veículo no pátio |
| Check-out / entrega | Confirmação de saída e devolução do veículo ao cliente |
| Permanência | Período entre a entrada e a entrega do veículo |
| Diária | Bloco de 24 horas de permanência, contado da entrada |
| Tolerância | 15 minutos adicionais após cada bloco de 24 horas antes de iniciar nova diária |
| Vaga | Posição física de estacionamento |
| Código de retirada | Código único de 6 caracteres gerado no check-in |
| Manobrista | Funcionário que recebe, estaciona e busca veículos |
| Caixa | Funcionário que calcula, cobra e registra pagamentos |
| Divergência | Diferença entre o valor cobrado e o valor efetivamente pago |
| Trilha de auditoria | Registro imutável das ações críticas do sistema |
| Dado pessoal | Informação relacionada a pessoa natural identificada ou identificável, conforme a LGPD |
| Tarifa | Valor da diária configurado pelo Administrador |
| Comprovante não fiscal | Documento de conferência da cobrança, sem validade fiscal |

### B. Referências

- `docs/entrevista.md` — entrevista de levantamento de requisitos com Alexandre Rech.
- `README.md` — contexto do projeto e stack sinalizado.
- `.opencode/skills/generate-prd/references/prd-template.md` — estrutura utilizada para este PRD.

### C. Matriz de rastreabilidade

| Objetivo | Requisitos | Métrica |
| --- | --- | --- |
| OE-001 | RF-004, RF-005, RF-006, RF-007, RF-024 | M-002, M-006 |
| OE-002 | RF-008, RF-009, RF-010, RF-013 | M-004 |
| OE-003 | RF-014, RF-015, RF-016, RF-017, RF-018, RF-021 | M-003 |
| OE-004 | RF-011, RF-012, RF-013, RF-019 | M-001 |
| OE-005 | RF-019, RF-020, RF-021, RF-022, RF-023 | M-005 |
| OE-006 | RF-001, RF-002, RF-003, RF-024, RF-025 | M-006 |

### D. Registro de ambiguidades

| ID | Tipo | Impacto | Origem | Ação | Pergunta feita | Resposta/Premissa | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AMB-001 | Regra de negócio | Bloqueante | Documento | Perguntar | Como o valor é calculado a partir do tempo de permanência? | Blocos de 24 h com tolerância de 15 min | Resolvida |
| AMB-002 | Escopo | Bloqueante | Documento | Perguntar | Quais capacidades devem entrar no MVP? | Operação completa | Resolvida |
| AMB-003 | Comportamento | Bloqueante | Documento | Perguntar | Como identificar cliente e veículo no check-in? | Placa + código único | Resolvida |
| AMB-004 | Integração | Alto | Documento | Perguntar | O sistema processa ou apenas registra pagamentos? | Apenas registra | Resolvida |
| AMB-005 | Comportamento | Alto | Documento | Perguntar | Quais perfis de usuário o MVP deve ter? | Administrador, Manobrista e Caixa | Resolvida |
| AMB-006 | Comportamento | Médio | Inferência | Assumir | Quais são os estados da retirada? | Aguardando, em busca, entregue e cancelada | Premissa confirmada |
| AMB-007 | Dados | Alto | Documento | Perguntar | O sistema deve registrar a vaga exata? | Sim, vaga individual por veículo | Resolvida |
| AMB-008 | Regra de negócio | Alto | Documento | Perguntar | Qual a regra para ticket perdido? | Administrador confirma, sem taxa automática, com auditoria | Resolvida |
| AMB-009 | Métrica | Alto | Documento | Perguntar | Qual é a métrica principal? | Tempo médio de retirada | Resolvida |
| AMB-010 | Restrição | Alto | Documento | Perguntar | Por quanto tempo manter dados pessoais e fotos? | 12 meses | Resolvida |
| AMB-011 | Escopo | Médio | Documento | Perguntar | O MVP atende uma ou várias unidades? | Uma unidade | Resolvida |
| AMB-012 | Escopo | Médio | Inferência | Assumir | Haverá emissão fiscal no MVP? | Não; fica para fase futura | Premissa confirmada |
| AMB-013 | Escopo | Alto | Documento | Perguntar | Quem opera a interface no MVP? | Somente a equipe | Resolvida |
| AMB-014 | Regra de negócio | Alto | Documento | Perguntar | As tarifas variam por quê? | Tabela única em BRL | Resolvida |
| AMB-015 | Regra de negócio | Médio | Inferência | Assumir | A tarifa é a do check-in ou do check-out? | Registrada no check-in | Premissa confirmada |
| AMB-016 | Regra de negócio | Médio | Inferência | Assumir | Como tratar entrega com saldo pendente? | Alerta + autorização do Administrador | Premissa confirmada |
| AMB-017 | Escopo | Médio | Inferência | Assumir | O sistema opera offline? | Não; exige conexão | Premissa confirmada |
| AMB-018 | Escopo | Médio | Inferência | Assumir | Haverá reconhecimento de placa? | Não; digitação manual | Premissa confirmada |
| AMB-019 | Dados | Médio | Inferência | Assumir | Nome e telefone do cliente são obrigatórios? | Opcionais | Premissa confirmada |
| AMB-020 | Restrição | Médio | Inferência | Assumir | Qual fuso e moeda? | America/Sao_Paulo e BRL | Premissa confirmada |
| AMB-021 | Comportamento | Médio | Inferência | Assumir | Quantas fotos por evento? | Até 5 | Premissa confirmada |
| AMB-022 | Escopo | Baixo | Inferência | Assumir | O comprovante é impresso? | Opcional e não fiscal | Premissa confirmada |
| AMB-023 | Termo | Baixo | Inferência | Assumir | O que é uma diária? | Bloco de 24 h | Premissa confirmada |
| AMB-024 | Restrição | Médio | Inferência | Registrar em aberto | Qual o prazo de retenção financeira? | Financeiro pelo prazo legal; dados pessoais por 12 meses | Em aberto |
| AMB-025 | Métrica | Médio | Inferência | Registrar em aberto | Quais são baseline e metas de M-001 e M-003? | A definir com o negócio | Em aberto |
| AMB-026 | Restrição | Médio | Inferência | Registrar em aberto | Qual é o volume esperado? | A definir | Em aberto |
| AMB-027 | Regra de negócio | Médio | Inferência | Assumir | O que fazer sem vaga livre? | Bloquear o check-in e alertar | Premissa confirmada |
| AMB-028 | Escopo | Médio | Inferência | Assumir | Haverá sessão formal de caixa? | Não; relatórios diários | Premissa confirmada |
| AMB-029 | Escopo | Baixo | Inferência | Assumir | Qual formato de exportação? | CSV no MVP; PDF opcional | Premissa confirmada |
| AMB-030 | Restrição | Baixo | Inferência | Assumir | Qual stack do projeto? | .NET, SQL Server e Angular | Premissa confirmada |
| AMB-031 | Comportamento | Médio | Inferência | Assumir | É possível cancelar uma retirada? | Sim, com motivo e auditoria | Premissa confirmada |
| AMB-032 | Regra de negócio | Médio | Inferência | Assumir | É possível pagar acima do saldo? | Não; o sistema bloqueia | Premissa confirmada |

> Nenhuma ambiguidade com impacto **Bloqueante** permanece em aberto na versão final.
