

```python
import  warnings
warnings.simplefilter('ignore')

import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import matplotlib
%matplotlib inline
from scipy.stats import ttest_ind
matplotlib.style.use('ggplot')

%matplotlib inline
```


```python
test = pd.read_csv('dataset/Translation_Test/test_table.csv', parse_dates=['date'])
test.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>user_id</th>
      <th>date</th>
      <th>source</th>
      <th>device</th>
      <th>browser_language</th>
      <th>ads_channel</th>
      <th>browser</th>
      <th>conversion</th>
      <th>test</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>315281</td>
      <td>2015-12-03</td>
      <td>Direct</td>
      <td>Web</td>
      <td>ES</td>
      <td>NaN</td>
      <td>IE</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>497851</td>
      <td>2015-12-04</td>
      <td>Ads</td>
      <td>Web</td>
      <td>ES</td>
      <td>Google</td>
      <td>IE</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>848402</td>
      <td>2015-12-04</td>
      <td>Ads</td>
      <td>Web</td>
      <td>ES</td>
      <td>Facebook</td>
      <td>Chrome</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>290051</td>
      <td>2015-12-03</td>
      <td>Ads</td>
      <td>Mobile</td>
      <td>Other</td>
      <td>Facebook</td>
      <td>Android_App</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>548435</td>
      <td>2015-11-30</td>
      <td>Ads</td>
      <td>Web</td>
      <td>ES</td>
      <td>Google</td>
      <td>FireFox</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
user = pd.read_csv('dataset/Translation_Test/user_table.csv')
user.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>user_id</th>
      <th>sex</th>
      <th>age</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>765821</td>
      <td>M</td>
      <td>20</td>
      <td>Mexico</td>
    </tr>
    <tr>
      <th>1</th>
      <td>343561</td>
      <td>F</td>
      <td>27</td>
      <td>Nicaragua</td>
    </tr>
    <tr>
      <th>2</th>
      <td>118744</td>
      <td>M</td>
      <td>23</td>
      <td>Colombia</td>
    </tr>
    <tr>
      <th>3</th>
      <td>987753</td>
      <td>F</td>
      <td>27</td>
      <td>Venezuela</td>
    </tr>
    <tr>
      <th>4</th>
      <td>554597</td>
      <td>F</td>
      <td>20</td>
      <td>Spain</td>
    </tr>
  </tbody>
</table>
</div>




```python
test.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 453321 entries, 0 to 453320
    Data columns (total 9 columns):
    user_id             453321 non-null int64
    date                453321 non-null datetime64[ns]
    source              453321 non-null object
    device              453321 non-null object
    browser_language    453321 non-null object
    ads_channel         181877 non-null object
    browser             453321 non-null object
    conversion          453321 non-null int64
    test                453321 non-null int64
    dtypes: datetime64[ns](1), int64(3), object(5)
    memory usage: 31.1+ MB



```python
user.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 452867 entries, 0 to 452866
    Data columns (total 4 columns):
    user_id    452867 non-null int64
    sex        452867 non-null object
    age        452867 non-null int64
    country    452867 non-null object
    dtypes: int64(2), object(2)
    memory usage: 13.8+ MB


It looks like there are some missing value in the user table.


```python
print("Test:\t",len(test['user_id'].unique()), len(test['user_id'].unique()) == len(test))
print("User:\t",len(user['user_id'].unique()), len(user['user_id'].unique()) == len(user))
```

    Test:	 453321 True
    User:	 452867 True



```python
data = pd.merge(left=test, right=user, how='left', on='user_id' )
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>user_id</th>
      <th>date</th>
      <th>source</th>
      <th>device</th>
      <th>browser_language</th>
      <th>ads_channel</th>
      <th>browser</th>
      <th>conversion</th>
      <th>test</th>
      <th>sex</th>
      <th>age</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>315281</td>
      <td>2015-12-03</td>
      <td>Direct</td>
      <td>Web</td>
      <td>ES</td>
      <td>NaN</td>
      <td>IE</td>
      <td>1</td>
      <td>0</td>
      <td>M</td>
      <td>32.0</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>1</th>
      <td>497851</td>
      <td>2015-12-04</td>
      <td>Ads</td>
      <td>Web</td>
      <td>ES</td>
      <td>Google</td>
      <td>IE</td>
      <td>0</td>
      <td>1</td>
      <td>M</td>
      <td>21.0</td>
      <td>Mexico</td>
    </tr>
    <tr>
      <th>2</th>
      <td>848402</td>
      <td>2015-12-04</td>
      <td>Ads</td>
      <td>Web</td>
      <td>ES</td>
      <td>Facebook</td>
      <td>Chrome</td>
      <td>0</td>
      <td>0</td>
      <td>M</td>
      <td>34.0</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>3</th>
      <td>290051</td>
      <td>2015-12-03</td>
      <td>Ads</td>
      <td>Mobile</td>
      <td>Other</td>
      <td>Facebook</td>
      <td>Android_App</td>
      <td>0</td>
      <td>1</td>
      <td>F</td>
      <td>22.0</td>
      <td>Mexico</td>
    </tr>
    <tr>
      <th>4</th>
      <td>548435</td>
      <td>2015-11-30</td>
      <td>Ads</td>
      <td>Web</td>
      <td>ES</td>
      <td>Google</td>
      <td>FireFox</td>
      <td>0</td>
      <td>1</td>
      <td>M</td>
      <td>19.0</td>
      <td>Mexico</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.columns
```




    Index(['user_id', 'date', 'source', 'device', 'browser_language',
           'ads_channel', 'browser', 'conversion', 'test', 'sex', 'age',
           'country'],
          dtype='object')



## 1. Confirm that the test is actually negative.

First, we need to prove that in the control group, spain performs better.


```python
groupby_country = data[data['test'] == 0][['conversion', 'country']].groupby('country').mean()
groupby_country = groupby_country.reset_index()
groupby_country = groupby_country.sort_values('conversion', ascending = False )

# Visualization
fig, ax = plt.subplots(figsize=(18, 6))
sns.barplot(x='country', y='conversion', data=groupby_country, ax=ax)
plt.show()
```


![png](Index_files/Index_10_0.png)



```python
# Visualization
fig, ax = plt.subplots(figsize=(18, 6))
sns.barplot(x='country', y='conversion', hue='test', data=data, ax=ax)
plt.show()
```


![png](Index_files/Index_11_0.png)


From the barplot above, it seems that the control groups perform worse. We need some statistal methods to prove it.


```python
# A/B test
test_data = data[data['country'] != 'Spain']
test_val = test_data[test_data['test'] == 1]['conversion'].values
cont_val = test_data[test_data['test'] == 0]['conversion'].values
```


```python
print(test_val.mean(), '\t', cont_val.mean())
```

    0.043424713982118966 	 0.04833042316066309



```python
# Welch Two Sample t-test
print(ttest_ind(test_val, cont_val, equal_var=False))
```

    Ttest_indResult(statistic=-7.3939374121344805, pvalue=1.4282994754055316e-13)


## 2. Explain why that might be happening. Are the localized translations really worse?


Likely, there is for some reason some segment of users more likely to end up in test or in control, this segment had a significantly above/below conversion rate and this affected the overall results.


```python
data = data[data['country'] != 'Spain']
```


```python
# Visualization of different dates
fig, ax = plt.subplots(nrows=1, ncols=2, figsize=(18, 6))
sns.countplot(x='date', hue='test', data=data, ax=ax[0])
ax[0].set_title('Count Plot of Date', fontsize=16)

sns.barplot(x='date', y='conversion', hue='test', data=data, ax=ax[1])
ax[1].set_title('Mean Conversion Rate per Date', fontsize=16)
plt.tight_layout()
plt.show()
```


![png](Index_files/Index_18_0.png)



```python
# Visualization of different source
fig, ax = plt.subplots(nrows=1, ncols=2, figsize=(18, 6))
sns.countplot(x='source', hue='test', data=data, ax=ax[0])
ax[0].set_title('Count Plot of Source', fontsize=16)

sns.barplot(x='source', y='conversion', hue='test', data=data, ax=ax[1])
ax[1].set_title('Mean Conversion Rate per Source', fontsize=16)
plt.tight_layout()
plt.show()
```


![png](Index_files/Index_19_0.png)



```python
# Visualization of different devices
fig, ax = plt.subplots(nrows=1, ncols=2, figsize=(18, 6))
sns.countplot(x='device', hue='test', data=data, ax=ax[0])
ax[0].set_title('Count Plot of Device', fontsize=16)

sns.barplot(x='device', y='conversion', hue='test', data=data, ax=ax[1])
ax[1].set_title('Mean Conversion Rate per Device', fontsize=16)
plt.tight_layout()
plt.show()
```


![png](Index_files/Index_20_0.png)



```python
# Visualization of different language
fig, ax = plt.subplots(nrows=1, ncols=2, figsize=(18, 6))
sns.countplot(x='browser_language', hue='test', data=data, ax=ax[0])
ax[0].set_title('Count Plot of Browser Language', fontsize=16)

sns.barplot(x='browser_language', y='conversion', hue='test', data=data, ax=ax[1])
ax[1].set_title('Mean Conversion Rate per Broswer Language', fontsize=16)
plt.tight_layout()
plt.show()
```


![png](Index_files/Index_21_0.png)



```python
# Visualization of different ad channel
fig, ax = plt.subplots(nrows=1, ncols=2, figsize=(18, 6))
sns.countplot(x='ads_channel', hue='test', data=data, ax=ax[0])
ax[0].set_title('Count Plot of Ads Channel', fontsize=16)

sns.barplot(x='ads_channel', y='conversion', hue='test', data=data, ax=ax[1])
ax[1].set_title('Mean Conversion Rate per Ads Channel', fontsize=16)
plt.tight_layout()
plt.show()
```


![png](Index_files/Index_22_0.png)



```python
# Visualization of different browsers
fig, ax = plt.subplots(nrows=1, ncols=2, figsize=(18, 6))
sns.countplot(x='browser', hue='test', data=data, ax=ax[0])
ax[0].set_title('Count Plot of Browser', fontsize=16)

sns.barplot(x='browser', y='conversion', hue='test', data=data, ax=ax[1])
ax[1].set_title('Mean Conversion Rate per Browser', fontsize=16)
plt.tight_layout()
plt.show()
```


![png](Index_files/Index_23_0.png)



```python
# Visualization of different gender
fig, ax = plt.subplots(nrows=1, ncols=2, figsize=(18, 6))
sns.countplot(x='sex', hue='test', data=data, ax=ax[0])
ax[0].set_title('Count Plot of Sex', fontsize=16)

sns.barplot(x='sex', y='conversion', hue='test', data=data, ax=ax[1])
ax[1].set_title('Mean Conversion Rate per Sex', fontsize=16)
plt.tight_layout()
plt.show()
```


![png](Index_files/Index_24_0.png)



```python
# Visualization of different age
fig, ax = plt.subplots(nrows=1, ncols=2, figsize=(18, 6))
sns.countplot(x='age', hue='test', data=data, ax=ax[0])
ax[0].set_title('Count Plot of Age', fontsize=16)

sns.barplot(x='age', y='conversion', hue='test', data=data, ax=ax[1])
ax[1].set_title('Mean Conversion Rate per Age', fontsize=16)
plt.tight_layout()
plt.show()

```


![png](Index_files/Index_25_0.png)



```python
# Visualization of different country
fig, ax = plt.subplots(nrows=1, ncols=2, figsize=(18, 6))
sns.countplot(x='country', hue='test', data=data, ax=ax[0])
ax[0].set_title('Count Plot of Country', fontsize=16)

sns.barplot(x='country', y='conversion', hue='test', data=data, ax=ax[1])
ax[1].set_title('Mean Conversion Rate per Country', fontsize=16)
plt.tight_layout()
plt.show()
```


![png](Index_files/Index_26_0.png)


### We can see that, except for country, all test groups perform worse. So take a closer look at country.


In an ideal world, the distribution of people in test and control for each segment should be the same. There are many ways to       this. One way is to build a decision tree where the variables are the user dimensions and the outcome variable is whether the user is in test or control. If the tree splits, it means that for given values of that variable you are more likely to end up in test or control.


```python
data = pd.merge(left=test, right=user, how='left', on='user_id' )
data = data[data['country'] != 'Spain']
```


```python
import h2o
from h2o.frame import H2OFrame
from h2o.estimators import H2OGradientBoostingEstimator
```


```python
# Initialize H2O cluster
h2o.init()
h2o.remove_all()

h2o_df = H2OFrame(data)
h2o_df['test'] = h2o_df['test'].asfactor()
h2o_df['ads_channel'] = h2o_df['ads_channel'].asfactor()
h2o_df.summary()
```

    Checking whether there is an H2O instance running at http://localhost:54321 . connected.



<div style="overflow:auto"><table style="width:50%"><tr><td>H2O cluster uptime:</td>
<td>27 days 0 hours 8 mins</td></tr>
<tr><td>H2O cluster timezone:</td>
<td>America/Los_Angeles</td></tr>
<tr><td>H2O data parsing timezone:</td>
<td>UTC</td></tr>
<tr><td>H2O cluster version:</td>
<td>3.24.0.3</td></tr>
<tr><td>H2O cluster version age:</td>
<td>1 month and 24 days </td></tr>
<tr><td>H2O cluster name:</td>
<td>H2O_from_python_luyao_mfrbb4</td></tr>
<tr><td>H2O cluster total nodes:</td>
<td>1</td></tr>
<tr><td>H2O cluster free memory:</td>
<td>1.608 Gb</td></tr>
<tr><td>H2O cluster total cores:</td>
<td>4</td></tr>
<tr><td>H2O cluster allowed cores:</td>
<td>4</td></tr>
<tr><td>H2O cluster status:</td>
<td>locked, healthy</td></tr>
<tr><td>H2O connection url:</td>
<td>http://localhost:54321</td></tr>
<tr><td>H2O connection proxy:</td>
<td>None</td></tr>
<tr><td>H2O internal security:</td>
<td>False</td></tr>
<tr><td>H2O API Extensions:</td>
<td>Amazon S3, XGBoost, Algos, AutoML, Core V3, Core V4</td></tr>
<tr><td>Python version:</td>
<td>3.7.1 final</td></tr></table></div>


    Parse progress: |█████████████████████████████████████████████████████████| 100%



<table>
<thead>
<tr><th>       </th><th>user_id           </th><th>date               </th><th>source  </th><th>device  </th><th>browser_language  </th><th>ads_channel  </th><th>browser    </th><th>conversion         </th><th>test  </th><th>sex  </th><th>age               </th><th>country    </th></tr>
</thead>
<tbody>
<tr><td>type   </td><td>int               </td><td>time               </td><td>enum    </td><td>enum    </td><td>enum              </td><td>enum         </td><td>enum       </td><td>int                </td><td>enum  </td><td>enum </td><td>int               </td><td>enum       </td></tr>
<tr><td>mins   </td><td>1.0               </td><td>1448841600000.0    </td><td>        </td><td>        </td><td>                  </td><td>             </td><td>           </td><td>0.0                </td><td>      </td><td>     </td><td>18.0              </td><td>           </td></tr>
<tr><td>mean   </td><td>499889.0781244147 </td><td>1449046579636.8518 </td><td>        </td><td>        </td><td>                  </td><td>             </td><td>           </td><td>0.04569170117971106</td><td>      </td><td>     </td><td>27.128149893414037</td><td>           </td></tr>
<tr><td>maxs   </td><td>1000000.0         </td><td>1449187200000.0    </td><td>        </td><td>        </td><td>                  </td><td>             </td><td>           </td><td>1.0                </td><td>      </td><td>     </td><td>70.0              </td><td>           </td></tr>
<tr><td>sigma  </td><td>288708.31795337563</td><td>125496796.59789945 </td><td>        </td><td>        </td><td>                  </td><td>             </td><td>           </td><td>0.208815895504632  </td><td>      </td><td>     </td><td>6.774760413689504 </td><td>           </td></tr>
<tr><td>zeros  </td><td>0                 </td><td>0                  </td><td>        </td><td>        </td><td>                  </td><td>             </td><td>           </td><td>383192             </td><td>      </td><td>     </td><td>0                 </td><td>           </td></tr>
<tr><td>missing</td><td>0                 </td><td>0                  </td><td>0       </td><td>0       </td><td>0                 </td><td>0            </td><td>0          </td><td>0                  </td><td>0     </td><td>0    </td><td>454               </td><td>0          </td></tr>
<tr><td>0      </td><td>497851.0          </td><td>2015-12-04 00:00:00</td><td>Ads     </td><td>Web     </td><td>ES                </td><td>0            </td><td>IE         </td><td>0.0                </td><td>1     </td><td>M    </td><td>21.0              </td><td>Mexico     </td></tr>
<tr><td>1      </td><td>290051.0          </td><td>2015-12-03 00:00:00</td><td>Ads     </td><td>Mobile  </td><td>Other             </td><td>1            </td><td>Android_App</td><td>0.0                </td><td>1     </td><td>F    </td><td>22.0              </td><td>Mexico     </td></tr>
<tr><td>2      </td><td>548435.0          </td><td>2015-11-30 00:00:00</td><td>Ads     </td><td>Web     </td><td>ES                </td><td>0            </td><td>FireFox    </td><td>0.0                </td><td>1     </td><td>M    </td><td>19.0              </td><td>Mexico     </td></tr>
<tr><td>3      </td><td>540675.0          </td><td>2015-12-03 00:00:00</td><td>Direct  </td><td>Mobile  </td><td>ES                </td><td>2            </td><td>Android_App</td><td>0.0                </td><td>1     </td><td>F    </td><td>22.0              </td><td>Venezuela  </td></tr>
<tr><td>4      </td><td>863394.0          </td><td>2015-12-04 00:00:00</td><td>SEO     </td><td>Mobile  </td><td>Other             </td><td>2            </td><td>Android_App</td><td>0.0                </td><td>0     </td><td>M    </td><td>35.0              </td><td>Mexico     </td></tr>
<tr><td>5      </td><td>261625.0          </td><td>2015-12-04 00:00:00</td><td>Direct  </td><td>Mobile  </td><td>ES                </td><td>2            </td><td>Android_App</td><td>0.0                </td><td>1     </td><td>M    </td><td>31.0              </td><td>Bolivia    </td></tr>
<tr><td>6      </td><td>10427.0           </td><td>2015-12-04 00:00:00</td><td>Ads     </td><td>Mobile  </td><td>ES                </td><td>1            </td><td>Android_App</td><td>0.0                </td><td>0     </td><td>F    </td><td>33.0              </td><td>Mexico     </td></tr>
<tr><td>7      </td><td>8343.0            </td><td>2015-11-30 00:00:00</td><td>Ads     </td><td>Mobile  </td><td>ES                </td><td>3            </td><td>Android_App</td><td>1.0                </td><td>0     </td><td>M    </td><td>37.0              </td><td>Colombia   </td></tr>
<tr><td>8      </td><td>73335.0           </td><td>2015-12-03 00:00:00</td><td>SEO     </td><td>Web     </td><td>ES                </td><td>2            </td><td>IE         </td><td>0.0                </td><td>1     </td><td>F    </td><td>29.0              </td><td>Uruguay    </td></tr>
<tr><td>9      </td><td>234023.0          </td><td>2015-12-03 00:00:00</td><td>SEO     </td><td>Web     </td><td>ES                </td><td>2            </td><td>Chrome     </td><td>0.0                </td><td>0     </td><td>F    </td><td>19.0              </td><td>El Salvador</td></tr>
</tbody>
</table>



```python
feature = ['user_id', 'date', 'source', 'device', 'browser_language',
       'ads_channel', 'browser', 'sex', 'age',
       'country']
target = 'test'
```


```python
# Build random forest model
model = H2OGradientBoostingEstimator(ntrees = 2, max_depth = 2)
model.train(x=feature, y=target, training_frame=h2o_df)
```

    gbm Model Build progress: |███████████████████████████████████████████████| 100%



```python
print(model)
```

    Model Details
    =============
    H2OGradientBoostingEstimator :  Gradient Boosting Machine
    Model Key:  GBM_model_python_1559673801301_326
    
    
    ModelMetricsBinomial: gbm
    ** Reported on train data. **
    
    MSE: 0.24491029334582834
    RMSE: 0.4948841211292077
    LogLoss: 0.6828506675678578
    Mean Per-Class Error: 0.4311918615796265
    AUC: 0.5707957958344152
    pr_auc: 0.6567622518302756
    Gini: 0.1415915916688304
    Confusion Matrix (Act/Pred) for max f1 @ threshold = 0.5178860424703127: 



<div style="overflow:auto"><table style="width:50%"><tr><td><b></b></td>
<td><b>0</b></td>
<td><b>1</b></td>
<td><b>Error</b></td>
<td><b>Rate</b></td></tr>
<tr><td>0</td>
<td>31.0</td>
<td>185525.0</td>
<td>0.9998</td>
<td> (185525.0/185556.0)</td></tr>
<tr><td>1</td>
<td>16.0</td>
<td>215967.0</td>
<td>0.0001</td>
<td> (16.0/215983.0)</td></tr>
<tr><td>Total</td>
<td>47.0</td>
<td>401492.0</td>
<td>0.4621</td>
<td> (185541.0/401539.0)</td></tr></table></div>


    Maximum Metrics: Maximum metrics at their respective thresholds
    



<div style="overflow:auto"><table style="width:50%"><tr><td><b>metric</b></td>
<td><b>threshold</b></td>
<td><b>value</b></td>
<td><b>idx</b></td></tr>
<tr><td>max f1</td>
<td>0.5178860</td>
<td>0.6995166</td>
<td>4.0</td></tr>
<tr><td>max f2</td>
<td>0.5174320</td>
<td>0.8533700</td>
<td>5.0</td></tr>
<tr><td>max f0point5</td>
<td>0.5301836</td>
<td>0.5926951</td>
<td>3.0</td></tr>
<tr><td>max accuracy</td>
<td>0.5306366</td>
<td>0.5407868</td>
<td>2.0</td></tr>
<tr><td>max precision</td>
<td>0.6060161</td>
<td>0.8996130</td>
<td>0.0</td></tr>
<tr><td>max recall</td>
<td>0.5174320</td>
<td>1.0</td>
<td>5.0</td></tr>
<tr><td>max specificity</td>
<td>0.6060161</td>
<td>0.9977635</td>
<td>0.0</td></tr>
<tr><td>max absolute_mcc</td>
<td>0.5873741</td>
<td>0.2062759</td>
<td>1.0</td></tr>
<tr><td>max min_per_class_accuracy</td>
<td>0.5306366</td>
<td>0.3999817</td>
<td>2.0</td></tr>
<tr><td>max mean_per_class_accuracy</td>
<td>0.5873741</td>
<td>0.5688081</td>
<td>1.0</td></tr></table></div>


    Gains/Lift Table: Avg response rate: 53.79 %, avg score: 53.78 %
    



<div style="overflow:auto"><table style="width:50%"><tr><td><b></b></td>
<td><b>group</b></td>
<td><b>cumulative_data_fraction</b></td>
<td><b>lower_threshold</b></td>
<td><b>lift</b></td>
<td><b>cumulative_lift</b></td>
<td><b>response_rate</b></td>
<td><b>score</b></td>
<td><b>cumulative_response_rate</b></td>
<td><b>cumulative_score</b></td>
<td><b>capture_rate</b></td>
<td><b>cumulative_capture_rate</b></td>
<td><b>gain</b></td>
<td><b>cumulative_gain</b></td></tr>
<tr><td></td>
<td>1</td>
<td>0.0102954</td>
<td>0.6060161</td>
<td>1.6724913</td>
<td>1.6724913</td>
<td>0.8996130</td>
<td>0.6060161</td>
<td>0.8996130</td>
<td>0.6060161</td>
<td>0.0172189</td>
<td>0.0172189</td>
<td>67.2491310</td>
<td>67.2491310</td></tr>
<tr><td></td>
<td>2</td>
<td>0.1266801</td>
<td>0.5873741</td>
<td>1.4869246</td>
<td>1.5020057</td>
<td>0.7997989</td>
<td>0.5873741</td>
<td>0.8079108</td>
<td>0.5888891</td>
<td>0.1730553</td>
<td>0.1902742</td>
<td>48.6924588</td>
<td>50.2005738</td></tr>
<tr><td></td>
<td>3</td>
<td>0.6332262</td>
<td>0.5306366</td>
<td>0.9307772</td>
<td>1.0450544</td>
<td>0.5006539</td>
<td>0.5306366</td>
<td>0.5621222</td>
<td>0.5422903</td>
<td>0.4714816</td>
<td>0.6617558</td>
<td>-6.9222751</td>
<td>4.5054399</td></tr>
<tr><td></td>
<td>4</td>
<td>0.9997186</td>
<td>0.5301836</td>
<td>0.9223922</td>
<td>1.0000870</td>
<td>0.4961437</td>
<td>0.5301836</td>
<td>0.5379348</td>
<td>0.5378520</td>
<td>0.3380498</td>
<td>0.9998055</td>
<td>-7.7607789</td>
<td>0.0086982</td></tr>
<tr><td></td>
<td>5</td>
<td>1.0</td>
<td>0.5174320</td>
<td>0.6910015</td>
<td>1.0</td>
<td>0.3716814</td>
<td>0.5176972</td>
<td>0.5378880</td>
<td>0.5378464</td>
<td>0.0001945</td>
<td>1.0</td>
<td>-30.8998467</td>
<td>0.0</td></tr></table></div>


    
    Scoring History: 



<div style="overflow:auto"><table style="width:50%"><tr><td><b></b></td>
<td><b>timestamp</b></td>
<td><b>duration</b></td>
<td><b>number_of_trees</b></td>
<td><b>training_rmse</b></td>
<td><b>training_logloss</b></td>
<td><b>training_auc</b></td>
<td><b>training_pr_auc</b></td>
<td><b>training_lift</b></td>
<td><b>training_classification_error</b></td></tr>
<tr><td></td>
<td>2019-07-01 12:05:49</td>
<td> 0.004 sec</td>
<td>0.0</td>
<td>0.4985624</td>
<td>0.6902734</td>
<td>0.5</td>
<td>0.0</td>
<td>1.0</td>
<td>0.4621120</td></tr>
<tr><td></td>
<td>2019-07-01 12:05:49</td>
<td> 0.523 sec</td>
<td>1.0</td>
<td>0.4965328</td>
<td>0.6861928</td>
<td>0.5691115</td>
<td>0.6926047</td>
<td>1.6724913</td>
<td>0.4621120</td></tr>
<tr><td></td>
<td>2019-07-01 12:05:50</td>
<td> 0.752 sec</td>
<td>2.0</td>
<td>0.4948841</td>
<td>0.6828507</td>
<td>0.5707958</td>
<td>0.6567623</td>
<td>1.6724913</td>
<td>0.4620747</td></tr></table></div>


    Variable Importances: 



<div style="overflow:auto"><table style="width:50%"><tr><td><b>variable</b></td>
<td><b>relative_importance</b></td>
<td><b>scaled_importance</b></td>
<td><b>percentage</b></td></tr>
<tr><td>country</td>
<td>7758.7045898</td>
<td>1.0</td>
<td>0.9995396</td></tr>
<tr><td>age</td>
<td>1.8242611</td>
<td>0.0002351</td>
<td>0.0002350</td></tr>
<tr><td>browser</td>
<td>1.7491521</td>
<td>0.0002254</td>
<td>0.0002253</td></tr>
<tr><td>user_id</td>
<td>0.0</td>
<td>0.0</td>
<td>0.0</td></tr>
<tr><td>date</td>
<td>0.0</td>
<td>0.0</td>
<td>0.0</td></tr>
<tr><td>source</td>
<td>0.0</td>
<td>0.0</td>
<td>0.0</td></tr>
<tr><td>device</td>
<td>0.0</td>
<td>0.0</td>
<td>0.0</td></tr>
<tr><td>browser_language</td>
<td>0.0</td>
<td>0.0</td>
<td>0.0</td></tr>
<tr><td>ads_channel</td>
<td>0.0</td>
<td>0.0</td>
<td>0.0</td></tr>
<tr><td>sex</td>
<td>0.0</td>
<td>0.0</td>
<td>0.0</td></tr></table></div>


    



```python
data.groupby(['country','test']).size()
```




    country      test
    Argentina    0        9356
                 1       37377
    Bolivia      0        5550
                 1        5574
    Chile        0        9853
                 1        9884
    Colombia     0       27088
                 1       26972
    Costa Rica   0        2660
                 1        2649
    Ecuador      0        8036
                 1        7859
    El Salvador  0        4108
                 1        4067
    Guatemala    0        7622
                 1        7503
    Honduras     0        4361
                 1        4207
    Mexico       0       64209
                 1       64275
    Nicaragua    0        3419
                 1        3304
    Panama       0        1966
                 1        1985
    Paraguay     0        3650
                 1        3697
    Peru         0       16869
                 1       16797
    Uruguay      0         415
                 1        3719
    Venezuela    0       16149
                 1       15905
    missing      0         245
                 1         209
    dtype: int64



 Argentina and Uruguay together have 80% test and 20% control!
  Not a great success given that the goal was to improve conversion rate, but at least we know that a localized translation didn’t make things worse!


```python
countries = [name for name in data['country'].unique() if name is not np.nan]

for country in countries:
    test_val = data[(data['country'] == country) & (data['test'] == 1)]['conversion'].values
    cont_val = data[(data['country'] == country) & (data['test'] == 0)]['conversion'].values
    
    test_mean = test_val.mean()
    cont_mean = cont_val.mean()
    p_val = ttest_ind(test_val, cont_val, equal_var=False).pvalue
    
    print('{0:15s} {1:15.5f} {2:15.5f} {3:10f}'.format(country, test_mean, cont_mean, p_val))
```

    Mexico                  0.05119         0.04949   0.165544
    Venezuela               0.04898         0.05034   0.573702
    Bolivia                 0.04790         0.04937   0.718885
    Colombia                0.05057         0.05209   0.423719
    Uruguay                 0.01291         0.01205   0.879764
    El Salvador             0.04795         0.05355   0.248127
    Nicaragua               0.05418         0.05265   0.780400
    Peru                    0.05060         0.04991   0.771953
    Costa Rica              0.05474         0.05226   0.687876
    Chile                   0.05130         0.04811   0.302848
    Argentina               0.01373         0.01507   0.335147
    Ecuador                 0.04899         0.04915   0.961512
    Guatemala               0.04865         0.05064   0.572107
    Honduras                0.04754         0.05091   0.471463
    Paraguay                0.04923         0.04849   0.883697
    Panama                  0.04937         0.04680   0.705327
    missing                 0.05742         0.07755   0.392492



```python

```
