# Moderna: Modeling Volatility with GARCH

Today, we’ll model and forecast Moderna equity volatility (ticker: MRNA) using generalized autoregressive conditional heteroskedacity(GARCH). I was inspired by a recent a16z interview with Moderna’s CEO Stephane Bancel and a Datacamp course. The purpose is to understand and model volatility in MRNA’s returns, particularly during the pandemic, in order to better manage my investment risk and fine-tune my options strategy.

GARCH provides a more realistic, reliable model for volatility (risk) by incorporating the clustered, time-varying character of volatility. Periods of high or low volatility tend to persist – volatility is more likely to be high at time t if it was also high at time t-1. Rising prices tend to be accompanied with falling volatility (steady uptick) and falling prices tend to be accompanied with rising volatility (panic selling). GARCH(p,q) models variance as a weighted average of past residuals up to lag p and weighted average of variance up to lag q. For example, GARCH(1,1) states that variance of time t equals a constant, omega, plus alpha x residual squared of time t-1, plus beta x variance of time t-1:

𝜎2𝑡 = 𝜔 + (𝛼∗𝑟𝑒𝑠𝑖𝑑𝑢𝑎𝑙2𝑡−1) + (𝛽∗𝜎2𝑡−1)

𝛼 represents how volatility reacts to new information. The larger the 𝛼, the larger the immediate impact expressed as residuals (prediction errors). For daily observations, 𝛼 is typically between 0.5 (stable) to around 0.1 (jumpy). The larger the 𝛽, the longer the duration of the impact. 𝛽 is typically between 0.85 and 0.98. Taken together, volatility with high 𝛼 and low 𝛽 tend to be more spiky. Key requirements are:

𝜔, 𝛼, 𝛽 are non-negative
𝛼 + 𝛽 < 1 (mean-reverting)

In the long run, 𝜎 = 𝜔 / (1−𝛼−𝛽)

If our GARCH model is working well, it will account for all predictable components and its residuals will be uncorrelated (white noise).

We’ll also take into account asymmetric shocks with GJR-GARCH and exponential GARCH (EGARCH), which include a conditional parameter for differing impact of bad and good news on volatility. For example, GJR-GARCH is represented as:

𝜎2𝑡 = 𝜔 + (𝛼∗𝑟𝑒𝑠𝑖𝑑𝑢𝑎𝑙2𝑡−1) + (𝛾∗𝐼∗𝑟𝑒𝑠𝑖𝑑𝑢𝑎𝑙2𝑡−1) + (𝛽∗𝜎2𝑡−1)

The additional parameter 𝛾 takes into account asymmetric shock, where I is an indicator function that equals 1 if the unconditional standard deviation is less than 0. A negative and significant 𝛾 means bad news increases volatility more than good news of similar magnitude. A positive and significant 𝛾 means that good news increases volatility more than bad news of the same magnitude.

We’ll specify and fit several models, check goodness of fit, select the optimal model, check parameters and standardized residuals, create a rolling window forecast, backtest, and quantify value at risk (VaR). Try it out on a company you’re interested in! In a future post, we’ll see how GARCH works with a portfolio.

Volatility is only one piece of the puzzle. Next steps:

    Model expected returns separately by fitting an ARIMA model
    Run Monte Carlo simulations
    From those draws, generate a realistic high confidence band for future expected MRNA’s returns
