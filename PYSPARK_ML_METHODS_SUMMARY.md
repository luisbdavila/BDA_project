# Notebook API Inventory

## Scope
This inventory was checked against the solved notebooks for weeks 01 through 10. It covers the APIs, methods, functions, and notable helper routines that appear in code cells, with markdown-only mentions excluded unless they are backed by real code.

The most important correction from the earlier ML-only summary is that several notebooks before Week 7 are not ML notebooks at all, and Week 8/9 contain more than the headline ML classes: they also use pipeline, tuning, embedding, deep-learning, and general Spark transformation APIs.

---

## Week 01 - Functional Programming
**Notebook:** `functional_programming_solved.ipynb`

**Libraries and APIs used**
- `pandas.read_csv()` and basic `DataFrame` accessors such as `shape`, `columns`, `head()`, and `to_dict("records")`
- String methods such as `str.lower()`, `str.strip()`, `str.split()`, and `str.translate()`
- `re` for regex-based text handling
- `pathlib.Path` for file paths
- Python built-ins and language constructs: `sum()`, list comprehensions, generator expressions, loops, custom functions, and classes

**What is actually happening**
- The notebook is about functional programming concepts, not Spark ML.
- The main work is text cleanup, data reshaping, and demonstrating imperative vs functional vs object-oriented style.

---

## Week 02 - Map/Reduce
**Notebook:** `map_reduce_solved.ipynb`

**Libraries and APIs used**
- `pandas.read_csv()` and `DataFrame.to_dict(orient="records")`
- `collections.defaultdict`
- `nltk.download()` and `stopwords.words("english")`
- `string.punctuation`
- Custom MapReduce helpers such as `run_mapreduce()`, `shuffle_and_sort()`, custom mapper functions, and custom reducer functions

**What is actually happening**
- This is a pure-Python MapReduce implementation, not Spark ML.
- The notebook covers tokenization, stop-word removal, grouping by key, and result aggregation.

---

## Week 03 - Spark RDD Introduction
**Notebook:** `spark_rdd_introduction_solved.ipynb`

**Libraries and APIs used**
- `SparkSession.builder.master(...).appName(...).getOrCreate()`
- `SparkContext` and low-level RDD creation
- RDD methods: `parallelize()`, `textFile()`, `map()`, `flatMap()`, `filter()`, `reduceByKey()`, `distinct()`, `sortBy()`, `collect()`, `count()`, `take()`, `first()`, `cache()`, `repartition()`, `coalesce()`, `getNumPartitions()`
- Python standard-library helpers for CSV parsing and timing, including `csv.reader` and `time`
- Custom helper such as `parse_csv_line()`

**What is actually happening**
- This week focuses on RDD transformation/action semantics, lazy evaluation, partitioning, caching, and word-count style processing.
- It does not use Spark ML.

---

## Week 04 - Spark DataFrame API
**Notebook:** `lab4_solved.ipynb`

**Libraries and APIs used**
- `SparkSession.builder.master(...).appName(...).config(...).getOrCreate()`
- DataFrame reader: `spark.read.csv(..., header=True, inferSchema=True)`
- Column and expression helpers from `pyspark.sql.functions`, especially `col`, `when`, `desc`, `max`, `min`, `avg`, `sum`, `countDistinct`, and `round`
- DataFrame methods: `select()`, `filter()`, `where()`, `withColumn()`, `withColumnRenamed()`, `drop()`, `dropDuplicates()`, `distinct()`, `dropna()`, `fillna()`, `groupBy()`, `agg()`, `count()`, `mean()`, `max()`, `min()`, `orderBy()`, `sort()`, `show()`
- Plotting helpers through the notebook’s DataFrame plotting interface, including `plot.scatter()` and `plot.box()`

**What is actually happening**
- This is core DataFrame work: projection, filtering, mutation, aggregation, ordering, null handling, and visualization.
- No ML APIs are used here.

---

## Week 05 - Spark SQL
**Notebook:** `lab5_solved.ipynb`

**Libraries and APIs used**
- `SparkSession.builder.master(...).appName(...).config(...).getOrCreate()`
- `spark.read.option(...).csv()` with explicit options such as `header` and `inferSchema`
- `StructType` and `StructField` for explicit schema definition
- `printSchema()` and `createOrReplaceTempView()`
- `spark.sql(...)` for SQL execution
- `spark.createDataFrame(...)` for building DataFrames from Python data

**SQL and query patterns used**
- `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, `JOIN`, `LEFT JOIN`, `LIMIT`
- `CASE WHEN` expressions
- CTEs via `WITH`
- Aggregates such as `COUNT`, `AVG`, `MIN`, `MAX`, `SUM`, and `COUNT(DISTINCT ...)`

**What is actually happening**
- Week 5 is SQL-centric, not ML-centric.
- The notebook demonstrates relational querying and schema handling inside Spark.

---

## Week 06 - Nested Data, JSON, Arrays, and Structs
**Notebook:** `lab6_solved.ipynb`

**Libraries and APIs used**
- `SparkSession`
- `pyspark.sql.functions` as `F`
- `StructType`, `StructField`, `ArrayType`, `StringType`, `DoubleType`, and related nested schemas
- Reader options such as `multiLine`, `quote`, `escape`, and `unescapedQuoteHandling`
- Column functions and expressions: `trim()`, `regexp_replace()`, `from_json()`, `coalesce()`, `expr()`, `try_cast()`, `regexp_extract()`, `flatten()`, `element_at()`, `array()`, `split()`
- Column access helpers: `getItem()`, `getField()`
- DataFrame methods: `withColumn()`, `select()`, `show()`, `printSchema()`, `limit()`

**What is actually happening**
- This week is about parsing messy nested data and turning strings into Spark arrays and structs.
- `from_json()` and `try_cast()` are the most important data-shaping functions here.
- There is still no Spark ML work in this notebook.

---

## Week 07 - Spark ML Foundations
**Notebook:** `lab7_solved.ipynb`

**Spark SQL and DataFrame APIs used alongside ML**
- `spark.read.parquet()`
- `printSchema()`, `select()`, `show()`, `groupBy()`, `count()`, `orderBy()`, `na.drop()`, `count()`
- `withColumn()` and `cast()` for label and boolean conversion

**Spark ML feature engineering and statistics**
- `StringIndexer`
- `OneHotEncoder`
- `VectorAssembler`
- `Normalizer`
- `Tokenizer`
- `HashingTF`
- `IDF`
- `Correlation.corr()`
- `DenseVector` and `SparseVector`

**Spark ML models and evaluation**
- `LogisticRegression`
- `DecisionTreeClassifier`
- `RandomForestClassifier`
- `GBTClassifier`
- `BinaryClassificationEvaluator`

**Non-Spark ML / helper code used in the notebook**
- `pyod.models.iforest.IForest` inside `applyInPandas`
- `pandas` for executor-side partition processing
- `sklearn.metrics.roc_curve` and `sklearn.metrics.auc` inside helper evaluation code
- `matplotlib` and `seaborn` for plots
- Plotly-based bar plots via the notebook plotting API

**Custom helpers and workflows**
- A custom `score_with_isolation_forest()` function for partition-wise anomaly scoring
- A custom `evaluate_model()` helper for model evaluation and ROC plotting
- Train/test splitting with `randomSplit([0.7, 0.3], seed=42)`
- Caching with `.cache()` before model fitting
- Model fitting with `.fit()` and prediction with `.transform()`

**What is actually happening**
- This is the first notebook with substantial Spark ML usage.
- The main flow is: clean data, index and encode categories, assemble features, normalize numerics, fit classifiers, and evaluate with AUC/ROC.

---

## Week 08 - Spark ML Pipelines, Tuning, Embeddings, and Clustering
**Notebook:** `lab8_solved.ipynb`

**Spark ML pipeline and preprocessing APIs**
- `Pipeline`
- `PipelineModel`
- `StringIndexer`
- `OneHotEncoder`
- `VectorAssembler`
- `Imputer`
- `MinMaxScaler`
- `Tokenizer`
- `Word2Vec`
- `PCA`

**Spark ML models and tuning APIs**
- `LogisticRegression`
- `RandomForestClassifier`
- `KMeans`
- `BinaryClassificationEvaluator`
- `ClusteringEvaluator`
- `ParamGridBuilder`
- `CrossValidator`
- `Summarizer`
- `VectorUDT`
- `Vectors`

**DataFrame and persistence APIs used with ML**
- `spark.read.parquet()`
- `na.drop()`
- `withColumn()` and `cast(IntegerType())`
- `randomSplit()` and `cache()`
- `write.mode("overwrite").save()` and `PipelineModel.load()`
- Accessing fitted pipeline stages with `.stages`

**What is actually happening**
- The notebook goes beyond a simple pipeline and includes several ML patterns:
	- a manual preprocessing pipeline,
	- a fitted `PipelineModel`,
	- cross-validation with a parameter grid,
	- word embeddings via `Word2Vec`,
	- PCA-based dimensionality reduction,
	- and KMeans clustering on learned embeddings.
- The notebook also distinguishes estimators from transformers and shows how to avoid leakage by fitting only on training data.

**Important verified details**
- `Imputer` and `MinMaxScaler` are not just mentioned in markdown; they are used in code.
- `Word2Vec` is genuinely used in code, including `.fit()`, `.transform()`, `.findSynonyms()`, and `.getVectors()`.
- `PCA` is also used in code as part of the Word2Vec + PCA pipeline.

---

## Week 09 - PyTorch, TorchDistributor, and Distributed Inference
**Notebook:** `lab9_solved.ipynb`

**Spark ML / Spark integration APIs**
- `TorchDistributor`
- `predict_batch_udf`
- `array_to_vector`
- `StringIndexer`
- `LogisticRegression`
- `MulticlassClassificationEvaluator`
- `spark.read.format("binaryFile")`
- `repartition()` and `cache()`
- `spark.read.parquet()` and `write.mode("overwrite").parquet()`

**PyTorch and torchvision APIs used**
- `torch`
- `torch.nn` as `nn`
- `torch.nn.functional` as `F`
- `torch.optim` as `optim`
- `nn.Module`, `nn.Sequential`, `nn.Conv2d`, `nn.MaxPool2d`, `nn.Flatten`, `nn.Linear`, `nn.Dropout`, `nn.ReLU`
- `optim.Adam`
- `F.cross_entropy`
- `model.train()`, `model.eval()`, `model.to(device)`, `model.state_dict()`, `model.load_state_dict()`
- `DataLoader`, `TensorDataset`, `random_split`
- `datasets.MNIST()`
- `transforms.Compose()`, `Resize()`, `CenterCrop()`, `ToTensor()`, `Normalize()`
- `torchvision.models.resnet50()` and `ResNet50_Weights.DEFAULT`
- `PIL.Image.open()` and `.convert("RGB")`

**Other libraries used**
- `tarfile`, `pathlib.Path`, `io`
- `pandas`
- `numpy`
- HuggingFace `transformers.pipeline`

**What is actually happening**
- The notebook has three major workflows:
	- a TorchDistributor MNIST CNN training example,
	- a Spark-based ResNet feature extraction and Spark ML classification pipeline,
	- and a DistilBERT sentiment-analysis inference pipeline using `predict_batch_udf`.
- The PyTorch training code is intentionally self-contained because `TorchDistributor` serializes the function to worker processes.

**Important verified details**
- The CNN architecture really uses `Conv2d`, `MaxPool2d`, `Flatten`, `Linear`, and `Dropout`.
- `predict_batch_udf` is used for both image feature extraction and text sentiment inference.
- `array_to_vector` is used to convert Spark array columns into ML-ready vectors.

---

## Week 10
**Status:** no week-10 notebook was present in the workspace snapshot.

The `week10` folder contains data material, but there is no solved notebook to inventory in the same way as the earlier weeks.

---

## Cross-Week Summary

### Weeks with no Spark ML APIs
- Week 01: functional programming only
- Week 02: custom MapReduce only
- Week 03: RDD fundamentals only
- Week 04: DataFrame API and visualization only
- Week 05: Spark SQL only
- Week 06: nested data and parsing only
- Week 10: no notebook available to inspect

### Weeks with Spark ML or adjacent ML tooling
- Week 07: classical Spark ML classification, feature engineering, and evaluation
- Week 08: Spark ML pipelines, embeddings, clustering, tuning, and persistence
- Week 09: Spark ML integration with PyTorch, TorchDistributor, and distributed inference

### Key Spark ML families used across the course
- Feature engineering: `StringIndexer`, `OneHotEncoder`, `VectorAssembler`, `Normalizer`, `Tokenizer`, `HashingTF`, `IDF`, `Imputer`, `MinMaxScaler`, `Word2Vec`, `PCA`
- Classification: `LogisticRegression`, `DecisionTreeClassifier`, `RandomForestClassifier`, `GBTClassifier`
- Clustering: `KMeans`
- Evaluation: `BinaryClassificationEvaluator`, `MulticlassClassificationEvaluator`, `ClusteringEvaluator`
- Tuning and pipelines: `Pipeline`, `PipelineModel`, `ParamGridBuilder`, `CrossValidator`
- Statistics and utilities: `Correlation.corr()`, `Summarizer`, `Vectors`, `VectorUDT`, `array_to_vector`
- Distributed inference and training: `TorchDistributor`, `predict_batch_udf`

## Final Check Notes
- Week 8 includes more than the earlier summary captured: `Imputer`, `MinMaxScaler`, `Word2Vec`, and `PCA` are all used in actual code.
- Week 9 is broader than Spark ML alone: most of it is PyTorch and torchvision code orchestrated through Spark.
- Week 04 and Week 06 are data-wrangling notebooks, not ML notebooks.

