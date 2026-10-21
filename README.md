**INTEGRANTES:** 
* Igor Luís Galo
* Yuri Codreanski Collinett Guimarães
* Rafael Shivaom Gonçalves Rodrigues
* Vinícius de Paiva Leles

# Segmentação de Clientes de Cartão de Crédito com Aprendizado Não Supervisionado

**UFSJ** || Inteligência Artificial || Profa. DSc. Érica C.R. Carvalho

---

## Visão Geral do Projeto
O projeto aborda o problema de **segmentação comportamental de clientes de cartão de crédito** utilizando técnicas de **Aprendizado de Máquina Não Supervisionado (Clustering)**. 

### Contexto e Relevância
Identificar agrupamentos naturais em bases de dados de clientes (analisando fatores como hábitos de consumo, endividamento e liquidação substancialmente distintos, etc.) sem a necessidade de rotulação prévia possibilita:
- Concessão de limites de crédito proporcionaisao risco do usuário;
- Campanhas de marketing e benefícios direcionados (ex.: recompensas por compras pontuais vs. incentivo a parcelamentos);
- Identificação precoce de comportamento de risco (ex.: dependência de saques rotativos e pagamento mínimo de faturas);
- Ações de retenção para clientes de alta renda e uso frequente.

---

## Conjunto de Dados
**FONTE:** Credit Card Dataset for Clustering (Kaggle);
**ARQUIVO:** `CC GENERAL.csv`;
**INSTÂNCIAS:** 8.950 clientes;
**DIMENSIONALIDADE:** 18 colunas originais (1 chave de identificação e 17 atributos comportamentais contínuos).

### Principais atributos:
- `BALANCE`: Saldo devedor remanescente na conta para compras;
- `BALANCE_FREQUENCY`: Frequência de atualização do saldo (entre 0 e 1);
- `PURCHASES`: Volume financeiro total de compras realizadas;
- `ONEOFF_PURCHASES` / `INSTALLMENTS_PURCHASES`: Gastos divididos entre compras à vista e compras parceladas;
- `CASH_ADVANCE`: Volume financeiro sacado como adiantamento de dinheiro;
- `PURCHASES_FREQUENCY`/ `CASH_ADVANCE_FREQUENCY`: Frequência de compras e saques;
- `CREDIT_LIMIT`: Limite de crédito total concedido ao cliente;
- `PAYMENTS`: Valor total liquidado pelo cliente no período de observação;
- `MINIMUM_PAYMENTS`: Pagamento mínimo registrado;
- `PRC_FULL_PAYMENT`: Percentual de meses em que a fatura foi liquidada integralmente;
- `TENURE`: Tempo de serviço do cartão (em meses).

---

## Etapas de Pré-Processamento Implementadas

### Eliminação manual de atributos
**AÇÃO:** Remoção da coluna `CUST_ID`. </br>
**JUSTIFICATIVA:** O identificador exclusivo do cliente constitui uma chave artificial sem padrão estatístico generalizável. Sua permanência distorce métricas de distância geométrica (como a Distância Euclidiana).

### Limpeza de Dados (Tratamento de Dados Faltantes)
**AÇÃO:** Identificação de nulos em `CREDIT_LIMIT` (1 registro) e `MINIMUM_PAYMENTS`.</br>
**JUSTIFICATIVA:** Ambas as variáveis apresentam distribuições assimétricas com caudas longas (outiliers). A mediana preserva a integridade volumétrica da base sem sofrer as distorções que a média provocaria.

### Amostragem de Dados
**AÇÃO:** Extração de subconjunto representativo de 30% da base via amostragem aleatória simples (`random_state=42`).</br>
**JUSTIFICATIVA:** Demonstração prática do uso de amostragens para testes rápidos de prototipação e redução de esforço computacional em grandes volumes de dados.

### Transformação de Dados (Padronização)
**AÇÃO:** Aplicação do StandardScaler do scikit-learn.</br>
**JUSTIFICATIVA:** Algoritmos de agrupamento dependem da distância entre vetores no espaço euclidiano. Variáveis na ordem de dezenas de milhares (ex.: `CREDIT_LIMIT`) sobrepujariam variáveis limitadas ao intervalo $[0, 1]$ (ex.: `PURCHASES_FREQUENCY`). A padronização centraliza cada atributo na média $\mu = 0$ com desvio padrão unitário $\sigma = 1

### Redução de Dimensionalidade (PCA)
**AÇÃO:** Projeção linear do espaço de 17 atributos para 2 Componentes Principais (`n_components=2`).</br>
**JUSTIFICATIVA:** Além de condensar as direções de máxima variância global dos dados, viabiliza a inspeção visual da distribuição de densidade em gráficos de dispersão bidimensionais.

---

## Próximos Passos
0. Pré-Processamento Concluído;
1. Otimização de Hiperparâmetros;
    - Método do Cotovelo;
    - Coeficiente de Silhueta para k de 2 a 10.
2. Treinamento de Modelagem
    - Modelo Principal: K-Means com K ótimo;
    - Modelo Comparativo: DBSCAN ou Agrupamento Hierárquico.
3. Interpretação Semântica dos Perfis (Personas)
4. Finalização dos Entregáveis;
    - Relatório Técnico em PDF;
    - Estruturação dos Slides de Apresentação.

---  

## Instruções de Instalação e Execução
**PRÉ-REQUISITOS:**
- Python 3.10+;
- Jupyter Notebook.

### Configuração do Ambiente Virtual
1. Clone o repositório:
    ```
    git clone <URL_DO_REPOSITORIO>
    cd Trabalho-Pratico-IA
    ```
2. Crie e ative o ambiente virtual:
    ```
    python3 -m venv venv
    source venv/bin/activate
    ```
3. Instale as dependências:
    ```
    pip install --upgrade pip
    pip install notebook ipykernel pandas numpy scikit-learn matplotlib seaborn
    ```
4. Registre o kernel no Jupyter:
    ```
    python -m ipykernel install --user --name=venv-tp1 --display-name "Python (venv-tp1)"

    ```
5. Execute o Jupyter Notebook:
    ```
    jupyter notebook pre_processamento_tp1.ipynb

    ```
