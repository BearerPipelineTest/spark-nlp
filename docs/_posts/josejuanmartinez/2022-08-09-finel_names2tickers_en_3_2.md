---
layout: model
title: Company Names -> Ticker Linking
author: John Snow Labs
name: finel_names2tickers
date: 2022-08-09
tags: [en, finance, companies, tickers, nasdaq, licensed]
task: Entity Resolution
language: en
edition: Spark NLP for Finance 1.0.0
spark_version: 3.2
supported: true
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

This is an Entity Resolution / Entity Linking model, which is able to provide Ticker / Trading Symbols using a Company Name as an input. You can use any NER which extracts Organizations / Companies / Parties to then send the output to this Entity Linking model and get the Ticker / Trading Symbol (given the company has one).

## Predicted Entities



{:.btn-box}
[Live Demo](https://nlp.johnsnowlabs.com/financial_company_normalization){:.button.button-orange}
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/finance/models/finel_names2tickers_en_1.0.0_3.2_1660040016121.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
from johnsnowlabs.extensions.finance.chunk_classification.resolution import SentenceEntityResolverModel

documentAssembler = DocumentAssembler()\
      .setInputCol("text")\
      .setOutputCol("ner_chunk")

embeddings = UniversalSentenceEncoder.pretrained("tfhub_use", "en") \
      .setInputCols("ner_chunk") \
      .setOutputCol("sentence_embeddings")
    
resolver = SentenceEntityResolverModel.pretrained("finel_names2tickers", "en", "finance/models") \
      .setInputCols(["ner_chunk", "sentence_embeddings"]) \
      .setOutputCol("name")\
      .setDistanceFunction("EUCLIDEAN")

pipelineModel = PipelineModel(
      stages = [
          documentAssembler,
          embeddings,
          resolver])

lp = LightPipeline(pipelineModel)

lp.fullAnnotate("apple")
```

</div>

## Results

```bash
+--------+---------+----------------------------------------------+-------------------------+-----------------------------------+
|   chunk|    code |                                     all_codes|             resolutions |                      all_distances|
+--------+---------+----------------------------------------------+-------------------------+-----------------------------------+
|  apple |   Apple | [Apple, Apple inc., Apple INC. , Apple Inc.] |[AAPL, AAPL, AAPL, AAPL] |  [0.0000, 0.1093, 0.1093, 0.1093] |
+--------+---------+----------------------------------------------+-------------------------+-----------------------------------+
```

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|finel_names2tickers|
|Type:|finance|
|Compatibility:|Spark NLP for Finance 1.0.0+|
|License:|Licensed|
|Edition:|Official|
|Input Labels:|[sentence_embeddings]|
|Output Labels:|[company_ticker]|
|Language:|en|
|Size:|20.3 MB|
|Case sensitive:|false|

## References

https://data.world/johnsnowlabs/list-of-companies-in-nasdaq-exchanges