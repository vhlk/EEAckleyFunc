# Algoritmos evolutivo e genético para otimização de função
Desenvolvimento de um Algoritmo Evolucionário (Estratégia Evolutiva) para a determinação do ponto de mínimo global da função de Ackley
Grupo : 
 - Alexandra Zarzar (avz)
 - Luana Porciuncula (lpb)
 - Victor Hugo Kunst (vhlk)


## Evolucionário
### Descrição esquemática do algoritmo evolucionário implementado
![Descrição esquemática](https://raw.githubusercontent.com/vhlk/EEAckleyFunc/main/descricao_esquematica.png?token=GHSAT0AAAAAABYT2KLRFM3TLIEAPMPU37OGYZCPP5A)


### Descrição dos processos de:
#### a. Representação das soluções (indivíduos)
Cada individuo da solução será uma lista de tamanho D (neste caso, D=30), onde cada elemento é no máximo 15 e no mínimo -15 (para ackley, já para a função de schwefel, seguimos o range recomendado no site de (-500, 500))
#### b. Função de Fitness
A propria função de otimização (ackley ou schwefel). Quanto menor o valor, melhor o fitness.
#### c. População (tamanho, inicialização, etc)
Utilizamos uma população de tamanho 30. 
A população é inicializada com valores aleatórios do entre o range de Xi (-15, 15 ou -500, 500).
Para criar pressão de seleção, geramos uma quantidade de filhos 7 vezes o tamanho da população (210).
#### d. Processo de seleção de pais
2 pais são selecionados de forma uniforme e aleatória para cada recombinação.
#### e. Operadores Genéticos (Recombinação e Mutação)
São selecionados 210 (quantidade de filhos) duplas de pais, e cada um fará uma recombinação para gerar 1 filho.
Para a recombinação, fizemos duas implementações: Local discreta (seleciona aleatoriamente um Xi de cada pai) e Local intermediaria (o Xi do filho é o valor médio entre o Xi dos dois pais). 

Para a mutação, mutamos todos os filhos gerados, seguindo o caso de "Mutação não correlacionada com um σ".
Primeiro geramos um novo σ, com base no σ anterior do individuo => σ' = σ × exp(τ × N(0,1)). Usando a taxa de aprendizagem τ como 1/√(D).
O novo σ, é limitado sendo no minimo um ε0 (EPSILON_0) para evitar passos muito pequenos. E também sendo no máximo um ε1 (EPSILON_1) para evitar passos muito grandes, tendo em vista que o range de xi é limitado (-15,15 e -500,500)
Em seguida, mutacionamos o individuo usando o novo σ => xi + σ' × N(0,1)
Após mutacionar o individuo, comparamos o fitness antigo com o novo fitness do individuo, avaliando quantas mutações foram um "sucesso" (isto é, o fitness novo foi menor que o antigo - Será utilizado para ajustar a mutação por meio do "1/5 da regra de sucesso" após a seleção de sobreviventes).
#### f. Processo de seleção por sobrevivência
Para a seleção de sobreviventes fizemos 2 implementações, uma geracional (geração anterior é descartada, e sobrevivem os melhores filhos da população) e outra elitista (sobrevivem os melhores da população)
#### g. Condições de término do Algoritmo Evolucionário
As condições de termino são: 
 - (Encontrou solução) Se o algoritmo alcançou a solução (se é proximo de 0 numa margem de erro de 0.005)
 - (Número máximo de iterações) Se o algoritmo alcançou o número limite de iterações
 - (Early stopping) Se o algoritmo passou 50 iterações sem melhorar o melhor fitness em 0.0005


## Genético
### Descrição dos processos de:
#### a. Representação das soluções (indivíduos)
Cada individuo da solução será uma lista de tamanho D (neste caso, D=30), onde cada elemento é no máximo 15 e no mínimo -15.
#### b. Função de Fitness
A propria função de otimização ackley. Quanto menor o valor, melhor o fitness.
#### c. População (tamanho, inicialização, etc)
Utilizamos uma população de tamanho 100.
A população é inicializada com valores aleatórios do entre o range de Xi (-15, 15).
#### d. Processo de seleção de pais
Seleção de pais é feito por roleta (Dá uma chance para cada indivíduo da população, proporcional ao seu fitness, e realiza um sorteio de 2 pais).
#### e. Operadores Genéticos (Recombinação e Mutação)
É realizada uma recombinação do tipo cut-and-crossfill com ponto de corte no gene 11. Para mutação, foram realizadas 3 tipos:
1. Por perturbação: Um conjunto dos genes de tamanho um terço do total é permutado entre si;
2. Por offset com N(0,7): Foi adicionado um offset aleatório (N(0,7)) a um terço dos genes (escolhidos aleatoriamente);
3. Por offset com N(0,1): Foi adicionado um offset aleatório (N(0,1)) a um terço dos genes (escolhidos aleatoriamente);
#### f. Processo de seleção por sobrevivência
Elistista: os melhores indivíduos são escolhidos (escolha baseada nos resultados anteriores).
#### g. Condições de término do Algoritmo Genético
As condições de termino são: 
 - (Encontrou solução) Se o algoritmo alcançou a solução (se é proximo de 0 numa margem de erro de 0.005)
 - (Número máximo de iterações) Se o algoritmo alcançou o número limite de iterações (10.000)