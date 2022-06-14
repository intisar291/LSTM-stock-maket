# LSTM-stock-maket
### __**Intro**__ 
An accurate future movement of the financial time series data with respect to its high variation and complex non-linear dimensionality is becoming big challenge for many professional analysts and investors.This analysis used the closing price return to predict the future prices. The output of our model is the total return of the next ten days prediction. Once the future price is predicted, we will build up a quantitative trading strategy based on the prediction.
We have chosen this [**LSTM**](https://en.wikipedia.org/wiki/Long_short-term_memory) model for some certain reasons. First is to overcome the vanishing gradients problem of a traditional recurrent neural network (RNN) model and predict the stock price movement more accurately. Secondly, we want to introduce our predicted value as a continuous value.

Share types are divided into 3 parts.
- Large caps
- Indices
- Comodities & Currencies

### **Data & Model**

_**Dataset**_-->> We selected 15 stock indices to test the prediction ability of the proposed model. All of these data are collected from Yahoo! Finance and Onvista.

_**Outlier detection**_-->> For removing outliers from the data, we introduced [**z-score**](https://medium.com/clarusway/z-score-and-how-its-used-to-determine-an-outlier-642110f3b482). Any value with z- score greater than 3 has been removed as an outlier.

_**Data scaling**_-->> We have used [**MinMax scaler**](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html) to normalize our data and keep the values within the range between 0 to 1. We have selected 80% of data for training and 20% for testing.

_**Features**_-->> We have selected the [**log returns**](https://investmentcache.com/magic-of-log-returns-concept-part-1/) of the closing price as our features to train the model.

_**Model Setup**_-->>
Input time step– Our “look back” or previous time step as input is 90. It indicates that our model will take last 90 days share prices as input to predict the price of next 91th day.Number of neurons and layer-There are 2 layers in this model. First layer consists of 70 neurons and the last layer, which is [**‘Dense’**](https://analyticsindiamag.com/a-complete-understanding-of-dense-layers-in-neural-networks/#:~:text=Like%20we%20use%20LSTM%20layers,stages%20of%20the%20neural%20network) (output layer), is consist of 1 neurons.

Activation functions– In this LSTM model, there are two default [**activation function**](https://towardsdatascience.com/activation-functions-neural-networks-1cbd9f8d91d6); [**‘tanh’**](https://www.baeldung.com/cs/sigmoid-vs-tanh-functions) & [**‘sigmoid’**](https://www.baeldung.com/cs/sigmoid-vs-tanh-functions). Among 3 sigmoid function, one is used as ‘Forget gate’ another one as ‘Input gate Layer’, another one to select the desired output. 2 ‘tanh’ functions are used to create vector of new emphasized values. At last stage, we assigned the activation function ‘linear’ to get the output in continuous value.

Dropout- We have taken 0.1 as our [**dropout**](https://keras.io/api/layers/regularization_layers/dropout/) value. 

Epochs- An [**epoch**](https://radiopaedia.org/articles/epoch-machine-learning) indicates the whole training data has been passed through the network. In our model, we have 300 epochs. 

Batch- As the training data are being advanced through the network, we split the training data into a [**batch size**](https://stats.stackexchange.com/questions/153531/what-is-batch-size-in-neural-network). We defined the batch size to be 150.  

Loss function & Optimizer - We have chosen the MSE as our [**loss function**](https://en.wikipedia.org/wiki/Loss_function#:~:text=In%20mathematical%20optimization%20and%20decision,cost%22%20associated%20with%20the%20event.) because it is mostly used for time series analysis as one of the effective loss functions. We used the optimizer [**Adam**](https://machinelearningmastery.com/adam-optimization-algorithm-for-deep-learning/#:~:text=Adam%20is%20a%20replacement%20optimization,sparse%20gradients%20on%20noisy%20problems.)  because of its high performance and fast convergence compared to other alternative optimizers.

Error calculation-We have used the root mean squared error of training and testing data as our error measurement.

For illustration, visualizations related to large caps are provided in this file.

First is the training and validation loss for 3 large caps.


![Training   validation](https://user-images.githubusercontent.com/83521671/173610129-d1a10559-c7b2-404d-9da1-38bba944c8c3.JPG)

Second is the RMSE comparison for all large cap companies.

![analysis2](https://user-images.githubusercontent.com/83521671/173610593-1840c222-ae03-4092-b115-108e99b79ca7.JPG)

Furthur, we have training and testing dataset log values as a overlap over original.

![Large cap training](https://user-images.githubusercontent.com/83521671/173611886-86088323-eacb-484c-89c0-33b98efd8baa.JPG)


**Trading Strategy:**-->> Trading strategy is based on the our 10 days prediction. We used daily basis trading. If the prediction of next day shows a positive return, we will invest €10000 today and sell it next day with more return ,as like going [**long**](https://www.investopedia.com/terms/l/long.asp) for that stock. If the prediction shows a negative return for next day , we will borrow that stock today to sell it, and next day we will buy it back to lender by keeping profit form price fall or going [**short**](https://www.investopedia.com/terms/s/short.asp) for that stock. We have added €2+0.11% as trading cost.


![Up down](https://user-images.githubusercontent.com/83521671/173617456-208b8c44-9787-4b1d-928b-ff6480d6973d.JPG) 
![10 days](https://user-images.githubusercontent.com/83521671/173623990-8e5bca96-1c20-48d2-b43b-7a1e2665d25e.JPG)


### **More analysis**
For furthur analysis, we included volatiliy, max dropdown, total return by comparison to covid-19 situation on stock market. Presented table is just a glimpse of it.


![volatility](https://user-images.githubusercontent.com/83521671/173624682-15a0edaa-ac12-4097-bb11-bf429ad52d2f.JPG)

This is the drop down return of large caps. 
![dropdown](https://user-images.githubusercontent.com/83521671/173626696-64aeb1e7-235d-4647-8dc9-c5fae36f6c3c.png)

### **Conclusion** 
In this highly volatile market, it is still difficult even for a strong model to predict actual long-term movement. Where a lot of investor is trying to predict using this type of model, considering an demand and supply behaviour of stock, a lot of investor will try to sell or buy at the same time and cause the return to fall or to rise. Which will go opposite direction of predicted return. Considering all conditions, we found this type model to be a good choice for predicting stock price movement on daily basis trading.

### **References**
- Yahoo! Finance - https://finance.yahoo.com/
- Onvista - https://www.onvista.de/
- Degiro - https://www.degiro.de/?_ga=2.53740846.768932194.1612563755-1041573335.1612445051
- “Stock Trading with Neural Networks” by ERIK IHRÉN, SEAN WENSTRÖM
- Christopher Olah - https://colah.github.io/posts/2015-08-Understanding-LSTMs/
- “Predicting the Trend of Stock Market Index Using the Hybrid Neural Network Based on Multiple Time Scale Feature Learning” by Yaping Hao and Qiang Gao





