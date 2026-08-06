---
title: "Rítimo Circadiano"
author: "Ana Maria F Costa"
format: html
editor: visual
---

# Estudo do Rítimo Circadiano

O banco de dados foi tirado do site da PhysioNet, seque link: <https://physionet.org/content/mmash/1.0.0/> . É um site inteiro com banco de dados de "sinais fisolígicos complexos", como é dito no site. É mantido pelos: Laboratório de Fisiologia Computacional do MIT, apoiado pelo Instituto Nacional de Imagens Biomédicas e Bioengenharia (NIBIB), e pelo, Instituto Nacional do Coração, Pulmão e Sangue (NHLBI).

O dataset é o do "Monitoramento multinível da atividade e do sono em pessoas saudáveis", só com 22 homens.

A análise é feita pelo ActiGraph wGT3X-BT. Um **actigraph** é um dispositivo vestível que mede: movimento corporal, atividade física, padrões de sono e repouso. Eles detectam o movimento, a intensidade, a postura e a direção. Na prática é tipo um smartwatch científico.

## Primeira análise:

- movimento corporal

- atividade física

- padrões de sono

- repouso

### As variáveis medem:

- Axis1/2/3 - aceleração nos eixos

- Steps - passos

- HR - frequência cardíaca

- Inclinometer - posição corporal

- Vector Magnitude - intensidade total do movimento

- Os dados são de 2 dias, o tempo é medido em segundos.

Primeiro vamos analisar a frequência cardíaca que está na coluna HR. Junto com ela a coluna Gender (Gênero) e Age (idade) do user_info.csv, para calcular o HR máx.

```{r}
# Importando os dados:

# 1. Criando os caminhos dos arquivos de forma limpa
caminhos_actigraph <- paste0("/home/delfos/Documentos/UEPB_2026jun28/PEDRO_MONTEIRO/TIME_SERIES_ANALYSIS/TIME_SERIES_PROJECTS/RITMO_CIRCADIANO/DATA/DB/DataPaper/user_", 1:22, "/Actigraph.csv")

caminhos_info <- paste0("/home/delfos/Documentos/UEPB_2026jun28/PEDRO_MONTEIRO/TIME_SERIES_ANALYSIS/TIME_SERIES_PROJECTS/RITMO_CIRCADIANO/DATA/DB/DataPaper/user_", 1:22, "/user_info.csv")

#Carregando as listas SEM usar o laço FOR (Muito mais seguro)
listaHeartRate <- lapply(caminhos_actigraph, read.csv)
listaInfoUsers <- lapply(caminhos_info, read.csv)




```

```{r}
# Verificando os dados do Actigraph.csv

View(listaHeartRate[[1]])
str(listaHeartRate[[1]])  #Por aqui sei a quant de observações e variáveis e o tipo.
names(listaHeartRate[[1]])
#Nomes das colunas.
#Por aqui sei que os dados estão em dias/horas até segundos.
```

```{r}
# Verificando os dados do user_info.csv

View(listaInfoUsers[[1]])
str(listaInfoUsers[[1]])  #Por aqui sei a quant de observações e variáveis e o tipo.
names(listaInfoUsers[[1]])
#Nomes das colunas.
#Por aqui sei que os dados estão em dias/horas até segundos.
```

```{r}
# Análise Exploratória do Actigraph.csv

# Conta quantos valores "NA" existem no seu arquivo
sum(is.na(listaHeartRate[[1]]))
# Estatística Descritiva
lapply(listaHeartRate,summary)
# Só col HR
lapply(listaHeartRate,function(x) summary(x$HR))
```

```{r}
# Análise Exploratória do user_info.csv

# Conta quantos valores "NA" existem no seu arquivo
sum(is.na(listaInfoUsers[[1]]))

# Forçando o R a pegar APENAS a primeira linha de cada user_info 
# (Isso mata qualquer linha em branco extra que esteja gerando os NAs no rbind)
listaInfoUsers_limpa <- lapply(listaInfoUsers, function(df) df[1, , drop = FALSE])

# Empilhando rows
descritiva_info_users<-do.call(rbind,listaInfoUsers_limpa)
#do.call
#rbind = row binb = juntar linhas

summary(descritiva_info_users)

#Quem tem Age=0 ?
descritiva_info_users[descritiva_info_users$Age==0,]
#User18

# Substituindo o 0 por 27 (mediana)
descritiva_info_users$Age[descritiva_info_users$Age == 0] <- 27

descritiva_info_users

summary(descritiva_info_users)
# Age min = 20 OK
```

No User 18 a coluna idade está 0. Coloquei a mediana das idades (27 anos), pois precisarei calcular o HR_máx, o cálculo é dado pela idade.

Olhando para as estatísticas descritivas do HR (Heart Rate = Frequência cardíaca) há alguns erros de amplitude.

- Mínimos absurdos: O *User 3*, *User 5*, *User 9* e o *User 19* apresentam mín de 3 bpm. O *User 10* tem mínimo de 8 bpm. O *User 2* tem mínimo de 10 bpm.

  OBS: Nosso coração tem de 60 à 100 bpm em repouso, sendo um adulto saudável médio. E de 40 à 60 bpm em atletas ou pessoas com bom condicionamento físico. O coração é mais eficiente, ele bombeia mais sangue por batimento. **Valores de alerta:** Abaixo de 50 bpm em não-atletas (Bradicardia) ou acima de 100 bpm constantemente (Taquicardia) merecem investigação médica.

- Máximos absurdos:

  Terei que calcular com a idade a frequência máx para cortar os outliers. Para cada indivíduo haverá um HR máx diferente.

  Estão divididos em:

  - Repouso
  - Zona 1 (Leve)
  - Zona 2 (Moderada)
  - Zona 3 (Aeróbica)
  - Zona 4 (Limiar)
  - Zona 5 (Máxima)

```{r}
  # Calculando O HR máx de cada users.
  # Zona(repouso/exercícios)
  listaHeartRate_comZonas<-lapply(1:22,function(i){
    # Extrai o df só do user i do momento.
    dfActigraph<-listaHeartRate[[i]]
    # Extrai o df só do user i do momento.
    dfInfo<-listaInfoUsers[[i]]
    # [1] é pq tem só 1 line
    idadesUsers<-dfInfo$Age[1]
    # Calculando o HR máx teórico
    hrMaxTeorico<-208-(0.7*idadesUsers)
    # Criando as novas colunas no df HR
    dfActigraph$HR_Max<-hrMaxTeorico
    dfActigraph$Esforco_por_cento<-(dfActigraph$HR/hrMaxTeorico)*100
    # Classifica as zonas usando a função cut()
    dfActigraph$Zona_Treino <- cut(
      dfActigraph$Esforco_por_cento,
      breaks = c(-Inf, 50, 60, 70, 80, 90, Inf),
      labels = c("Repouso", "Zona 1 (Leve)", "Zona 2 (Moderada)",
                 "Zona 3 (Aeróbica)", "Zona 4 (Limiar)", "Zona 5 (Máxima)"),
      right = TRUE
    )
    # Colocando tudo no seu lugar de volta
    return(dfActigraph)
  })
  # Dando nomes as listas
    names(listaHeartRate_comZonas)<-paste0("User_",1:22)
```

```{r}
# Vizualizando
#View(listaHeartRate_comZonas)
#View(listaHeartRate_comZonas[[7]])
```

Foi criado mais três colunas:\
- HR_max (a frequência cardíaca máxima) - Esforco_por_cento (a porcentagem do HR, vizualiza melhor o quanto de esforço o indivíduo faz) - Zona_Treino\
- Repouso - Zona 1 (Leve) - Zona 2 (Moderada) - Zona 3 (Aeróbica) - Zona 4 (Limiar) - Zona 5 (Máxima)

```{r}
listaHeartRate_Filtrada<-lapply(listaHeartRate_comZonas,function(df){
  # Filtra o HR memor que 35 e maior que o HR_Max calculado
  df$HR[df$HR < 35 | df$HR > df$HR_Max] <- NA
  return(df)
})

# Resumo descritivo da coluna HR para cada um dos 22 usuários de uma vez só:
resumo_geral <- do.call(rbind, lapply(listaHeartRate_Filtrada, function(df) {
  summary(df$HR)
}))
rownames(resumo_geral) <- names(listaHeartRate_Filtrada)
# Exibe uma tabela limpa com Min, Max, Média de todos
print(resumo_geral)
```

A análise exploratória revelou artefatos de medição severos nos dados de frequência cardíaca bruta (com registros implausíveis variando de 3 a 8 bpm). Após a aplicação de um filtro fisiológico (limiar inferior de 35 bpm e superior baseado no $HR_{máx}$ teórico de cada indivíduo), observou-se uma perda de dados proeminente em usuários específicos, com destaque para o User_15 (1.450 dados omissos). Essa perda é atribuída a limitações práticas no acoplamento da cinta peitoral necessária para o registro de HR no dispositivo ActiGraph wGT3X-BT, causadas por deslocamento mecânico durante o sono ou perda temporária de condutividade elétrica na pele.

# Parei aqui

Como a quantidade de NAs perto do total de dados (67.936) é muito pequena (1.450 não chega a 2% do total do User 15), nós podemos imputar esses dados (preencher os vazios) usando uma técnica de séries temporais chamada LOCF (Last Observation Carried Forward), que basicamente repete o último valor válido até o sensor voltar a funcionar.


### Série Temporal

# Caçar o padrão apneia do sono

```{r}
#Criando a Série Temporal

serieHeartRate<-ts(dadosHeartRate$HR)
plot(serieHeartRate,
     type="l",  #Gráfico desenhado c/line conectando os pontos.
     main="Série Temporal de Frequência Cardíaca",
     ylab="HR",
     xlab="Tempo (segundos)"
     )


# Aplicando o operador diferença:

serieHeartRate_diff<-diff(serieHeartRate)
plot(serieHeartRate_diff,
     type="l",  #lines
     main="Série Temporal Diferenciada",
     ylab="Diferença",
     xlab="Tempo (segundos)"
     )
```

```{r}
# Em horas:

# Série original:
tempo_horas <- (1:length(serieHeartRate))/3600

plot(tempo_horas[1:5000],
     serieHeartRate[1:5000],
     type = "l",
     main = "Série Temporal da Frequência Cardíaca",
     ylab = "HR",
     xlab = "Tempo (horas)")

# Série com o operador diferença:
plot(tempo_horas[1:4999],
     serieHeartRate_diff[1:4999],
     type = "l",
     main = "Série Temporal Diferenciada",
     ylab = "Diferença",
     xlab = "Tempo (horas)")
```

A série temporal representa medições contínuas da frequência cardíaca (HR) ao longo do tempo, registradas em intervalos de segundos. Contendo 67 936 observações de 13 variáveis de um único usuário do actigraph.Em seguida, aplicou-se o operador diferença para analisar as variações entre observações consecutivas e avaliar o comportamento estacionário da série.

\
A série diferenciada ficou com média oscilando em torno de zero, sem tendência, mostrando apenas variações instantâneas. Mas visível no boxplot. Essas variações instantâneas são picos extremos de mudança brusca da frequência cardíaca. Pode ser uma atividade física intensa, um erro de medição ou ruído.

```{r}
# Calculando a média móvel:

media_movel <- filter(serieHeartRate,
                      rep(1/500, 500),
                      sides = 2)

plot(serieHeartRate,
     type = "l",
     main = "Série Temporal com Média Móvel",
     ylab = "HR",
     xlab = "Tempo")

lines(media_movel,
      col = "red",
      lwd = 2)


# Vizualizando a média pelo box plot:

boxplot(split(serieHeartRate,
              ceiling(seq_along(serieHeartRate)/10000)),
        main = "Boxplots por Blocos de Tempo",
        xlab = "Blocos",
        ylab = "HR")
```
