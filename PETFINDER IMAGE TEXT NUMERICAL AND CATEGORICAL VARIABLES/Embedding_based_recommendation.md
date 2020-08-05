##Embeddings

**What are Embeddings?**

Embeddings are a way to represent jobs/candidates using learned vectors. Simply put, it is a mapping of discrete objects to a sequence of continuous numbers. Given enough data, we can train an algorithm to understand relationships between entities and automatically learn features to represent them. This automated approach is powerful as it allows us to scale more easily. For a platform like ours, being able to scale is top of mind as new jobs/candidates are constantly being added to our system.
We create two types of embeddings. One, that is created by a content recommender system based on the text data and the other that is created by a collaborative recommender model. And finally using dot products from both the embeddings we create a final application score for applications that are not yet created.

**Content Model:-**
For the Content model first we find all the relevant text fields and get the union of all the columns into one and name it "ALL_TEXT". 
This column contains all the important information about the candidates/jobs.

For candidates we consider these text fields:-

*'id' -> 'primary_experience_name', 'keywords', 'address_name', 'skill_names', 'fa_names', 'languages_known', 'skill_names', 'resume_content'*

For Jobs we consider these text fields:- 

*'id' -> 'title', 'slug', 'company_name', 'mandatory_keywords', 'optional_keywords', 'fa', 'fa_category', 'places_name', 'description_text'*
 
Then using the NLP Language models (**BERT Sentence Transformers**) we get the embeddings for the text and thus we get the embedding representation of the Jobs/Candidate text.
 
**Collaborative Model:-**
We consider good application to be the application where candidate is a good fit for that job, if the candidate goes to the later stages of the application process we consider as positive feedback. By this logic we compute the target based on the best application stage the application reached. We assign **"0"** if the candidate is rejected for that application.
Based on this we create a Candidate-Job interaction matrix having Application Scores as target. This matrix is then used to generate Candidate and Job embeddings based on training such that the dot product of the job and candidates provides us the Target Application Stage. These Embeddings are then stored to be used by our post processing logics.
 
The importance of getting these embeddings is that through these vectors we can quickly get the "RELEVANT" JOBS/CANDIDATES by taking the cosine similarities to find the similar jobs/candidates or finding candidates for jobs or vice versa by using dot products. The task of content based model is to generate embeddings for the jobs/candidates which later will be stored in elastic search for finding relevant candidates/jobs by using vectors/embeddings.
 
**Measuring Similarity with Dot Product**

We now have two sets of embeddings where the corresponding dimensions represent the same features. To understand how much a candidate will be suitable for a given job, we simply need to quantify similarity between their embeddings.
One approach is to compute their dot product. Recall that given two n-dimensional vectors, a and b, where:

![User A and B Embeddings](PETFINDER IMAGE TEXT NUMERICAL AND CATEGORICAL VARIABLES/users_a_b.png)
 
their dot product is defined as:

![A and B Dot Product](PETFINDER IMAGE TEXT NUMERICAL AND CATEGORICAL VARIABLES/dot_product.png)
 
Intuitively, the higher the values for the corresponding features, the higher the likelihood that the candidate will be suitable for that job.
 
 
 
 
Measuring Similarity with Cosine Similarity
Also when we want to compute for the similar jobs/ similar candidates, it is done by using Cosine Similarities 

![Cosine Similarity](PETFINDER IMAGE TEXT NUMERICAL AND CATEGORICAL VARIABLES/cosine_similarity.png)
                              
This returns a score between 0 and 1, the more the similar the two documents the higher the score we will get.

The task of having vectors and having them stored in the Elastic Search is that the preprocessing and getting Vcetors for all the Jobs/Candidates is a very computationally expensive task and us preprocessing and storing everything helps us to retrieve relevant candidates/ jobs quickly using the Vector based search.
Once we have the vectors for Jobs/Candidates then its very convenient to do the downstream tasks like getting similar Jobs/Candidates (using Cosine Similarity) and Jobs/Candidates Recommendation (using Dot Products) as explained above.

##Post Processing Logic Goes here
