



# hackathon-MS-Azure

## Equipe

<a href="https://github.com/rockiir/hackathon-MS-Azure/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=rockiir/hackathon-MS-Azure" />
</a>

Made with [contributors-img](https://contrib.rocks).

## Exercício proposto
Após baixar o arquivo pelo link: https://download.inep.gov.br/microdados/microdados_enem_2019.zip
O arquivo a ser usado está dentro da pasta dados, com o nome de "MICRODADOS_ENEM_2019.csv"

#### Desafio 1 - (3 pontos) Existe alguma correlação entre as notas das provas objetivas? Comente.

#### Tarefa

Quem é bom em matemática, também vai bem em ciências da natureza? Nesse problema, você selecionará os campos da prova objetiva:

NU_NOTA_CN Nota da prova de Ciências da Natureza NU_NOTA_CH Nota da prova de Ciências Humanas NU_NOTA_LC Nota da prova de Linguagens e Códigos NU_NOTA_MT Nota da prova de Matemática

Você deverá realizar uma análise de regressão para descobrir se é possível prever a nota da prova de Ciências da Natureza caso se saiba a nota de outra.

### Passo 1 - Criar grupo de recursos
#### Criação de um Workspace 
- Grupo de recursos: Hackathon
- Nome do workspace: Hackathon
- Região: Sul do Brasil


![](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/1-Criando%20grupo%20de%20recursos.jpg)

## Na guia Gerir, na opção Computação
### Passo 2-  Criando uma instancia de computação
Logo após criar o grupo de execussões fora criada a instância de computação com a seguinte configuração:
- Tipo de máquina virtual: CPU
- Tamanho da máquina virtual: Standard_DS11_v2 
- Nome de computação: DadosEnem 
- Habilitar o acesso SSH: não selecionado


![Criando uma instancia de computação](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/2-Criando%20uma%20instancia%20de%20computa%C3%A7%C3%A3o.png)

### Passo 3 -  Criação de um cluster de calculo
Em seguida criamos o cluster de cálculo com a seguinte configuração:
- Prioridade da máquina virtual: dedicada
- Tipo de máquina virtual: CPU
- Tamanho da máquina virtual: Standard_DS11_v2
- Nome de computação: DadosEnem1
- Número mínimo de nós: 0
- Número máximo de nós: 2
- Segundos de espera antes de reduzir verticalmente: 120
- Habilitar o acesso SSH: não selecionado

![Criação de um cluster de calculo](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/3%20-%20Cria%C3%A7%C3%A3o%20de%20um%20cluster%20de%20calculo.png)

## Na guia Author, na opção Designer/Estruturador 
### Passo 4 -  Criação de um pipeline
Com os clusters prontos seguimos para a criação de pipeline com o nome "Notas do enem"


![Criação de um pipeline](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/4%20-Cria%C3%A7%C3%A3o%20de%20um%20pipeline.png)
## Na guia recursos, na opção Conjunto de Dados

### Passo 5 -Criação de um conjunto de dados 
Mas antes  não podemos esquecer do conjunto de dados, que se revelou o maior desafio de todo o projeto, pois o conjunto de dados do enem 2019 era muito grande, então tivemos repetidos problemas ao fazer upload do arquivo e ao analisá-los, porém depois de muito tentar conseguimos fazer o upload.
- Envie o arquivo "MICRODADOS_ENEM_2019.csv"
- O Azure já vem com todas as opções selecionadas corretamente
- Como o ficheiro é grande pode haver demora para carregar o arquivo (não feche a aba, pois ele cancela o processo)

![-Criação de um conjunto de dados](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/5-%20Cria%C3%A7%C3%A3o%20de%20um%20conjunto%20de%20dados.png)

Na seção esquemas, desmarque todas as colunas que não são necessárias

![Detalhes](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/5.2-%20Detalhes.png)


Deixe apenas as colunas com as notas de todas as áreas de conhecimento
![Selecionando as colunas com notas](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/5.4-%20Selecionando%20as%20colunas%20com%20notas.png)


## Na guia Author, na opção Designer/Estruturador, abra o pipeline criado anteriormente
### Passo 6 - Selecionando as colunas
Voltando ao pipeline.
Selecione a seção "Conjuntos de dados de exemplo (Datasets)" e arraste o conjunto de dados criado para tela.
Selecionando as colunas das notas de Ciências da Natureza e Matemática
![Selecionando as colunas](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/6.2-Selecionando%20as%20colunas.png) 

A partir da seção "Transformação de Dados (Data Tranformation)" arrastar o módulo "Selecionar Colunas no Conjunto de Dados" conectando a saída do módulo "NotasEnem"
![Limpando dados](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/6.3-%20Limpando%20dados.png)

Em seguida ligue ao Normalize Data como mostrado acima, setamos o transformation method como MinMax e selecionamos a coluna de matematica.

![Normalizando dados (MinMax)](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/6.4-%20Normalizando%20dados%20MinMax.png)
A partir da seção "Transformação de Dados" arrastar o módulo "Limpar Dados Ausentes (Clean Missing Data)" conectando a saída do módulo "Selecionar Colunas no Conjunto de Dados" - Selecionar a coluna das notas de Matemática
- Taxa mínima de valores ausentes: 0,0
- Taxa máxima de valores ausentes: 1,0
- Modo de limpeza: remover linha inteira
### Passo 7 -  Pipeline completo
O pipeline ficou da seguinte forma.

![Pipeline parte1 completo](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/7-%20Pipeline%20parte1%20completo.png)

### Passo 8 -Avaliando o modelo
Selecione Enviar e execute o pipeline como um novo experimento
![Avaliando o modelo](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/15-%20Avaliando%20o%20modelo.jpg)

### Passo 9 -Resultados parciais obtidos
Abaixo os resultados obtidos nessa etapa do processo.
- Na seção "Transformações de Dados", selecione o módulo "Dividir Dados". Em seguida, conecte a saída Conjunto de dados transformado (à esquerda) do módulo Normalizar dados para a entrada do módulo Dividir dados.
- Modo de divisão: dividir linhas
- Fração das linhas no primeiro conjunto de dados de saída: 0,5
- Semente aleatória: 123
- Divisão estratificada: Falso

![Resultados parciais obtidos](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/15.1-Resultados%20parciais%20obtidos.jpg)

### Passo 10  -- Pipeline de Inferencia sem alterações


![Pipeline de Inferencia sem alterações](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/17-%20Pipeline%20de%20Inferencia%20sem%20altera%C3%A7%C3%B5es.jpg)

### Passo 11 -dados CSV

![dados CSV](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/18-%20dados%20CSV.jpg)

### Passo 12 - script Python

![script Python](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/22-%20script%20Python.jpg)

### Passo 13 - Pipeline de inferencia_finalizado

![Pipeline de inferencia_finalizado](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/23-%20Pipeline%20de%20inferencia_finalizado.jpg)

### Passo 14 - resultado continuação

![resultado continuação](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/24.1-%20resultado%20continua%C3%A7%C3%A3o.jpg)

24.2- resultado continuação

![resultado continuação](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/24.2-%20resultado%20continua%C3%A7%C3%A3o.jpg)

24.3-resultado continuação

![resultado continuação](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/24.3-resultado%20continua%C3%A7%C3%A3o.jpg)

24-resultado
Quando o pipeline for concluído, selecione o módulo "Executar script Python" e, no painel de configurações, na guia "Saída + logs", visualize o Conjunto de dados de resultados para ver a nota de Ciências da Natureza prevista  nos dados de entrada.

Com base nos resultados obtidos para uma nota 700.0 em Matemática, a nota prevista para Ciências da Natureza será: 557.8304

Agora o modelo é capaz de prever notas da área de Ciências da Natureza com base nas notas de matemática.
![resultado](https://github.com/rockiir/hackathon-MS-Azure/blob/main/images/24-resultado.jpg)
