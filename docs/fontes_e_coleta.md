# Fontes e Coleta dos Dados

## 1. Objetivo desta etapa

Esta etapa tem como objetivo identificar, avaliar e documentar os conjuntos de dados que serão utilizados no MVP. A seleção das bases considera sua relação com as perguntas de negócio, a confiabilidade da fonte, a disponibilidade pública, o formato, o período, a granularidade e a possibilidade de integração com outros conjuntos de dados.

Nenhum conjunto será incorporado definitivamente ao pipeline antes da análise de sua estrutura, qualidade e compatibilidade com os objetivos do projeto.

---

## 2. Critérios para seleção das bases

Os conjuntos de dados serão avaliados de acordo com os seguintes critérios:

- Relação com as perguntas de negócio.
- Origem oficial e confiável.
- Disponibilidade pública.
- Existência de documentação ou descrição dos campos.
- Formato adequado para processamento.
- Período de referência disponível.
- Frequência de atualização.
- Granularidade das informações.
- Presença de identificadores que permitam integração.
- Licença ou condições de utilização.
- Ausência de dados pessoais ou confidenciais.

---

## 3. Inventário preliminar das fontes

| Órgão | Conjunto de dados | Segmento | Utilização prevista | Situação |
|---|---|---|---|---|
| PREVIC | Estatística de Benefícios e População - EBP | Fechada | Analisar participantes ativos, aposentados, pensionistas, sexo e faixa etária | Confirmada |
| PREVIC | Montante Arrecadado | Fechada | Analisar contribuições recebidas pelas entidades e planos | Em validação |
| PREVIC | Demonstrativos Contábeis | Fechada | Identificar patrimônio, provisões e informações contábeis | Em validação |
| PREVIC | Demonstrativos Atuariais | Fechada | Identificar modalidade, características e premissas dos planos | Em validação |
| PREVIC | Cadastro de Entidades e Planos - CadPrevic | Fechada | Identificar entidades, planos, modalidades e localização das sedes | Em validação |
| SUSEP | Sistema de Estatísticas da SUSEP | Aberta | Analisar contribuições, resgates, provisões e evolução do mercado | Em validação |
| IBGE | PNAD Contínua e/ou SIDRA | Socioeconômico | Obter informações de renda, trabalho, idade e população dos estados do Sudeste | Em validação |

A indicação “Em validação” significa que a base ainda será examinada quanto aos campos, período, formato e granularidade antes de ser incorporada ao pipeline.

---

## 4. Fonte confirmada: PREVIC - EBP

### 4.1 Identificação

- **Órgão responsável:** Superintendência Nacional de Previdência Complementar - PREVIC.
- **Conjunto:** Estatística de Benefícios e População - EBP.
- **Segmento:** Previdência complementar fechada.
- **Página oficial:** https://www.gov.br/previc/pt-br/acesso-a-informacao-1/dados-abertos/estatistica-de-beneficio-e-populacao-ebp
- **Acesso:** público.
- **Situação no projeto:** fonte confirmada, aguardando seleção dos arquivos e inspeção das colunas.

### 4.2 Utilização prevista

A base será avaliada para responder às perguntas relacionadas ao perfil dos participantes da previdência complementar fechada, considerando:

- Participantes ativos.
- Aposentados.
- Beneficiários de pensão.
- Sexo.
- Faixa etária.
- Entidade e plano, quando disponíveis.
- Período de referência.

### 4.3 Limitações iniciais

A EBP representa participantes vinculados às entidades e aos planos de previdência fechada. Ela não representa toda a população brasileira e não deve ser interpretada como uma pesquisa domiciliar.

A localização da sede de uma entidade não representa necessariamente o estado de residência de seus participantes. Essa diferença será considerada nas análises territoriais.

---

## 5. Metadados da coleta

Durante a ingestão na camada Bronze, serão acrescentados, quando aplicável, os seguintes metadados:

| Campo | Descrição |
|---|---|
| `fonte_dado` | Órgão ou sistema de origem |
| `nome_arquivo_origem` | Nome original do arquivo coletado |
| `url_origem` | Endereço oficial da fonte |
| `data_coleta` | Data em que o arquivo foi obtido |
| `periodo_referencia` | Período ao qual os dados se referem |
| `formato_origem` | Formato original do arquivo |
| `data_processamento` | Data e hora da ingestão no Databricks |
| `camada` | Camada do pipeline em que o dado está armazenado |

---

## 6. Estratégia de coleta

A coleta seguirá as seguintes etapas:

1. Acessar a fonte oficial.
2. Registrar o endereço e a data de acesso.
3. Selecionar o período adequado para a análise.
4. Baixar o arquivo sem modificar seu conteúdo.
5. Registrar o nome e o formato original.
6. Armazenar o dado bruto na camada Bronze.
7. Acrescentar metadados de rastreabilidade.
8. Conferir se a quantidade de registros carregados corresponde ao arquivo original.
9. Preservar a fonte bruta para possibilitar reprocessamento e auditoria.

---

## 7. Licenças e condições de utilização

As licenças e condições de utilização serão verificadas individualmente nas páginas oficiais. O projeto utilizará exclusivamente dados públicos e agregados, mantendo a identificação da fonte e a data de acesso.

Não serão utilizados dados pessoais, confidenciais ou pertencentes a empresas sem autorização.

---

## 8. Registro das decisões

| Data | Decisão | Justificativa |
|---|---|---|
| [Preencher] | Utilizar dados da PREVIC, SUSEP e IBGE | As fontes permitem estudar os segmentos fechado, aberto e o contexto socioeconômico |
| [Preencher] | Não utilizar dados do INSS | O escopo está restrito à previdência complementar |
| [Preencher] | Manter aberta a definição final dos períodos | Os períodos comuns dependerão da disponibilidade e compatibilidade das bases |

Este documento será atualizado durante o desenvolvimento do MVP conforme os arquivos forem analisados e as decisões técnicas forem tomadas.
