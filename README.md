MVP – Poupança Previdenciária Complementar
Projeção da taxa de reposição de renda com enfoque na Região Sudeste

Aluno(a): Lidiane Nunes da Silva Celestino
Curso: Ciência de dados e Analytics
Instituição: PUC-Rio
Plataforma: Databricks Free Edition
Status: Em desenvolvimento

Sumário
Introdução
Descrição do problema
Objetivos
Perguntas de negócio
Escopo do projeto
Fontes e coleta dos dados
Arquitetura do pipeline
Modelagem dos dados
Processo de carga e transformação
Análise da qualidade dos dados
Análise e solução do problema
Simulação da taxa de reposição
Memória de cálculo
Conclusão
Autoavaliação
Trabalhos futuros
Como reproduzir o projeto
Referências

1. Introdução

A previdência complementar é um instrumento de acumulação de recursos destinado à formação de uma renda adicional para a aposentadoria. No Brasil, ela está dividida em dois segmentos: previdência complementar aberta, acessível ao público em geral, e previdência complementar fechada, destinada a grupos vinculados a empresas, associações ou outras entidades instituidoras.

Este projeto busca aplicar conceitos de Engenharia de Dados na construção de um pipeline em nuvem, utilizando dados públicos para analisar a poupança previdenciária complementar brasileira e estimar seu potencial de reposição da renda durante a aposentadoria.

2. Descrição do problema

A manutenção do padrão de vida durante a aposentadoria depende, entre outros fatores, da capacidade de acumulação de recursos ao longo da vida profissional. Nesse contexto, a previdência complementar aberta e fechada representa uma alternativa para a formação de renda futura adicional.

Entretanto, apenas conhecer o volume de contribuições ou o patrimônio acumulado não permite compreender se o nível atual de poupança será suficiente para preservar a renda dos participantes na aposentadoria.

Por isso, este MVP pretende analisar dados públicos da previdência complementar brasileira e indicadores socioeconômicos da Região Sudeste, estimando, por meio de simulações, qual percentual da renda atual poderá ser reposto futuramente caso sejam mantidos determinados níveis de contribuição.

O trabalho não incluirá benefícios ou contribuições do Regime Geral de Previdência Social – INSS. Os resultados representarão exclusivamente a renda potencial proporcionada pela previdência complementar.

As projeções serão apresentadas como cenários estimados, e não como garantias individuais de benefício futuro. Todas as premissas e fórmulas utilizadas serão documentadas para permitir a conferência e reprodução dos cálculos.

3. Objetivos
   
3.1 Objetivo geral

Construir um pipeline de dados na nuvem que integre informações públicas da previdência complementar aberta e fechada com indicadores socioeconômicos, permitindo analisar o nível atual de poupança previdenciária e simular a taxa potencial de reposição de renda na aposentadoria, com enfoque na Região Sudeste.

3.2 Objetivos específicos
Coletar dados públicos da PREVIC, SUSEP e IBGE.
Documentar as fontes, formas de coleta, períodos e licenças de utilização.
Armazenar os dados brutos na camada Bronze.
Limpar, validar e padronizar os dados na camada Silver.
Construir tabelas analíticas na camada Gold.
Analisar participantes, contribuições, patrimônio e características dos planos.
Comparar os segmentos aberto e fechado apenas nos indicadores compatíveis.
Examinar diferenças socioeconômicas entre Espírito Santo, Minas Gerais, Rio de Janeiro e São Paulo.
Projetar o patrimônio futuro em diferentes cenários.
Converter o patrimônio projetado em renda mensal estimada.
Calcular a taxa de reposição proporcionada pela previdência complementar.
Documentar as premissas, fórmulas e memórias de cálculo.
Identificar limitações dos dados e possibilidades de trabalhos futuros.

4. Perguntas de negócio
Como evoluíram as contribuições e os recursos acumulados na previdência complementar?
Qual é a participação dos segmentos aberto e fechado no sistema de previdência complementar?
Qual é o perfil dos participantes da previdência fechada por idade, sexo e situação no plano?
Como renda, idade e situação de trabalho diferem entre os estados da Região Sudeste?
Qual patrimônio poderá ser acumulado caso determinados níveis de contribuição sejam mantidos?
Qual renda mensal complementar esse patrimônio poderá proporcionar na aposentadoria?
Qual percentual da renda de referência poderá ser reposto pela previdência complementar?
Como a taxa de reposição se altera conforme idade, contribuição, rentabilidade e prazo de acumulação?
Qual contribuição seria necessária para alcançar metas de reposição de 40%, 60% ou 80%?
Quais limitações dos dados impedem estimar individualmente a situação de toda a população?

As perguntas que não puderem ser respondidas serão mantidas e discutidas na conclusão e na autoavaliação, conforme a disponibilidade e a granularidade das bases encontradas.

5. Escopo do projeto
Incluído no escopo
Previdência complementar aberta.
Previdência complementar fechada.
Indicadores socioeconômicos da Região Sudeste.
Dados agregados e públicos.
Simulações financeiras por perfis e cenários.
Taxa de reposição gerada exclusivamente pela previdência complementar.
Fora do escopo
Benefícios e contribuições do INSS.
Previsões individualizadas.
Recomendações pessoais de investimento.
Garantia de rentabilidade ou benefício futuro.
Utilização de dados pessoais ou confidenciais.

A localização da sede de uma entidade de previdência fechada não será interpretada automaticamente como local de residência de seus participantes. Essa limitação será observada nas análises territoriais.

6. Fontes e coleta dos dados

As fontes inicialmente previstas são:

Fonte	Segmento	Informações esperadas
PREVIC	Previdência fechada	Entidades, planos, participantes, contribuições, benefícios, patrimônio e investimentos
SUSEP	Previdência aberta	Contribuições, resgates, provisões e informações de mercado
IBGE	Socioeconômico	População, idade, rendimento e situação de trabalho

Os conjuntos de dados, períodos, endereços de acesso, formatos, licenças e datas de coleta serão definidos após a análise de disponibilidade e granularidade.

7. Arquitetura do pipeline

O projeto utilizará a Arquitetura Medalhão:

flowchart LR
    A[Fontes oficiais] --> B[Bronze: dados brutos]
    B --> C[Silver: dados tratados]
    C --> D[Gold: dados analíticos]
    D --> E[Análises e simulações]
Camada Bronze

Armazenará os dados conforme recebidos das fontes oficiais, preservando sua estrutura original e acrescentando metadados de controle da ingestão.

Camada Silver

Conterá os dados limpos, tipados, padronizados, deduplicados e submetidos às regras de qualidade.

Camada Gold

Conterá as tabelas e indicadores preparados para responder às perguntas de negócio e alimentar as simulações da taxa de reposição.

8. Modelagem dos dados

Esta seção apresentará o modelo de dados, as tabelas, chaves, relacionamentos, granularidades, regras de negócio, linhagem e o catálogo de dados.

Status: aguardando a seleção e análise das bases.

9. Processo de carga e transformação

Esta seção documentará os procedimentos de extração, carga, limpeza, padronização, integração e persistência dos dados no Databricks.

Status: aguardando o início da implementação.

10. Análise da qualidade dos dados

A qualidade será avaliada por atributo, considerando:

Valores nulos.
Registros duplicados.
Tipos de dados incorretos.
Valores mínimos e máximos.
Categorias inesperadas.
Valores negativos ou incompatíveis.
Consistência entre períodos.
Integridade das chaves.
Compatibilidade entre fontes.

Status: aguardando a ingestão dos dados.

11. Análise e solução do problema

Esta seção apresentará os resultados técnicos e a discussão de cada pergunta de negócio.

Status: aguardando a construção das camadas Silver e Gold.

12. Simulação da taxa de reposição

A taxa de reposição será calculada pela relação entre a renda mensal complementar projetada e a renda mensal utilizada como referência:

[
Taxa\ de\ reposição =
\frac{Renda\ complementar\ mensal\ projetada}
{Renda\ mensal\ de\ referência}
\times 100
]

Serão avaliados cenários conservador, moderado e otimista, considerando diferentes taxas reais de rentabilidade, idades, prazos de acumulação, contribuições e períodos de recebimento.

13. Memória de cálculo

Todas as simulações apresentarão:

Dados de entrada.
Fonte de cada dado.
Premissas adotadas.
Fórmulas utilizadas.
Resultados intermediários.
Patrimônio final projetado.
Renda mensal estimada.
Taxa de reposição calculada.
Identificação do cenário.
Data de processamento.

A memória completa será adicionada após a validação das bases e das premissas.

14. Conclusão

A conclusão apresentará uma síntese dos resultados, as respostas obtidas, os objetivos atingidos e as limitações identificadas.

Status: será elaborada após a conclusão das análises.

15. Autoavaliação

A autoavaliação discutirá:

Objetivos alcançados.
Perguntas respondidas e não respondidas.
Principais dificuldades.
Soluções adotadas.
Conhecimentos adquiridos.
Evolução durante o projeto.
Limitações do MVP.
Possibilidades de aprimoramento.

As dificuldades serão registradas durante todo o desenvolvimento para que esta seção represente fielmente a experiência de construção do projeto.

16. Trabalhos futuros

Esta seção indicará novas bases, métricas, funcionalidades e análises que poderão enriquecer o projeto futuramente.

17. Como reproduzir o projeto

Ao final, esta seção apresentará as instruções necessárias para executar os notebooks, carregar as bases e reconstruir as tabelas e resultados.

18. Referências

As fontes oficiais, documentos técnicos e referências metodológicas serão registradas com seus respectivos endereços e datas de acesso.
