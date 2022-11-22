# Earthquake Prediction using Bayesian LSTM Neural Networks

**Goal:** predict earthquake occurences in Japan from annual historic data.

I considered earthquake records in Japan from 1960 to 2021 collected through the United States Geological Survey's API ([USGS](https://www.usgs.gov/)).
Earthquake occurence count with magnitude greater than $m_i$ in a given time window is a time series. By creating $z$ different geographical zones, and by adding different magnitude thresholds $m_i$ for $i \in \{1, ..., M \}$, we get $z \times M$ time series.

![image](https://user-images.githubusercontent.com/85329709/203319566-b68adf9d-6299-495d-b33e-9bb6a50cca58.png)

I then explore the relationship (if any) between those multivariate time series. 

## Magnitude Modelling

We can use extreme value theory to model the highest magnitude by zone and by time period. We use the following statistics to define extremes:
$$M_k^q = \max(X_{1k}^q, X_{2k}^q, ..., X_{ok}^q)$$
with $q = 1, ..., z$ and $o$ the number of earthquake in zone $q$ for the time period $k$.  

I then fitted $(M_k^q)^* = (M_k^q - b_k)/a_k$ to a Generalized Extreme Value (GEV) distribution.

![image](https://user-images.githubusercontent.com/85329709/203325228-e106ffb9-fd68-4f34-b3fb-c5c0c0cb1941.png)
![image](https://user-images.githubusercontent.com/85329709/203325437-01bc567d-59ed-45b5-894f-6ac9a8fa48aa.png)


A second approach to modellign magnitude is to use Bayesian Neural Networks (BNN). BNNs let us know the incertitude related to the model's prediction (epistemic uncertainty). Here we use Bayes by Backprop as the inference scheme with an epistemic uncertainty of 0.38.

![image](https://user-images.githubusercontent.com/85329709/203326656-88d18cd2-d9af-477b-859b-50f02a4ccb22.png)

## Discussion

The results are rather disappointing for the BNNs. Not only do they generalize poorly, their interpretability is poor and they do not allow to compute excess probabilities (i.e. probability of being above an attachment point, to the exhaustion point, ...) and are thus not really useful in the (re)insurance industry.
