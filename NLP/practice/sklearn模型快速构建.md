### sklearn快速构建模型 {#sklearn快速构建模型}

#### 查验数据 {#查验数据}

check\_data：csv表

```
import pandas as pd
train = pd.read_csv("../data/train.csv")

test = pd.read_csv("../data/test.csv")
train.info()
test.info()
```

#### 数据预处理（以Tranic为例） {#数据预处理以tranic为例}

```
y_train = train['Survived']
```

如果只有train，对数据进行分隔

```
X_train, X_test, y_train, y_test = train_test_split(
    train_X, train_y, test_size=0.3)
```

##### 特征选择 {#特征选择}

人工选择特征（经验）

[特征选择方法总结](http://blog.csdn.net/lihaitao000/article/details/51213563?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

```
selected_features = ['Pclass', 'Sex', 'Age', 'Embarked', 'SlibSp', 'Parch', 'Fare']
x_train = train[selected_features]
x_test = test[selected_features]

x_columns = [x for in train.columns if x not in ['row.names', 'name', 'Survived']
x_train = train[x_columns]

x_train.drop(['row.names', 'name', 'Survived'], axis=1)
```

决策树选择特征

```py
import numpy as np
from sklearn import feature_selection

# 最佳性能特征筛选
for i in range(1, 100, 2):
   fs = feature_selection.SelectPrentile(feature_selection.chi2, percentile=i)
   x_train_fs = fs.fit_transform(x_train, y_train)
   scores = cross_val_score(model, x_train_fs, y_train, cv=5)
   results = np.append(results, scores.mean()

opt = np.where(results == results.max())[0]
```

```
from sklearn import metrics
from sklearn.ensemble import ExtraTreesClassifier
model = ExtraTreesClassifier()
model.fit(X, y)

# display the relative importance of each attribute
print (model.feature_importances_)
```

[scikit-learn系列之特征选择](http://www.jianshu.com/p/8f6f94f1d275)

##### 缺失数据补全 {#缺失数据补全}

通过查验数据可以看出哪些数据不全

```
# 类别型补充最多的
print x_train['Embarked'].value_counts()

x_train['Embarked'].fillna('S', inplace=True)
x_test['Embarked'].fillna('S', inplace=True)
x_train['Age'].fillna(x_train['Age'].mean(), inplace=True)
x_test['Age'].fillna(x_test['Age'].mean(), inplace=True)
x_train['Fare'].fillna(x_train['Age'], inplace=True)
# 避免噪音
x_train = x_train.dropna(how='any')

x_train.fillna('UNKNOWN', inplace=True)
```

##### 特征向量化 {#特征向量化}

```
from sklearn.feature_extract import DictVectorizer

## 普通特征
dict_vec = DictVectorizer(sparse=False)
x_train = dict_vec.fit_transform(x_train.to_dict(orient='record')
print dict_vec.feature_names_
x_test = dict_vec.fit_transform(x_test.to_dict(orient='record')


## 文本特征向量化
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer

# vec = CountVectorizer()
# vec = TfidfVectorizer()
# 去掉停用词
vec = CountVectorizer(analyzer='word', stop_words='english')
x_train = vec.fit_transform(x_train)
x_test = vec.transform(x_test)

print vec.get_feature_names()
```

##### 特征向量标准化 {#特征向量标准化}

```
from sklearn.preprocessing import StandardScaler
ss = StandardScaler()
x_train = ss.fit_transform(x_train)
x_test = ss.transform(x_test)


from sklearn import preprocessing

# normalize the data attributes
normalized_X = preprocessing.normalize(X)
# standardize the data attributes
standardized_X = preprocessing.scale(X)
```

#### 模型选择 {#模型选择}

GBDT &gt; DL &gt; RF &gt; LR &gt;SVM

模型调用

```
## GBDT
from sklearn.ensemble import GradientBoostingClassifier, GradientBoostingRegressor
# 分类
gbc = GradientBoostingClassfier(random_state=10)
# 回归
gbr = GradientBoostingRegressor()

## RF
from sklearn.ensemble import RandomForestClassifier, RandomForestRegressor, ExtraTreesRegressor
# 分类
rfc = RandomForestClassifier()
# 回归
etr = ExtraTreesRegressor()
rfr = RandomForestRegressor()

## LR
from sklearn.linear_model import LogisticRegression, Ridge, Lasso
# 分类
lr = LogisticRegression()
# 特征非线性变换
from sklearn.preprocessing import PolynamialFeatures
poly2 = PolynamialFeatures(degree=2)
x_train_poly2.fit_transform(x_train)
lasso = Lasso()
lrrd = Ridge()
from sklearn.linear_model import SGDClassifier
sgd = SGDClassifier()

## SVM
from sklearn.svm import LinearSVC, SVC, SVR
# 分类
lsvc = LinearSVC()
svc = SVC()
parms_svc = {'svc_gamma': np.logspace(-2, 1, 4), 'svc_c': np.logspace(-1, 1, 3)}
# 回归
svr = SVR()
parmas_svr = {'kernel': ['linear', 'poly', 'rbf']}

## 贝叶斯
from sklearn.naive_bayes import MultinomialNB, GaussianNB
mnb = MultinomialNB()
gnb = GaussianNB()

## k近邻
from sklearn.neighbars import KNeighborsClassifier, KNeighborsRegressor
knn = KNeighborsClassifier()
# 回归
uni_knr = KNeighborsRegressor(weights="uniform")

## 
决策树
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor
dtc = DecisionTreeClassifier()
dtr = DecisionTreeRegressor()

## 聚类
from sklearn.cluster import KMeans
kmeans = KMeans(n_cluster=10)
from sklearn.decomposition import PCA
# 高维压缩到2维
estimator = PCA(n_component=2)
x_pca = estimator.fit_transform(x_digists)
```

模型选择

```
from sklearn import metrics
from sklearn.cross_validation import cross_val_score
from sklearn.metrics import classification_report, r2_score, mean_squared_error, mean_absolute_error
from sklearn.cross_validation import train_test_split

# 聚类
from sklearn.metrics import silhouette_score
from scipy.spatial.distance import cdist

def model_quality(model, X_train, Y_train=None):
    # 数据分割
    # x_train, x_test, y_train, y_test = train_test_split(X, Y, test_size=0.25, random_state=33)
    # model.fit(x_train, y_train)
    # model_y_predict = model.predict(x_test)
    # model.score(X_test, y_test)
    # metrics.accuracy_score(y, y_pred)
    # 详细分类性能
    # model_y_predict = model.predict(x_test)
    # classification_report(y_test, model_y_predict)
    # classification_report(y_test, model_y_predict, target_names=[])
    # confusion_matrix()
    # 回归性能评估
    # r2_score(y_test, model_y_predict)
    # mean_squared_error(ss_y.inverse_transform(y_test), ss_y.inverse_transform(model_y_predict))
    # mean_absolute_error(y_test, model_y_predict)
    # 特征贡献度
    # np.sort(zip(model.feature_importances_, boston.feature_names), axis=0)
    # 聚类性能评估
    # model.fit(X_train)
    # y_pred = model.predict(X_test)
    # metrics.adjusted_rand_score(y_test, y_pred)
    # 轮廓系数
    # silhouette_score(X, model.labels_, metric='euclidean')
    # 肘部观察法预估聚类簇个数
    #for k in range(1, 10):
    #    kmeans = Kmeans(n_clusters=k)
    #    kmeans.fit(X)
    #    sum(np.min(cdist(X, keans.cluster_centers_, 'euclidean'), axis=1))/X.shape[0]
    return cross_val_score(model, X_train, Y_train, cv=5, scoring='accuracy').mean()
```

#### 搜索最优参数 {#搜索最优参数}

```
params_gbc = {
    "n_estimators": range(50, 1000, 50), # 默认100
    "learning_rate": [0.05, 0.1, 0.25, 0.5, 1.0], #默认0.1
    "max_features": range(7, 20, 2), #
    "subsample": [0.5, 0.6, 0.65, 0.7, 0.75, 0.8, 0.9], # 默认1，合适0.5~0.8
    "max_depth": range(10, 100, 5),
    "min_samples_split": range(100, 1900, 200),
    "min_samples_leaf": range(60, 101, 10),
    "alpha": [0.7, 0.8, 0.9], # 默认0.9
}

from sklearn.grid_search import GridSearchCV
from sklearn.pipeline import Pipeline
def select_params(model, params, x_train, y_train):
    # pipeline 简化系统搭建流程
    # clf = Pipeline([('vect', TfidfVectorizer(stop_words='english', analyzer='word')), ('svc', SVC())])
    # gs = GridSearchCV(clf, params, verbose=2, refit=True, cv=3)
    # n_jobs = -1全部cpu多线程
    # scoring='mean_squared_error'回归
    gs = GridSearchCV(model, params, scoring='roc_auc', n_jobs=-1, cv=5, verbose=2, refit=True)
    gs.fit(x_train, y_train)
    print gs.cv_results_
    gs.grid_scores_, gs.best_params_, gs.best_score_
    return gs
```

```
from sklearn.learning_curve import validation_curve

#建立参数测试集
param_range = np.logspace(-6, -2.3, 5)

#使用validation_curve快速找出参数对模型的影响
train_loss, test_loss = validation_curve(
    SVC(), X, y, param_name='gamma', param_range=param_range, cv=10, scoring='mean_squared_error')
```

有时随机从给定区间中选择参数是很有效的方法，然后根据这些参数来评估算法的效果进而选择最佳的那个。

```
import numpy as np
from scipy.stats import uniform as sp_rand
from sklearn.linear_model import Ridge
from sklearn.grid_search import RandomizedSearchCV
# prepare a uniform distribution to sample for the alpha parameter
param_grid = {'alpha': sp_rand()}
# create and fit a ridge regression model, testing random alpha values
model = Ridge()
rsearch = RandomizedSearchCV(estimator=model, param_distributions=param_grid, n_iter=100)
rsearch.fit(X, y)
print(rsearch)
# summarize the results of the random parameter search
print(rsearch.best_score_)
print (rsearch.best_estimator_.alpha)
```

#### 模型保存 {#模型保存}

```
import pickle

#保存Model(注:save文件夹要预先建立，否则会报错)
with open('save/clf.pickle', 'wb') as f:
    pickle.dump(clf, f)

#读取Model
with open('save/clf.pickle', 'rb') as f:
    clf2 = pickle.load(f)
    #测试读取后的Model
    print(clf2.predict(X[0:1]))
```

```
from sklearn.externals import joblib

#保存Model(注:save文件夹要预先建立，否则会报错)
joblib.dump(clf, 'save/clf.pkl')

#读取Model
clf3 = joblib.load('save/clf.pkl')

#测试读取后的Model
print(clf3.predict(X[0:1]))

# [0]
```

joblib在使用上比较容易，读取速度也相对pickle快。

#### 输出 {#输出}

```
best_y_predict = gs.predict(x_test)
best_submission = pd.DataFrame({'PassengerID': test['PassangerID'], 'Survived': best_y_predict})
best_submission.to_csv("../result/best_submission.csv", index=False)
```

[从零开始sklearn搭建步骤git](https://github.com/Suluo/Kaggle/tree/master/standard/sklearn)

[scikit-learn的主要模块和基本使用](http://blog.csdn.net/u013066730/article/details/54314136)

