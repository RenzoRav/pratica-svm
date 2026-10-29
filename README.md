# Prática — SVM no Titanic

Atividade de classificação para comparar SVM e Random Forest na previsão de sobrevivência dos passageiros do Titanic.

O notebook utiliza a estrutura e a preparação do arquivo original da atividade, fornecido pelo usuário. As lacunas foram preenchidas, todas as células foram executadas e as respostas foram escritas a partir dos resultados obtidos.

## Arquivos

- `pratica_svm.ipynb`: atividade completa, com saídas salvas e respostas em linguagem simples.
- `titanic.csv`: cópia local da base para consulta ou execução sem acesso à fonte remota.
- `requirements.txt`: versões utilizadas na execução.

## Como executar

O notebook foi testado com Python 3.14. Crie um ambiente e instale as dependências:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

Abra `pratica_svm.ipynb` no Visual Studio Code com as extensões Python e Jupyter, selecione o ambiente `.venv` e execute as células de cima para baixo com `Shift + Enter`.

Também é possível abrir o arquivo no Google Colab. A preparação original lê a base diretamente da internet. Para execução offline, altere apenas o valor de `url` para `"titanic.csv"` e mantenha o arquivo no diretório de trabalho.

As versões disponíveis em outros ambientes podem causar pequenas diferenças nos resultados.

## Preparação original preservada

- Base com 891 passageiros e alvo `survived`.
- Variáveis: classe, sexo, idade, irmãos/cônjuges, pais/filhos, tarifa e porto de embarque.
- Idades ausentes preenchidas pela mediana da base.
- Portos ausentes preenchidos com `S`.
- Sexo convertido para 0 e 1.
- Porto de embarque convertido em três colunas por `get_dummies`.
- Nove colunas finais de entrada.
- Validação cruzada estratificada com cinco divisões, embaralhamento e semente 42.
- Acurácia como métrica, com média de treino e validação nas mesmas divisões para todos os modelos.

A célula de preparação foi mantida exatamente como no material enviado. Diferentemente da primeira versão deste repositório, esta versão inclui o porto de embarque e não utiliza um preenchimento de valores ausentes dentro do pipeline.

## Atividade resolvida

- Random Forest com 300 árvores e profundidade máxima 6.
- Comparação de SVM sem escala e com StandardScaler.
- Comparação de C=0.01 e C=100.
- Comparação de kernel linear e RBF.
- Avaliação de gamma=10.
- GridSearchCV com nove combinações de C e gamma.
- Respostas de todas as questões e tabela final ordenada pela validação.

O melhor candidato da busca também foi incluído na tabela final. O parâmetro `return_train_score=True` foi acrescentado à busca para registrar sua pontuação de treino.

## Resultados desta execução

| Modelo | Treino | Validação |
| --- | --- | --- |
| Random Forest (referência) | 87,3% | 83,3% |
| SVM com escala | 84,1% | 82,7% |
| SVM ajustado (GridSearchCV) | 84,1% | 82,5% |
| SVM C=100 | 88,0% | 79,8% |
| SVM kernel linear | 79,0% | 78,7% |
| SVM gamma=10 | 91,2% | 75,0% |
| SVM sem escala | 68,3% | 67,8% |
| SVM C=0.01 | 61,6% | 61,6% |

A melhor combinação da grade foi `C=1` e `gamma=0.1`. A Random Forest ficou cerca de 0,8 ponto percentual à frente desse candidato. O SVM padrão com escala alcançou 82,7%, ligeiramente acima do melhor candidato da grade. A busca não garante melhora quando a grade não contém o valor padrão efetivo de gamma.

## Limites da comparação

Para respeitar a atividade, a preparação original foi preservada. Ela calcula a mediana da idade antes da validação cruzada, o que permite que informações da parte de validação participem desse preenchimento. Em uma aplicação real, esse tratamento deve ser ajustado apenas nos dados de treino de cada divisão, dentro de um pipeline.

A pontuação do GridSearchCV também é usada para escolher os parâmetros. Portanto, ela não substitui uma avaliação final em dados independentes. A escolha para produção precisa ser confirmada com um conjunto de teste separado ou validação cruzada aninhada.

## Fonte dos dados

[Titanic — repositório seaborn-data](https://github.com/mwaskom/seaborn-data/blob/master/titanic.csv).
