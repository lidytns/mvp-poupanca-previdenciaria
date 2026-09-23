# Fontes e Coleta dos Dados

## 1. Objetivo

Este documento registra as fontes efetivamente utilizadas no MVP, os recortes adotados e as decisões de coleta. A seleção considerou a relação com as perguntas de negócio, a confiabilidade da fonte, a disponibilidade pública, o formato, o período, a granularidade e a possibilidade de integração entre as bases.

O projeto utiliza somente dados públicos e agregados da PREVIC, SUSEP e IBGE. Não foram utilizados dados pessoais ou confidenciais.

---

## 2. Critérios de seleção

As bases foram selecionadas de acordo com os seguintes critérios:

- relação com as perguntas de negócio;
- origem oficial e confiável;
- disponibilidade pública;
- existência de documentação ou descrição dos dados;
- formato adequado para processamento;
- período compatível com o escopo do projeto;
- granularidade adequada às análises;
- possibilidade de integração por ano e Unidade da Federação, quando aplicável;
- ausência de dados pessoais ou confidenciais.

---

## 3. Fontes utilizadas

| Órgão | Conjunto de dados | Segmento | Recorte utilizado | Finalidade | Situação |
|---|---|---|---|---|---|
| PREVIC | Estatística de População e Benefícios (EPB) | Previdência complementar fechada | Brasil, 2025 | Analisar participantes ativos, aposentados e beneficiários de pensão por sexo e faixa etária | Utilizada |
| PREVIC | Demonstração Estatística de Investimentos e de Planos (DSI) | Previdência complementar fechada | Brasil, 2025 | Identificar planos e entidades e apoiar a análise das movimentações | Utilizada |
| SUSEP | Dados estatísticos da previdência complementar aberta | Previdência complementar aberta | Espírito Santo, Minas Gerais, Rio de Janeiro e São Paulo, de 2012 a 2025 | Analisar contribuições, resgates, participantes e produtos previdenciários | Utilizada |
| IBGE/SIDRA | Tabela 7444 | Socioeconômico | Região Sudeste, de 2012 a 2025 | Obter o rendimento médio mensal por UF | Utilizada |
| IBGE/SIDRA | Tabela 6407 | Socioeconômico | Região Sudeste, de 2012 a 2025 | Obter a população por UF, sexo e faixa etária | Utilizada |

---

## 4. Arquivos da PREVIC

Foram processados os seguintes arquivos:

- `DSI_2025.csv`;
- `EPB_1SEMESTRE_2025.csv`;
- `EPB_2SEMESTRE_2025.csv`.

Os arquivos foram disponibilizados sem cabeçalhos descritivos. Por esse motivo, a camada Bronze preservou a estrutura original, enquanto a identificação, a seleção e a tipagem dos campos foram realizadas na camada Silver.

### Utilização

As bases da PREVIC foram utilizadas para:

- caracterizar participantes ativos, aposentados e beneficiários de pensão;
- analisar a distribuição por sexo e faixa etária;
- identificar entidades e planos;
- analisar movimentações e mudanças de entidade;
- controlar coberturas incompletas e diferenças de conciliação.

### Limitações

As bases representam participantes vinculados a entidades e planos de previdência complementar fechada. Elas não representam toda a população brasileira.

A localização da sede de uma entidade não corresponde necessariamente ao estado de residência dos participantes. Por isso, a previdência fechada foi analisada em âmbito nacional e não foi utilizada nas projeções por UF.

Página oficial: [PREVIC — Estatística de População e Benefícios (DE e DSI)](https://www.gov.br/previc/pt-br/sistemas/informacoes-sobre-os-sistemas-previc/estatistica-de-populacao-e-beneficios-de-e-dsi)

---

## 5. Dados da SUSEP

Os dados da SUSEP foram utilizados para analisar a previdência complementar aberta nos quatro estados da Região Sudeste entre 2012 e 2025.

As bases incluem informações agregadas sobre:

- contribuições;
- resgates;
- benefícios pagos;
- quantidade de participantes;
- entidades;
- produtos previdenciários.

Na camada Silver, foram realizados a padronização dos campos, a conversão dos valores monetários e o tratamento das diferentes codificações encontradas nos arquivos. Na camada Gold, os dados foram agregados por UF, ano e produto.

O PGBL foi escolhido como produto de referência para as simulações de patrimônio, renda mensal e taxa de reposição.

Página oficial: [SUSEP — Dados abertos](https://www.gov.br/susep/pt-br/acesso-a-informacao/dados-abertos)

---

## 6. Dados do IBGE/SIDRA

Foram utilizadas duas tabelas do Sistema IBGE de Recuperação Automática (SIDRA):

- [Tabela 7444 — rendimento médio mensal](https://sidra.ibge.gov.br/tabela/7444);
- [Tabela 6407 — população por sexo e grupos de idade](https://sidra.ibge.gov.br/tabela/6407).

Os arquivos do IBGE possuíam títulos, cabeçalhos em múltiplas linhas, notas metodológicas e estrutura em formato largo. A camada Bronze preservou os arquivos recebidos. Na camada Silver, os dados foram convertidos para formato longitudinal e padronizados por ano, UF, sexo e faixa etária, conforme aplicável.

Os indicadores do IBGE foram integrados aos dados da SUSEP por UF e ano para contextualizar as análises da previdência aberta e calcular o percentual da contribuição média sobre o rendimento de referência.

---

## 7. Metadados e rastreabilidade

Durante a ingestão na camada Bronze, foram acrescentados, quando aplicável, os seguintes metadados:

| Campo | Descrição |
|---|---|
| `_arquivo_origem` | Nome ou caminho do arquivo coletado |
| `_data_ingestao` | Data e hora da ingestão no Databricks |

Além desses campos, os notebooks e esta documentação registram a fonte, o período de referência, as regras de transformação e as limitações de cada conjunto.

---

## 8. Processo de coleta e ingestão

O processo adotado foi:

1. acessar a fonte oficial;
2. selecionar o período compatível com o projeto;
3. baixar os arquivos sem alterar seu conteúdo;
4. registrar o nome e a origem dos arquivos;
5. armazenar os dados brutos no volume do Databricks;
6. carregar os arquivos na camada Bronze;
7. acrescentar metadados de rastreabilidade;
8. comparar as quantidades de registros antes e depois da gravação;
9. preservar as fontes brutas para permitir reprocessamento e auditoria;
10. tratar e validar os dados nas camadas Silver e Gold.

As gravações utilizaram formato Delta e sobrescrita controlada, permitindo a reexecução do pipeline sem duplicação de registros.

---

## 9. Decisões finais

| Decisão | Justificativa |
|---|---|
| Utilizar dados da PREVIC, SUSEP e IBGE | As fontes permitem analisar a previdência complementar fechada, a previdência aberta e o contexto socioeconômico |
| Não utilizar dados do INSS | O escopo está restrito à previdência complementar |
| Analisar a previdência fechada em âmbito nacional | As bases utilizadas não permitem identificar com segurança a residência dos participantes por UF |
| Aplicar as projeções somente à previdência aberta | Os dados agregados da SUSEP e do IBGE possuem recorte compatível por UF e ano |
| Utilizar o PGBL como produto de referência | O produto possui aderência ao objetivo de simular acumulação e renda complementar |
| Utilizar 2025 como ano de referência das projeções | É o período final comum adotado nas bases utilizadas |
| Adotar horizontes de 10, 20 e 30 anos | Os horizontes permitem comparar o efeito do tempo de acumulação |
| Adotar cenários reais de 2%, 4% e 6% ao ano | Os cenários conservador, base e otimista permitem comparar a sensibilidade dos resultados à rentabilidade |

---

## 10. Condições de utilização

O projeto utiliza exclusivamente dados públicos e agregados, mantendo a identificação das fontes. Os resultados possuem finalidade acadêmica e não devem ser interpretados como previsões individuais, garantias de benefício ou recomendações financeiras.
