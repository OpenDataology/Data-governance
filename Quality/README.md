# Text Property Outliers
Examining outliers may help you gain insights that you couldn’t have reached from taking an aggregate look or by inspecting random samples. 
For example, it may help you understand you have some corrupt samples (e.g. texts without spaces between words), 
or samples you didn’t expect to have (e.g. texts in Norwegian instead of English). 
In some cases, these outliers may help debug some performance discrepancies (the model can be excused for failing on a totally blank text). In more extreme cases, the outlier samples may indicate the presence of samples interfering with the model’s training by teaching the model to fit “irrelevant” samples.

# Property Label Correlation
The check estimates for every text property (such as text length, language etc.) its ability to predict the label by itself.

This check can help find a potential bias in the dataset - the labels being strongly correlated with simple text properties such as percentage of special characters, sentiment, toxicity and more.

This is a critical problem that can result in a phenomenon called “shortcut learning”, where the model is likely to learn this property instead of the actual textual characteristics of each class, as it’s easier to do so. In this case, the model will show high performance on text collected under similar conditions (e.g. same source), but will fail to generalize on other data (for example, when production receives new data from another source). This kind of correlation will likely stay hidden without this check until tested on the actual problem data.

For example, in a classification dataset of true and false statements, if only true facts are written in detail, and false facts are written in a short and vague manner, the model might learn to predict the label by the length of the statement, and not by the actual content. In this case, the model will perform well on the training data, and may even perform well on the test data, but will fail to generalize to new data.

The check is based on calculating the predictive power score (PPS) of each text property. In simple terms, the PPS is a metric that measures how well can one feature predict another (in our case, how well can one property predict the label). For further information about PPS you can visit the ppscore github or the following blog post: RIP correlation. Introducing the Predictive Power Score

# Special Characters
The SpecialCharacters check looks for text sample in which the percentage of special characters out of all characters is significant. 
Such samples can be an indicator for a problem in the data pipeline that require attention. Additionally, such examples may be problematic for the model to predict on. 
For example, a text sample with many emojis may be hard to predict on and a common methodology will be to replace them with a textual representation of the emoji.

# Unknown Tokens
The Unknown Tokens check is designed to help you identify samples that contain tokens not supported by your tokenizer. 
These not supported tokens can lead to poor model performance, as the model may not be able to understand the meaning of such tokens. 
By identifying these unknown tokens, you can take appropriate action, such as updating your tokenizer or preprocessing your data to handle them.

# Text Data Duplicates
The TextDuplicates check finds multiple instances of identical or nearly identical (see text normalization) samples in the Dataset. 
Duplicate samples increase the weight the model gives to those samples. If these duplicates are there intentionally (e.g. as a result of intentional oversampling, or due to the dataset’s nature it has identical-looking samples) this may be valid, 
however if this is a hidden issue we’re not expecting to occur, it may be an indicator for a problem in the data pipeline that requires attention.

# Conflicting Labels
The ConflictingLabels check finds identical or nearly identical (see text normalization) samples in the dataset that have different labels. 
Conflicting labels can lead to inconsistencies and confusion for the model during training. Identifying such samples can help in cleaning the data and improving the model’s performance.
