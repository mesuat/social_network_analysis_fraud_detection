# social_network_analysis
THE USE OF NETWORK ANALYSIS FOR CREDIT CARD FRAUD DETECTION

This is a project report prepared for the course of Social Network Analysis at the Sabanci University.

Objective

Credit card fraud is one of the major issues for financial institutions. These frauds have resulted in huge financial losses as the fraudulent transactions have been large value transactions. According to resources, in 1999, out of 12 billion transactions made annually, approximately 10 million—or one out of every 1200 transactions—turned out to be fraudulent. 

In this project, the main aim is whether social network analysis would help to detect fraudulent transactions and so that, institutions would block them.

Dataset

In this project, we used a synthetic data which contains information about customer’s age, gender, Zip Code, the amount and the type of transaction, merchant and its Zip Code and also the amount of transaction and whether the transaction is fraudulent or not. The dataset does not contain any personal data. There are 594.643 transactions of 4.312 customers from 50 merchants in a period of 180 days. Among these transactions, 7200 of them are fraudulent. 

As an example, the first 4 rows of data can be found below.
 
step	customer	age	gender	zipcodeOri	merchant	zipMerchant	category	amount	fraud
0	C1093826151	4	M	28007	M348934600	28007	es_transportation	4.55	0
0	C352968107	2	M	28007	M348934600	28007	es_transportation	39.68	0
0	C2054744914	4	F	28007	M1823072687	28007	es_transportation	26.89	0
0	C1760612790	3	M	28007	M348934600	28007	es_transportation	17.25	0

The dataset is unbalanced. The ratio of non-fraudulent transactions to fraudulent ones is about 80. The histogram for count of non-fraudulent / fraudulent transactions can be found below.

 
-	Number of normal examples:       587.443
-	Number of fraudulent examples:      7.200

In general, fraudulent transactions’ amounts are higher compared to non-fraudulent ones. Mainly, the amount is the most decisive variable when determining fraudulent transactions.
This histogram located below shows the number of transactions (y-axis) in terms of its amount (x-axis) in terms of its type (fraudulent or not).

 

Network and Its Analysis

For the project, whole dataset is converted into networks where “customers” and “merchants” are nodes and transactions between these are edges. Transaction’s related information such as “amount”, “category” and whether it is fraudulent or not are considered as edge’s attributes. Some statistics and plots of the network can be found below.

Statistics
Number of Nodes:	162
Number of Edges:	594643
Average Degree:	286
Diameter	4

 

Modelling

-	Before Social Network Analysis

Before social network analysis, dataset is divided into two equal parts: train and test. Then train dataset is divided into two with the ratio of 80-20. First part is used for training the model and the second one is used for its validation. For modelling, LightGBM is used. It is gradient boosting (GBDT, GBRT, GBM or MART) framework based on decision tree algorithms, used for ranking, classification and many other machine learning tasks. No feature engineering or processing is done. All categorical features are left as LightGBM can handle them after labeling.

Train and validation datasets are used to model and to determine decision threshold. A value above that threshold indicates "fraudulent"; a value below indicates "non-fraudulent”. Because dataset is unbalanced, the threshold is fine-tuned such that F1-Score is biggest.

After modeling and determining threshold, about %65 (2.126) of 3.290 fraudulent transactions can be detected.  As a result, confusion matrix, classification report and feature importance are as follow.

	Confusion Matrix
	Non-Fraudulent	Fraudulent
Non-Fraudulent	293.121	911
Fraudulent	1.164	2.126

	Classification Report
	Precision	Recall	F1-Score	Support
Non-Fraudulent	1	1	1	294.032
Fraudulent	0.7	0.65	0.65	3.290
 	 	 	 	 
Micro avg.	0.99	0.99	0.99	297.322
Macro avg.	0.85	0.82	0.83	297.322
Weighted avg.	0.99	0.99	0.99	297.322

 

-	Using Social Network Analysis

Researchers have used social networks to improve the detection performances obtained from the various techniques. By building a bipartite network “card” – “merchant”, linking a card to a merchant if the cardholder made a transaction at that merchant. Then this network is projected onto two simple networks: card and merchant. Cards are linked if they have purchased at the same merchants and merchants are linked if they saw the same cards.
 
The type of behavior we want to uncover is that fraudsters steal card information and then use it at a small group of merchants, for which they have put in place the circuit to resell the goods they have misappropriated. Social variables are computed using the network structure (for example: the number of neighbors, the clustering coefficient of the node, the size of the node’s community) or using also the nodes attributes (for example: average number of transactions of neighbors on period, average amount of fraud of neighbors on period). Finally, social aggregates are social variables built on sliding windows (for example, average amount of fraud in community last week). This procedure allowed us to derive features and to improve the accuracy of our prediction.

For feature extraction, social variables are computed using only the network structure (for example: the number of neighbors who made fraudulent transaction, between / eigenvector centrality), or using also the nodes attributes (amount of fraud of neighbors on period). 

In addition to these, for link prediction SimRank algorithm is used. This algorithm is based on the intuition of “two objects are similar if they are referenced by similar objects.”

Since some are uncorrelated with target variable (whether fraudulent or not), centrality measures and fraudulent degree variables have worsened the result. However, SimRank algorithm caused improvement in accuracy. With this, about %82 (2687) of fraudulent transactions are detected. It means that, network analysis and especially SimRank algorithm has increased the performance of the model about 26 percent. 

As a result, confusion matrix, classification report and feature importance are as follow.

	Confusion Matrix
	Non-Fraudulent	Fraudulent
Non-Fraudulent	293.170	862
Fraudulent	603	2.687

	Classification Report
	Precision	Recall	F1-Score	Support
Non-Fraudulent	1	1	1	294.032
Fraudulent	0.76	0.82	0.79	3.290
 	 	 	 	 
Micro avg.	1	1	1	297.322
Macro avg.	0.88	0.91	0.89	297.322
Weighted avg.	1	1	1	297.322

 
 
Result

In recent years, with advances in e-commerce and payment methods, the usage of credit card in shopping have been increasing rapidly. The total amount of credit card transactions attract fraudsters and they search new methods to hijack information of credit card users. Financial institutions should develop net techniques to detect fraudulent transactions. In this project, we aim to utilize network analysis for credit card fraud detection.

In our findings, SimRank algorithm helped us to increase performance our model about 26 percent and out of 3.290 fraudulent transactions, the number of items identified has increased from 2.126 to 2.687.
