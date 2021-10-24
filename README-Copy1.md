# Machine_Learning_Trading_Bot
This project applies machine learning Support Vector Machine and AdaBoost models to an algorithmic trading strategy.


## Technologies
The project uses the following technologies:

* *Pandas, NumPy, Pathlib, Hvplot, and Matplotlib* for general programming and visualizations in Jupyter Lab

* *SKLearn* for the preprocessing and split of the data, particularly *StandardScaler*  and *DateOffset*; *SVM, AdaBoostClassifier*, and *DecisionTreeClassifier* as the applied machine learning algorithms, and *metrics* for the evaluation of the results.

We used `Python 3.7` in the coding.


## Instalation Guide
The file is a jupyter notebook. If you don't have jupyter lab, you can install it following the instruction here:

https://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html

### Usage

This is a jupyter notebook with a pre-run code. You can go through it and see code as well as results. 

If you look to reuse the code, and do not have experience on jupyter lab, please refer:
https://www.dataquest.io/blog/jupyter-notebook-tutorial/

## Contributors
This project was coded by Paola Carvajal Almeida, paola.antonieta@gmail.com.

Contact email: paola.antonieta@gmail.com
LinkedIn profile: https://www.linkedin.com/in/paolacarvajal/


## License
This project uses a MIT license. This license allows you to use the licensed material at your discretion, as long as the original copyright and license are included in your work files. This license does not contain a patent grant,  and liberate the authors of any liability from the use of this code.

-------------------------------------

# Summary Evaluation Report

In this analysis we apply two differents machine learning models to a long/short trading strategy. 

The ideal strategy would go long when the period return is positive; and short when it's negative. The idea is being able to predict the sign of the return before it happens. For that purpose, we applied two Machine Learning algorithms, using as features two simple moving averages (SMA), one with a short term window, and one with a longer term window. We are going to compare our models to the "original" return that would be obtained in a buy and hold strategy, which is a strategy that buys at the beggining of the period and sells at the end.

The machine learning algorithms applied are the Support Vector Machine (SVM), and the AdaBoost. Both models were tried with a baseline *short window* of 4 days and a baseline *long window* of 100 days. I will use the term *short window* as equivalent to *fast window*, and *long window* as equivalent to *slow window*. In the case of SVM, we also performed tunning of the amount of data used for training and testing, which we call the "split window size", as well as the size of the SMAs' windows.

The conclusions follow.


## Conclusions Support Vector Machine model

### SVM Baseline Model
The baseline model shows that the SVM long/short Strategy is able to perform better than the buy and hold original strategy. That means, using the model to generate long and short trades during the period generates a larger return than just holding the asset in the period. In the plot below it is possible to see that the performance is very similar at the beggining. However, at the end of August 2018, the strategy is able to assert in a short trade that was key to make an important difference in cumulative returns: 52% ROI instead of 39% ROI with the original return.

The plot shows the growth of one dollar invested in the long/short strategy versus the buy-and-hold strategy.

![Cummulative Baseline SVM](images/CummulativeBaselineSVM.png)


In terms of accuracy, the image below shows the Accuracy Report. We can see that the model is just slightly better than flipping a coin, with an accuracy of 55%. We can also see that the model is much better in predicting going long than going short, since the f1-score is 0.71 for predicting 1, and just 0.07 for predicting -1.


<img src="images/Classification_Report_Baseline.png" width="400" />



### Tunning of the SVM model

**What impact resulted from increasing or decreasing the training window?**

There is not a significant impact in terms of accuracy when selecting different sizes of windows, as can be seen in the image below. The SVM tends to predict to go long most of the time at any size. The accuracy ranges between 0.55 and 0.57 over all the range of changes from one month to 55. In all cases the model is better than the buying and hold.

<img src="images/results_window_tunned_Accuracy.png" width="600" />

There is an impact on the performance, though. I would not select the training window size just based on performance, but since the accuracy does not change much, I would select a split window of 20 months since at that size we get the maximum ROI of 88%, and the same accuracy than other window sizes.

<img src="images/results_window_tunned_ROI.png" width="600" />

Numerical results of the plots above can be seen in the table below, order by ROI descending:

<img src="images/Iterations_for_split_window.png" width="500" />


**What impact resulted from increasing or decreasing either or both of the SMA windows?**

We see a similar pattern that the previous results in terms of the accuracy and profitability. 

* **Accuracy:** we see that small windows sizes for both fast and slow windows generate more consistent values and higher accuracy. Also, we see that the fast window size became more relevant when the slow window surpass 110 days. For those cases, very short term (very fast) windows generate better accuracy. The range of change of the accuracy is very low, as it can be seen in the color scale, which ranges from 0.535 to 0.565. The good news is that always the values are above 50%, which means, the model is better than flipping a coin. Still we would like to be better, though.

<img src="images/results_tunned_sma_Accuracy.png" width="600" />

* **Performance:** we see a similar pattern than for accuracy in terms of the relevance of the windows sizes. However, the differences in performance are much more notorious at higher sizes of long term windows. As is clear in the heatmap below, long windows above 150 days results in low performance when combined with short windows of about half their size. It makes sense that considering too much of the past it may be not profitable for trading strategies. Long term windows are usually used for allocation for long term investments, not for daily or algoritmic trading.

<img src="images/results_tunned_sma_ROI.png" width="600" />


The selected combination of the windows is as follows, considering the best combination of accuracy performance:

    > Long Window:130 days
    
    > Short: 77 days
    
    > Accuracy: 0.566
    
    > ROI: 87.7%
   
 The cummulative returns are represented in the following plot, which represents the growth of one dollar invested in the long/short and the buy-hold strategies:
 

![Cummulative Baseline SVM](images/CummulativeTunnedSVM.png)


Notice that the accuracy improved, as well as the metrics for long calls.

<img src="images/results_SVM_tunned_final.png" width="400" />





## Second Machine Learning Algorithm Applied: AdaBoost

The baseline AdaBoost machine learning algorithm performs very similar than the SVM baseline model. Below you can see that the accuracy is the same in both. The AdaBoost has a slightly lower f1-score. However, the AdaBoost has better indicators on the prediction of negative values. Still way under 50%, so it is still pretty bad. 

<img src="images/ClassificationAdaBoost_versus_SVM.png" width="400" />

The performance of AdaBoost long/short strategy is worse in a signficant part of the period than the original Buy & Hold strategy. This is not good. This model is not going to be tuned. However, before tunning, it would be good to include some other indicator that is able to predict the negative returns. For example, look for increases on the VIX, or other kinds of market indicators that can imply falls.

![CummulativeBaselineAdaBoost](images/CummulativeBaselineAdaBoost.png)






