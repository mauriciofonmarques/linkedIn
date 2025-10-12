Projeto de Detecção de Fraude em Cartões de Crédito

1. Visão Geral e Objetivo
Este projeto demonstra a construção de um modelo de Machine Learning para a detecção de transações fraudulentas em cartões de crédito. O objetivo principal foi desenvolver uma solução robusta, aplicando as melhores práticas da indústria para lidar com dados tabulares e, principalmente, com o desafio do extremo desbalanceamento de classes, uma característica intrínseca a problemas de fraude.

O dataset utilizado foi o "Credit Card Fraud", disponível no Kaggle. A grande vantagem desta base de dados é a sua interpretabilidade, com features claras como distance_from_home e online_order, permitindo uma análise mais conectada ao negócio.

------------------------------------------------------------------

2. Metodologia Passo a Passo
O projeto foi estruturado em quatro etapas principais, seguindo um fluxo de trabalho profissional de ciência de dados.

Etapa 1: Análise Exploratória dos Dados (EDA)
O primeiro passo foi entender profundamente os dados com os quais estávamos trabalhando.

Estrutura dos Dados: O dataset contém 1.000.000 de transações e 8 colunas, incluindo features numéricas contínuas (como distance_from_home e ratio_to_median_purchase_price) e features binárias (como repeat_retailer e online_order).

O Grande Desafio - Desbalanceamento: A análise revelou que apenas 8.74% das transações eram fraudulentas. Este é o principal desafio do projeto: criar um modelo que aprenda a identificar um evento extremamente raro sem ignorá-lo.


Etapa 2: Pré-processamento e Estruturação do Pipeline
Com os dados compreendidos, o próximo passo foi prepará-los para o modelo. Para garantir um processo limpo, robusto e reprodutível, utilizamos um Pipeline do Scikit-learn.

Escalonamento (StandardScaler): As features numéricas contínuas possuíam escalas muito diferentes. Aplicamos o StandardScaler para normalizá-las, garantindo que nenhuma feature dominasse o modelo apenas por ter valores maiores.

Organização com ColumnTransformer: Para aplicar o escalonamento apenas nas colunas necessárias e manter as colunas binárias intactas, utilizamos o ColumnTransformer. Esta ferramenta é essencial para organizar o pré-processamento em cenários com tipos de dados mistos.

Encapsulamento com Pipeline: Todo o fluxo de pré-processamento foi encapsulado junto com o modelo dentro de um Pipeline. Isso não apenas organiza o código, mas também previne vazamento de dados (data leakage), garantindo que as informações do conjunto de teste não influenciem o treinamento.


Etapa 3: Escolha e Treinamento do Modelo
A escolha do algoritmo é crucial para o sucesso do projeto.

Modelo: Optamos pelo LGBMClassifier (LightGBM), um algoritmo de Gradient Boosting conhecido por sua performance e eficiência com dados tabulares. É uma das ferramentas padrão da indústria para problemas de classificação.

Técnica para Desbalanceamento: Para "forçar" o modelo a prestar atenção na classe minoritária (fraude), utilizamos o parâmetro scale_pos_weight. Calculamos a proporção entre as classes normal e fraudulenta e usamos esse valor para dar um "peso" muito maior aos erros cometidos em transações fraudulentas. Isso instrui o modelo de que errar uma fraude é muito mais custoso do que errar uma transação normal.


Etapa 4: Validação Robusta com StratifiedKFold
Avaliar um modelo em dados desbalanceados com uma simples divisão de treino/teste pode levar a conclusões enganosas. Por isso, adotamos uma estratégia de validação mais robusta.

Validação Cruzada Estratificada: Utilizamos o StratifiedKFold com 5 "dobras" (splits). Essa técnica divide os dados 5 vezes, garantindo que a proporção de 8.74% de fraudes seja mantida em cada uma das divisões. O modelo é treinado e avaliado 5 vezes, e o resultado final é a média da performance, eliminando o fator sorte e fornecendo uma medida muito mais confiável da sua verdadeira capacidade.

------------------------------------------------------------------

3. Resultados e Conclusões
Os resultados obtidos foram extraordinários, validando a eficácia da metodologia aplicada.

Métrica

Resultado Médio

O que Significa?

Recall 
99.94%

O modelo foi capaz de identificar corretamente 99.94% de todas as fraudes que ocorreram. Esta é a métrica mais importante, pois minimiza as perdas financeiras.

Precisão
97.89%

Quando o modelo alertou que uma transação era fraudulenta, ele estava certo 97.89% das vezes. Isso minimiza o impacto negativo sobre clientes legítimos (falsos positivos).

ROC AUC
1.0000

O modelo demonstrou uma capacidade ótima de distinguir entre uma transação normal e uma fraudulenta.

------------------------------------------------------------------

Conclusão Final
Este projeto demonstrou com sucesso a construção de um sistema de detecção de fraudes de ponta a ponta. Embora o desempenho perfeito seja um indicativo de que o dataset é sintético (em um cenário real, os resultados seriam menores), a metodologia empregada é utilizada na indústria.
O uso de um Pipeline organizado, o tratamento correto do desbalanceamento de classes com scale_pos_weight e a validação robusta com StratifiedKFold são as técnicas que garantem a criação de um modelo confiável e de alto impacto para o negócio.

