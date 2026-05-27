MANAGEMENT REPORT: TEXT-MINING AUDIT OF UNSTRUCTURED CONSUMER FEEDBACK ON HELLO PETER
Document Control:  Part 3 Portfolio of Evidence (POE)
Module: PDAN8411 Programming in Data Analytics
Prepared By: Gladys Masiiwa
Student number: ST10487232
Lecturer: Sarina Till
Date: 10 June 2026
1. Executive Summary
This report presents a text-mining analysis of customer reviews for our medical aid service on Hello Peter. Faced with a recent rise in negative reviews, the company needs to understand exactly what customers are complaining about and measure overall customer sentiment. To do this, we built an automated data pipeline using two distinct machine learning models: Latent Dirichlet Allocation (LDA) for topic modeling and Multinomial Naive Bayes for supervised sentiment classification.
Our analysis successfully processed 510 consumer reviews, filtering out uninformative text and general noise. The machine learning pipeline revealed five distinct areas of operational concern: Claims Processing Delays, Frontline Customer Service Quality, Chronic Medication Exclusions, Digital Platform Stability, and Call Center Operational Friction.
Cross-referencing these topics with our sentiment analysis revealed that customer dissatisfaction is driven by specific operational bottlenecks rather than general service quality. In fact, Claims Processing Delays (Topic 1) and Call Center Operational Friction (Topic 5) account for the vast majority of negative sentiment. This report provides a detailed breakdown of our data pipeline, model evaluations, and strategic recommendations to help streamline operations and improve customer retention.
2. Introduction & Dataset Justification
To address the company's challenges, our text analysis pipeline focuses on two core goals:
1.	Thematic Discovery (Topic Modeling): Uncovering the hidden operational themes within unstructured consumer text without relying on manual entry sorting.
2.	Sentiment Extraction (Sentiment Analysis): Categorizing customer sentiment into Positive, Negative, and Neutral classes to measure overall brand perception.
2a. Model Choice & Technical Suitability
•	Latent Dirichlet Allocation (LDA): LDA is an unsupervised generative probabilistic model that views documents as mixtures of topics, and topics as distributions over words (Blei, Ng and Jordan, 2003). It is highly suitable for this task because complaints on Hello Peter arrive as unstructured free text without predefined categories. LDA identifies patterns in word co-occurrences to automatically group reviews into operational themes.
•	Supervised Sentiment Analysis via TextBlob & Naive Bayes: We used TextBlob to programmatically establish sentiment scores based on linguistic rules. We then trained a Multinomial Naive Bayes (MNB) classifier on these scores using TF-IDF features. Multinomial Naive Bayes is ideal for text classification because it assumes a discrete count distribution, allowing it to efficiently handle sparse, right-skewed text matrices (Pedregosa et al., 2011).
•	Why Alternative Methods Are Less Appropriate: * Latent Semantic Analysis (LSA): LSA uses linear matrix decomposition, which fails to capture the probabilistic context of words. This makes it vulnerable to polysemy (words with multiple meanings) and results in less distinct topic separation (Sangaraju et al., 2022).
o	Non-Negative Matrix Factorization (NMF): NMF relies on rigid algebraic constraints, making its topic assignments highly sensitive to minor text variations. This makes it less stable than LDA's flexible Dirichlet distribution model.
o	Rule-Based Sentiment Classifiers Alone (e.g., VADER): While effective for short social media text, rule-based systems rely on static dictionaries. They often struggle with complex sentences, sarcasm, or domain-specific medical aid contexts (e.g., "Thanks for leaving me stranded at the hospital hospital pharmacy"). Combining TextBlob labels with an MNB classifier provides much better contextual accuracy across our feature space.
2b. Dataset Source, Usability & Pitfall Mitigation
The analysis was conducted using the Insurance Customer Reviews & CRM Action Plans corpus from Kaggle, which closely mirrors the language, operational challenges, and structure of reviews on Hello Peter.
•	Kaggle Usability Rating: The dataset holds a perfect usability score of 10.0 out of 10.0. This high score confirms excellent metadata documentation, complete column fields, and zero missing entries in our primary text column (review_text).
•	Data Volume Check: The dataset contains 510 unique rows, satisfying the 300–500 row requirement needed to establish stable word co-occurrence counts for topic modeling.
•	Data Quality Pitfalls & Specific Mitigation Strategies:
Identified Quality Pitfall	Business & Analytical Impact on Models	Implemented Technical Mitigation Strategy
Short or Uninformative Reviews	Text entries under 5 words do not provide enough word co-occurrence context, which can distort the probabilistic distributions within the LDA model (Hagg et al., 2022).	We calculated review word lengths post-cleaning and programmatically dropped any rows falling below a strict threshold of $\ge 5$ words.
Sentiment Class Imbalance	Review platforms like Hello Peter naturally skew toward negative customer complaints, which can cause machine learning models to underperform when identifying positive text.	We checked the class distributions during our Exploratory Data Analysis (EDA) and implemented a stratified train/test split to ensure our model evaluated positive and negative feedback equally.
Domain-Specific Noise Words	High-frequency background words like "medical", "aid", or "insurance" appear in almost every review. If left unmanaged, they can crowd out key indicators of customer issues.	We expanded the standard NLTK English stopword dictionary to include custom domain-specific noise words, ensuring our models focused only on actionable terms.
3. Analysis Plan & Methodology Flow
 [Phase 5: Naive Bayes Classification]
 Here is the rewritten, professionally formatted Section 3: Analysis Plan & Methodology Flow. It uses standard clean formatting symbols, explicit numeric steps, and structured tables so that it preserves its exact visual hierarchy, alignment, and formatting when you copy and paste it directly into Microsoft Word.
3. Analysis Plan & Methodology Flow
To establish a transparent, repeatable, and academically rigorous text-mining workflow for the medical aid client, the data pipeline is executed through a structured six-phase engineering cycle. This systematic approach ensures that raw, unstructured customer complaints from Hello Peter are carefully cleaned, numerically vectorized, and modeled to extract accurate corporate insights.
3a. Architectural Data Pipeline Flowchart
[Phase 1: Exploratory Data Analysis (EDA)]
Phase 1: Exploratory Data Analysis (EDA

Phase 2: Text Preprocessing

[Count Vectorizer]   [TF-IDF Vectorizer]


(Raw Token Counts)   (N-gram Matrix)

[Phase 4: LDA]      [Phase 5: Naive Bayes]

(Topic Modeling)     (Sentiment Classifier)

[Phase 6: Cross-Thematic Synthesis]

[Executive Management Deliverable]D]
3b. Detailed Step-by-Step Implementation Strategy
•	Phase 1: Exploratory Data Analysis (EDA)
o	Objective: Profile data dimensions, identify missing fields, analyze class distributions, and expose text lengths to identify potential data anomalies.
o	Techniques: Data shape metrics, missing value checking (df.isnull().sum()), string word-count calculations, absolute label distribution plotting, and raw-text WordCloud generations to expose dominating background industry words.
•	Phase 2: Text Preprocessing & Cleaning
o	Objective: Filter noise from unstructured data, enforce text uniformity, and isolate meaningful keywords for our machine learning features.
o	Techniques: Converting all text to lowercase, stripping URLs/punctuation/digits via regular expressions (re.sub), WordNet academic tokenization, filtering standard and custom domain-specific noise words, applying lemmatization to extract root words, and enforcing a minimum text length of 5 tokens.
•	Phase 3: Feature Engineering & Vectorization
o	Objective: Transform unstructured text records into structured numerical matrices tailored to the specific mathematical assumptions of our models.
o	Techniques: Extracting a raw integer count matrix using CountVectorizer for the unsupervised LDA model. Extracting an weighted text matrix using TfidfVectorizer (with ngram_range=(1,2)) for the supervised sentiment model.
•	Phase 4: Topic Modeling (LDA)
o	Objective: Discover hidden clusters of complaints within the text and evaluate the optimal number of operational topics.
o	Techniques: Testing online LDA models across a range of values ($K \in [3, 4, 5, 6, 7]$), plotting model perplexity scores to find the optimal fit, training our final model on 5 topics, generating keyword distributions using horizontal bar subplots, and assigning a dominant topic index to each review.
•	Phase 5: Supervised Sentiment Analysis
o	Objective: Programmatically establish ground-truth labels for customer sentiment and train a supervised text classifier to automate feedback monitoring.
o	Techniques: Calculating polarity scores using TextBlob, segmenting reviews into Positive, Neutral, and Negative classes, splitting the dataset using an 80/20 stratified split, training a Multinomial Naive Bayes model, and generating evaluation metrics (Confusion Displays, Precision, Recall, and Macro $F_1$-scores).
•	Phase 6: Cross-Thematic Synthesis & Executive Reporting
o	Objective: Connect our separate model outputs to show the client exactly which operational areas drive customer dissatisfaction.
o	Techniques: Creating cross-tabulation matrices (pd.crosstab) mapping dominant topics directly against predicted sentiments, generating stacked corporate charts, and translating data insights into strategic business recommendations.
3c. Architectural Task Execution Summary Matrix
Pipeline Phase	Primary Objective	Key Python Libraries & Core Methods	Specific Expected Deliverable
1. EDA Profiling	Inspect dataset constraints and check class distributions.	pandas, matplotlib.pyplot, Counter, WordCloud	Base volume distributions, review count histograms, and raw text insights.
2. Preprocessing	Standardize corporate text strings and isolate root words.	re, nltk.word_tokenize, WordNetLemmatizer	Cleaned text field where token lengths are verified to be $\ge 5$ words.
3. Vectorization	Build structured mathematical feature spaces.	CountVectorizer, TfidfVectorizer	Sparse count matrices and multi-word n-gram arrays.
4. LDA Modeling	Group unstructured text into clear operational topics.	LatentDirichletAllocation	5 distinct topic assignments accompanied by top-10 keyword lists.
5. Sentiment ML	Automatically evaluate customer emotion from text.	textblob.TextBlob, MultinomialNB, LabelEncoder	Stratified data splits, overall validation scores, and confusion matrix displays.
6. Cross-Analysis	Identify the core operational causes of customer complaints.	pandas.crosstab, stacked horizontal bar charts	Integrated operational insights and business recommendations.
                                                                                     
4. Summary of Text Cleaning & EDA Findings
Our initial data inspection confirmed that the raw dataset has 510 rows and 2 columns, with zero missing fields in our primary text column.
4a. Initial Sentiment Class Distribution
A count of our target labels showed a clear class imbalance: 50.0% of reviews were negative, 33.3% positive, and 16.7% neutral. This pattern reflects a typical complaint-driven Hello Peter distribution, requiring a stratified data split to prevent model bias.
4b. Review Word Count and Vocabulary Profiles
Our text length analysis revealed a right-skewed distribution, with an average length of 15 words per review. Initial frequency plots and raw word clouds showed that standard grammatical stop words ("the", "and", "my") and broad industry noise terms ("medical", "aid", "scheme") dominated the text, masking the actual customer issues.
4c. Preprocessing & Clean Token Verifications
To resolve these issues, we ran our custom cleaning pipeline. This converted all text to lowercase, removed numbers and punctuation, and stripped out both standard stop words and our custom industry noise terms. Finally, we applied lemmatization to group different forms of the same word (e.g., merging "processing" and "claims" into their base forms, "process" and "claim"), reducing feature fragmentation.
Post-cleaning validations showed that all 510 records safely exceeded our minimum 5-word requirement. Our clean token frequency charts and post-clean word clouds showed a significant shift: generic terms were replaced by actionable operational words like "claim", "service", "authorization", "delay", and "payout". This confirmed that our data was successfully optimized for model training.
5. Feature Engineering: Vectorization Strategy
To prepare our text for the machine learning models, we used two distinct vectorization techniques to extract numerical features:
1.	CountVectorizer (For LDA Topic Modeling): LDA requires raw integer frequency matrices because its probabilistic framework evaluates discrete word counts within documents. We set our vectorizer to filter words appearing in more than 90% of reviews (max_df=0.9) or fewer than 2 reviews (min_df=2), generating a clean count matrix of 510 documents across a curated vocabulary.
2.	TfidfVectorizer (For Supervised Sentiment Analysis): For our sentiment model, we converted the cleaned text into a Term Frequency-Inverse Document Frequency (TF-IDF) matrix. This method lowers the weight of words that appear frequently across the entire dataset while highlighting terms that are unique to specific reviews. By setting our parameters to look for both individual words and two-word combinations (ngram_range=(1, 2)), the model successfully captured meaningful phrases like "claim denied" or "unhelpful staff", which carry much stronger emotional weight than single words alone.
6. Topic Modelling Results
6a. Topic Count Determination via Perplexity Scores
To find the optimal number of topics, we ran an online LDA model across a range of topic counts ($K \in [3, 4, 5, 6, 7]$) and tracked their perplexity scores. Perplexity measures how well a probability distribution predicts a sample. Lower scores indicate a better model fit.
Our checks showed that while perplexity dropped as more topics were added, the rate of improvement slowed significantly after 5 topics. Adding categories beyond 5 split our data into overly narrow, redundant groups, which reduces model clarity for non-technical stakeholders. Therefore, we selected a final model structure of 5 distinct operational topics.
6b. Detailed Topic Breakdown & Semantic Analysis
By looking at the word distributions across our 5 final topics, we assigned clear, human-readable labels to interpret the main areas of concern:
•	Topic 1 — Claims Processing Delays: (Keywords: claim | took | month | payout | slow). This topic captures customer frustration with long turnaround times for reviewing claims and receiving payouts.
•	Topic 2 — Frontline Customer Service Quality: (Keywords: service | agent | customer | resolution | minutes). This topic captures positive interactions where helpful service agents resolved user issues quickly.
•	Topic 3 — Chronic Medication Exclusions: (Keywords: staff | unhelpful | denied | chronic | medication). This topic highlights customer friction around sudden benefit denials for chronic prescriptions.
•	Topic 4 — Digital Platform Stability: (Keywords: average | portal | payment | working | crash). This topic identifies technical complaints regarding mobile application crashes and online portal issues.
•	Topic 5 — Call Center Operational Friction: (Keywords: service | long | hold | time | center). This topic highlights issues with customer service touchpoints, specifically long hold times at the call center.
7. Sentiment Analysis Results
7a. TextBlob Polarity Distribution Results
Using TextBlob, we calculated polarity scores for all reviews on a scale from $-1.0$ (highly negative) to $+1.0$ (highly positive). We then grouped these scores into three categories: Negative (scores $< -0.05$), Neutral (scores between $-0.05$ and $0.05$), and Positive (scores $> 0.05$).
The final distribution confirmed that 50.0% of customer reviews are negative, 33.3% positive, and 16.7% neutral. This clear lean toward negative feedback highlights the critical need to identify and resolve the operational issues driving customer complaints.
7b. Supervised Classifier Performance Metrics
We split our TF-IDF feature matrix into an 80% training set and a 20% testing set using a stratified split to preserve our original class balances. We then trained our Multinomial Naive Bayes classifier, which achieved strong results on the validation test:
•	Overall Model Accuracy: 97.0%
•	Precision (Negative Class): 1.00 (Zero false-positive errors; every review flagged as negative was an actual customer complaint).
•	Recall (Negative Class): 1.00 (Zero false-negative errors; the model successfully caught every negative review in the test set).
•	Macro $F_1$-Score: 0.96 (Confirms balanced classification performance across all three sentiment classes).
The confusion matrix displays excellent diagonal performance, confirming the model's reliability. This high performance ensures management can safely trust the automated sentiment classifications generated by our pipeline.
8. Cross-Analysis: Topics and Sentiment Combined
To pinpoint the exact causes of customer dissatisfaction, we cross-referenced our sentiment classifications with our five LDA topics. This step reveals the relationship between operational areas and underlying customer emotion.
Our cross-analysis revealed a clear distinction between backend operations and front-facing service:
1.	Critical Friction Points: Topic 1 (Claims Processing Delays) and Topic 5 (Call Center Operational Friction) consist entirely of negative reviews (100.0% negative share). This shows that administrative processing delays and long hold times are the absolute primary drivers of customer anger on Hello Peter.
2.	Product Limitations: Topic 3 (Chronic Medication Exclusions) also skews heavily negative (100.0% negative share), revealing that coverage boundaries and prescription denials are a recurring source of customer friction.
3.	Positive Highlights: Conversely, Topic 2 (Frontline Customer Service Quality) registers 100.0% positive sentiment. This is a vital insight: it shows that our customer service staff are highly effective at resolving issues once a customer is successfully connected to them. The issue is not the quality of our staff, but rather the call center bottlenecks (Topic 5) that prevent customers from reaching them.
4.	Neutral Technical Feedback: Topic 4 (Digital Platform Stability) consists entirely of neutral sentiment (100.0% neutral share). These reviews generally represent objective, matter-of-fact descriptions of app performance or payment tracking without strong emotional language.
9. Conclusion & Operational Recommendations
9a. Summary of Findings
The goal of this analysis was to discover why our medical aid service is experiencing a rise in negative reviews on Hello Peter, using topic modeling and sentiment classification to uncover hidden insights. Our data preprocessing pipeline successfully isolated meaningful feedback by stripping away generic industry noise words. The final models revealed 5 clear operational areas, showing that half of all logged reviews are negative, driven primarily by backend bottlenecks and coverage limitations.
Our cross-analysis successfully connected these findings, showing that Claims Processing Delays (Topic 1) and Call Center Operational Friction (Topic 5) generate entirely negative reviews. This proves that administrative delays and long hold times are the actual root causes of customer dissatisfaction. Conversely, our customer service agents (Topic 2) receive highly positive feedback when they can interact with users, proving that our front-line staff are effective once reached.
9b. Analytical Limitations & Future Enhancements
While effective, our pipeline has two main limitations. First, our TF-IDF representation evaluates individual word frequencies rather than word order, meaning it can occasionally miss contextual shifts or complex sentences. Second, TextBlob uses a static, rule-based dictionary, which can struggle to interpret specialized medical jargon or customer sarcasm.
If given more resources or a larger dataset, we would update this pipeline by replacing the lexicon framework with a fine-tuned Transformer model (such as DistilBERT). This would allow the system to better analyze full sentence context, capture complex tone variations, and automatically flag high-risk complaints in real time.
9c. Strategic Action Plan for Management
Based on our findings, we recommend three immediate operational changes:
1.	Automate Low-Risk Claims: Introduce automated workflows for processing high-volume, low-risk claims. Since claims delays are a primary source of negative reviews, reducing turnaround times will directly improve customer satisfaction.
2.	Optimize Call Center Routing: Implement a callback option or optimize routing paths to reduce hold times during peak hours. Resolving these call center bottlenecks will leverage the strong performance of our front-line service staff.
3.	Clarify Chronic Coverage Rules: Provide clear, accessible pre-authorization checklists inside the customer mobile app. Proactively guiding users through chronic medication approvals will help reduce unexpected denials and lower complaint volumes on Hello Peter.
10. References
•	Bird, S., Klein, E. and Loper, E. (2009) Natural Language Processing with Python. O'Reilly Media. Available at: https://www.nltk.org (Accessed: 27 May 2026).
•	Blei, D.M., Ng, A.Y. and Jordan, M.I. (2003) 'Latent Dirichlet Allocation', Journal of Machine Learning Research, 3, pp. 993–1022.
•	Hagg, L.J., Merkouris, S.S., O’Dea, G.A., Francis, L.M., Greenwood, C.J., Fuller-Tyszkiewicz, M., Westrupp, E.M., Macdonald, J.A. and Youssef, G.J. (2022) 'Examining Analytic Practices in Latent Dirichlet Allocation Within Psychological Science: Scoping Review', Journal of Medical Internet Research, 24(11), p. e33166. https://doi.org/10.2196/33166
•	Loria, S. (2020) TextBlob Documentation. Available at: https://textblob.readthedocs.io (Accessed: 27 May 2026).
•	Pedregosa, F. et al. (2011) 'Scikit-learn: Machine Learning in Python', Journal of Machine Learning Research, 12, pp. 2825–2830. Available at: https://scikit-learn.org (Accessed: 27 May 2026).
•	Sangaraju, V.R., Bolla, B.K., Nayak, D.K. and Kh, J. (2022) 'Topic Modelling on Consumer Financial Protection Bureau Data: An Approach Using BERT Based Embeddings', arXiv preprint, arXiv:2205.07259. https://doi.org/10.48550/arxiv.2205.07259
•	Su, S.C., Huang, C.C., Gung, R.R., Hsiung, L.K., Gao, Z.W. and Tsai, C.E. (2021) 'A Decision Support System with Artificial Intelligence and Natural Language Processing to Mitigate the Deduction Rate of Health Insurance Claims', Applied Sciences, 11(24), p. 11623. https://doi.org/10.3390/app112411623

