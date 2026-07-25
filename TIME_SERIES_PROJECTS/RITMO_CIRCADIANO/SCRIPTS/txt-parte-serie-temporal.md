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
