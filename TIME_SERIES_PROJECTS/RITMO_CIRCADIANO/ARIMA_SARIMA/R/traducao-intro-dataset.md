# TRADUÇÃO DO SITE DO DATASET
https://physionet.org/content/mmash/1.0.0/

## Multilevel Monitoring of Activity and Sleep in Healthy People
(Monitoramento multinível da atividade e do sono em pessoas saudáveis.)

 Alessio Rossi ,  Eleonora Da Pozzo ,  Dario Menicagli ,  Chiara Tremolanti ,  Corrado Priami ,  Alina Sirbu ,  David Clifton ,  Claudia Martini ,  David Morelli

Published: June 19, 2020. Version: 1.0.0 

## Resumo
O conjunto de dados MMASH (Monitoramento Multidimensional da Atividade e do Sono em Pessoas Saudáveis) fornece 24 horas de dados contínuos de batimento cardíaco, dados de acelerômetro triaxial, qualidade do sono, atividade física e características psicológicas (ou seja, nível de ansiedade, eventos estressantes e emoções) de 22 participantes saudáveis. Além disso, biomarcadores salivares (ou seja, cortisol e melatonina) e um registro de atividades também foram incluídos neste conjunto de dados. O conjunto de dados MMASH permitirá que pesquisadores testem as correlações entre atividade física, qualidade do sono e características psicológicas.

## Contexto
Dispositivos vestíveis de monitoramento de atividades, que coletam dados 24 horas por dia, 7 dias por semana, têm se tornado cada vez mais populares para monitorar a atividade física, a frequência cardíaca (FC) e a qualidade do sono. A combinação desses dados possibilita o desenvolvimento de ferramentas capazes de prever o bem-estar dos usuários. Acreditamos que esses dados podem ser extremamente benéficos para a comunidade científica, pois podem contribuir para pesquisas em diversas áreas, permitindo a avaliação das relações entre características físicas, psicológicas e fisiológicas.

## Métodos

Os dados foram coletados e fornecidos pela BioBeats (biobeats.com) em colaboração com pesquisadores da Universidade de Pisa. A BioBeats atua no setor de ciências da saúde, produzindo dispositivos vestíveis de IoT com o objetivo de detectar o estresse psicofisiológico das pessoas. Os dados foram registrados por cientistas do esporte e da saúde, psicólogos e químicos com o objetivo de avaliar a resposta psicofisiológica a estímulos estressantes e o sono.

Foram recrutados 22 homens jovens e saudáveis. Antes do início do estudo, os participantes assinaram um termo de consentimento livre e esclarecido. Este termo forneceu informações sobre o protocolo de pesquisa, os possíveis riscos e o uso dos dados, em conformidade com o Regulamento Geral de Proteção de Dados (RGPD): Regulamento (UE) 2016/679 do Parlamento Europeu e do Conselho, de 27/04/2016, relativo à proteção das pessoas singulares no que diz respeito ao tratamento de dados pessoais e à livre circulação desses dados. De acordo com a Declaração de Helsínquia, revisada em 2013, o estudo foi aprovado pelo Comitê de Ética da Universidade de Pisa (nº 0077455/2018). No início da coleta de dados, as características antropométricas (idade, altura e peso) dos participantes foram registradas. Simultaneamente, os participantes responderam a um conjunto de questionários iniciais que forneciam informações sobre seu estado psicológico: Questionário de Matutinidade-Vespertinidade (MEQ), Inventário de Ansiedade Traço-Estado (STAI-Y), Índice de Qualidade do Sono de Pittsburgh (PSQI) e Escala de Evitação/Inibição Comportamental (BIS/BAS). Durante o teste, os participantes utilizaram dois dispositivos continuamente por 24 horas: um monitor de frequência cardíaca (Mostrador de frequência cardíaca Polar H7 - Polar Electro Inc., Bethpage, NY, EUA) para registrar os batimentos cardíacos e o intervalo entre batimentos, e um actígrafo (ActiGraph wGT3X-BT - ActiGraph LLC, Pensacola, FL, EUA) para registrar informações actigráficas, como dados de acelerômetro, qualidade do sono e atividade física. Além disso, o humor percebido (Escala de Afetos Positivos e Negativos - PANAS) foi registrado em diferentes horários do dia (ou seja, às 10h, 14h, 18h, 22h e 9h do dia seguinte). Adicionalmente, os participantes preencheram o Inventário Diário de Estresse (IDE) antes de dormir, para resumir os eventos estressantes do dia.

Duas vezes ao dia (antes de dormir e ao acordar), os participantes coletaram amostras de saliva em casa, em frascos apropriados. As amostras de saliva foram utilizadas para extrair RNA e medir a indução de genes específicos do relógio biológico, bem como para avaliar hormônios específicos. Um período de abstinência de drogas de pelo menos uma semana foi exigido dos participantes do estudo.

## Descrição dos Dados

O MMASH consiste em sete arquivos para cada participante (a descrição de cada coluna do arquivo CSV é fornecida abaixo):

#### user_info.csv - características antropométricas do participante:

- gênero: M e F referem-se a Masculino e Feminino, respectivamente.

- altura expressa em centímetros (cm).

- peso expresso em quilogramas (kg).

- idade expressa em anos.

#### sleep.csv - informações sobre a duração e a qualidade do sono do participante:

- Data de Deitar: 1 e 2 referem-se ao primeiro e segundo dia de coleta de dados, respectivamente.

- Horário de Deitar: horário do dia (horas:minutos) em que o usuário foi para a cama.

- Data de Levantar: 1 e 2 referem-se ao primeiro e segundo dia de coleta de dados, respectivamente.

- Horário de Levantar: horário do dia (horas:minutos) em que o usuário saiu da cama.

- Data de Início: 1 e 2 referem-se ao primeiro e segundo dia de coleta de dados, respectivamente.

- Hora de Início: horário do dia (horas:minutos) em que o usuário adormece.

- Eficiência de Latência: porcentagem do tempo de sono em relação ao tempo total de sono na cama.

- Total de Minutos na Cama: minutos passados ​​na cama por noite.

- Tempo Total de Sono (TTS): duração do sono por noite, expressa em minutos.

- Tempo Acordado Após o Início do Sono (TAS): tempo gasto acordado após adormecer pela primeira vez.

- Número de Despertares durante a noite

- Duração Média do Despertar: tempo em segundos gasto acordado durante a noite.

- Índice de Movimento: número de minutos sem movimento, expresso como uma porcentagem da fase de movimento (ou seja, número de períodos com movimento do braço).

- Índice de Fragmentação: número de minutos com movimento, expresso como uma porcentagem da fase imóvel (ou seja, número de períodos sem movimento do braço).

- Índice de Fragmentação do Sono: razão entre os índices de Movimento e Fragmentação.

#### RR.csv - dados de intervalo batimento a batimento:

- ibi_s: tempo em segundos entre dois batimentos consecutivos.

- Dia: 1 e 2 referem-se ao primeiro e segundo dia de coleta de dados, respectivamente.

- Hora: horário do dia em que ocorreu a batida do coração (horas:minutos:segundos)

#### questionário.csv - pontuações para todos os questionários:

MEQ: Valor do Questionário de Matutinidade-Vespertinidade. A pontuação do cronotipo varia de 16 a 86: pontuações de 41 ou menos indicam tipos vespertinos, pontuações de 59 ou mais indicam tipos matutinos e pontuações entre 42 e 58 indicam tipos intermediários [1].

STAI1: Valor da Ansiedade de Estado obtido pelo Inventário de Ansiedade Traço-Estado. Os resultados variam de 20 a 80. Pontuações inferiores a 31 podem indicar ansiedade baixa ou ausente, pontuações entre 31 e 49 indicam um nível médio de ansiedade ou níveis limítrofes e pontuações superiores a 50 indicam um alto nível de ansiedade ou resultados positivos no teste [2].

STAI2: Valor da Ansiedade Traço obtido pelo Inventário de Ansiedade Traço-Estado. Os resultados variam de 20 a 80. Pontuações inferiores a 31 podem indicar baixa ou nenhuma ansiedade, pontuações entre 31 e 49 indicam um nível médio de ansiedade ou níveis limítrofes, e pontuações superiores a 50 indicam um alto nível de ansiedade ou resultados positivos nos testes [2].

PSQI: Índice de Qualidade do Sono de Pittsburgh. Ele fornece uma pontuação de 0 a 21, com valores inferiores a 6 indicando boa qualidade do sono [3].

BIS/BAS: Índice de evitação/inibição comportamental [4]. As escalas BIS/BAS são uma medida típica da teoria da sensibilidade ao reforço que estabelece raízes biológicas em características de personalidade, derivadas de diferenças neuropsicológicas. As escalas BIS/BAS compreendem uma medida de autorrelato de tendências de evitação e aproximação que contém quatro subfatores (uma pontuação alta em uma das subescalas descreve o grau dessa característica temperamental para o indivíduo, de acordo com a amostra original):

- A faceta BIS reflete a sensibilidade do sujeito a eventos aversivos que promovem comportamentos de evitação.
- A motivação descreve a persistência individual e a intensidade motivacional.
- A recompensa corresponde à responsividade à recompensa, que indica uma propensão a demonstrar um maior grau de emoção positiva para a conquista de objetivos.
- A diversão corresponde à busca por diversão, que está relacionada à impulsividade e à recompensa imediata devido a estímulos sensoriais ou situações de risco.

Estresse diário: O Inventário de Estresse Diário (DSI) é uma medida de autoavaliação com 58 itens que permite à pessoa indicar os eventos que vivenciou nas últimas 24 horas. Após indicar qual evento ocorreu, ela indica o nível de estresse do evento em uma escala Likert de 1 (ocorreu, mas não foi estressante) a 7 (me fez entrar em pânico). A pontuação varia de 0 a 406. Quanto maior o valor, maior a frequência e a intensidade dos eventos e o estresse diário percebido [5].

PANAS: Escala de Afetos Positivos e Negativos. Atribui uma pontuação entre 5 e 50 para emoções positivas e negativas [6]. Quanto maior o valor do PANAS, maior a emoção percebida. As colunas com os números 10, 14, 22 e 9+1 referem-se ao horário do dia em que o questionário foi preenchido. 9+1 indica as 9h da manhã do segundo dia de coleta de dados.

#### Activity.csv - lista das categorias de atividades ao longo do dia. As categorias são (as atividades listadas abaixo correspondem ao ID numérico de cada atividade no arquivo CSV):

1. Dormir.

2. Deitar.

3. Sentar, por exemplo, estudar, comer e dirigir.

4. Movimento leve, por exemplo, caminhada lenta/moderada, tarefas domésticas e trabalho.

5. Movimento moderado, por exemplo, caminhada rápida e andar de bicicleta.

6. Movimento intenso, por exemplo, academia, corrida.

7. Comer.

8. Uso de telas pequenas, por exemplo, smartphone e computador.

9. Uso de telas grandes, por exemplo, TV e cinema.

10. Consumo de bebidas com cafeína, por exemplo, café ou refrigerante.

11. Fumar.

12. Consumo de álcool. As colunas 'Início' e 'Fim' referem-se à hora do dia (horas:minutos) em que o evento ocorreu, enquanto a coluna 'Dia' refere-se ao dia em que ocorreu (1 e 2 referem-se ao primeiro e segundo dia de registro de dados, respectivamente).

#### Actigraph.csv - dados do acelerômetro e inclinômetro registrados ao longo do dia:

- Eixo 1: Dados brutos de aceleração do eixo X expressos em Newton-metro.

- Eixo 2: Dados brutos de aceleração do eixo Y expressos em Newton-metro.

- Eixo 3: Dados brutos de aceleração do eixo Z expressos em Newton-metro.

- Passos: número de passos por segundo.

- FC: batimentos por minuto (bpm).

- Inclinômetro Desligado: valores iguais a 1 referem-se à desativação do inclinômetro. Os valores são registrados por segundo.

- Inclinômetro em Pé: valores iguais a 1 referem-se à posição em pé do usuário, enquanto 0 refere-se a outras posições do usuário. Os valores são registrados por segundo.

- Inclinômetro Sentado: valores iguais a 1 referem-se à posição sentada do usuário, enquanto 0 refere-se a outras posições. Os valores são reportados por segundo.

- Inclinômetro Deitado: valores iguais a 1 referem-se à posição deitada do usuário, enquanto 0 refere-se a outras posições. Os valores são reportados por segundo.

- Magnitude Vetorial: movimento vetorial derivado dos dados brutos de aceleração, expresso em Newton-metro.

- Dia: 1 e 2 referem-se ao primeiro e segundo dia de registro de dados, respectivamente.

- Hora: horário do dia em que a batida cardíaca ocorreu (horas:minutos:segundos)

#### saliva.csv -
concentrações de genes do relógio biológico e hormônios na saliva antes de dormir e ao acordar. Duas amostras por participante estão incluídas, uma antes de dormir e outra ao acordar, conforme indicado na coluna de dados "Amostra". Os níveis de melatonina são apresentados em μg de melatonina por μg de proteína, enquanto os níveis de cortisol são apresentados em μg de cortisol por 100 μg de proteína. Não foram fornecidos dados de concentração de genes do relógio biológico e hormônios para o Usuário_21 devido a um problema nas amostras de saliva que não permitiu a análise.

#### Notas de Uso

Até onde sabemos, o MMASH é o primeiro conjunto de dados a fornecer diversos aspectos da vida cotidiana das pessoas, como respostas cardiovasculares, percepções psicológicas (por exemplo, estresse, ansiedade e emoções), qualidade do sono, informações sobre movimento (por exemplo, dados de acelerômetro de pulso e passos) e descrições de atividades por hora. Devido à complexidade desses dados, especialistas de diversas áreas de pesquisa podem utilizá-los para investigar a relação entre vários aspectos das respostas psicofisiológicas, obtendo uma visão completa da vida diária dos usuários. Por exemplo, é possível investigar a relação entre a qualidade do sono percebida (PSQI) e a observada (por exemplo, melatonina, cortisol, índice de fragmentação do sono e duração do sono) por características individuais, como estresse diário, nível de ansiedade, emoções percebidas ao longo do dia anterior e atividades diárias. Além disso, algoritmos de aprendizado de máquina podem ser desenvolvidos para detectar atividades diárias, humor, emoções, predisposição individual a reagir a eventos aversivos ou positivos e estresse, com base em respostas cardiovasculares (por exemplo, frequência cardíaca e variabilidade da frequência cardíaca) e/ou dados de actigrafia. Esses algoritmos poderiam ser usados ​​para prever a rotina das pessoas utilizando dados de acelerômetros e respostas cardiovasculares que são continuamente registradas por dispositivos de pulso, cada vez mais populares graças aos avanços tecnológicos das últimas duas décadas. Esses são apenas alguns exemplos de todos os possíveis tópicos de pesquisa que poderiam surgir com o uso desse conjunto de dados. A principal razão para o lançamento do MMASH é a dificuldade de coletar esse tipo de dado por um longo período. Esse conjunto de dados proporcionaria a pesquisadores e empresas a oportunidade de obter dados fundamentais sobre diversas respostas psicofisiológicas para desenvolver modelos preditivos e, assim, avaliar passivamente o cotidiano das pessoas por meio de dispositivos de pulso que estimam seu bem-estar.


