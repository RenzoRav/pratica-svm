# Prática — SVM no Titanic

Exercício completo em Jupyter Notebook para comparar SVM e Random Forest na previsão de sobrevivência dos passageiros do Titanic.

## Arquivos

- `pratica_svm.ipynb`: notebook executado, com todas as lacunas preenchidas, saídas salvas e respostas em linguagem simples.
- `titanic.csv`: base local com 891 passageiros.
- `requirements.txt`: versões utilizadas na execução.

## Como executar

O notebook foi testado com Python 3.14. Crie um ambiente e instale as dependências:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

Abra `pratica_svm.ipynb` no Visual Studio Code com as extensões Python e Jupyter, selecione o ambiente `.venv` e execute as células de cima para baixo com `Shift + Enter`.

Também é possível usar o Google Colab: abra o notebook e envie `titanic.csv` para o diretório de trabalho da sessão. As versões dos pacotes no Colab podem produzir pequenas diferenças nos resultados.

## Preparação e avaliação

Como a célula de preparação da aula não foi fornecida, o notebook inclui uma preparação própria e reproduzível:

- Entradas: `pclass`, `sex`, `age`, `sibsp`, `parch` e `fare`.
- Alvo: `survived`, com 0 para morte e 1 para sobrevivência.
- Sexo convertido para 0 e 1, sem utilizar colunas que revelam a resposta.
- Dados ausentes preenchidos pela mediana calculada no treino de cada divisão.
- Escalonamento ajustado apenas no treino, dentro do pipeline.
- Validação cruzada estratificada de cinco divisões, com embaralhamento e semente 42.
- Mesmas divisões para todos os modelos e acurácia como métrica.
- Busca de nove combinações de C e gamma por GridSearchCV.

## Resultados

| Modelo | Treino | Validação |
| --- | --- | --- |
| Random Forest, 300 árvores e profundidade 6 | 87,5% | 83,3% |
| SVM sem escala | 68,2% | 67,8% |
| SVM com escala | 83,5% | 82,5% |
| SVM C=0.01 | 61,6% | 61,6% |
| SVM C=100 | 86,2% | 80,6% |
| SVM linear | 78,7% | 78,7% |
| SVM gamma=10 | 89,8% | 76,2% |
| SVM ajustado | 83,4% | 82,9% |

A melhor combinação foi `C=1` e `gamma=0.1`. A Random Forest ficou aproximadamente 0,3 ponto percentual à frente do SVM ajustado. Os valores de treino são médias das acurácias nas partes de treino da validação cruzada, não avaliações independentes.

A pontuação do GridSearchCV é usada na escolha dos parâmetros e pode ser otimista. O exercício não inclui um conjunto de teste independente. Antes de uma aplicação real, a escolha precisa ser confirmada com teste separado ou validação cruzada aninhada.

Os resultados dependem da preparação e das divisões adotadas; não representam uma reprodução exata de uma célula de preparação da aula que não foi disponibilizada.

## Fonte dos dados

[Titanic — repositório seaborn-data](https://github.com/mwaskom/seaborn-data/blob/master/titanic.csv).
