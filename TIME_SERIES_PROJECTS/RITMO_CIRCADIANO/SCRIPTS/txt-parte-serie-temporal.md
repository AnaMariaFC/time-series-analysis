O mínimo bpm registrado é de 53, que é o estado de repouso. A  máxima bpm foi de 177, que é de exercício físico intenso. A mediana foi de 86 bpm, como ela é um valor que divide os dados no meio, podemos concluir que ela está monstando a tendência central do ciclo de batimentos cardíacos. Com a média de 84,72 bpm, um valor quase igual a mediana, reforça mais ainda que a tendência do ciclo se cencontrar em torno de $(86+84.72)/2=85,36$.
Um jeito melhor de tiramos uma faixa central como sendo uma "*zona habitual de vigília*", o nosso modo acordado, é olhando para o IQR (Inter Quartile Range ou Intervalo Interquartil) pois representa os 50% centrais dos dados. Que vai de *Q_1* = 70 bpm a *Q_3* = 97 bpm.  Logo, a zona habitual de vigília oscila em 27 valores de batimentos.
Com isso podemos tirar conclusões que abaixo do *Q_1* de 70bpm pode indicar um estado de repouso ou trabalho sentado. E acima do *Q_3* de 97bpm pode indicar uma caminhada até a parada do ônibus, uma ansiedade, uma subida as escadas, uma xícara de café,etc.
É considerado um outlier (ou dado discrepante) de acordo com a regra clássica do Boxplot de Turkey, fences rules (regra das cercas), se:
$$\text{Limite Superior} = Q_3 + (1{,}5 \times \text{IQR})$$

Logo, valores acima de 137,5 bpm são considerados outliers.


________codigo de baixo antigo________


```{r}
serieHR2 <- ts(na.omit(listaHeartRate_Filtrada[[1]]$HR))

acf(serieHR2,
    main = "Função de Autocorrelação")
```

O que ele mostra é que:

No lag 0, a autocorrelação é 1, como sempre acontece. Nos primeiros atrasos (lags 1, 2, 3, ...), a autocorrelação continua muito alta (próxima de 1). À medida que o lag aumenta, ela diminui muito lentamente, permanecendo acima de aproximadamente 0,8 mesmo no lag 48.

```{r}
# Calcula a autocorrelação sem desenhar o gráfico
acf_hr <- acf(serieHR2,
              plot = FALSE)

# Visualizar a estrutura
str(acf_hr)

```

**lag (Os passos no tempo):** Indica a distância temporal. O 0 é o momento atual, 1 é 1 passo atrás, 2 são 2 passos atrás, e assim por diante (até 48 passos no seu caso, já que o vetor vai de 1 a 49).

**acf (O coeficiente de correlação):** É um número que varia entre -1 e 1:\
- Correlação positiva perfeita (o lag 0 sempre terá ACF = 1, pois um dado é identicamente igual a si mesmo).\
- 0: Nenhuma correlação (o passado não ajuda a prever o presente).\
- 1: Correlação negativa perfeita (se o passado subiu, o presente cai).

**Altíssima Persistência (Memória Longa):** Note que mesmo após 4 passos (lag 4), a autocorrelação ainda está em 0.974. Isso significa que os batimentos cardíacos mudam de forma gradual. Se o seu coração estava batendo a 100 bpm há 4 segundos/minutos atrás, há uma chance gigantesca de ele continuar muito perto de 100 bpm agora. Os dados têm "memória".

**Série Não-Estacionária (Provavelmente):** Quando o valor da ACF decai de forma muito, muito lenta (como 0.996 -\> 0.990 -\> 0.982), isso é um forte indício estatístico de que a sua série temporal possui uma tendência clara ou não é estacionária. O coração passa longos períodos em patamares diferentes (como o período de sono vs. período acordado que vimos no gráfico anterior).

________________Suavização Exponencial____________

Como a frequência cardíaca oscila bastante, é difícel saber em quantos bpms ela está no momento. "Suavizar" nesse caso seria como achar um ponteiro. Em vez de confiar no valor exato de cada milésimo de segundo, se faz uma média dos últimos segundos para ter uma ideia mais estável e confiável da tendência. Onde os dados mais novos contam mais que os dados mais antigos.

O problema da Média Simples: Se você quiser prever o amanhã usando a média simples dos últimos 10 dias, você dá o mesmo peso para o dia de hoje e para o dia de 10 dias atrás:

    Previsão = (Dia1 + Dia2 + ... + Dia10) / 10

O problema? Se o mercado mudou, se o clima mudou, ou se o comportamento do cliente mudou ontem, a média simples demora 10 dias para perceber essa mudança, porque o dado antigo ainda tem o mesmo peso que o novo.


O que exatamente está sendo "suavizado"?
O nome é confuso porque parece que você está suavizando os dados observados. Mas na verdade, você está suavizando a previsão.
A Suavização Exponencial pega a oscilação bruta dos dados (cheios de ruído) e extrai o que importa: o nível subjacente (a posição média atual) e, nos modelos mais complexos, a tendência (para onde isso está indo) e a sazonalidade (o padrão cíclico).


**1. SES (Simple Exponential Smoothing) - Suavização Exponencial Simples**

    - **Para que serve:** Dados sem tendência e sem sazonalidade (apenas nível).

    - **Como funciona:** A previsão para o próximo período é uma média ponderada entre o último valor observado e a última previsão.

    - **Fórmula conceitual:** Previsão(t+1) = α × (Valor real em t) + (1-α) × Previsão(t)

    - O α (alfa) é o fator de suavização (entre 0 e 1). Quanto maior o α, mais o modelo "reage" a mudanças bruscas; quanto menor, mais "suave" e estável é a linha.
    
**2. Holt (ou Holt Linear) - Suavização Exponencial com Tendência**

    - **Para que serve:** Dados que apresentam tendência (crescimento ou queda constante), mas sem sazonalidade.

    - **Como funciona:** É o SES, mas com uma equação extra para capturar a inclinação da tendência. Ele usa dois fatores de suavização:

        - α (alfa) para suavizar o nível (valor atual).

        - β (beta) para suavizar a inclinação (tendência de crescimento/queda).

    - Ele prevê uma linha reta inclinada para o futuro.
    
**3. Holt-Winters (ou Suavização Exponencial Tripla)**

    - Para que serve: Dados que apresentam tendência + sazonalidade (padrões que se repetem em ciclos, como vendas de Natal, calor no verão, etc.).

    - Como funciona: É o Holt, mas com uma terceira equação para capturar a sazonalidade. Ele usa três fatores de suavização:

        - α (alfa) para o nível

        - β (beta) para a tendência

        - γ (gama) para a sazonalidade

    - Ele ainda se divide em duas versões:

        - Aditivo: Quando a sazonalidade tem amplitude constante (ex: vende-se sempre 1.000 unidades a mais no verão, independente do ano).

        - Multiplicativo: Quando a sazonalidade cresce com o tempo (ex: vende-se 20% a mais no verão; se as vendas sobem, o pico sazonal também sobe).
        
        
*** Por que a suavização exponencial se aplica perfeitamente a dados contínuos?***

- **Ela trabalha com valores reais (contínuos):** Suas equações (nível, tendência e sazonalidade) são baseadas em médias ponderadas e somas. Elas aceitam qualquer número decimal, ao contrário de modelos para dados discretos (como regressão logística, que exige 0 ou 1, ou modelos de contagem, como Poisson).

- **Ela não exige estacionariedade rigorosa:** Diferente de modelos ARIMA (que exigem que a média e a variância sejam constantes ao longo do tempo), a Suavização Exponencial abraça a mudança. Ela foi feita justamente para rastrear a evolução contínua dos dados, ajustando-se suavemente a novas médias e novas inclinações a cada novo ponto registrado.

- **Atualização contínua e recursiva (online learning):** Em dados contínuos (como temperatura, preço de ações, ou sinais de sensores), você recebe medições o tempo todo. A Suavização Exponencial é recursiva: ela não precisa guardar todo o histórico para refazer o cálculo. Ela apenas pega o novo valor, aplica o α, e atualiza o estado atual do modelo. Isso a torna extremamente leve e rápida para streaming de dados.

- **Peso exponencial decai de forma contínua:** Ao contrário da média móvel simples (que corta bruscamente os dados antigos), a Suavização Exponencial atribui pesos que diminuem numa curva suave e contínua (curva exponencial). Isso evita "saltos" bruscos nas previsões quando um dado muito antigo sai da janela de cálculo.
