# Azure_ML
Trabalhando com Machine Learning na Pártica no Azure ML


## Configuração do Azure Machine Learning

Para começar, siga as instruções abaixo:

1. Acesse o portal Azure em [https://portal.azure.com](https://portal.azure.com) usando suas credenciais da Microsoft.

2. Selecione **+ Criar um recurso**, pesquise por **Machine Learning** e crie um novo recurso Azure Machine Learning com as seguintes configurações:
   - **Assinatura:** Sua assinatura Azure.
   - **Grupo de recursos:** Crie ou selecione um grupo de recursos.
   - **Nome:** Insira um nome único para seu espaço de trabalho.
   - **Região:** Selecione a região geográfica mais próxima.
   - **Conta de armazenamento:** Anote a nova conta de armazenamento padrão que será criada para seu espaço de trabalho.
   - **Chave do cofre:** Anote o novo cofre de chaves padrão que será criado para seu espaço de trabalho.
   - **Insights do aplicativo:** Anote o novo recurso de insights do aplicativo que será criado para seu espaço de trabalho.
   - **Registro de contêiner:** Nenhum (um será criado automaticamente na primeira vez que você implantar um modelo em um contêiner).

3. Selecione **Review + create**, e depois **Create**. Aguarde até que seu espaço de trabalho seja criado (isso pode levar alguns minutos) e então vá para o recurso implantado.

4. Selecione **Launch studio** (ou abra uma nova guia do navegador e acesse [https://ml.azure.com](https://ml.azure.com)) e faça login no Azure Machine Learning Studio usando sua conta Microsoft. Feche todas as mensagens exibidas.

5. No Azure Machine Learning Studio, você deverá ver seu espaço de trabalho recém-criado. Caso contrário, selecione **All workspaces** no menu à esquerda e depois selecione o espaço de trabalho que você acabou de criar.
O Aprendizado de Máquina Automatizado permite que você experimente vários algoritmos e parâmetros para treinar múltiplos modelos e identificar o melhor para seus dados. Neste exercício, você usará um conjunto de dados de detalhes históricos de aluguel de bicicletas para treinar um modelo que prevê o número de aluguéis de bicicletas que devem ser esperados em um determinado dia, com base em características sazonais e meteorológicas.

Citação: Os dados usados neste exercício são derivados do Capital Bikeshare e são usados de acordo com o acordo de licença de dados publicado.

No Azure Machine Learning studio, visualize a página de Aprendizado de Máquina Automatizado (em Authoring).

Crie um novo trabalho de Aprendizado de Máquina Automatizado com as seguintes configurações, usando Next conforme necessário para progredir pela interface do usuário:

### Configurações básicas:

- **Nome do trabalho:** mslearn-bike-automl
- **Novo nome do experimento:** mslearn-bike-rental
- **Descrição:** Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
- **Tags:** nenhum

### Tipo de tarefa e dados:

- Selecione o tipo de tarefa: Regressão
- Selecione o conjunto de dados: Crie um novo conjunto de dados com as seguintes configurações:
  - **Tipo de dados:**
    - Nome: bike-rentals
    - Descrição: Dados históricos de aluguel de bicicletas
    - Tipo: Tabular
  - **Fonte de dados:**
    - Selecione De arquivos da web
    - URL da web: [https://aka.ms/bike-rentals](https://aka.ms/bike-rentals)
  - **Configurações:**
    - Formato do arquivo: Delimitado
    - Delimitador: Vírgula
    - Codificação: UTF-8
    - Cabeçalhos de coluna: Somente o primeiro arquivo tem cabeçalhos
    - Linhas de ignoradas: Nenhuma
    - O conjunto de dados contém dados de várias linhas: não selecione
  - **Esquema:**
    - Inclua todas as colunas, exceto Path

Clique em Criar. Depois que o conjunto de dados for criado, selecione o conjunto de dados bike-rentals para continuar a enviar o trabalho de Aprendizado de Máquina Automatizado.

### Configurações da tarefa:

- Tipo de tarefa: Regressão
- Conjunto de dados: bike-rentals
- Coluna de destino: Aluguéis (inteiro)
- Configurações de adicional:
  - Métrica primária: Erro médio quadrático raiz normalizado
  - Explicar o melhor modelo: Não selecionado
  - Usar todos os modelos suportados: Não selecionado. Você restringirá o trabalho para tentar apenas alguns algoritmos específicos.
  - Modelos permitidos: Selecione apenas RandomForest e LightGBM — normalmente você gostaria de tentar o máximo possível, mas cada modelo adicionado aumenta o tempo necessário para executar o trabalho.
  - Limites:
    - Máximo de tentativas: 3
    - Máximo de tentativas concorrentes: 3
    - Máximo de nós: 3
    - Limiar de pontuação da métrica: 0.085 (para que se um modelo atingir uma pontuação de métrica de erro médio quadrático raiz normalizado de 0.085 ou menos, o trabalho termine.)
    - Tempo limite: 15
    - Tempo limite da iteração: 15
    - Ativar término antecipado: Selecionado
### Validação e teste:

- Tipo de validação: Divisão de treinamento-validação
- Porcentagem de dados de validação: 10
- Conjunto de dados de teste: Nenhum

### Cálculo:

- Selecione o tipo de cálculo: Sem servidor
- Tipo de máquina virtual: CPU
- Nível da máquina virtual: Dedicado
- Tamanho da máquina virtual: Standard_DS3_V2*
- Número de instâncias: 1
  * Se sua assinatura restringir os tamanhos de VM disponíveis para você, escolha qualquer tamanho disponível.

Envie o trabalho de treinamento. Ele começará automaticamente.

Aguarde o término do trabalho. Pode levar um tempo — agora pode ser um bom momento para uma pausa para o café!

### Revisar o melhor modelo:

Quando o trabalho de Aprendizado de Máquina Automatizado for concluído, você poderá revisar o melhor modelo treinado.

Na guia Visão geral do trabalho de Aprendizado de Máquina Automatizado, observe o resumo do melhor modelo. Captura de tela do resumo do melhor modelo do trabalho de Aprendizado de Máquina Automatizado com uma caixa ao redor do nome do algoritmo.

Nota: Você pode ver uma mensagem sob o status “Aviso: pontuação de saída especificada pelo usuário atingida…”. Esta é uma mensagem esperada. Continue para a próxima etapa.

Selecione o texto sob o nome do algoritmo para o melhor modelo para ver seus detalhes.

Selecione a guia Métricas e selecione os gráficos de resíduos e previstos_reais se ainda não estiverem selecionados.

Revise os gráficos que mostram o desempenho do modelo. O gráfico de resíduos mostra os resíduos (as diferenças entre os valores previstos e reais) como um histograma. O gráfico previstos_reais compara os valores previstos com os valores reais.

### Implantar e testar o modelo:

Na guia Modelo para o melhor modelo treinado pelo seu trabalho de Aprendizado de Máquina Automatizado, selecione Implantar e use a opção Serviço da Web para implantar o modelo com as seguintes configurações:
- Nome: prever-alugueis
- Descrição: Prever aluguéis de bicicletas
- Tipo de cálculo: Instância de contêiner do Azure
- Habilitar autenticação: Selecionado

Aguarde o início da implantação - isso pode levar alguns segundos. O status de implantação para o endpoint prever-alugueis será indicado na parte principal da página como Executando.

Aguarde o status de implantação mudar para Sucedido. Isso pode levar de 5 a 10 minutos.

### Testar o serviço implantado:

Agora você pode testar seu serviço implantado.

No Azure Machine Learning Studio, no menu à esquerda, selecione Pontos de Extremidade e abra o ponto de extremidade em tempo real prever-alugueis.

Na página do ponto de extremidade em tempo real prever-alugueis, visualize a guia Teste.

Na janela de entrada de dados para testar o ponto de extremidade, substitua o JSON de modelo pelo seguinte dados de entrada:
```bash
  {
   "Inputs": { 
     "data": [
       {
         "day": 1,
         "mnth": 1,   
         "year": 2022,
         "season": 2,
         "holiday": 0,
         "weekday": 1,
         "workingday": 1,
         "weathersit": 2, 
         "temp": 0.3, 
         "atemp": 0.3,
         "hum": 0.3,
         "windspeed": 0.3 
       }
     ]    
   },   
   "GlobalParameters": 1.0
 }


```
Clique no botão Teste.

Revise os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada - semelhante a isso:

```
 {
   "Results": [
     444.27799000000000
   ]
 }
```
