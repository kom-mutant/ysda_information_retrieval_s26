### Task 1. Provide basic collection stats (20)

1. num of documents
2. Collection size in words
3. Avg. document length in words
4. # unique words (types)
5. Avg. word length
6. Avg. unique word (type) length

Описание коллекции:
{
    '# documents': 369721,
    'Collection size in words': 73093729,
    'Avg. document length in words': 197.7,
    '# unique words (types)': 794568,
    'Avg. word length': 4.795,
    'Avg. unique word (type) length': 7.714}
}
### Task 2. Frequency list

Build a word frequency list, i.e. a list of unique words in the collection along with their counts (frequencies), sorted by decreasing counts.

Cразу построим список стоп-слов из топа.

Fetch the English stopword list from the **Sebleier** gist (the NLTK English stopword list, 179 words). GitHub redirects the raw URL to the latest file revision.

Rank   Word                        Count  In stoplist
-------------------------------------------------------
1      the                     5,311,943  yes
2      of                      2,532,382  yes
3      in                      2,355,417  yes
4      and                     2,231,007  yes
5      a                       1,557,760  yes
6      to                      1,407,261  yes
7      was                     1,078,183  yes
8      he                        685,303  yes
9      is                        676,829  yes
10     as                        661,622  yes
11     for                       616,110  yes
12     on                        610,583  yes
13     with                      535,843  yes
14     by                        526,627  yes
15     s                         497,472  yes
16     at                        482,839  yes
17     his                       440,633  yes
18     from                      432,326  yes
19     it                        398,308  yes
20     that                      306,808  yes
21     an                        279,228  yes
22     which                     221,057  yes
23     first                     217,204  no
24     are                       206,921  yes
25     were                      205,684  yes
26     has                       195,167  yes
27     also                      186,055  no
28     she                       184,297  yes
29     after                     166,616  yes
30     its                       161,439  yes

Топ слов не из стоп-листа.

Rank   Word                        Count
-------------------------------------------------------
23     first                     217,204
27     also                      186,055
32     one                       157,573
37     new                       139,850
38     two                       136,427
42     school                    119,897
44     university                107,386
50     time                       95,373
54     1                          89,967
55     years                      88,565
56     year                       85,032
59     later                      80,738
60     became                     80,151
61     city                       77,682
63     world                      77,013
64     three                      76,155
66     known                      73,969
69     state                      70,280
71     national                   69,849
72     part                       69,635
73     united                     68,263
74     2                          67,911
76     born                       66,729
79     made                       65,449
80     team                       64,448
81     may                        62,403
83     season                     61,403
84     second                     61,249
85     south                      60,500
86     high                       60,138
==============

Стоп-слов в тексте:   29,387,785
Share of collection:          40.2%
Top-30 words not in stoplist: ['first', 'also']

How many occurrences of stopwords are there in the collection? Do all TOP-30 most frequent words occur in the stopword list? Would you recommend expanding the stopword list with some frequent words from the collection?


**Analysis.** Стоп-слова составляют 40-60% of any natural-language corpus (to claude: can you give me links to research). Этот корпус - не исключение, 40.2% слов в коллекции - стоп-слова.

Некоторые слова в `not_in_sw` that are domain-neutral high-frequency items (e.g. *also*, *one*, *two*, *used*, *new*) are strong candidates for stoplist expansion: they carry no discriminative power for retrieval and inflate postings lists unnecessarily.

Слово one's часто используется в знании "himself", "чей-то" 
(
    One should not use mobile phones when driving.
) 

Цитируя Cambridge Dictionary, 

"""
As a personal pronoun (both subject and object), one can be used to refer to ‘people in general’. We often use one in making generalisations, especially in more formal styles. However, if one is used too much, it can make the speaker sound too formal. One takes a third person singular verb:
"""

Однако для корректного учета указанных данных нужна статистика.


### Закон Ципфа

Zipf's law predicts frequency $\propto 1/\text{rank}$, which in log–log coordinates yields a straight line with slope $\approx -1$.

(placeholder for FIGURE 1 Hips Law)

### Закон Хипса

Vocabulary grows sublinearly with corpus size: $V(n) = K \cdot n^{\beta}$, where $\beta \in [0.4,\, 0.6]$ typically. A straight line in log–log coordinates confirms the law. Vocabulary size is sampled once per document for efficiency.

(placeholder for Figure 2 Zipf's law)

### Task 3. Build a TOP-31 frequency list of word bigrams (15)

Bigram                              Count  Has sense?
-------------------------------------------------------
of the                            768,320  no 
in the                            546,863  no 
to the                            256,451  no 
at the                            201,351  no 
and the                           170,394  no 
on the                            169,708  no 
he was                            163,554  no 
as a                              163,073  no 
for the                           159,502  no 
by the                            130,810  no 
with the                          120,118  no 
from the                          115,219  no 
the first                         104,264  no 
it was                            101,387  no 
as the                             96,915  no 
it is                              95,880  no 
is a                               82,161  no 
was a                              78,342  no 
in a                               76,786  no 
one of                             70,278  no 
was the                            69,881  no 
of a                               69,345  no 
with a                             63,880  no 
is the                             63,672  no 
to be                              62,139  no 
and was                            58,478  no 
during the                         56,159  no 
and a                              56,121  no 
part of                            54,180  no 
the united                         49,922  no 
united states                      48,952  yes

Как видим, первая биграмма, являющаяся отдельной лексической единицей, находится на 31 позиции списка самых популярных биграм. Посмотрим, как выглядят самые популярные лексически осмысленные биграммы, с 31 по 515 позицию.

Number                                           pair count                    
------------------------------------------------------------------
31                                      united states  48952               
41                                           new york  38477               
59                                        high school  29895               
91                                          world war  20915               
253                                            war ii  10432               
255                                         york city  10401               
266                                       los angeles  10042               
325                                         two years  9013                
343                                       new zealand  8764                
436                                       years later  7412                
438                                        best known  7391                
455                                        also known  7198                
488                                    united kingdom  6924                
515                                        first time  6640  

**Criteria for keeping a bigram as a dictionary entry in the inverted index:**

1. **No stopwords** — both components must be content words; stopword-headed bigrams (*of the*, *in a*) add noise and explode index size.
2. **Alphabetic** — both words must pass `str.isalpha()` (removes punctuation artifacts and numeric tokens).
3. **Minimum frequency** — count ≥ 5 avoids indexing hapax bigrams that waste space.
4. **Semantic cohesion** — the pair should name an entity, concept, or fixed expression (*new york*, *world war*, *solar system*); high PMI quantifies this.
5. **Not a generic phrase** — *very good*, *new year* score high on frequency but low on retrieval value; PMI relative to individual word frequencies helps distinguish them.


The **Content?** column below applies criteria 1 and 2 as a fast first-pass filter.

### Task 4. Morphological processing (25)

**Porter Stemmer** 

The Porter stemmer is **fully rule-based** and has **no tunable hyperparameters**. NLTK provides the canonical 1980 algorithm. We prefer it over Snowball or Lancaster as it is the standard IR baseline: moderately aggressive, producing human-readable stems with minimal over-stemming. Expected outcome: reduced vocabulary size due to suffix conflation (*running* → *run*, *studies* → *studi*).


Spacy lemmatization

**Model: `en_core_web_sm`** — spaCy's smallest English pipeline. Rationale:

- Smallest download footprint and fastest load time; a larger model adds no benefit for lemmatization of plain text.
- Uses a hash-based lookup table plus morphological rules — adequate quality for pre-tokenised, lowercased input.
- `parser` and `ner` components are disabled (not needed here), reducing per-document latency by ≈ 40 %.

Expected outcome: vocabulary size smaller than stemming because lemmatisation uses linguistic knowledge rather than suffix stripping (*better* → *good*, *was* → *be*).

**Model: `bert-base-uncased`** — WordPiece subword tokenizer. Rationale:

- `uncased` matches the lowercased collection — no casing signal is discarded.
- `base` is the standard IR benchmark variant; ]
- The 30,522-subword vocabulary and merge rules are fixed from pre-training — **no tunable hyperparameters**.

Expected outcome: total token count *exceeds* the original word count (rare surface forms are split into `##`-prefixed subpieces), but vocabulary size *decreases* (subwords are shared across many surface forms, giving better coverage).

Часть со сравнением.


Метод обработки 	Всего токенов 	Средняя длина документов 	Unique tokens 	Avg token length 	Avg unique length
0 	Без доп.токенизации 	73093729 	197.70 	794568 	4.80 	7.71
1 	Porter Stemming 	73093729 	197.70 	684579 	4.26 	7.34
2 	spaCy lemmatization 	73106364 	197.73 	759322 	4.59 	7.64
3 	BERT tokenization 	81185427 	219.59 	27528 	4.52 	6.63

**Что мы видим?**

