# TDI CAPSTONE PROJECT

CONSTRUCTMENT

Engineering Code Search Engine with Natural Language Processing

Making Tables Searchable

By José Pagán

## Background
The US Building and Engineering Code is comprised of several technical documents that regulate the work of over 500,000 engineers and architects in the United States at the Federal level.  The Code’s most important document is the ACI318-14, a 524-page technical document.  Currently, if an engineer or architect wants to find a section or table in the ACI-318-14, he needs to manually search the 524 pages of the document himself.  This task is typically done by downloading a pdf version of the code from the internet and manually searching the document with a simple word search tool.  Once found, the relevant information needs to be extracted manually.  This task is very tedious and time-consuming, as technical words tend to be repeated hundreds of times within the document.
The Constructment application is a search engine that assists architects and engineers when searching technical engineering documents.  Regular search engines cannot identify subtle differences in technical terms as they tend to group similar words together and use algorithms based on simple word count.  They provide a lot of irrelevant information and waste the engineer and architect’s time parsing through the search results.
My contribution to this project is to expand the application’s machine learning capabilities to include tables as possible answers for user questions.  Since there are over 250 tables throughout the Code, this additional functionality significantly can reduce the time spent by architects and engineers searching for tables in the Code.  The format, size, and content of these tables varies significantly making the tables difficult to locate and extract manually.
My section of the project uses natural language processing and artificial intelligence to generate thousands of sentence-like text sequences from the contents of each table in the ACI-318-14.  The sequences are then compared with the surrounding text, based on several similarity measures, and the top 10 sentences are selected.  Similarity measures used include frequency and number of words per sentence.  These sentences are then fed into the application’s retriever model, to appropriately identify sequences as possible answers to user questions and point the user to the corresponding table.
The data for the project is comprised of the tables and surrounding text in the Building Code Requirements for Structural Concrete (ACI 318-14) and the Commentary on Building Code Requirements for Structural Concrete (ACI 318R-14) of 2014.

## Jupyter Notebooks
The project was developed in Python using Jupyter Notebooks.  A brief outline of the workflow is provided below:

Table Extraction:  Table data is extracted from the PDF version of the ACI-318-14 into CSV files using Camelot.  A CSV file is created for each table containing the page number in which it is found, and the text contained inside the table.

Seed Text Combinations: The information from the CSV files is used to generate seed text for each table.  The seed text is generated from a random sample of all five-word combinations from the words in each table, including the table name.  A maximum of 67 words, including the table name, are used to generate the combinations.  For each table, 1,000 five-word phrases are generated as seed text and saved to a text file.  This seed text is used in a later step to generate one thousand sentence-like text sequences for each table.

Sentence Length Distribution: The sentence length distribution for each chapter of the ACI-318-14 is computed.  Histograms and violin plots are used to visualize the results.  Sentence length statistics including mode, mean and standard deviation are computed.  Based on these results, a sequence length of 17 is selected for one thousand sentence-like text sequences for each table.

Preliminary Language Model:  A sequence generator is used to generate one thousand sentence-like text sequences for each table in the ACI-318-14 as follows:

- (i) word embeddings for the ACI-318-14 text are generated using the GloVe pre-trained word embedding model from Stanford University;
- (ii) a recurrent neural network using two Long-Short-Term Memory (LSTM) hidden layers, a dense layer and a soft-max layer is trained on the entire text of the ACI-318-14 to predict the next word that occurs in a sequence of text from the ACI-318-14;
- (iii) once trained, the network is used to generate sentence-like text sequences by starting it off with the seed text for each table and then continuing the sequence with words chosen according to the probabilities output by the network.

The model uses Tensorflow and Keras to generate the sequential Neural Network.  The Hyperopt optimization library was used to train the model with different parameters including:

- number of neurons per Long Short Term Memory hidden layers
- number of neurons per dense layer
- regularization parameters
- learning rate
- embedding size

Different runs were made using different optimizers (Adam and SGD), batch sizes, number of epochs and evaluations.  The best model was selected.

Get Similarity Score:  For each sequence generated, a similarity score is computed against each sentence in the corresponding chapter of the ACI-318-14.  (The text from the chapters was previously extracted using Textract.)  The top 10 most similar sentences to each sequence are selected and saved.  The selection is made based on cosine similarity of the mean word embeddings for the sentences and sequences.  A Jaccard similarity test is also available.  (Word embeddings are generated using the Glove pretrained word embedding model.)

Retriver Model: The top 10 sentences for each sequence will be fed into Constructum's retriever application to appropriately identify sequences as possible answers to user questions and point the user to the corresponding table.

Visualizations:

- Sentence Length Distribution Histograms by Chapter
- Sentence Length Distribution Violin Plots by Chapter
- Sentence Length Distribution for ACI-318-14
- Word embedding maps


## Preliminary Results:
Table extraction and seed text generation have been successfully completed.  A sequence length of 17 was selected and a function was created to extract the top X most similar sentences for each sequence using cosine and jaccard similarities.

## TDI CAPSTONE PROJECT CHECKLIST

1. Buisness objective - to expand the Constructment’s Engineering Code Search Engine to include tables as possible answers for user questions.
2. Data Ingestion - The ACI-318-14 524-page pdf file was processed to extract its tables using Camelot.  The tables were saved as csv files.  The csv files were read and processed to generate 1,000 five-word seed text sequences for each table.
3. Visualizations - Histograms and Violin Plots were used to determine optimal sequence length to be used with Constructment's retriever model.  An embedding map (scatter plot) was created to illustrate the distance between sample word vectors in 2D using GenSim word2vec and PCA.
4. Machine Learning - The project uses Natural Language Processing and Deep Learning as discussed above.
5. Deliverable - Various notebooks were created for portions of the project as described above.
