# MVP - Poupança Previdenciária Complementar

## Projeção da taxa de reposição de renda com enfoque na Região Sudeste

**Aluno(a):** Lidiane Nunes da Silva Celestino<br>
**Curso:** Ciências de Dados e Analytics<br>
**Instituição:** PUC-Rio<br>
**Plataforma:** Databricks Free Edition<br>
**Status:** Em desenvolvimento

---

## Sumário

1. [Introdução](#1-introdução)
2. [Descrição do problema](#2-descrição-do-problema)
3. [Objetivos](#3-objetivos)
4. [Perguntas de negócio](#4-perguntas-de-negócio)
5. [Escopo do projeto](#5-escopo-do-projeto)
6. [Fontes e coleta dos dados](#6-fontes-e-coleta-dos-dados)
7. [Arquitetura do pipeline](#7-arquitetura-do-pipeline)
8. [Modelagem dos dados](#8-modelagem-dos-dados)
9. [Processo de carga e transformação](#9-processo-de-carga-e-transformação)
10. [Análise da qualidade dos dados](#10-análise-da-qualidade-dos-dados)
11. [Análise e solução do problema](#11-análise-e-solução-do-problema)
12. [Simulação da taxa de reposição](#12-simulação-da-taxa-de-reposição)
13. [Memória de cálculo](#13-memória-de-cálculo)
14. [Conclusão](#14-conclusão)
15. [Autoavaliação](#15-autoavaliação)
16. [Trabalhos futuros](#16-trabalhos-futuros)
17. [Como reproduzir o projeto](#17-como-reproduzir-o-projeto)
18. [Referências](#18-referências)

---

## 1. Introdução

A previdência complementar é um instrumento de acumulação de recursos destinado à formação de uma renda adicional para a aposentadoria. No Brasil, ela está dividida em dois segmentos: previdência complementar aberta, acessível ao público em geral, e previdência complementar fechada, destinada a grupos vinculados a empresas, associações ou outras entidades instituidoras.

Este projeto aplica conceitos de Engenharia de Dados na construção de um pipeline em nuvem, utilizando dados públicos para analisar a poupança previdenciária complementar brasileira e estimar seu potencial de reposição da renda durante a aposentadoria.

## 2. Descrição do problema

A manutenção do padrão de vida durante a aposentadoria depende, entre outros fatores, da capacidade de acumulação de recursos ao longo da vida profissional. Entretanto, apenas conhecer o volume de contribuições ou o patrimônio acumulado não permite compreender se o nível atual de poupança será suficiente para preservar a renda dos participantes na aposentadoria.

Este MVP pretende analisar dados públicos da previdência complementar brasileira e indicadores socioeconômicos da Região Sudeste. Por meio de simulações, será estimado qual percentual da renda atual poderá ser reposto futuramente caso sejam mantidos determinados níveis de contribuição.

O trabalho não incluirá benefícios ou contribuições do Regime Geral de Previdência Social - INSS. Os resultados representarão exclusivamente a renda potencial proporcionada pela previdência complementar.

As projeções serão apresentadas como cenários estimados, e não como garantias individuais de benefício futuro. Todas as premissas e fórmulas serão documentadas para permitir a conferência e reprodução dos cálculos.

## 3. Objetivos

### 3.1 Objetivo geral

Construir um pipeline de dados na nuvem que integre informações públicas da previdência complementar aberta e fechada com indicadores socioeconômicos, permitindo analisar o nível atual de poupança previdenciária e simular a taxa potencial de reposição de renda na aposentadoria, com enfoque na Região Sudeste.

### 3.2 Objetivos específicos

- Coletar dados públicos da PREVIC, SUSEP e IBGE.
- Documentar fontes, formas de coleta, períodos e licenças.
- Armazenar os dados brutos na camada Bronze.
- Limpar, validar e padronizar os dados na camada Silver.
- Construir tabelas analíticas na camada Gold.
- Analisar participantes, contribuições, patrimônio e características dos planos.
- Comparar os segmentos aberto e fechado somente em indicadores compatíveis.
- Examinar diferenças entre Espírito Santo, Minas Gerais, Rio de Janeiro e São Paulo.
- Projetar o patrimônio futuro em diferentes cenários.
- Converter o patrimônio projetado em renda mensal estimada.
- Calcular a taxa de reposição proporcionada pela previdência complementar.
- Documentar premissas, fórmulas e memórias de cálculo.
- Identificar limitações e possibilidades de trabalhos futuros.

## 4. Perguntas de negócio

1. Como evoluíram as contribuições e os recursos acumulados na previdência complementar?
2. Qual é a participação dos segmentos aberto e fechado?
3. Qual é o perfil dos participantes da previdência fechada por idade, sexo e situação no plano?
4. Como renda, idade e situação de trabalho diferem entre os estados do Sudeste?
5. Qual patrimônio poderá ser acumulado caso determinados níveis de contribuição sejam mantidos?
6. Qual renda mensal complementar esse patrimônio poderá proporcionar na aposentadoria?
7. Qual percentual da renda de referência poderá ser reposto pela previdência complementar?
8. Como a taxa de reposição muda conforme idade, contribuição, rentabilidade e prazo?
9. Qual contribuição seria necessária para alcançar metas de reposição de 40%, 60% ou 80%?
10. Quais limitações impedem estimar individualmente a situação de toda a população?

As perguntas que não puderem ser respondidas serão mantidas e discutidas na conclusão e na autoavaliação, conforme a disponibilidade e a granularidade das bases encontradas.

## 5. Escopo do projeto

### Incluído no escopo

- Previdência complementar aberta e fechada.
- Indicadores socioeconômicos da Região Sudeste.
- Dados agregados e públicos.
- Simulações financeiras por perfis e cenários.
- Taxa de reposição gerada exclusivamente pela previdência complementar.

### Fora do escopo

- Benefícios e contribuições do INSS.
- Previsões individualizadas.
- Recomendações pessoais de investimento.
- Garantias de rentabilidade ou benefício futuro.
- Dados pessoais ou confidenciais.

A localização da sede de uma entidade fechada não será interpretada automaticamente como local de residência dos participantes. Essa limitação será observada nas análises territoriais.

## 6. Fontes e coleta dos dados

| Fonte | Segmento | Informações esperadas |
|---|---|---|
| PREVIC | Previdência fechada | Entidades, planos, participantes, contribuições, benefícios, patrimônio e investimentos |
| SUSEP | Previdência aberta | Contribuições, resgates, provisões e informações de mercado |
| IBGE | Socioeconômico | População, idade, rendimento e situação de trabalho |

Os conjuntos de dados, períodos, endereços, formatos, licenças e datas de coleta serão definidos após a análise de disponibilidade e granularidade.

## 7. Arquitetura do pipeline

O projeto utilizará a Arquitetura Medalhão:

```mermaid
flowchart LR
    A[Fontes oficiais] --> B[Bronze: dados brutos]
    B --> C[Silver: dados tratados]
    C --> D[Gold: dados analíticos]
    D --> E[Análises e simulações]
```

- **Bronze:** dados brutos preservados como recebidos.
- **Silver:** dados limpos, tipados, padronizados e validados.
- **Gold:** tabelas e indicadores preparados para análises e simulações.

## 8. Modelagem dos dados

Esta seção apresentará tabelas, chaves, relacionamentos, granularidades, regras de negócio, linhagem e catálogo de dados.

**Status:** aguardando a seleção e análise das bases.

## 9. Processo de carga e transformação

Esta seção documentará extração, carga, limpeza, padronização, integração e persistência no Databricks.

**Status:** aguardando o início da implementação.

## 10. Análise da qualidade dos dados

A qualidade será avaliada por atributo, considerando valores nulos, duplicidades, tipos incorretos, limites esperados, categorias inesperadas, integridade das chaves e compatibilidade entre fontes.

**Status:** aguardando a ingestão dos dados.

## 11. Análise e solução do problema

Esta seção apresentará os resultados técnicos e a discussão de cada pergunta de negócio.

**Status:** aguardando a construção das camadas Silver e Gold.

## 12. Simulação da taxa de reposição

**Taxa de reposição = (renda complementar mensal projetada / renda mensal de referência) x 100**

Serão avaliados cenários conservador, moderado e otimista, considerando diferentes rentabilidades reais, idades, prazos, contribuições e períodos de recebimento.

## 13. Memória de cálculo

Cada simulação apresentará dados de entrada e suas fontes, premissas, fórmulas, resultados intermediários, patrimônio final, renda estimada, taxa de reposição, cenário e data de processamento.

## 14. Conclusão

A conclusão apresentará uma síntese dos resultados, respostas obtidas, objetivos atingidos e limitações identificadas.

## 15. Autoavaliação

A autoavaliação discutirá objetivos alcançados, perguntas respondidas, dificuldades, soluções adotadas, conhecimentos adquiridos, evolução durante o projeto, limitações e possibilidades de aprimoramento.

As dificuldades serão registradas durante todo o desenvolvimento para que esta seção represente fielmente a experiência do projeto.

## 16. Trabalhos futuros

Esta seção indicará novas bases, métricas, funcionalidades e análises que poderão enriquecer o projeto.

## 17. Como reproduzir o projeto

Ao final, esta seção apresentará as instruções para executar os notebooks, carregar as bases e reconstruir tabelas e resultados.

## 18. Referências

As fontes oficiais, documentos técnicos e referências metodológicas serão registrados com seus endereços e datas de acesso.
