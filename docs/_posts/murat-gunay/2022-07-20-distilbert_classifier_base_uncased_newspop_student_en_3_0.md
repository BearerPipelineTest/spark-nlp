---
layout: model
title: English DistilBertForSequenceClassification Base Uncased model (from mrm8488)
author: John Snow Labs
name: distilbert_classifier_base_uncased_newspop_student
date: 2022-07-20
tags: [open_source, distilbert, sequence_classifier, classification, newspop, en]
task: Text Classification
language: en
edition: Spark NLP 4.0.0
spark_version: 3.0
supported: true
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

Pretrained DistilBERT Classification model, adapted from Hugging Face and curated to provide scalability and production-readiness using Spark NLP. `distilbert-base-uncased-newspop-student` is a Spanish model originally trained by `mrm8488`.

## Predicted Entities

`palestine`, `obama`, `microsoft`, `economy`

{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/public/models/distilbert_classifier_base_uncased_newspop_student_en_4.0.0_3.0_1658326819970.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
documentAssembler = DocumentAssembler() \
    .setInputCol("text") \
    .setOutputCol("document")

tokenizer = Tokenizer() \
    .setInputCols("document") \
    .setOutputCol("token")
  
seq = DistilBertForSequenceClassification.pretrained("distilbert_classifier_base_uncased_newspop_student","en") \
    .setInputCols(["document", "token"]) \
    .setOutputCol("class")
    
pipeline = Pipeline(stages=[documentAssembler, tokenizer, seq])

data = spark.createDataFrame([["PUT YOUR STRING HERE."]]).toDF("text")

result = pipeline.fit(data).transform(data)
```
```scala
val documentAssembler = new DocumentAssembler() 
      .setInputCol("text") 
      .setOutputCol("document")

val tokenizer = new Tokenizer() 
    .setInputCols(Array("document"))
    .setOutputCol("token")

val seq = DistilBertForSequenceClassification.pretrained("distilbert_classifier_base_uncased_newspop_student","en") 
    .setInputCols(Array("document", "token")) 
    .setOutputCol("class")

val pipeline = new Pipeline().setStages(Array(documentAssembler, tokenizer, seq))

val data = Seq("PUT YOUR STRING HERE.").toDS.toDF("text")

val result = pipeline.fit(data).transform(data)
```
</div>

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|distilbert_classifier_base_uncased_newspop_student|
|Compatibility:|Spark NLP 4.0.0+|
|License:|Open Source|
|Edition:|Official|
|Input Labels:|[document, token]|
|Output Labels:|[class]|
|Language:|en|
|Size:|249.8 MB|
|Case sensitive:|true|
|Max sentence length:|128|

## References

https://huggingface.co/mrm8488/distilbert-base-uncased-newspop-student
