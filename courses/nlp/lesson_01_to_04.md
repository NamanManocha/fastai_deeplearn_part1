# NLP:  Lessons 1 to 4

## Lesson 2 [Topic Modeling with SVD & NMF (NLP video 2)](www.youtube.com/watch?v=tG3pUwmGjsc)
* spacy doesn't offer a stemmer, because it doesn't think it should be used
* Google [sentencepiece](https://github.com/google/sentencepiece)
  * performs sub-word tokens
* NMF (non-negative matrix factorization) is not unique, but can be more interpretable

To check time of a step:  
```python
%time u, s, v = np.linalg.svd(vectors, full_matrices=False)
```

## Lesson 3

- stemming:  getting roots of words  (chops off end, "poor man's lemmatization")
- lemmatization:  (fancier)
- lemmatization is more computationally expensive than stemming
- stemming is quicker and easier
 
### Pre-processing
- when you have less data, do this pre-processing
- do you think your model can handle the complexity:  
 - if you're using neural networks, don't do lemmatization, because that is throwing away information
 - if you have a simpler model, can't learn as much complexity, so do this pre-processing

### Factorization is analagous to matrix decomposition

### What are the nice properties that matrices in an SVD decomposition have?
- A = USV
- U:  orthonormal; columns or rows are orthonormal to each other; the columns are orthogonal and pairwise normalized.  (dot product of two columns is 0. dot product of column with itself gives us 1)
- S:  diagonal matrix; everything off diagonals is 0; capture an idea of importance, singular values, descending order: capture biggest one first, non-negative, scale of U and V is both 1
- V:  same properties as U, but transpose of rows are orthonormal to each other
- NMF:  special property in decomposition is **non-negative** AND matrix is **sparse** (sparse means many of the values are zero)

## Linear Algebra
- 3 Blue 1 Brown: Essence of Linear Algebra [playlist on YouTube](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab0
- video Chapter 3 [Linear transformation and matrices](https://youtu.be/kYB8IZa5AuE)

## Reviewing spreadsheet
- first matrix:  TF-IDF (term document matrix)
 - rows:  author_title
 - cols:  words
- use SVD to decompose TF-IDF matrix into 3 matrices
 - U rows:  author_title
 - U cols:  topics1 to x
- S:  diagonal matrix  (singular values in descending order, most important one at (1, 1) position in matrix)
 - S rows:  topics
 - S cols:  topics
- V:  
 - V rows:  topics
 - V cols:  words
 
 ## Advantages / Disadvantages of SVD vs NMF
 - NMF: non-negative values, can be more interpretable
 - SVD: can have negative values for topic
 - SVD: is an exact decomposition; can fully represent the input matrices
 - NMF:  not exact
 - NMF:  need to set the number of topics you want, that's hyperparameter that you are choosing; 
 - SVD:  traditional SVD: you are getting as many singular values as you have documents, (assuming fewer documents than vocab words)
 - SVD:  there is opportunity to look at singular values and see that when they get little, these topics may be so unimportant that I can ignore them and chop them off.  But that also means with SVD, you're doing extra work.  
  - so it is both giving you more information and extra work
 - SVD:  on a big matrix is **slow**
  - in example on a 10K by 10K matrix, it is very slow
  - one way to address this is to use **randomized SVD**

### Full vs Reduced SVD
- Full SVD will have U and V both be square matrices
 - that involves making up some columns for U that don't directly depend on the data A
 - for S matrix (singular value), also adding some rows of pure zeroes
- in practice, you are usually going to be using reduced SVD, it's quicker to calculate and you are often not needing to use/ turn it into an orthonormal basis

## Lesson 4 [Sentiment Classification of Movie Reviews (using Naive Bayes, Logistic Regression, Ngrams](https://youtu.be/hp2ipC5pW4I)

### 
- in Jupyter notebook, type `?? URLs` to pull up documentation
- `itos` = integer to string  [is type list]
- `stoi` = string to integer  [is type dictionary], dict is good to search by string, 
- `movie_reviews.vocab.itos[230:240]` are ordered by **frequency**
- `movie_reviews.vocab.stoi['language']` gives 917
- if you want human-readable, use strings
- if you want something the algorithm can process, use numbers
- it's not a 1-to-1 mapping because several words can have the same index
 - we can have a lot of words mapping to "unknown", many things will map to capital letter, 

### Creating term document matrix
- a matrix with lots of zeroes is called **sparse**
- 