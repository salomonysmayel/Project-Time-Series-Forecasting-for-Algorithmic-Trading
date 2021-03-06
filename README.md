
## Salomon Ysmáyel

# Time Series Forecasting for Algorithmic Trading
## by Multi Step Recurrent Neural Network LSTM Model.


Moving Average Convergence Divergence (MACD) generates a crossover signal for securities technical analysis. MACD is an indicator of momentum that results useful by showing that a reversal on a securities price trend has occurred. By studying the relationship between two moving averages calculated at different periods, one "slow" and one "fast", we can assess when an upward on downward trend is well on the way. This is by no means a future estimation since all the information used to calculate the estimator is the past of the time series itself, but it helps to smooth out the price movements by doing some depuration of the noise. The signal that a reversal has occurred is when the graph of the slow and fast Exponential Moving Averages cross. This is called Crossover signal.

The MACD has proven useful at Algorithmic Trading. It doesn't mean that by just following the signal we are going to be able to profit. Just following the signal would always put the investor in a "late to the party" situation. But it can help prevent mayor loses or ride on some upward momentum to recover certain position. Especially if the trend proves to be strong. So having the signals well tuned can be very useful to support an algorithmic trading strategy. It gives a good picture of what is already happening. 

The question that fueled this project was. Is there a way to forecast the crossover signal a bit sooner than by just MACD using Deep Learning? This would make the MACD even more useful for algorithmic trading purposes. As it happens, there is a Deep Neural Network model that is especially good at working with time series, Recurrent Neural Networks. When time has an influence on the features, then using a regular feedforward Neural Network we lose important information that is related to time. RNNs are good at studing sequences to predict what may come next even several steps ahead. This is achieved by taking each input as a sequence where the first section acts as the features and the following section as the target. Then we start feeding the network with these sequences (in this case the closing price of certain stock) and moving forward from the first point until we reach the end of the data. The sub sequences all have the same size and contain both our features and targets. To test the model we use future points in time that the training process never saw.

The values of Fast and Slow EMA were used to train a RNN model. Once the model was trained a future sequence was extracted from the historic prices and given to the model to predict the next 6 days. This was performed on days were the a crossover signal was generated. For instance, if the MACD shows a signal on 12-31-2019 the data given to the trained model is the prices from  12-01-2019 to 12-24-2019 to forecast the prices from 12-25-2019 to 12-31-2019 and see if the predicted values would forecast the signal sometime prior to 12-31-2019. 

The data used was the closing prices for Devon Energy Company, because of its volatility. A MACD on this data generates many crossover points. The section of the time series used to train the model was from 02-17-2015 to 4-27-2018. And the data used to predict was from 4-27-2018 to the present.

The following is a plot of the historic prices for Devon Energy and the MACD with short window of 10 days and long window of 40 days. The crossover signals are shown.

![model setup](/images/crossover.png)

Four signal points from the sequence 4-27-2018 to the present were studied.

![model setup](/images/real1.png)

![model setup](/images/real2.png)

![model setup](/images/real3.png)

![model setup](/images/real4.png)

The model was trained on both the slow and the fast time series. one for the 10 day window and one for the 40 day window sequence. The model is a stacked recurrent neural network. 

To prepare the data fitted on the model first we have to split it, this is done usign the function sequence_split that takes three input values. The whole sequence itself, the number of steps in (for each sub sequence) and the number of steps out or targets. This function returns two arrays, one containing all the sequences n_steps_in long (features) and one containing the sequences n_steps_out long (targets). Then all this time series are passed one by one to the network for training in the form of arrays. The hyper parameters values are: 100 neurons for the first two networks and the last stacked network having n_steps_out neurons, since we want to obtain n_steps_out days of prices in the future. In this case 6. 100 was chosen with trial and error. 100 gave results better than 200 and 10. The activation function used was ReLu because of if effectiveness on most Deep learning algorithms. 

![model setup](/images/model.png)

The results obtained are the following:

![model setup](/images/result1df.png)

![model setup](/images/result2df.png)

![model setup](/images/result3df.png)

![model setup](/images/result4df.png)

Plotting the real crossover signal along with the forecasted crossover signal we can see that in 3 of the 4 studied cases the model predicted a crossover signal before the MACD. 

![model setup](/images/result1plot.png)

![model setup](/images/result2plot.png)

![model setup](/images/result3plot.png)

![model setup](/images/result4plot.png)

This results show that there is a possibility that an RNN can be used to refine trend analysis making certain technical indicators more powerful. 
