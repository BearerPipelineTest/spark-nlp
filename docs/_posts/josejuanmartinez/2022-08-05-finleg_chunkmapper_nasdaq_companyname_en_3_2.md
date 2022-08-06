---
layout: model
title: Mapping Company Names to NASDAQ Tickers
author: John Snow Labs
name: finleg_chunkmapper_nasdaq_companyname
date: 2022-08-05
tags: [en, finance, licensed]
task: Chunk Mapping
language: en
edition: Spark NLP for Finance 1.0.0
spark_version: 3.2
supported: true
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

This model allows you to, given an extracted name of a company, get information about that company, including the Industry, the Sector and the Trading Symbol (ticker).

## Predicted Entities



{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/finance/models/finleg_chunkmapper_nasdaq_companyname_en_1.0.0_3.2_1659713215269.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
document_assembler = DocumentAssembler()\
      .setInputCol('text')\
      .setOutputCol('document')

tokenizer = Tokenizer()\
      .setInputCols("document")\
      .setOutputCol("token")

embeddings = WordEmbeddingsModel.pretrained('glove_100d') \
        .setInputCols(['document', 'token']) \
        .setOutputCol('embeddings')

ner_model = NerDLModel.pretrained("onto_100", "en") \
        .setInputCols(["document", "token", "embeddings"]) \
        .setOutputCol("ner")
 
ner_converter = NerConverter()\
      .setInputCols(["document", "token", "ner"])\
      .setOutputCol("ner_chunk")\
      .setWhiteList(["ORG"])

CM = ChunkMapperModel()\
      .pretrained('finleg_chunkmapper_nasdaq_companyname', 'en', 'finance/models')\
      .setInputCols(["ner_chunk"])\
      .setOutputCol("mappings")\
      .setRel('ticker')

pipeline = Pipeline().setStages([document_assembler,
                                 tokenizer, 
                                 embeddings,
                                 ner_model, 
                                 ner_converter, 
                                 CM])
                                 
text = ["""Altaba Inc. is a company which ..."""]

test_data = spark.createDataFrame([text]).toDF("text")

model = pipeline.fit(test_data)
res= model.transform(test_data)

res.select('mappings').collect()
```

</div>

## Results

```bash
[Row(mappings=[Row(annotatorType='labeled_dependency', begin=0, end=10, result='AABA', metadata={'sentence': '0', 'chunk': '0', 'entity': 'Altaba Inc.', 'relation': 'ticker', 'all_relations': ''}, embeddings=[]), Row(annotatorType='labeled_dependency', begin=0, end=10, result='Altaba Inc.', metadata={'sentence': '0', 'chunk': '0', 'entity': 'Altaba Inc.', 'relation': 'company_name', 'all_relations': ''}, embeddings=[]), Row(annotatorType='labeled_dependency', begin=0, end=10, result='Altaba', metadata={'sentence': '0', 'chunk': '0', 'entity': 'Altaba Inc.', 'relation': 'short_name', 'all_relations': ''}, embeddings=[]), Row(annotatorType='labeled_dependency', begin=0, end=10, result='Asset Management', metadata={'sentence': '0', 'chunk': '0', 'entity': 'Altaba Inc.', 'relation': 'industry', 'all_relations': ''}, embeddings=[]), Row(annotatorType='labeled_dependency', begin=0, end=10, result='Financial Services', metadata={'sentence': '0', 'chunk': '0', 'entity': 'Altaba Inc.', 'relation': 'sector', 'all_relations': ''}, embeddings=[])])]
```

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|finleg_chunkmapper_nasdaq_companyname|
|Type:|finance|
|Compatibility:|Spark NLP for Finance 1.0.0+|
|License:|Licensed|
|Edition:|Official|
|Input Labels:|[ner_chunk]|
|Output Labels:|[mappings]|
|Language:|en|
|Size:|210.4 KB|

## References

https://data.world/johnsnowlabs/list-of-companies-in-nasdaq-exchanges