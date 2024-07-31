# keyword-extraction
Ectracting keyword from .docx document


# Keyword extraction using KeyBERT model
overview
The automated technique of extracting words and phrases is known as keyword extraction. We currently have simple software tools that can be used to extract keywords and keyphrases thanks to techniques like Rake and YAKE. These models, however, frequently operate more on statistical characteristics of the text than on its semantic similarities. Instead, we can convert words and documents into vectors that accurately capture their meaning using the BERT model, which is a bi-directional transformer.
In this project, we make use of KeyBERT, a straightforward and basic keyword extraction method that makes use of BERT embeddings. We outline the procedures for keyword extraction in the sections below.

# Step 1: Word extraction and text pre-processing
We start by creating a list of candidate keywords from a document. By defining the deep_clean function, we apply all essential text pre-processing techniques to the document. After reading each document through the docx2txt function, the following operations are performed for data pre-processing.
    • Punctuations, links, stopwords, mentions and \r\n new line characters removal.
    • Uncapitalized conversion.
    • Removal and replacement of contractions (e.g., “can not” instead of can't").
    • Special characters removal such as "&" and "$" presented in some words.
    • Multiple sequential spaces removal.
Then, the variable texts_clean is simply a list of strings that includes our candidate keywords.

# Step 2. Keyword extraction
There are numerous ways to create BERT embeddings, including Flair, Hugginface Transformers, and, as of recently with their 3.0 release, even spaCy. However, using the keybert package is preferred because it enables us to complete embeddings generation and keyword extraction in a single phase. So, with just three lines of code, we can locate words and phrases that are relevant to a body of text.

# 2.1. Model
Let's use the KeyBERT class to construct an object. A model known as all-MiniLM-L6-v2 is utilized by default. Other models can be used, though. Pass the model name to the first positional parameter to use a different model. For multilingual text, the author suggests adopting a model named paraphrase-multilingual-MiniLM-L12-v2.
kw_model = KeyBERT()

# 2.2. Single Keyword Extraction
To output the results, we simply need to call the extract_keywords() method of our KeyBERT object. To the first positional parameter, we will pass the document, and to the "top n" parameter, the number of keys we want to receive.
The result is a list of tuples, where the first index in each tuple is the string value for the key and the second value is the distance of the key, which can be thought of as a score to indicate the model's confidence, which ranges from 0 to 1, with higher values denoting greater confidence.

Simple_keywords = kw_model.extract_keywords(texts_clean, keyphrase_ngram_range=(1, 1), stop_words=None, top_n=10)

# 2.2.3. phrase extraxtion and Diversification
We only investigated single words as potential keys in the preceding section. This can be changed to take a variety of tokens into account. By giving a tuple to the keyphrase_ngram_range  option, this is achieved. Additionally, we could want the model to generate a wider variety of keys. In order to diversity the output, we apply an approach called max sum similarity by setting the use use_maxsum parameter to true and giving an integer to nr_candidates.
keywords_with_diversity =  kw_model.extract_keywords(texts_clean, keyphrase_ngram_range=(3, 3), stop_words='english', use_maxsum=True,  nr_candidates=20, top_n=10)

the function Simple_diverse_keywords_generation is defined to generate the single and diverse keyword extraction for each document and save them in extracted_file.csv file. And that's it!
