# Bitcoin-Price-Predictor
I wanted to make a proof of principle code book that used random forest on historical macroeconomic metrics to predict the price of Bitcoin in 7 days time. My plan is to create a functional tool that can be used on any stock ticker or crypto. I am hoping to turn this into a functional app at some point, but I need to conduct a lot of experiments to hopefully make it as accurate / error free as possible.

I used a Random Forest Regressor on the last 5 years of price movements for BTC (bitcoin). This can be sources from yfinance, a handy Python Library you can incorporate into Python.

I chose Random Forest because it is good on capturing complex, non-linear relationships in financial markets. It averages predictions across multiple decision trees, and is good at avoiding overfitting. It can also be trained surprisingly fast (a few seconds on a H100!) [this suggests I could add a lot more variables into the data in future).

I decided to see if the price of USD, Etherium, GLD, and the S&P500 impacted BTC price. There is lots of debate on how macroecononic trends influences prices of things like gold, bitcoin etc, but I wanted to let the AI find the patterns and hopefully cut through the subjective noise. I also looked at 5 and 20 day moving averages, which is useful to see overall volatility in BTC prices.

Specifically:
ETH - I figured it would be highly correlated with BTC.
GLD - As a safe haven asset, I wondered if its price was linked to BTC.
S&P500 - THis measures the overall market.
USD - People say a weaker dollar correlates with capital going into risky assets like BTC, as those assets become cheaper.

To avoid a look ahead buas, I used information from the previous day.

Metric used was RMSE, which penalises larger errors. Using this metric penalises errors highly. Using RF also allows the model to operate as a black box in the sense it avoids noise and just focuses on the meaningful variables.

Running the model over the split data from the last 5 years yielded these results:


Training Random Forest Regressor on Lagged Features (No Dates)...
Training complete.

--- Random Forest Validation Results (Lagged Features) ---
New RMSE: 0.071355

--- Top 5 Feature Importances ---

^TNX       0.224828

ETH-USD    0.152131

Std_20     0.118769

MA_5       0.104065

MA_20      0.100474

A RMSE of .07 means the predictions are around 7% wrong, which is obviously a problem for an accurate predictor on a asset like bitcoin, but it's a good start I think.

Also what was interesting was how the most important factor affecting BTC's price was (unexpectedly tbh) the ten year treasury bond yield.

I will try to explain why:

Bond yields and the price of risky assets usually have an inverse relationship ( https://www.investopedia.com/terms/b/bond-yield.asp ) because when yields rise, bonbd become more attractive to investors. But, BTC has now become a long term asset more than a risky short term bet, so it was interested to see that it seems people still see BTC as a speculative asset, and not like a 'normal' fita curreny (seems the USD is safe for now).

Price of Etherium was correlated to BTC price, which is no surprise really. People likely buy bitcoin for the same reason they buy any other crypto.

Going forward, I will do the following:

Establish Baseline Performance: 
Crucially, compare against a simple Null Model.Why: A Null Model (e.g., predicting the average historical 7-day return every time) provides the minimum standard. If the complex Random Forest does not outperform this baseline, its complexity is not justified.

Model Diversity: Test other sequential/time-series models like Gradient Boosting Machines (GBM) or Recurrent Neural Networks (RNNs) (e.g., LSTMs) to potentially capture more complex temporal dependencies.

Feature Engineering: Introduce Fourier transforms or Wavelets to extract time-frequency-domain features, which often improve signal processing in financial data.

Volatility Modelling: Add features derived from GARCH (Generalised Autoregressive Conditional Heteroskedasticity) models to better forecast market risk and volatility.

Hyperparameter Optimisation: Perform a systematic search (e.g., Grid Search or Bayesian Optimisation) for the optimal Random Forest parameters to squeeze out better performance.Signal Evaluation:

Beyond RMSE, evaluate the model's performance using financial metrics like the Sharpe Ratio or Maximum Drawdown on a simulated trading strategy to better assess its practical, risk-adjusted profitability.

Over the next few weeks I will broaden this out to include things like gold price. I hope to be able to use any sticker, not just BTC using the same model and system to find a fairly accurate tool people can use to predict stocks going forwards. Also, it does not need to be 7 days neccesarily, I can broaden (or shorten) the time horizon, but I did this just to get the ball rolling and provide an example of how AI can be used to predict stocks/crypto prices based on historical data.

