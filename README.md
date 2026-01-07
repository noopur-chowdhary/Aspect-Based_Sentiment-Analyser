# Aspect-Based_Sentiment-Analyser

**Overview**
Traditional sentiment analysis often fails to capture the complexity of human communication. A single review can praise the food while criticizing the service, or use sarcasm that confuses basic models.
This project implements Aspect-Based Sentiment Analysis (ABSA), moving beyond binary "Positive/Negative" classification to identify specific Aspect Terms and their corresponding Polarity.

**Dataset Source**
The primary dataset used in this project, Restaurants_Train_v2.csv, originates from the SemEval-2014 Task 4 (Subtask 1 & 2).
Source: SemEval-2014 Task 4 Website
Original Publication: Pontiki, M., et al. (2014). "SemEval-2014 Task 4: Aspect Based Sentiment Analysis."
Description: This dataset consists of real-world restaurant reviews manually annotated for specific aspect terms (e.g., "service," "pizza") and their corresponding sentiments (positive, negative, neutral).

**Data preparation**
 The CSV file is loaded into a pandas DataFrame and  initial inspection is done. The dataset contains 3,693 rows and 6 columns, with the following structure: We have columns Index(['id', 'Sentence', 'Aspect Term', 'polarity', 'from', 'to'], dtype='object'). The from and to columns indicate the character-level position of the aspect term within the sentence.

**Model Initialization, Testing, and Inference**
We use a pretrained DeBERTa-v3 model fine-tuned for Aspect-Based Sentiment Analysis (ABSA). The model jointly encodes a sentence and a specified aspect term to predict aspect-level sentiment. After initialization, the model is tested using a sample sentence containing multiple aspects. The test demonstrates that the model correctly distinguishes sentiment for different aspects within the same sentence (e.g., negative sentiment for crust and positive sentiment for toppings). The same inference function can then be applied to CSV data containing sentence–aspect pairs, producing Negative, Neutral, or Positive sentiment predictions.

**Batch Sentiment Analysis with Progress Tracking**
This step applies the aspect-based sentiment model to the entire dataset in batches. Each row is processed by passing the sentence and its corresponding aspect term to the prediction function. A progress bar (via tqdm) is used to monitor processing speed and completion status. The predicted sentiment for each entry is stored in a new column, enabling efficient large-scale inference with basic error handling.

Model Evaluation and Visualization

After generating predictions, the model’s performance is evaluated by comparing the true sentiment labels with the predicted sentiments. A classification report is produced to summarize precision, recall, and F1-score for each sentiment class. Additionally, a confusion matrix is visualized using a heatmap to clearly show correct predictions and misclassifications across sentiment categories, helping to assess overall model behavior and error patterns.

**Automatic Aspect Extraction for Real-World Reviews**
n the current implementation, aspect terms (e.g., “crust”) are manually provided to the model. However, in real-world applications—such as analyzing thousands of Amazon reviews—the relevant aspects are not known in advance. To address this, we first build an aspect extraction step. Using spaCy, we analyze the grammatical structure of each sentence and automatically identify potential aspects. These aspects are typically nouns that are modified by adjectives, making them strong candidates for aspect-based sentiment analysis.
