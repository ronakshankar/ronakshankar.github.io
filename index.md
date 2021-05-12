# **Question Matching using BERT**

## **Introduction**

Community Question Answering (CQA) has become a primary means for people to acquire knowledge, where people are free to ask questions or submit answers. To enhance the efficiency of the service, similar question identification becomes a core task in CQA which aims to find a similar question from the archived repository whenever a new question is asked. However, it has long been a challenge to properly measure the similarity between two questions due to the inherent variation of natural language, i.e., there could be different ways to ask the same question or different questions sharing similar expressions. For our project, we would like to find an efficient way of question matching using concepts from Information Retrieval and Machine Learning. 


# **Dataset.**

The Quora Question Pair dataset contains more than 400 thousand unique question pairs. The Quora question pair dataset contains the following columns. 
Variable Name | Description |
--- | --- | 
qid1 | Question 1 ID |
qid2 | Question 2 ID |
question1 | Full text of Question 1 |
question2 | Full text of Question 2 |
is_duplicate | If Question 1 and Question 2 are similar |


Question 1 and Question 2 will be the Independent variables and is_duplicate be the prediction value. The total dataset is split into Training and Testing data with 80%-20% split. Further in training data, the data is again split by the same percentage for training data and validation data. The Testing data will be the final dataset to evaluate after hyperparameter tuning to minimize data snooping. 

# **Data Preprocessing**
1.	Remove rows with empty questions.
2.	Replaced contractions such as ‘can’t’ with ‘can not’, ‘what’s with ‘what is’, etc.
3.	Removed special characters.
4.	Split the sentence into tokens.
5.	Add the special [CLS] and [SEP] tokens to start and end of questions.
6.	Map the tokens to their IDs.
7.	Pad or truncate all sentences to the fixed length which is set to maximum length of questions in the dataset.
8.	Create the attention masks to distinguish real tokens from [PAD] tokens.
9.	Create token type ids to indicate which token belongs to which question.

![alt text](https://github.com/ronakshankar/ronakshankar.github.io/blob/main/preprocessing.png)


# **BERT Model**

Bidirectional Encoder Representations from Transformers (BERT) was released and pre-trained by Google for natural language processing tasks in late 2018. It consists of 12 transformer encoding layers (or 24 for large BERT). BERT relies on a Transformer - its attention mechanism learns contextual relationships between words in a text. Since BERT’s goal is to generate a language representation model, it only needs the encoder part. BERT is a pre-trained model using plain text corpus. We fine-tuned bert-base-uncased model by adding just an additional output layer to classify duplicate questions.

![BERT](https://github.com/ronakshankar/ronakshankar.github.io/blob/main/BERT.png)

# **Training**

We trained the data using training dataset and do hyper parameter tuning over validation dataset. We split the total dataset by 80%-20% as training and Testing dataset. After the split, we are splitting the training section 80-20% as training dataset and validation dataset. We use the preprocessed the data as input and the labels as ground truth. 

**Hyper Parameter Tuning**
Few variables that have been tuned: 
- Number of Layer – 5,7,12,
- Activation function – Relu, Gelu
- Number of units
- Dropout rate – 0.1, 0.2, 0.3
- Number of attention head -12, 24
- Learning rate – 2e-5, 1e-3, 5e-3
- Epochs – 2, 5, 10, 50

## **Results :**

![RESULT](https://github.com/ronakshankar/ronakshankar.github.io/blob/main/Results.png)


The optimal parameter for our model was : 

- Gelu Activation, 
- 12 Layers,
- 10 Epochs
- 2e-5 learning rate
- Dropout rate 0.2
- 12 Attention head

![RESULT](https://github.com/ronakshankar/ronakshankar.github.io/blob/main/Training.png?raw=true)

## **Demo :**
A user gives two questions to the model and the model cleans the questions, pre process over the data. The preprocessed data will be given to the model which predicts whether the questions are duplicate or not. 
The following is a screenshot for the Demo

![alt text](https://github.com/ronakshankar/ronakshankar.github.io/blob/main/Demo%20output.png)
