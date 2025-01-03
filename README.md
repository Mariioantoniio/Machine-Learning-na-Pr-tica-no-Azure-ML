# Machine-Learning-na-Prtica-no-Azure-ML
# Explorando o Automated Machine Learning no Azure Machine Learning

Este guia descreve como utilizar o recurso de aprendizado de máquina automatizado (AutoML) no Azure Machine Learning para treinar, avaliar, implantar e testar um modelo. Este exercício deve levar cerca de 35 minutos para ser concluído.

---

## Pré-requisitos

1. **Conta no Azure**: Certifique-se de possuir uma conta ativa no Azure.
2. **Azure CLI instalado**: [Guia de instalação](https://learn.microsoft.com/cli/azure/install-azure-cli).
3. **Conceitos básicos em Machine Learning**.

---

## Passo a Passo

### 1. Criar um Workspace do Azure Machine Learning

1. Acesse o [portal do Azure](https://portal.azure.com) e faça login.
2. Selecione **+ Criar um recurso**, procure por **Machine Learning** e crie um novo recurso com as configurações:
   - **Assinatura**: Sua assinatura do Azure.
   - **Grupo de recursos**: Crie ou selecione um grupo de recursos.
   - **Nome**: Nome único para seu workspace.
   - **Região**: East US.
   - **Conta de armazenamento, Key Vault e Application Insights**: Deixe as opções padrão.
3. Clique em **Revisar + criar** e, em seguida, **Criar**. Aguarde a criação do workspace.

4. No **painel de visão geral** do recurso criado, clique em **Grupo de recursos**.
5. No menu à esquerda, selecione **Controle de acesso (IAM)** e atribua a função de administrador:
   - Selecione **Adicionar atribuição de função**.
   - Procure por **Microsoft.MachineLearningServices/workspaces/datastores/listsecrets/action**.
   - Escolha **Azure AI Administrator** e adicione seu usuário.
   - Clique em **Revisar + Atribuir**.

### 2. Iniciar o Azure Machine Learning Studio

1. No recurso do workspace, clique em **Launch studio** ou acesse [Azure ML Studio](https://ml.azure.com).
2. No ML Studio, selecione o workspace recém-criado.

### 3. Configurar o AutoML para Treinar um Modelo

1. No ML Studio, navegue até **Automated ML** na seção **Authoring**.
2. Crie um novo trabalho de AutoML com as configurações:

   - **Nome do trabalho**: Use o valor padrão.
   - **Novo experimento**: `mslearn-bike-rental`
   - **Descrição**: `Automated machine learning for bike rental prediction`

3. Configure o dataset:
   - Crie um novo dataset:
     - **Nome**: `bike-rentals`
     - **Descrição**: `Historic bike rental data`
     - **Tipo**: Tabela (mltable)
     - **Fonte**: Arquivo local (baixe os dados [aqui](https://aka.ms/bike-rentals)).
     - Armazene em `workspaceblobstore`.

4. Configure o tipo de tarefa:
   - **Tipo de tarefa**: Regressão.
   - **Coluna alvo**: `rentals`.
   - **Modelos permitidos**: Apenas `RandomForest` e `LightGBM`.

5. Ajuste as configurações:
   - **Número máximo de tentativas**: 3.
   - **Timeout do experimento**: 15 minutos.
   - **Habilitar terminação antecipada**: Ativado.

6. Configure a computação:
   - **Tipo**: Serverless.
   - **Máquina virtual**: Standard_DS3_V2.

7. Submeta o trabalho e aguarde a conclusão.

### 4. Revisar o Melhor Modelo

1. Após a conclusão, revise os detalhes do melhor modelo na aba **Overview** do trabalho de AutoML.
2. Visualize os gráficos de desempenho:
   - **Residuais**: Diferença entre valores previstos e reais.
   - **Predicted vs True**: Comparação entre valores previstos e reais.

### 5. Implantar e Testar o Modelo

1. Na aba **Model** do melhor modelo, selecione **Deploy** e configure:
   - **Máquina virtual**: Standard_DS3_V2.
   - **Contagem de instâncias**: 3.
   - **Endpoint**: Crie um novo endpoint com nome exclusivo.
   - **Inferencing data collection** e **Package model**: Desativados.

Aguarde a conclusão da implantação.



