## Algorithmic bias
Refers to systematic and unfair discrimination in outcomes produced by algorithms, especially those that use machine learning or are rooted in artificial intelligence. It occurs when an algorithm consistently favours or disadvantages certain individuals or groups - often unintentionally.

### Why this occurs
- **Biased data** - Algorithms typically learn patterns from historical data. If this data reflects social or historical inequality, the algorithm perpetuates them.
- **Biased design** - Developers may (unconciously) introduce bias through choices in model architecture, training objectives, or feature selection. 
- **Feedback loops** - Algorithmic outputs can reinforce themselves via external consequences. Predictive policing for example, can result in sending more police to areas labelled high-crime, leading to more arrests there and reinforcing the label.
Thus algorithms, particularly machine learning algorithms are not inherently neutral; they reflect the equity and biases of only the environment they were conceived in. 


## Racial Bias
Racial bias is one of the most prominent and harmful forms of algorithmic bias, manifesting when technology harms or disadvantages people based on ethnicity or race.

### Examples
- **Predictive Policing** 
	- The **COMPAS** algorithm used in U.S. court to predict recidivism rate was found to predict Black defendants as 'high risk' almost twice as often as white defendants.
- **Healthcare Algorithms**
	- A study in *Science* found that a healthcare algorithm used to allocate extra care resources systematically understimated the medical needs of Black patients as it used ***healthcare spending*** as a predictor for ***health need***. Due to systemic inequality, extending to economic inequality, Black patients often have less access to care, leading this algorithm to believe they did not require as many resources as people from other races for the same health issue.
- **Hiring Algorithms**
	- Amazon's hiring AI discontinued in 2018 learned to prefer male applications as historical hiring data showed more men hired in tech roles. The model penalised resumes containing word's like 'women's'.

## Bias in Facial recognition
Facial recognition technology is notorious for racial and gender bias. 
Studies by Joy Buolamwini and Timnit Gebru found that commercial facial recongition systems had:
- Error rates under 1% for light-skinned men
- Error rates up to 35% for dark-skinned women
Skewed heavily toward light-skinned male faces

Misidentification by law enforcement is a consequence of this bias, several wrongful arrests worldwide have been traced to biased facial recognition. Lack of accountability with regard to machine learning models too.
Erosion of privacy etc.

## Targeted Advertising and Bias
Advertising algorithms often reflect and amplify societal bias.
- Googles ad system once showed fewer high paying job ads to women than men
- Facebooks advertising tools allowed advertisers before 2019 reforms to exclude users by race from seeing housing, job, or credit ads, clearly violating anti-discrimination laws.
- Credt scoring systems can produce biased results if they infer sensitive traits indirectly through proxies like postcode and browsing behaviour.
Advertisement algorithms optimise for engagement and click-through rate, which can lead to dscriminatory targeting if certain groups statistically behave differently online.
Reinforcement bias can occur when certain groups are shown fewer lucrative activites, or shown more risk-involved activities, perpetuating economic inequality e.g., showing more gambling ads to those from destitute areas.

## Chatbots and Language models
Chatbots and generative AI systems learn from giant corpora, undoubtedly including racist, sexist, and other harmful content which serves to propagate discrimination. 

**Examples:**
- **Microsoft's Tay Chatbot** quickly began posting racist and misogynistic tweets after being exposed to toxic online users
- **LLMs** can unconciously (often via jailbreaking) reproduce stereotypes or associate negative sentiment with certain demographics.
Risks of harms, including encountering derogatory language and experiencing discrimination at the hands of chatbots who may reproduce racist, sexist, ableist, extremist or other harmful ideologies warrants transparency about model limitations and potential harms.


## How to build chatbots around sensitive topics
Schlesinger et al. suggest that instead of just enforcing a blacklist, chatbots should handle sensitive topics like race, power and justice carefully. Rather than assume can be avoided, anticipate 'race-talk' - explicitly collect and aggregate dialogs to train chatbots in more respectful 'race-talk'. Ensure diverse representation of cultural references inter-ethnically - diversity in those who build the database is more likely to lead to a diverse database too.

NLP systems modern focus is on the use of probabilistic language models. But these don't necessarily tell you whether a statement is 'problematic'. 
Need a growing capacity for **meaning-in-context** in NLP - the areas of semantics and pragmatics.

## Word embeddings and bias
Word embeddings are numerical representations of words in continuous vector space (technical lecture covers this). Models like **Word2Vec** represent words such that words with similar meaning are close together in that space. 
$$vector(king) - vector(man) + vector(woman) ≈ vector(queen)$$
Word embeddings are trained on large text corpora. They inherit the biases and stereotypes present in language.

Bolukbasi et al., demonstrated that word embeddings contain biases in their geometry that reflect gender stereotypes present in broader society.
![[Pasted image 20251013205807.png]]
Researchers have developed quantitative tools to measure embedding bias such as **WEAT** which measures association strengths between target words and attribute words e.g., word embeddings show that “male” words are more strongly associated with “career” and “science,” while “female” words align more with “family” and “arts.”

The consequences of biased word embeddings can propagate through various permutations of natural language systems. 
- Search engines - biased autocomplete or ranking
- Sentiment analysis - e.g., associating "black" names with negative sentiment
- Machine translation - translating gender neutral languages into gendered stereotypes 
	Google Translate used to translate Turkish sentences like:
	- “O bir doktor.” → “He is a doctor.”
	- “O bir hemşire.” → “She is a nurse.”