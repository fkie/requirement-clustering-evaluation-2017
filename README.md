# req-clustering-evaluation-2017

## Import from csv

sqlite3 alpha.db << EOF
create table alpha (ClusterAlgorithm, DistanceFunction, UsedFields, Tfidf, StopWords, Interpreted, Lemmatized, Source, Synonyms, GermaNetFunction, F1WeightedAvg REAL, F1WeightedStd REAL, CohesionAvg REAL, CohesionStd REAL, SeperationAvg REAL, SeperationStd REAL, SilhouetteAvg REAL, SilhouetteStd REAL, RuntimeAvg REAL);
.separator ";"
.import "alpha.csv" "alpha"
DELETE from alpha WHERE SeperationAvg = 'SeperationAvg';
DELETE from alpha WHERE ClusterAlgorithm = '';
EOF
