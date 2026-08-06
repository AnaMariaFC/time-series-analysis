# Introdução

#

## Falar o que é series temporais

### O que é ARIMA (AutoRegressive Integrated Moving Average)

    É o Modelo Autoregressivo Integrado de Média Móvel, usanda para prever séries temporais com tendência, mas sem sazonalidade. Seu objetivo principal é capturar os padrões passados de um conjunto de dados ordenados no tempo para prever seus valores futuros.
    
Por que a ARIMA precisa que os dados sejam estacionários?

    Para que padrões do passado continuem válidos no futuro. Lembrando que para ser estacionário a média deve ser constante, pois não apresenta tendência clara de alta e baixa. Também tem que ter variância constante, já que a dispersão, volatilidade dos dados é estável no tempo. E autocovariância constante, com isso a relação entre dois instantes no tempo depende apenas da distância (*lag*) entre eles, não do momento em que ocorrem. Há mais estabilidade nos dados.

A notação geral: 
                    ARIMA(p,d,q)
                    
Em que:
- AR(p): AutoRegressive (Autoregressivo)
    O parâmetro p representa a quantidade de períodos passados (lags) utilizados como preditores.

- I(d): Integrated (Integrado)
    O parâmetro d indica quantas vezes a operação de diferença foi aplicada. Lembrando que o processo de diferenciação é para tornar a série estacionária, removendo as tendências. Se a série for estacionária originalmente, d=0, o modelo reduz a uma simples ARMA(p,q).

- MA(q): Moving Average (Média Móvel)
    O parâmetro q representa o número de erros passados considerados.

### o que é SARIMA (Seasonal ARIMA)

    É o Modelo Autoregressivo Integrado de Média Móvel **Sazonal**. É como uma extensão da ARIMA para sazonalidade.
    
O que é sazonalidade?

    Padrões que se repetem em intervalos fixos e regulares de tempo, como diariamente, semanalmente, mensalmente ou anualmente.

A notação geral:
                    SARIMA(p,d,q)(P,D,Q)m
                    
Em que:
- p,d,q: É a parte não sazonal, a parte da ARIMA.
- P,D,Q: É a parte sazonal, onde:
        - P: termos autoregressivos sazonais
        - D: diferenças sazonais
        - Q: Termos de médias móveis sazonais
- m: Tamanho do ciclo
