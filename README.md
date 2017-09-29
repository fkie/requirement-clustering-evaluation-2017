# requirement-clustering-evaluation-2017

## Import from csv

	sqlite3 requirement-clustering-results.db << EOF
	
	create table alpha (ClusterAlgorithm, DistanceFunction, UsedFields, Tfidf, StopWords, Interpreted, Lemmatized, Source, Synonyms, GermaNetFunction, F1WeightedAvg REAL, F1WeightedStd REAL, CohesionAvg REAL, CohesionStd REAL, SeperationAvg REAL, SeperationStd REAL, SilhouetteAvg REAL, SilhouetteStd REAL, RuntimeAvg REAL);
	.separator ";"
	.import "alpha.csv" "alpha"
	DELETE from alpha WHERE SeperationAvg = 'SeperationAvg';
	DELETE from alpha WHERE ClusterAlgorithm = '';
	
	create table beta (ClusterAlgorithm, DistanceFunction, UsedFields, Tfidf, StopWords, Interpreted, Lemmatized, Source, Synonyms, GermaNetFunction, F1WeightedAvg REAL, F1WeightedStd REAL, CohesionAvg REAL, CohesionStd REAL, SeperationAvg REAL, SeperationStd REAL, SilhouetteAvg REAL, SilhouetteStd REAL, RuntimeAvg REAL);
	.separator ";"
	.import "beta.csv" "beta"
	DELETE from beta WHERE SeperationAvg = 'SeperationAvg';
	DELETE from beta WHERE ClusterAlgorithm = '';
	
	create table gamma (ClusterAlgorithm, DistanceFunction, UsedFields, Tfidf, StopWords, Interpreted, Lemmatized, Source, Synonyms, GermaNetFunction, F1WeightedAvg REAL, F1WeightedStd REAL, CohesionAvg REAL, CohesionStd REAL, SeperationAvg REAL, SeperationStd REAL, SilhouetteAvg REAL, SilhouetteStd REAL, RuntimeAvg REAL);
	.separator ";"
	.import "gamma.csv" "gamma"
	DELETE from gamma WHERE SeperationAvg = 'SeperationAvg';
	DELETE from gamma WHERE ClusterAlgorithm = '';
	
	create table delta (ClusterAlgorithm, DistanceFunction, UsedFields, Tfidf, StopWords, Interpreted, Lemmatized, Source, Synonyms, GermaNetFunction, F1WeightedAvg REAL, F1WeightedStd REAL, CohesionAvg REAL, CohesionStd REAL, SeperationAvg REAL, SeperationStd REAL, SilhouetteAvg REAL, SilhouetteStd REAL, RuntimeAvg REAL);
	.separator ";"
	.import "delta.csv" "delta"
	DELETE from delta WHERE SeperationAvg = 'SeperationAvg';
	DELETE from delta WHERE ClusterAlgorithm = '';
		
	create table zeta (ClusterAlgorithm, DistanceFunction, UsedFields, Tfidf, StopWords, Interpreted, Lemmatized, Source, Synonyms, GermaNetFunction, F1WeightedAvg REAL, F1WeightedStd REAL, CohesionAvg REAL, CohesionStd REAL, SeperationAvg REAL, SeperationStd REAL, SilhouetteAvg REAL, SilhouetteStd REAL, RuntimeAvg REAL);
	.separator ";"
	.import "zeta.csv" "zeta"
	DELETE from zeta WHERE SeperationAvg = 'SeperationAvg';
	DELETE from zeta WHERE ClusterAlgorithm = '';
	
	EOF


## Verification

	select "alpha", count(*) from alpha
	UNION ALL 
	select "beta", count(*) from beta
	UNION ALL 
	select "delta", count(*) from delta
	UNION ALL 
	select "gamma", count(*) from gamma
	UNION ALL 
	select "zeta", count(*) from zeta

dataset | count
----|----
alpha|7728
beta|7728
gamma|7728
delta|7728
zeta|7728


## Best performances per dataset

	select "alpha", ROUND(max(F1WeightedAvg), 2), ROUND(avg(F1WeightedAvg), 2) from alpha
	UNION ALL 
	select "beta", ROUND(max(F1WeightedAvg), 2), ROUND(avg(F1WeightedAvg), 2) from beta
	UNION ALL 
	select "delta", ROUND(max(F1WeightedAvg), 2), ROUND(avg(F1WeightedAvg), 2) from delta
	UNION ALL 
	select "gamma", ROUND(max(F1WeightedAvg), 2), ROUND(avg(F1WeightedAvg), 2)  from gamma
	UNION ALL 
	select "zeta", ROUND(max(F1WeightedAvg), 2), ROUND(avg(F1WeightedAvg), 2) from zeta


dataset|max F1|avg F1
-----|-----|-----
alpha|0.37|0.19
beta|0.71|0.42
gamma|0.54|0.32
delta|0.66|0.37
zeta|0.54|0.3

## Single best parameter performances per dataset and algorithm

	select ClusterAlgorithm, DistanceFunction, UsedFields, Tfidf, StopWords, Interpreted, Lemmatized, Source, Synonyms, GermaNetFunction, ROUND(max(F1WeightedAvg), 2) BestF1 from alpha
	group by ClusterAlgorithm
	order by BestF1 desc
	
### Alpha

ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|ROUND(max(F1WeightedAvg)
----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----
ClusterART|Not needed|1|false|true|false|true|false|false|Shotgun|0.37
FuzzyCMeans2320|CosineDistance|7|true|false|false|true|false|false|null|0.36
KMeans2320|CosineDistance|7|true|false|false|true|false|true|null|0.35
EM|Not needed|7|false|true|true|false|false|false|null|0.34
Neural Gas|CosineDistance|4|true|true|true|true|false|false|null|0.32
HC|ManhattanDistance|1|true|false|true|true|false|true|OneAncestor|0.3


### Beta

ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|ROUND(max(F1WeightedAvg)
----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----
|ClusterART|Not needed|4|false|true|false|true|false|true|Shotgun|0.71|
|Neural Gas|CosineDistance|7|false|true|false|false|true|false|null|0.68|
|FuzzyCMeans1520|CanberraDistance|4|true|false|true|false|false|false|null|0.65|
|KMeans1520|EuclideanDistance|4|true|true|true|false|false|false|null|0.57|
|HC|EuclideanDistance|7|false|true|false|true|true|true|Shotgun|0.56|
|EM|Not needed|4|true|true|false|true|true|false|null|0.49|


### Gamma

ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|ROUND(max(F1WeightedAvg)
----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----
|ClusterART|Not needed|7|true|true|false|true|false|false|OneAncestor|0.54|
|HC|EuclideanDistance|4|false|false|false|true|true|true|null|0.52|
|FuzzyCMeans1920|ManhattanDistance|4|false|true|false|false|true|false|null|0.49|
|KMeans1920|ManhattanDistance|7|true|false|false|false|true|false|null|0.49|
|Neural Gas|EuclideanDistance|4|false|false|true|false|false|false|null|0.49|
|EM|Not needed|4|true|true|false|false|true|false|null|0.46|


### Delta

ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|ROUND(max(F1WeightedAvg)
----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----
|ClusterART|Not needed|7|true|false|false|true|true|true|Shotgun|0.66|
|Neural Gas|EuclideanDistance|7|false|true|false|false|true|false|null|0.54|
|HC|ManhattanDistance|7|false|false|false|true|true|false|OneAncestor|0.52|
|FuzzyCMeans1620|ManhattanDistance|7|false|true|false|true|true|true|null|0.5|
|KMeans1620|ManhattanDistance|4|false|true|false|false|true|false|null|0.49|
|EM|Not needed|4|true|true|false|true|true|false|Shotgun|0.48|


### Zeta

ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|ROUND(max(F1WeightedAvg)
----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----
|EM|Not needed|4|true|false|false|true|false|false|null|0.54|
|Neural Gas|CosineDistance|7|false|true|true|true|false|true|OneAncestor|0.51|
|FuzzyCMeans820|EuclideanDistance|4|true|false|false|false|false|false|null|0.49|
|ClusterART|Not needed|1|false|true|false|true|false|false|Shotgun|0.46|
|KMeans820|CosineDistance|4|true|true|true|true|false|false|null|0.45|
|HC|ManhattanDistance|1|false|false|true|true|false|false|OneAncestor|0.42|


## Yield of Requirement Templating Approaches

	select UsedFields, Interpreted, round(max(F1WeightedAvg), 2) from alpha
	group by UsedFields, Interpreted
	
### Alpha

UsedFields|Interpreted|BestF1
-----|------|------
|1|false|0.37|
|1|true|0.37|
|7|false|0.36|
|4|true|0.34|
|7|true|0.34|
|4|false|0.33|


### Beta

UsedFields|Interpreted|BestF1
-----|------|------
|4|false|0.71|
|7|false|0.68|
|7|true|0.67|
|4|true|0.65|
|1|false|0.64|
|1|true|0.56|


### Gamma

UsedFields|Interpreted|BestF1
-----|------|------
|7|false|0.54|
|4|false|0.52|
|4|true|0.5|
|7|true|0.5|
|1|false|0.48|
|1|true|0.47|


### Delta

UsedFields|Interpreted|BestF1
-----|------|------
|7|false|0.66|
|4|false|0.65|
|7|true|0.6|
|4|true|0.54|
|1|false|0.53|
|1|true|0.52|

### Zeta

UsedFields|Interpreted|BestF1
-----|------|------
|4|false|0.54|
|7|true|0.51|
|1|false|0.49|
|1|true|0.48|
|7|false|0.47|
|4|true|0.45|


## Impact of Requirement Text Templating Schemes

	select ClusterAlgorithm, DistanceFunction, UsedFields, Tfidf, StopWords, Interpreted, Lemmatized, Source, Synonyms, GermaNetFunction, ROUND(max(F1WeightedAvg), 2) BestF1 from alpha
	group by UsedFields, Interpreted
	order by BestF1 desc

### Alpha

ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|ROUND(max(F1WeightedAvg)
----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----
|ClusterART|Not needed|1|false|true|false|true|false|false|Shotgun|0.37|
|ClusterART|Not needed|1|false|true|true|true|false|false|Shotgun|0.37|
|FuzzyCMeans2320|CosineDistance|7|true|false|false|true|false|false|null|0.36|
|KMeans2320|ManhattanDistance|4|false|true|true|true|true|true|OneAncestor|0.34|
|EM|Not needed|7|false|true|true|false|false|false|null|0.34|
|FuzzyCMeans2320|EuclideanDistance|4|true|true|false|true|false|true|OneAncestor|0.33|


### Beta

ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|ROUND(max(F1WeightedAvg)
----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----
|ClusterART|Not needed|4|false|true|false|true|false|true|Shotgun|0.71|
|Neural Gas|CosineDistance|7|false|true|false|false|true|false|null|0.68|
|ClusterART|Not needed|7|false|true|true|true|false|false|null|0.67|
|FuzzyCMeans1520|CanberraDistance|4|true|false|true|false|false|false|null|0.65|
|ClusterART|Not needed|1|false|true|false|true|true|false|null|0.64|
|Neural Gas|ManhattanDistance|1|false|false|true|true|true|true|OneAncestor|0.56|


### Gamma

ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|ROUND(max(F1WeightedAvg)
----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----
|ClusterART|Not needed|7|true|true|false|true|false|false|OneAncestor|0.54|
|ClusterART|Not needed|4|false|true|false|true|false|false|OneAncestor|0.52|
|ClusterART|Not needed|4|false|true|true|false|false|false|null|0.5|
|ClusterART|Not needed|7|true|true|true|true|false|true|OneAncestor|0.5|
|HC|EuclideanDistance|1|false|false|false|true|true|false|OneAncestor|0.48|
|ClusterART|Not needed|1|false|false|true|true|false|true|Shotgun|0.47|


### Delta

ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|ROUND(max(F1WeightedAvg)
----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----
|ClusterART|Not needed|7|true|false|false|true|true|true|Shotgun|0.66|
|ClusterART|Not needed|4|true|false|false|true|true|true|Shotgun|0.65|
|ClusterART|Not needed|7|false|true|true|true|false|false|null|0.6|
|ClusterART|Not needed|4|false|true|true|false|false|false|null|0.54|
|ClusterART|Not needed|1|false|false|false|true|true|true|null|0.53|
|ClusterART|Not needed|1|false|false|true|true|false|false|OneAncestor|0.52|


### Zeta

ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|ROUND(max(F1WeightedAvg)
----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----
|EM|Not needed|4|true|false|false|true|false|false|null|0.54|
|Neural Gas|CosineDistance|7|false|true|true|true|false|true|OneAncestor|0.51|
|FuzzyCMeans820|EuclideanDistance|1|true|false|false|true|false|false|OneAncestor|0.49|
|Neural Gas|CosineDistance|1|false|true|true|true|false|false|null|0.48|
|FuzzyCMeans820|EuclideanDistance|7|true|true|false|true|false|false|null|0.47|
|FuzzyCMeans820|CanberraDistance|4|true|false|true|true|false|false|null|0.45|



## Average F1 Performancs for isolated features

	select tfidf as "", round(avg(F1WeightedAvg), 2) TFIDFF1, StopWords.StopWordsF1, Interpreted.InterpretedF1, Lemmatized.LemmatizedF1, Source.SourceF1, Synonyms.SynonymsF1 from alpha
	left outer join (
	select Lemmatized, round(avg(F1WeightedAvg), 2) LemmatizedF1 from alpha
	group by Lemmatized
	) Lemmatized
	on Lemmatized.Lemmatized = tfidf
	left outer join (
	select StopWords, round(avg(F1WeightedAvg), 2) StopWordsF1 from alpha
	group by StopWords
	) StopWords
	on StopWords.StopWords = tfidf
	left outer join (
	select Interpreted, round(avg(F1WeightedAvg), 2) InterpretedF1 from alpha
	group by Interpreted
	) Interpreted
	on Interpreted.Interpreted = tfidf
	left outer join (
	select Source, round(avg(F1WeightedAvg), 2) SourceF1 from alpha
	group by Source
	) Source
	on Source.Source = tfidf
	left outer join (
	select Synonyms, round(avg(F1WeightedAvg), 2) SynonymsF1 from alpha
	group by Synonyms
	) Synonyms
	on Synonyms.Synonyms = tfidf
	group by tfidf

### alpha

|TFIDF|Stopwords|Interpreted|Lemmatized|Source|Synonyms|
-----|-----|-----|-----|-----|-----|
false|0.19|0.19|0.19|0.19|0.2|0.19
true|0.19|0.19|0.19|0.19|0.19|0.19





## Combination counts in the best 1%

	select ClusterAlgorithm, DistanceFunction, UsedFields, Tfidf, StopWords, Interpreted, Lemmatized, Source, Synonyms, GermaNetFunction, count(*) counts from (
	select * from alpha
	order by F1WeightedAvg desc
	limit 77) onepercent
	group by ClusterAlgorithm, DistanceFunction
	order by counts desc

	
### Alpha

ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|Count
------|------|------|------|------|------|------|------|------|------|------|
FuzzyCMeans2320|CosineDistance|7|false|false|true|true|true|true|OneAncestor|18
FuzzyCMeans2320|CanberraDistance|7|true|true|true|true|true|true|null|14
KMeans2320|ManhattanDistance|7|false|true|true|true|true|false|null|14
ClusterART|Not needed|1|true|false|true|true|true|false|Shotgun|12
FuzzyCMeans2320|EuclideanDistance|4|true|true|false|true|false|false|null|8
KMeans2320|CosineDistance|7|true|false|false|true|true|true|null|8
EM|Not needed|4|false|false|false|true|false|false|null|3


### Beta

ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|Count
------|------|------|------|------|------|------|------|------|------|------|
FuzzyCMeans1520|EuclideanDistance|7|true|false|false|true|true|true|OneAncestor|19
FuzzyCMeans1520|ChebyshevDistance|4|true|true|true|false|true|false|null|12
ClusterART|Not needed|7|false|true|true|true|true|false|OneAncestor|10
FuzzyCMeans1520|CanberraDistance|4|true|true|true|false|false|false|null|8
FuzzyCMeans1520|ManhattanDistance|4|false|true|true|false|true|false|null|8
Neural Gas|CosineDistance|7|false|false|false|true|true|true|OneAncestor|8
Neural Gas|CanberraDistance|7|true|false|false|false|true|false|null|6
FuzzyCMeans1520|CosineDistance|4|true|true|true|false|true|false|null|4
Neural Gas|EuclideanDistance|4|false|false|false|true|true|true|OneAncestor|2


### Gamma
ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|Count
------|------|------|------|------|------|------|------|------|------|------|
ClusterART|Not needed|7|true|false|false|true|true|false|null|34
HC|EuclideanDistance|4|true|false|false|true|true|false|null|26
HC|ManhattanDistance|4|true|false|false|false|true|false|null|16
FuzzyCMeans1920|ManhattanDistance|4|false|true|false|false|true|false|null|1


### Delta
ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|Count
------|------|------|------|------|------|------|------|------|------|------|
ClusterART|Not needed|4|true|true|false|false|true|false|null|77


### Zeta
ClusterAlgorithm|DistanceFunction|UsedFields|Tfidf|StopWords|Interpreted|Lemmatized|Source|Synonyms|GermaNetFunction|Count
------|------|------|------|------|------|------|------|------|------|------|
FuzzyCMeans820|CanberraDistance|4|false|false|false|true|false|true|null|27
FuzzyCMeans820|EuclideanDistance|4|true|true|false|true|false|true|OneAncestor|17
Neural Gas|CosineDistance|1|false|true|false|true|false|false|OneAncestor|16
Neural Gas|CanberraDistance|1|false|true|true|false|false|false|null|13
EM|Not needed|4|true|false|false|true|false|true|null|3
Neural Gas|ChebyshevDistance|1|true|false|false|true|true|true|Shotgun|1