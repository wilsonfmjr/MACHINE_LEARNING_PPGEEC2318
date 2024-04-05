Dez principais pontos a serem considerados no tratamento de dados de treinamento.

## Baseado no capítulo 4 do livro “Designing Machine Learning Systems” de Chip Huyen

## Introdução	

Os dados utilizados nos treinamentos de ML são fundamentais para o desenvolvimento do seu modelo. Saber como lidar com esses pode poupar tempo recursos no futuro. Neste texto serão apresentados dez pontos a serem considerados na coleta, amostragem, rotulagem, desequilíbrio na distribuição e aumento de dados. Estes pontos são cruciais no tratamento dos dados utilizados para treinar seu modelo. Eles foram extraídos do capítulo 4 do livro “Designing Machine Learning Systems” de Chip Huyen.

## 1. Amostragem Não Probabilística

Na amostragem não probabilística a seleção de dados não é baseada em nenhum critério de probabilidade. Podemos apontar quatro critérios que normalmente são  aplicados para amostragem não probabilística.

Primeiro, amostragem de conveniência, onde as amostras de dados são selecionadas com base na sua disponibilidade.

Segundo, amostragem de bola de neve, onde as amostras futuras são selecionadas com base em amostras existentes. Por exemplo, para copiar contas legítimas do Twitter sem ter acesso aos bancos de dados do Twitter, você começa com um pequeno número de contas, depois raspa todas as contas que elas seguem e assim por diante. 

Terceiro, amostragem de julgamento, onde os especialistas decidem quais amostras incluir.

Por fim, a amostragem de cota, onde você seleciona amostras com base em cotas para determinadas fatias de dados sem qualquer randomização. Por exemplo, ao fazer uma pesquisa, você pode querer 100 respostas de cada uma das faixas etárias: menos de 30 anos, entre 30 e 60 anos e acima de 60 anos, independentemente da distribuição etária real.

As amostras selecionadas por critérios não probabilísticos não são representativas dos dados do mundo real e, portanto, estão repletas de vieses de seleção, mas em muitos casos a seleção de dados para modelos de ML ainda é motivada pela conveniência.

## 2. Amostragem Probabilística

A amostragem não probabilística pode ser uma maneira rápida e fácil de coletar seus dados iniciais para lançar seu projeto. No entanto, para modelos confiáveis, você pode querer usar amostragem baseada em probabilidade.

Na amostragem aleatória Simples, você dá a todas as amostras da população probabilidades iguais de serem selecionadas. A vantagem deste método é que é fácil de implementar. A desvantagem é que categorias raras de dados podem não aparecer na sua seleção.

Já na amostragem estratificada, para evitar a desvantagem da amostragem aleatória simples, você pode primeiro dividir sua população nos grupos de seu interesse e coletar amostras de cada grupo separadamente. Dessa forma, não importa quão rara seja a classe, você garantirá que amostras delas serão incluídas na seleção. Uma desvantagem deste método de amostragem é que nem sempre é possível dividir todas as amostras em grupos. Sendo especialmente desafiador quando uma amostra pode pertencer a vários grupos, como no caso de tarefas multi rótulos.

Na amostragem ponderada, cada amostra recebe um peso, que determina a probabilidade de ela ser selecionada. Este método permite que você aproveite o conhecimento do domínio. Isso também ajuda no caso em que os dados que você possui vêm de uma distribuição diferente em comparação com os dados verdadeiros. Um conceito comum em ML que está intimamente relacionado à amostragem ponderada são os pesos amostrais. A amostragem ponderada é usada para selecionar amostras para treinar seu modelo, enquanto os pesos amostrais são usados para atribuir “pesos” ou “importância” às amostras de treinamento. Amostras com pesos mais altos afetam mais a função de perda. Alterar os pesos amostrais pode alterar significativamente os limites de decisão do seu modelo.

A amostragem de reservatório é um algoritmo especialmente útil quando você precisa lidar com dados de streaming, que geralmente são os que você tem em produção. O algoritmo envolve um reservatório, que pode ser uma matriz, e consiste em três etapas. Primeiro, coloque os primeiros k elementos no reservatório. Segundo, para cada entrada de n elementos, gere um número aleatório i tal que 1 ≤ i ≤ n. Por fim, se 1 ≤ i ≤ n, então substitua i-ésimo elemento no reservatório com n elementos. Caso contrário, não faça nada. Isso significa que todas as amostras têm chances iguais de serem selecionadas.

A amostragem por importância é um dos principais métodos de amostragem, não apenas no ML, pois permite-nos fazer amostragens de uma distribuição quando só temos acesso a outra distribuição. Um exemplo em que a amostragem por importância é usada no ML é a aprendizagem por reforço baseada em políticas.

## 3. Rotulagem Manual
	
O desempenho de um modelo de ML ainda depende muito da qualidade e da quantidade dos dados rotulados nos quais ele é treinado. A rotulagem de dados deixou de ser uma tarefa auxiliar para se tornar uma função central de muitas equipes de ML em produção. Ainda assim, o primeiro método que vem à mente dos cientistas de dados quando falam sobre rotulagem é a rotulagem manual. 

Adquirir rótulos manuais para seus dados é difícil por muitos e muitos motivos. Primeiro, a rotulagem manual de dados pode ser dispendiosa, especialmente se for necessária experiência no assunto. Em segundo lugar, a rotulagem manual representa uma ameaça à privacidade dos dados. Terceiro, a rotulagem manual é lenta. A rotulagem lenta leva a uma velocidade de iteração lenta e torna seu modelo menos adaptável a ambientes e requisitos em mudança. Se a tarefa ou os dados forem alterados, você terá que aguardar que seus dados sejam renomeados antes de atualizar seu modelo.

Muitas vezes, para obter dados rotulados suficientes, as empresas precisam usar dados de diversas fontes e contar com diversos anotadores com diferentes níveis de especialização. Desentendimentos entre anotadores são extremamente comuns. Quanto maior for o nível de conhecimento do domínio exigido, maior será o potencial para anotar discordâncias. Essas diferentes fontes de dados e anotadores também possuem diferentes níveis de precisão. Isso leva ao problema da ambiguidade ou multiplicidade dos rótulos. 

Usar indiscriminadamente dados de múltiplas fontes, gerados com diferentes anotadores, sem examinar sua qualidade pode fazer com que seu modelo falhe misteriosamente, pois novas amostras podem ser coletadas por anotadores que rotularam os dados com muito menos precisão do que os dados originais. É uma boa prática acompanhar a origem de cada uma das suas amostras de dados, bem como seus rótulos, uma técnica conhecida como linhagem de dados. A linhagem de dados ajuda você a sinalizar possíveis distorções em seus dados e a depurar seus modelos.

## 4. Rótulos Naturais
	
A rotulagem manual não é a única fonte de rótulos. Você pode trabalhar em tarefas com rótulos naturais. Tarefas com rótulos naturais são aquelas onde as previsões do modelo podem ser avaliadas automaticamente ou parcialmente avaliadas pelo sistema. Por exemplo, considere a previsão do preço das ações. Se o seu modelo prever o preço de uma ação nos próximos dois minutos, depois de dois minutos, você poderá comparar o preço previsto com o preço real. O exemplo canônico de tarefas com rótulos naturais são os sistemas de recomendação, onde o objetivo do sistema é recomendar aos usuários itens relevantes para eles. Muitas tarefas podem ser enquadradas como tarefas de recomendação, mesmo que sua tarefa não tenha rótulos inerentemente naturais, pode ser possível configurar seu sistema de uma forma que permita coletar algum feedback sobre seu modelo.

Para tarefas com rótulos naturais, o tempo que leva desde o momento em que uma previsão é apresentada até o momento em que o feedback sobre ela é fornecido é o comprimento do ciclo de feedback. Muitos sistemas de recomendação possuem ciclos curtos de feedback, como a recomendação de produtos relacionados na Amazon ou pessoas para seguir no Twitter. Se você trabalha com tipos de conteúdo mais longos, o ciclo de feedback pode levar horas, semanas ou até meses. A detecção de fraude é um exemplo de tarefa com longos ciclos de feedback.

Se você deseja extrair rótulos do feedback do usuário, é importante observar que existem diferentes tipos de feedback do usuário. Eles podem ocorrer em diferentes estágios durante a jornada do usuário em seu aplicativo e diferem em volume, intensidade do sinal e duração do ciclo de feedback.A escolha do comprimento correto da janela requer uma consideração cuidadosa, pois envolve a compensação entre velocidade e precisão. Uma janela curta significa que você pode capturar rótulos mais rapidamente, o que permite usar esses rótulos para detectar problemas com seu modelo e resolver esses problemas o mais rápido possível. No entanto, uma janela curta também significa que você pode rotular prematuramente uma recomendação como ruim antes de ela ser clicada.

## 5. Como lidar com a falta de rótulos
	
Como visto, são muitos os desafios relacionados à aquisição de rótulos suficientes de alta qualidade. Por isso, muitas técnicas foram desenvolvidas para resolver os problemas resultantes dessa falta de rótulos. Dentre essas técnicas podemos destacar a supervisão fraca, a semi-supervisão, a aprendizagem por transferência e a aprendizagem ativa.

Na supervisão fraca as pessoas dependem de heurísticas, que podem ser desenvolvidas com conhecimentos especializados na matéria, para rotular os dados. Uma das ferramentas de código aberto mais populares para supervisão fraca é o Snorkel, desenvolvido no Stanford AI Lab, na qual funções de rotulagem (Label Functions - LFs) são utilizadas para codificar as heurísticas e aplicadas as amostras que se deseja rotular. Em teoria, você não precisa de rótulos manuais para supervisão fraca. No entanto, para ter uma ideia da precisão dos seus LFs, recomenda-se um pequeno número de rótulos manuais que podem ajudá-lo a descobrir padrões em seus dados para escrever LFs melhores. A supervisão fraca é um paradigma simples, mas poderoso. No entanto, não é perfeito. Em alguns casos, os rótulos obtidos por uma supervisão fraca podem ser extremamente ruidosos para serem úteis.

Se a supervisão fraca aproveita a heurística para obter rótulos ruidosos, a semi-supervisão aproveita pressupostos estruturais para gerar novos rótulos com base num pequeno conjunto de rótulos iniciais. Ao contrário da supervisão fraca, a semi-supervisão requer um conjunto inicial de rótulos.

Um método clássico de semi-supervisão é o autotreinamento. Nele você começa treinando um modelo em seu conjunto existente de dados rotulados e usa esse modelo para fazer previsões para amostras não rotuladas. Supondo que as previsões com altas pontuações de probabilidade bruta estejam corretas, você adiciona os rótulos previstos com alta probabilidade ao seu conjunto de treinamento e treina um novo modelo nesse conjunto de treinamento expandido. Isso continua até que você esteja satisfeito com o desempenho do seu modelo.

Outro método de semi-supervisão assume que amostras de dados que compartilham características semelhantes compartilham os mesmos rótulos. A semelhança pode ser óbvia, como na tarefa de classificar o tema das hashtags do Twitter, mas na maioria dos casos, a semelhança só pode ser descoberta por métodos mais complexos. 

Um método de semis-supervisão que ganhou popularidade nos últimos anos é o método baseado em perturbações. Baseia-se na suposição de que pequenas perturbações em uma amostra não devem alterar seu rótulo. Então você aplica pequenas perturbações às suas instâncias de treinamento para obter novas instâncias de treinamento. As perturbações podem ser aplicadas diretamente às amostras (por exemplo, adicionando ruído branco às imagens) ou às suas representações (por exemplo, adicionando pequenos valores aleatórios à incorporação de palavras). As amostras perturbadas têm os mesmos rótulos que as amostras não perturbadas.

A aprendizagem por transferência refere-se à família de métodos em que um modelo desenvolvido para uma tarefa é reutilizado como ponto de partida para um modelo em uma segunda tarefa. Primeiro, o modelo base é treinado para uma tarefa base. A tarefa base geralmente é uma tarefa que possui dados de treinamento abundantes e baratos. O modelo treinado pode então ser usado para a tarefa na qual você está interessado – uma tarefa downstream – como análise de sentimento, detecção de intenção ou resposta a perguntas. A aprendizagem por transferência é especialmente atraente para tarefas que não possuem muitos dados rotulados. Mesmo para tarefas que possuem muitos dados rotulados, usar um modelo pré-treinado como ponto de partida pode muitas vezes aumentar significativamente o desempenho em comparação com o treinamento do zero.

A aprendizagem ativa é um método para melhorar a eficiência dos rótulos de dados. Essa técnica permite que os modelos de ML alcançar maior precisão com menos rótulos de treinamento se puderem escolher com quais amostras de dados aprender. Em vez de rotular amostras de dados aleatoriamente, você rotula as amostras que são mais úteis para seus modelos de acordo com algumas métricas ou heurísticas. O aprendizado ativo é especialmente adequado para quando um sistema funciona com dados em tempo real onde dados mudam o tempo todo. A aprendizagem ativa neste regime de dados permitirá que seu modelo aprenda de forma mais eficaz em tempo real e se adapte mais rapidamente a ambientes em mudança.
		
## 6. Desequilíbrio de classe

O desequilíbrio de classe normalmente se refere a um problema em tarefas de classificação onde há uma diferença substancial no número de amostras em cada classe dos dados de treinamento. O ML, especialmente o aprendizado profundo, funciona bem em situações em que a distribuição de dados é mais equilibrada, e geralmente não tão bem quando as classes estão fortemente desequilibradas. A primeira razão é que o desequilíbrio de classes geralmente significa que há sinal insuficiente para que seu modelo aprenda a detectar as classes minoritárias. A segunda razão é que o desequilíbrio de classes torna mais fácil para o seu modelo ficar preso em uma solução não ideal, explorando uma heurística simples em vez de aprender algo útil sobre o padrão subjacente dos dados. A terceira razão é que o desequilíbrio de classes leva a custos de erro assimétricos – o custo de uma previsão errada numa amostra da classe rara pode ser muito mais elevado do que uma previsão errada numa amostra da classe maioritária. O desequilíbrio de classes é a norma em ambientes do mundo real. Os eventos raros são frequentemente mais interessantes (ou mais perigosos) do que os eventos regulares, e muitas tarefas concentram-se na detecção desses eventos raros. Outra causa de desequilíbrio de classes, embora menos comum, são erros de rotulagem. Sempre que se deparar com o problema de desequilíbrio de classes, é importante examinar seus dados para compreender as causas disso.

## 7. Como lidar com o desequilíbrio de classes
	
Devido à sua prevalência em aplicações do mundo real, o desequilíbrio de classes tem sido exaustivamente estudado nas últimas duas décadas. O desequilíbrio de classe afeta as tarefas de maneira diferente com base no nível de desequilíbrio. Algumas tarefas são mais sensíveis ao desequilíbrio de classe do que outras. Muitas técnicas foram sugeridas para mitigar o efeito do desequilíbrio de classes. No entanto, à medida que as redes neurais se tornaram muito maiores e mais profundas, com maior capacidade de aprendizagem, alguns podem argumentar que não se deve tentar “consertar” o desequilíbrio de classes se é assim que os dados aparecem no mundo real. Um bom modelo deve aprender a modelar esse desequilíbrio.

A coisa mais importante a fazer ao enfrentar uma tarefa com desequilíbrio de classes é escolher as métricas de avaliação apropriadas. Métricas erradas lhe darão ideias erradas sobre o desempenho de seus modelos e, consequentemente, não serão capazes de ajudá-lo a desenvolver ou escolher modelos bons o suficiente para sua tarefa. A precisão geral e a taxa de erro são as métricas usadas com mais frequência para relatar o desempenho dos modelos de ML. No entanto, essas métricas são insuficientes para tarefas com desequilíbrio de classe porque tratam todas as classes igualmente, o que significa que o desempenho do seu modelo na classe majoritária dominará essas métricas. Isso é especialmente ruim quando a classe majoritária não é o que importa para você.

Além de escolher as métricas corretas para o seu problema, podemos utilizar outras abordagens para lidar com o desequilíbrio de classes, como os métodos ao nível de dados, o que significa alterar a distribuição dos dados para torná-la menos desequilibrada; e métodos em nível de algoritmo, o que significa mudar seu método de aprendizagem para torná-lo mais robusto ao desequilíbrio de classe.

## 8.  Métodos ao nível dos dados

Os métodos de nível de dados modificam a distribuição dos dados de treinamento para reduzir o nível de desequilíbrio e facilitar o aprendizado do modelo. Uma família comum de técnicas é a reamostragem. A reamostragem inclui sobreamostragem, adicionando mais instâncias das classes minoritárias, e subamostragem, removendo instâncias das classes majoritárias. A maneira mais simples de subamostrar é remover aleatoriamente instâncias da classe majoritária, enquanto a maneira mais simples de sobreamostrar é fazer cópias aleatoriamente da classe minoritária até obter uma proporção com a qual você esteja satisfeito. Ao reamostrar seus dados de treinamento, nunca avalie seu modelo em dados reamostrados, pois isso fará com que seu modelo se ajuste demais a essa distribuição reamostrada. A subamostragem corre o risco de perder dados importantes com a remoção de dados. A sobreamostragem corre o risco de sobreajuste nos dados de treinamento, especialmente se as cópias adicionadas da classe minoritária forem réplicas de dados existentes.

A subamostragem corre o risco de perder dados importantes com a remoção de dados. A sobreamostragem corre o risco de sobreajuste nos dados de treinamento, especialmente se as cópias adicionadas da classe minoritária forem réplicas de dados existentes. A técnica de aprendizado em duas fases pode mitigar esses riscos. Primeiro você treina seu modelo nos dados reamostrados. Esses dados reamostrados podem ser obtidos subamostrando aleatoriamente grandes classes até que cada classe tenha apenas instâncias. Em seguida, você ajusta seu modelo com base nos dados originais.

## 9. Métodos em nível de algoritmo

Os métodos em nível de algoritmo mantém a distribuição de dados de treinamento intacta, mas alteram o algoritmo para torná-lo mais robusto ao desequilíbrio de classe. Como a função custo orienta o processo de aprendizagem, muitos métodos em nível de algoritmo envolvem ajuste à função custo. A ideia principal é que, se houver duas instâncias, x1 e x2, e a perda resultante de fazer a previsão errada em x1 é maior que x2, o modelo priorizará fazer a previsão correta em x1 sobre fazer a previsão correta sobre x2. Ao dar maior peso às instâncias de treinamento que nos interessa, podemos fazer com que o modelo se concentre mais no aprendizado dessas instâncias.

Existem muitas maneiras de modificar esta função custo. Por exemplo, na aprendizagem sensível aos custos, a função de perda individual é modificada para ter em conta este custo variável. O problema com esta função de perda é que você precisa definir manualmente a matriz de custos, que é diferente para tarefas diferentes em escalas diferentes. Na técnica por perda equilibrada de classe, um modelo treinado em um conjunto de dados desequilibrado tenderá para as classes majoritárias e fará previsões erradas sobre as classes minoritárias. Mas ao analisarmos o modelo por fazer previsões erradas sobre as classes minoritárias para corrigir este viés, podemos tornar o peso de cada classe inversamente proporcional ao número de amostras dessa classe, de modo que as classes mais raras tenham pesos mais elevados. Já na técnica de perda focal, para um determinado conjunto de dados, alguns exemplos são mais fáceis de classificar do que outros, e o modelo pode aprender a classificá-los rapidamente. Mas ao incentivar o modelo a focar no aprendizado das amostras que ainda tem dificuldade de classificar ajustando a perda para que, se uma amostra tiver uma probabilidade menor de estar correta, ela tenha um peso maior.

## 10. Aumento de dados

O aumento de dados é uma família de técnicas usadas para aumentar a quantidade de dados de treinamento. Tradicionalmente, essas técnicas são usadas para tarefas que possuem dados de treinamento limitados, como em imagens médicas. No entanto, nos últimos anos, eles têm se mostrado úteis mesmo quando temos muitos dados – dados aumentados podem tornar nossos modelos mais robustos a ruídos e até mesmo a ataques adversários. Dentre as principais técnicas de aumento de dados, podemos destacar a transformações simples de preservação de rótulos; a perturbação, que é um termo para “adicionar ruídos”; e a síntese de dados.

Nas transformações simples de preservação de rótulos,  quando aplicada na visão computacional, a técnica mais simples de aumento de dados é modificar aleatoriamente uma imagem preservando seu rótulo. Você pode modificar a imagem cortando, invertendo, girando, invertendo (horizontal ou verticalmente), apagando parte da imagem e muito mais. No processamento de linguagem natural - PNL, você pode substituir aleatoriamente uma palavra por uma palavra semelhante, presumindo que essa substituição não mudaria o significado ou o sentimento da frase. 

A perturbação também é uma operação de preservação de rótulos, mas às vezes é usada para enganar os modelos para que façam previsões erradas. Adicionar amostras ruidosas aos dados de treinamento pode ajudar os modelos a reconhecer os pontos fracos em seus limites de decisão aprendidos e melhorar seu desempenho. O algoritmo DeepFool, encontra a mínima injeção de ruído possível necessária para causar uma classificação incorreta com alta confiança. Este tipo de aumento é chamado de aumento adversário. O aumento adversário é menos comum na PNL (uma imagem de um urso com pixels adicionados aleatoriamente ainda se parece com um urso, mas adicionar caracteres aleatórios a uma frase aleatória provavelmente a tornará algo sem sentido), mas a perturbação tem sido usada para tornar os modelos mais robustos. Um dos exemplos mais notáveis é o BERT.

Como a coleta de dados é cara e lenta, há muitas preocupações potenciais com a privacidade. Embora ainda estejamos longe de sermos capazes de sintetizar todos os dados de treinamento, é possível sintetizar alguns dados de treinamento para aumentar o desempenho de um modelo. Na PNL, os modelos podem ser uma maneira barata de inicializar seu modelo. Já na visão computacional, uma maneira direta de sintetizar novos dados é combinar exemplos. O uso de redes neurais para sintetizar dados de treinamento é uma abordagem interessante que está sendo pesquisada ativamente, mas ainda não é popular na produção.

## Conclusão
	
Tanto o volume como a qualidade dos dados são pontos fundamentais no desenvolvimento dos modelos de ML. Por isso, a forma como esses dados são coletados (ou gerados), interpretados, amostrados, rotulados e tratados merecem cada vez mais atenção. Neste sentido, muitas técnicas foram desenvolvidas para melhorar cada uma dessas etapas do processo de tratamento dos dados até que sejam adequados para o treinamento do modelo. Desta forma vale a pena investir tempo e recurso na aquisição e tratamento dos dados que serão utilizados no desenvolvimento do seu modelo, pois eles podem contribuir direta ou indiretamente para o sucesso ou fracasso do seu projeto.