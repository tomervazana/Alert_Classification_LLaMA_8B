# MITRE ATT&CK alert classification Solution:
My solution have these key elements on it:
* Few-Shot Prompting with Structured Examples (19 shot) on LLaMA-8B,
* Adjusting the [model params](https://console.groq.com/docs/text-chat)
* Text cleaning - simple cleaning (could get better)
* Replacing similar examples with different ones (without embedding model, but menually reading and removing, due to the instruction limitations)
* DS Undersampling - To balance the DS
* LabelEncoder - encoded the alert code before classification and then decoded them back after prediction
* Define the model behaviour with a "system" message
* JSON Format - The model handles one alert at a time, and return Json, then validate the stracture and return python class with the JSON attributes, it inherit from pydantic-BaseModel the JSON stracture is: {alert: alert text, uid: uid provided with it, code: The label which was encoded by the LabelEncoder, name: alert name, confidence_score: the confidence of the prediction}

Measurements:

As mentioned above, I took 19 samples to the prompt.

I used the other 81 samples to validate the model performance.

```
Number of correct predictions: 79
Number of incorrect predictions: 0
Confusion Matrix:
[[24  0  0  0  0  0]
 [ 0 26  0  0  0  0]
 [ 0  0 20  0  0  0]
 [ 0  0  0  6  0  0]
 [ 0  0  0  0  1  0]
 [ 0  0  0  0  0  2]]

Classification Report:
              precision    recall  f1-score   support

           0       1.00      1.00      1.00        24
           2       1.00      1.00      1.00        26
           4       1.00      1.00      1.00        20
           5       1.00      1.00      1.00         6
           6       1.00      1.00      1.00         1
           7       1.00      1.00      1.00         2

    accuracy                           1.00        79
   macro avg       1.00      1.00      1.00        79
weighted avg       1.00      1.00      1.00        79

Micro-Averaged Precision: 1.0
Micro-Averaged Recall: 1.0
Micro-Averaged F1 Score: 1.0
```
