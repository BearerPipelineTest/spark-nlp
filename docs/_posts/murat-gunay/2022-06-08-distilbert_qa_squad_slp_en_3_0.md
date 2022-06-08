---
layout: model
title: English DistilBertForQuestionAnswering model (from rowan1224)
author: John Snow Labs
name: distilbert_qa_squad_slp
date: 2022-06-08
tags: [en, open_source, distilbert, question_answering]
task: Question Answering
language: en
edition: Spark NLP 3.4.4
spark_version: 3.0
supported: true
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

Pretrained Question Answering model, adapted from Hugging Face and curated to provide scalability and production-readiness using Spark NLP. `distilbert-squad-slp` is a English model orginally trained by `rowan1224`.

{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/public/models/distilbert_qa_squad_slp_en_3.4.4_3.0_1654727731033.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
documentAssembler = MultiDocumentAssembler() \
    .setInputCols(["question", "context"]) \
    .setOutputCols(["document_question", "document_context"])

spanClassifier = DistilBertForQuestionAnswering.pretrained("distilbert_qa_squad_slp","en") \
    .setInputCols(["document_question", "document_context"]) \
    .setOutputCol("answer")\
    .setCaseSensitive(True)
    
pipeline = Pipeline(stages=[documentAssembler, spanClassifier])

data = spark.createDataFrame([["What is my name?", "My name is Clara and I live in Berkeley."]]).toDF("question", "context")

result = pipeline.fit(data).transform(data)
```
```scala
val documentAssembler = new MultiDocumentAssembler() 
      .setInputCols(Array("question", "context")) 
      .setOutputCols(Array("document_question", "document_context"))
 
val spanClassifer = DistilBertForQuestionAnswering.pretrained("distilbert_qa_squad_slp","en") 
    .setInputCols(Array("document", "token")) 
    .setOutputCol("answer")
    .setCaseSensitive(true)

val pipeline = new Pipeline().setStages(Array(documentAssembler, spanClassifier))

val data = Seq("What is my name?", "My name is Clara and I live in Berkeley.").toDF("question", "context")

val result = pipeline.fit(data).transform(data)
```
</div>

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|distilbert_qa_squad_slp|
|Compatibility:|Spark NLP 3.4.4+|
|License:|Open Source|
|Edition:|Official|
|Input Labels:|[document_question, document_context]|
|Output Labels:|[answer]|
|Language:|en|
|Size:|247.5 MB|
|Case sensitive:|true|
|Max sentence length:|512|

## References

- https://huggingface.co/rowan1224/distilbert-squad-slp