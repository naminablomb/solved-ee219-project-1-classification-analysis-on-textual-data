Download Link: https://assignmentchef.com/product/solved-ee219-project-1-classification-analysis-on-textual-data
<br>
Statistical classification refers to the task of identifying a category, from a predefined set, to which adata point belongs, given a training data set with known category memberships. Classification differsfrom the task of clustering, which concerns grouping data points with no predefined categorymemberships, where the objective is to seek inherent structures in data with respect to suitablemeasures. Classification turns out as an essential element of data analysis, especially when dealing witha large amount of data. In this project, we look into different methods for classifying textual data.Dataset and Problem Statement:In this project, we work with “20 Newsgroups” dataset. It is a collection of approximately 20,000newsgroup documents, partitioned (nearly) evenly across 20 different newsgroups, each correspondingto a different topic.We highly recommend using python as your programming language. The easiest way to load the data isto use the built-in dataset loader for 20 newsgroups from scikit-learn package. As an example, if youwant to load only “comp.graphics” category, then you can use the following command:For the purposes of this project we will be working with only 8 of the classes as shown in Table 1. Loadthe training and testing data for the following 8 subclasses of two major classes ‘Computer Technology’and ‘Recreational activity’.Table 1. Subclasses of ‘Computer technology’ and ‘Recreational activity’Computer technology Recreational activitycomp.graphicscomp.os.ms-windows.misccomp.sys.ibm.pc.hardwarecomp.sys.mac.hardwarerec.autosrec.motorcyclesrec.sport.baseballrec.sport.hockeya) In a classification problem one should make sure to properly handle any imbalance in the relativesizes of the data sets corresponding to different classes. To do so, one can either modify the penaltyfunction (i.e. assign more weight to errors from minority classes), or alternatively, down-sample themajority classes, to have the same number of instances as minority classes. To get started, perform thefollowing to the data of the 8 classes above: plot a histogram of the number of training documents perclass to check if they are evenly distributed. Note that the data set is already balanced and so in this casewe do not need to balance. But in general, as a data scientist you need to be aware of this issue.Modeling Text Data and Feature Extraction:The primary step in classifying a corpus of text is choosing a proper document representation. Thisrepresentation should not include too much irrelevant information, which may lead to computationalintractability and over fitting. One common representation of documents is called “Bag of Words”,where a document is represented as a histogram of term frequencies, or other statistics of the terms,within a fixed vocabulary. As such, a corpus of text can be summarized into a term-document matrixwhose entries are some statistic of the terms.First a common sense filtering is done to drop certain words or terms: To avoid unnecessarily largefeature vectors (vocabulary size), terms that are too frequent in almost every document, or are veryrare, are dropped out of the vocabulary. The same goes with special characters, common stop words (e.g. and, the etc.), In addition, appearances of words that share the same stem in the vocabulary (e. g.goes vs going) are merged into a single term.Next, one can consider using the normalized count of the vocabulary words in each document to buildrepresentation vectors. A popular numerical statistic to capture the importance of a word to adocument in a corpus is the Term Frequency-Inverse Document Frequency (TFxIDF) metric. Thismeasure takes into account count of the words in the document, as normalized by a certain function ofthe frequency of the individual words in the whole corpus. For example, if a corpus is about computeraccessories then words such as “computer” “software” “purchase” will be present in almost everydocument and their frequency is not a distinguishing feature for any document in the corpus. Thediscriminating words will most likely be those that are specialized terms describing different types ofaccessories and hence will occur in fewer documents. Thus, a human reading a particular document willusually ignore the contextually dominant words such as “computer”, “software” etc. and give moreimportance to specific words. This is like when going into a very bright room or looking at a brightobject, the human perception system usually applies a saturating function (such as a logarithm orsquare-root) to the actual input values before passing it on to the neurons. This makes sure that acontextually dominant signal does not overwhelm the decision-making processes in the brain. TheTFxIDF functions draw their inspiration from such neuronal systems.b) Perform the following on the documents of balanced data of 8 classes, to convert them intonumerical feature vectors. First tokenize each document into words. Then, excluding the stop words,punctuations, and using stemmed version of words, create a TFxIDF vector representations. TFxIDFvector representation of a document is defined as follows:������(�, �) = ��(�, �) ∗ ���(�)where ��(�, �) represents the frequency of term � in document d, and inverse document frequency isdefined as:���(�) = log [ � / ��(�)] + 1where � is the total number of documents, and ��(�) is the document frequency; the documentfrequency is the number of documents that contain the term �.Through part i, use two settings for minimum document frequency of vocabulary terms, namelymin_df=2 and min_df=5. Report the final number of terms you extracted.As for a list of stop words, you can refer to the list provided in scikit-learn package. You can run thefollowing commands to take a look at this list.c) In order to quantify how significant a word is to a class, we can define a measure called TFxICF. It isthe same as TFxIDF, except that a class sits in place of a document; that is for a term t and a class c, themeasure is computed as������(�, �) = ��(�, �) ∗ ���(�)where ��(�, �) represents the term frequency in a class � , and inverse class frequency is defined as:���(�) = log [ �&lt;=&gt;<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="96a9a9d6">[email protected]</a>? / ��(�)] + 1and �&lt;=&gt;<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2c13136c">[email protected]</a>? is the total number of classes, and ��(�) is the class frequency which is the number ofclasses within which there is at least a document containing the term �.Find the 10 most significant terms in each of the following classes with respect to TFxICF measure. Notethat in this part of your assignment, �&lt;=&gt;<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="cbf4f48b">[email protected]</a>? is 20. (You can use the unbalanced dataset of all 20 classesfor this part)comp.sys.ibm.pc.hardware, comp.sys.mac.hardware, misc.forsale, soc.religion.christian.Feature Selection:After these operations, the dimensionality of the representation vectors (TFxIDF vectors) ranges in theorder of thousands. However, the document-term tfidf matrix is sparse and low-rank. Besides, learningalgorithms may perform poorly in high-dimensional data, which is sometimes referred to as “The Curseof Dimensionality”. As a remedy, one can select a subset of the original features, which are morerelevant with respect to certain performance measure, or transform the features into a lowerdimensional space.In this project, we use Latent Semantic Indexing (LSI), a dimensionality reduction transform method,which minimizes mean squared residual between the original data and reconstruction from its low&#x2;dimensional approximation. Recall that our data is the term-document tfidf matrix, whose rowscorrespond to TFxIDF representation of the documents.The LSI representation is obtained by computing left and right singular vectors corresponding to thelargest values of the term-document tfidf matrix. LSI is similar to Principal Component Analysis (PCA),and see the lecture notes for their relationships.Eigenvectors corresponding to the dominant eigenvalues obtained by LSI correspond to most prevailingcombinations of terms in the corpus, which can be thought of as “topics” or “semantic concepts”. Thus,the new low dimensional representation is the magnitudes of the projections of the document vectorsonto these latent “topics”.Let � denote the � × � term-document matrix with rank � where each of the � columns represents adocument vector of dimension �. The singular value decomposition results in� = ���F,Where � is an � × � diagonal matrix of singular values of �, � is a � × � matrix of left singular columnvectors, and � is a � × � matrix of right singular vectors. Dropping all but � largest singular values andcorresponding singular vectors gives the following truncated approximation�H = �H�H�HF.The approximation is the best rank k approximation of D in the sense of minimizing the sum of squareddifferences between the entries of � and �H.The � × � matrix �H can now be used as a projection matrix to map each � × 1 document column vector� into a �-dimensional representation�H = �HF�.d) Apply LSI to the TFxIDF matrix corresponding to the 8 classes. and pick k=50; so each document ismapped to a 50-dimensional vector. Alternatively, reduce dimensionality through Non-Negative MatrixFactorization (NMF) and compare the results of the parts e-i using both methods.Learning AlgorithmsFor the following parts (e-h) of the project, your task would be to classify the documents into twocategories ‘‘Computer Technology’ vs ‘Recreational Activity’. Refer to Table 1 to find 4 subclassescomprising each of the two classes. In other words, you need to combine documents of sub-classes ofeach class to form the set of documents for each class.Classification accuracy can be evaluated using different measures called precision, recall, F-score, etc.Precision is the proportion of items placed in the category that are really in the category, and recall isthe proportion of items in the category that are actually placed in the category.Depending on application, the true positive rate (TPR) and the false positive rate (FPR) have differentlevels of significance. In order to characterize the trade-off between the two quantities we plot thereceiver operating characteristic (ROC) curve. For binary classification, the curve is created by plottingthe true positive rate against the false positive rate at various threshold settings on the probabilitiesassigned to each class (let us assume probability � for class 0 and 1 − � for class 1). In particular, athreshold � is applied to value of � to select between the two classes. The value of threshold � is sweptfrom 0 to 1 to produce a pair of precision and recall for each value of �.Linear Support Vector Machines have been proved efficient when dealing with sparse high dimensionaldatasets, including textual data. They have been shown to have good generalization accuracy, whilehaving low computational complexity.Linear Support Vector Machines aim to learn a vector of feature weights, �, and an intercept, �, giventhe training data set. Once the weights are learned, the label of a data point is determined bythresholding �F. � + � with 0, i.e. ����(�F. � + �). Alternatively, one produce probabilities that thedata point belongs to either class, by applying a logistic function instead of hard thresholding, i.e.calculating �(�F. � + �).The learning process involves solving the following optimization problem:min12 ∥ � ∥WW + � �Z[Z]s.t. y_ wa. x_ + b ≥ 1 − ξ_, and ξ_ ≥ 0, for all i ∈ {1, … , n}.Minimizing the sum of the slack variables corresponds to minimizing the loss function on the trainingdata. On the other hand, minimizing the first term, which is basically a regularization, corresponds tomaximizing the margin between the two classes. Note that in the objective function, each slack variablerepresents the amount of error that the classifier can tolerate for a given data sample. The tradeoffparameter � controls relative importance of the two components of the objective function. For instance,when � ≫ 1, misclassification of individual points is highly penalized, which is called “Hard MarginSVM”. In contrast, a “Soft Margin SVM”, which is the case when � ≪ 1, is very lenient towardsmisclassification of a few individual points as long as most data points are well separated.e) Use hard margin SVM classifier (SVC) by setting � to a high value, e.g. 1000, to separate thedocuments into ‘Computer Technology’ vs ‘Recreational Activity’ groups. In your submission, plot theROC curve, report the confusion matrix and calculate the accuracy, recall and precision of yourclassifier. Repeat the same procedure for soft margin SVC (set � to 0.001)f) Using a 5-fold cross-validation, find the best value of the parameter � in the range 10H − 3 ≤ � ≤3, � ∈ �}. Report the confusion matrix and calculate the accuracy, recall and precision of the classifier.g) Next, we use naïve Bayes algorithm for the same classification task. The algorithm estimates themaximum likelihood probability of a class given a document with feature set x, using Bayes rule, basedupon the assumption that given the class, the features are statistically independent.Train a multinomial naïve Bayes classifier and plot the ROC curve for different values of the threshold onclass probabilities. You should report your ROC curve along with that of the other algorithms. Again,Report the confusion matrix and calculate the accuracy, recall and precision of your classifier.h) Repeat the same task with the logistic regression classifier, and plot the ROC curve for differentvalues of the threshold on class probabilities. You should report your ROC curve along with that of theother algorithms. Provide the same evaluation measures on this classifier.i) Now, repeat part (h) by adding a regularization term to the optimization objective. Try both �] and �Wnorm regularizations and sweep through different regularization coefficients, ranging from very smallones to large ones. How does the regularization parameter affect the test error? How are thecoefficients of the fitted hyperplane affected? Why might one be interested in each type ofregularization?Multiclass Classification:So far, we have been dealing with classifying the data points into two classes. In this part, we exploremulticlass classification techniques through different algorithms.Some classifiers perform the multiclass classification inherently. As such, Naïve Bayes algorithm finds theclass with maximum likelihood given the data, regardless of the number of classes. In fact, theprobability of each class label is computed in the usual way, then the class with the highest probability ispicked; that is� = arg min&lt;∈s �(�|�).where � denotes a class to be chosen, and � denotes the optimal class.For SVM, however, one needs to extend the binary classification techniques when there are multipleclasses. A natural way to do so is to perform a one versus one classification on all |C|2 pairs of classes,and given a document the class is assigned with the majority vote. In case there is more than one classwith the highest vote, the class with the highest total classification confidence levels in the binaryclassifiers is picked.An alternative strategy would be to fit one classifier per class, which reduces the number of classifiers tobe learnt to |�|. For each classifier, the class is fitted against all the other classes. Note that in this case,the unbalanced number of documents in each class should be handled. By learning a single classifier foreach class, one can get insights on the interpretation of the classes based on the features.i) In this part, we aim to learn classifiers on the documents belonging to the classes mentioned in part b;namelycomp.sys.ibm.pc.hardware, comp.sys.mac.hardware, misc.forsale,soc.religion.christian.Perform Naïve Bayes classification and multiclass SVM classification (with both One VS One and One VSthe rest methods described above) and report the confusion matrix and calculate the accuracy, recalland precision of your classifiers