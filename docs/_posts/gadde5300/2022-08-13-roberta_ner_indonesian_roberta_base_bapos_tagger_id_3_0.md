---
layout: model
title: Indonesian RobertaForTokenClassification Base Cased model (from StevenLimcorn)
author: John Snow Labs
name: roberta_ner_indonesian_roberta_base_bapos_tagger
date: 2022-08-13
tags: [bert, ner, open_source, id]
task: Named Entity Recognition
language: id
edition: Spark NLP 4.1.0
spark_version: 3.0
supported: true
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

Pretrained RobertaForTokenClassification model, adapted from Hugging Face and curated to provide scalability and production-readiness using Spark NLP. `indonesian-roberta-base-bapos-tagger` is a Indonesian model originally trained by `StevenLimcorn`.

## Predicted Entities

`MD`, `JJ`, `NEG`, `UH`, `CD`, `CC`, `SYM`, `PRP`, `FW`, `SC`, `X`, `RB`, `RP`, `OD`, `IN`, `NN`, `NNP`, `VB`, `PR`, `NND`, `DT`, `WH`, `Z`

{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/public/models/roberta_ner_indonesian_roberta_base_bapos_tagger_id_4.1.0_3.0_1660405083788.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
documentAssembler = DocumentAssembler() \
        .setInputCol("text") \
        .setOutputCol("document")

sentenceDetector = SentenceDetectorDLModel.pretrained("sentence_detector_dl", "xx")\
       .setInputCols(["document"])\
       .setOutputCol("sentence")

tokenizer = Tokenizer() \
    .setInputCols("sentence") \
    .setOutputCol("token")

tokenClassifier = BertForTokenClassification.pretrained("roberta_ner_indonesian_roberta_base_bapos_tagger","id") \
    .setInputCols(["sentence", "token"]) \
    .setOutputCol("ner")

pipeline = Pipeline(stages=[documentAssembler, sentenceDetector, tokenizer, tokenClassifier])

data = spark.createDataFrame([["Saya suka Spark NLP"]]).toDF("text")

result = pipeline.fit(data).transform(data)
```
```scala
val documentAssembler = new DocumentAssembler() 
          .setInputCol("text") 
          .setOutputCol("document")

val sentenceDetector = SentenceDetectorDLModel.pretrained("sentence_detector_dl", "xx")
       .setInputCols(Array("document"))
       .setOutputCol("sentence")

val tokenizer = new Tokenizer() 
    .setInputCols(Array("sentence"))
    .setOutputCol("token")

val tokenClassifier = BertForTokenClassification.pretrained("roberta_ner_indonesian_roberta_base_bapos_tagger","id") 
    .setInputCols(Array("sentence", "token")) 
    .setOutputCol("ner")

val pipeline = new Pipeline().setStages(Array(documentAssembler,sentenceDetector, tokenizer, tokenClassifier))

val data = Seq("Saya suka Spark NLP").toDF("text")

val result = pipeline.fit(data).transform(data)
```
</div>

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|roberta_ner_indonesian_roberta_base_bapos_tagger|
|Compatibility:|Spark NLP 4.1.0+|
|License:|Open Source|
|Edition:|Official|
|Input Labels:|[document, token]|
|Output Labels:|[ner]|
|Language:|id|
|Size:|466.1 MB|
|Case sensitive:|true|
|Max sentence length:|128|

## References

- https://huggingface.co/StevenLimcorn/indonesian-roberta-base-bapos-tagger