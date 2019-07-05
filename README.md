# TREC 2019 Deep Learning Track Guidelines

## Timetable

* August 7: Submissions close
* August 21: Optional Docker images due for the Replicable Runs Initiative
* November 13-15: TREC conference

## Introduction

The Deep Learning Track studies information retrieval in a *large training data* regime. This is the case where the number of training queries with at least one positive label is at least in the tens of thousands, if not hundreds of thousands or more. This corresponds to real-world scenarios such as training based on click logs and training based on labels from shallow pools (such as the pooling in the TREC Million Query Track or the evaluation of search engines based on early precision).

Our main goal is to study what methods work best in this regime. For example, do the same methods that work on small data also work on large data? How much do methods improve when given more training data? What external data and weak supervision can be brought in to bear in this scenario, and how useful is it to combine full supervision with other forms of supervision including transfer learning?

Certain machine learning based methods, such as methods based on deep learning are known to require very large datasets for training. Lack of such large scale datasets has been a limitation for developing such methods for common information retrieval tasks, such as document ranking.  One of the goals of the track is to make such large-scale datasets publicly available, which could enable the development of different machine learning architectures without being constrained by the amount of training data. Through the evaluation methodologies we release as part of the track, we also enable participants to compare the performance of their methods with other state of the art methods.

Another novel aspect of this track is its involvement in the Replicable Runs Initiative (RRI), an exercise to make submissions replicable so that other teams can produce the same output.
This exercise is completely optional, but interested participants can (in the online submission form) designate any run to be an "RRI run".
The Docker images for replicating those runs will be due August 21 (two weeks after the run submission deadline).
See a separate page dedicated to the [Replicable Run Initiative (RRI)](https://github.com/osirrc/trec2019-rri) for additional details, including the specification of the Docker images.

## Deep Learning Track Tasks

The deep learning track has two tasks: Passage ranking and document ranking. You can submit up to three runs for each of these tasks.

Both tasks use a large human-generated set of training labels, from the [MS-MARCO](http://msmarco.org) dataset. The two tasks use the same test queries. They also use the same form of training data with usually one positive training document/passage per training query. In the case of passage ranking, there is a direct human label that says the passage can be used to answer the query, whereas for training the document ranking task we transfer the same passage-level labels to document-level labels.

Below the two tasks are described in more detail. 

### Document Ranking Task

The first task focuses on document ranking. We have two subtasks related to this: Full ranking and top-100 re-ranking.

In the full ranking (retrieval) subtask, you are expected to rank documents based on their relevance to the question, where documents can be retrieved from the full document collection provided. You can submit up to 1000 documents for this task. It models a scenario where you are building an end-to-end retrieval system.

In the re-ranking subtask, we provide you with an initial ranking of 100 documents from a simple IR system, and you are expected to re-rank the documents in terms of their relevance to the question. This is a very common real-world scenario, since many end-to-end systems are implemented as retrieval followed by top-k re-ranking. The re-ranking subtask allows participants to focus on re-ranking only, without needing to implement an end-to-end system. It also makes those re-ranking runs more comparable, because they all start from the same set of 100 candidates.

### Passage Ranking Rask

Similar to the document ranking task, the passage ranking task also has a full ranking and re-ranking subtasks.

In context of full ranking (retrieval) subtask, given a question, you are expected to rank passages from the full collection in terms of their likelihood of containing an answer to the question. You can submit up to 1000 passages for this end-to-end retrieval task.

In context of top-1000 re-ranking subtask, we provide you with an initial ranking of 1000 passages and you are expected to re-rank these passages based on their likelihood of containing an answer to the question. In this subtask, we can compare different re-ranking methods based on the same initial set of 1000 candidates, with the same rationale as described for the document re-ranking subtask.

### Use of external information

You are allowed to use external information while developing your runs. When you submit your runs, please fill in a form listing what evidence you used, for example an external corpus such as Wikipedia or a pre-trained model (e.g. word embeddings).

When submitting runs, participants will be able to indicate what resources they used. This could include the provided set of document ranking training data, but also optionally other data such as the passage ranking task labels or external labels or pretrained models. This will allow us to analyze the runs and break they down into types.

IMPORTANT NOTE: It is prohibited to use evidence from the MS-MARCO Question Answering task in your submission. That dataset reveals some minor details of how the MS MARCO dataset was constructed that would not be available in a real-world search engine; hence, should be avoided.

### Datasets

This year we have a document ranking dataset and a passage ranking dataset. The two datasets will share the same set of test queries, which will be released later.

#### Document ranking dataset

The document ranking dataset is based on source documents, which contained passages in the passage task. Although we have an incomplete set of documents that was gathered some time later than the passage data, the corpus is 3.2 million documents and our training set has 367,013 queries. For each training query, we map from a positive passage ID to the corresponding document ID in our 3.2 million. We do so on the assumption that a document that produced a relevant passage is usually a relevant document.

| Type   | Filename                                                                                                              | File size |              Num Records | Format                                                         |
|--------|-----------------------------------------------------------------------------------------------------------------------|----------:|-------------------------:|----------------------------------------------------------------|
| Corpus | [msmarco-docs.tsv](https://msmarco.blob.core.windows.net/msmarcoranking/msmarco-docs.tsv.gz)                          |     22 GB |               3,213,835  | tsv: docid, url, title, body                                   |
| Corpus | [msmarco-docs.trec](https://msmarco.blob.core.windows.net/msmarcoranking/msmarco-docs.trec.gz)                        |     22 GB |               3,213,835  | TREC DOC format (same content as msmarco-docs.tsv)                                               |
| Corpus | [msmarco-docs-lookup.tsv](https://msmarco.blob.core.windows.net/msmarcoranking/msmarco-docs-lookup.tsv.gz)            |    101 MB |               3,213,835  | tsv: docid, offset_trec, offset_tsv                            |
| Train  | [msmarco-doctrain-queries.tsv](https://msmarco.blob.core.windows.net/msmarcoranking/msmarco-doctrain-queries.tsv.gz)  |     15 MB |                 367,013  | tsv: qid, query                                                |
| Train  | [msmarco-doctrain-top100](https://msmarco.blob.core.windows.net/msmarcoranking/msmarco-doctrain-top100.gz)            |    1.8 GB |              36,701,116  | TREC submission: qid, "Q0", docid, rank, score, runstring      |
| Train  | [msmarco-doctrain-qrels.tsv](https://msmarco.blob.core.windows.net/msmarcoranking/msmarco-doctrain-qrels.tsv.gz)      |    7.6 MB |                 384,597  | TREC qrels format                                              |
| Train  | [msmarco-doctriples.py](https://github.com/microsoft/TREC-2019-Deep-Learning/blob/master/utils/msmarco-doctriples.py) |         - |                       -  | Python script generates training triples |
| Dev    | [msmarco-docdev-queries.tsv](https://msmarco.blob.core.windows.net/msmarcoranking/msmarco-docdev-queries.tsv.gz)      |    216 KB |                   5,193  | tsv: qid, query                                                |
| Dev    | [msmarco-docdev-top100](https://msmarco.blob.core.windows.net/msmarcoranking/msmarco-docdev-top100.gz)        |       27 MB |                     519,300  | TREC submission: qid, "Q0", docid, rank, score, runstring      |
| Dev    | [msmarco-docdev-qrels.tsv](https://msmarco.blob.core.windows.net/msmarcoranking/msmarco-docdev-qrels.tsv.gz)          |    112 KB |                   5,478  | TREC qrels format                                              |

#### Passage ranking dataset

This passage dataset is based on the public MS MARCO dataset, although our evaluation will be quite different. We will use a different set of test queries and we will use relevance judges to evaluate the quality of passage rankings in much more detail.

| Description                                           | Filename                                                                                                                | File size |                        Num Records | Format                                                         |
|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|----------:|-----------------------------------:|----------------------------------------------------------------|
| Collection                                | [collection.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/collection.tar.gz)                             |    2.9 GB |                         8,841,823  | tsv: pid, passage |
| Queries                                   | [queries.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/queries.tar.gz)                                   |   42.0 MB |                         1,010,916  | tsv: qid, query |
| Qrels Dev                                 | [qrels.dev.tsv](https://msmarco.blob.core.windows.net/msmarcoranking/qrels.dev.tsv)                                     |    1.1 MB |                            59,273  | TREC qrels format |
| Qrels Train                               | [qrels.train.tsv](https://msmarco.blob.core.windows.net/msmarcoranking/qrels.train.tsv)                                 |   10.1 MB |                           532,761  | TREC qrels format |
| Queries, Passages, and Relevance   Labels | [collectionandqueries.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/collectionandqueries.tar.gz)         |    2.9 GB |                        10,406,754  | |
| Train Triples Small                       | [triples.train.small.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/triples.train.small.tar.gz)           |   27.1 GB |                        39,782,779  | tsv: query, positive passage, negative passage |
| Train Triples Large                      | [triples.train.full.tsv.gz](https://msmarco.blob.core.windows.net/msmarcoranking/triples.train.full.tsv.gz)             |  272.2 GB |                       397,756,691  | tsv: query, positive passage, negative passage |
| Train Triples QID PID Format               | [qidpidtriples.train.full.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/qidpidtriples.train.full.tar.gz) |    5.7 GB |                       269,919,004  | tsv: qid, positive pid, negative pid |
| Top 1000 Train                            | [top1000.train.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/top1000.train.tar.gz)                       |  175.0 GB |                       478,016,942  | tsv: qid, pid, query, passage |
| Top 1000 Dev                              | [top1000.dev.tar.gz](https://msmarco.blob.core.windows.net/msmarcoranking/top1000.dev.tar.gz)                           |    2.4 GB |                         6,669,195  | tsv: qid, pid, query, passage |

## Submission instructions

We will be following a similar format as the ones used by most TREC submissions, which is repeated below. White space is used to separate columns. The width of the columns in the format is not important, but it is important to have exactly six columns per line with at least one space between the columns.

```text
1 Q0 pid1    1 2.73 runid1
1 Q0 pid2    1 2.71 runid1
1 Q0 pid3    1 2.61 runid1
1 Q0 pid4    1 2.05 runid1
1 Q0 pid5    1 1.89 runid1
```

, where:

* the first column is the topic (query) number.
* the second column is currently unused and should always be "Q0".
* the third column is the official identifier of the retrieved passage in context of passage ranking task, and the identifier of the retrieved document in context of document ranking task.
* the fourth column is the rank the passage/document is retrieved.
* the fifth column shows the score (integer or floating point) that generated the ranking. This score must be in descending (non-increasing) order.
* The sixth column is the ID of the run you are submitting.

## Evaluation and judging

As the official evaluation set, we will be using a set of 50 or more queries that are judged by NIST assessors. For this purpose, we will be using depth pooling and construct separate pools for the passage ranking and document ranking tasks. Passages/documents in these pools will then be labelled by NIST assessors using multi-graded judgments. The same set of queries will be used as the test set for both the passage retrieval and document retrieval tasks.

We may be making a superset of this official query set publicly available before the judgements for the official query set is available. The queries in this superset will be sparsely labelled, where the labels are directly reused from the MS-Marco dataset. More information regarding how these sparse labels were obtained can be found at <https://arxiv.org/abs/1611.09268>.

## Coordinators

Nick Craswell (Microsoft), Bhaskar Mitra (Microsoft), Emine Yilmaz (UCL) and Daniel Campos (Microsoft)

## Dataset files: Size on disk and md5sum

Since these are large files to download, here are the size in bytes and md5sum, as a reference.

### Document ranking

| Document ranking filename       | Size | MD5Sum        |
|---------------------------------|----------------:|----------------------------------|
| msmarco-docdev-qrels.tsv.gz     |           40960 | 2e00fe62ebfc29eb7ed219ba15f788c9 |
| msmarco-docdev-queries.tsv.gz   |           94208 | ac20593d71b9c32ab2633230f9cdf10d |
| msmarco-docdev-top100.gz        |         5705728 | ac10255edf321821b0ccd0f123037780 |
| msmarco-docs.trec.gz            |      8501800960 | d4863e4f342982b51b9a8fc668b2d0c0 |
| msmarco-docs.tsv.gz             |      8458567680 | 8e3529eea35ad7ee5009061c1f864bcf |
| msmarco-docs-lookup.tsv.gz      |        40382464 | 6be0a2f01bd9f35d8762eefef4fdb570 |
| msmarco-doctrain-qrels.tsv.gz   |         2387968 | e2b108a4f79ae1be3f97c356baff2ea0 |
| msmarco-doctrain-queries.tsv.gz |         6459392 | 4086d31a9cf2d7b69c4932609058111d |
| msmarco-doctrain-top100.gz      |       403566592 | be32fa12eb71e93014c84775d7465976 |

### Passage ranking

| Passage ranking filename        | Size | MD5Sum        |
|---------------------------------|----------------:|----------------------------------|
| collection.tar.gz               |      1035010048 | 87dd01826da3e2ad45447ba5af577628 |
| collectionandqueries.tar.gz     |      1057718272 | 31644046b18952c1386cd4564ba2ae69 |
| fulldocs.tsv.gz                 |      8432304128 | 835c0bb9db43d980db84c49309a4a43c |
| qidpidtriples.train.full.tar.gz |      2633560064 | 215a5204288820672f5e9451d9e202c5 |
| qrels.dev.tsv                   |         1204224 | 9157ccaeaa8227f91722ba5770787b16 |
| qrels.train.tsv                 |        10592256 | 733fb9fe12d93e497f7289409316eccf |
| queries.tar.gz                  |        18882560 | c177b2795d5f2dcc524cf00fcd973be1 |
| top1000.dev.tar.gz              |       686030848 | a909a9d5a8b00aa670b06f96100a7aa7 |
| top1000.eval.tar.gz             |       672067584 | c5b3c44abfb50676c254a8df89c3ccc5 |
| top1000.train.tar.gz            |     11499958272 | 4dc7b43b94c8bd64c630da9a06043cae |
| triples.train.full.tsv.gz       |     77877731328 | 8d509d484ea1971e792b812ae4800c6f |
| triples.train.small.tar.gz      |      7909744640 | 36e27d06e66b85957eb774b5504723a6 |

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Terms and Conditions

The MS MARCO datasets are intended for non-commercial research purposes only to promote advancement in the field of artificial intelligence and related areas, and is made available free of charge without extending any license or other intellectual property rights. The dataset is provided “as is” without warranty and usage of the data has risks since we may not own the underlying rights in the documents. We are not be liable for any damages related to use of the dataset. Feedback is voluntarily given and can be used as we see fit. Upon violation of any of these terms, your rights to use the dataset will end automatically.

## Legal Notices

Microsoft and any contributors grant you a license to the Microsoft documentation and other content
in this repository under the [Creative Commons Attribution 4.0 International Public License](https://creativecommons.org/licenses/by/4.0/legalcode),
see the [LICENSE](LICENSE) file, and grant you a license to any code in the repository under the [MIT License](https://opensource.org/licenses/MIT), see the
[LICENSE-CODE](LICENSE-CODE) file.

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at http://go.microsoft.com/fwlink/?LinkID=254653.

Privacy information can be found at https://privacy.microsoft.com/en-us/

Microsoft and any contributors reserve all other rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.
