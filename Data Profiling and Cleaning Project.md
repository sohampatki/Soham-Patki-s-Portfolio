Soham Patki <br>
Data Wrangling Project <br>


## **Data Access and Profiling:**


### Reading dataset and creating subset


```python
import pandas as pd
import numpy as np
data_frame = pd.read_csv("Passengers_Satisfaction_Survey.csv")
df_Business = data_frame.loc[data_frame['Class']=='Business']
df_Business
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
      <th>Gender</th>
      <th>Customer Type</th>
      <th>Age</th>
      <th>Type of Travel</th>
      <th>Class</th>
      <th>Flight Distance</th>
      <th>Inflight wifi service</th>
      <th>Departure/Arrival time convenient</th>
      <th>Ease of Online booking</th>
      <th>Food and drink</th>
      <th>Inflight entertainment</th>
      <th>Baggage handling</th>
      <th>Cleanliness</th>
      <th>Departure Delay in Minutes</th>
      <th>Arrival Delay in Minutes</th>
      <th>Satisfaction</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>Loyal Customer</td>
      <td>69.0</td>
      <td>Personal Travel</td>
      <td>Business</td>
      <td>-5421.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>3</td>
      <td>5</td>
      <td>1.0</td>
      <td>1</td>
      <td>3.0</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>Dissatisfied</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>Disloyal Customer</td>
      <td>41.0</td>
      <td>Business travel</td>
      <td>Business</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>neutral</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>Loyal Customer</td>
      <td>52.0</td>
      <td>Business travel</td>
      <td>Business</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>4</td>
      <td>1</td>
      <td>2.0</td>
      <td>2</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>Dissatisfied</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>Disloyal Customer</td>
      <td>21.0</td>
      <td>Business travel</td>
      <td>Business</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>neutral</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Female</td>
      <td>Loyal Customer</td>
      <td>57.0</td>
      <td>Business travel</td>
      <td>Business</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>4</td>
      <td>5</td>
      <td>2.0</td>
      <td>3</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Dissatisfied</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>104905</th>
      <td>Male</td>
      <td>Loyal Customer</td>
      <td>45.0</td>
      <td>Business travel</td>
      <td>Business</td>
      <td>3305.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>2</td>
      <td>3</td>
      <td>4.0</td>
      <td>4</td>
      <td>3.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>satisfied</td>
    </tr>
    <tr>
      <th>104906</th>
      <td>Female</td>
      <td>Loyal Customer</td>
      <td>35.0</td>
      <td>Business travel</td>
      <td>Business</td>
      <td>337.0</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>5</td>
      <td>2</td>
      <td>4.0</td>
      <td>4</td>
      <td>5.0</td>
      <td>54.0</td>
      <td>40.0</td>
      <td>satisfied</td>
    </tr>
    <tr>
      <th>104907</th>
      <td>Male</td>
      <td>Loyal Customer</td>
      <td>47.0</td>
      <td>Business travel</td>
      <td>Business</td>
      <td>3496.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>2</td>
      <td>2</td>
      <td>5.0</td>
      <td>5</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>satisfied</td>
    </tr>
    <tr>
      <th>104908</th>
      <td>Male</td>
      <td>Loyal Customer</td>
      <td>42.0</td>
      <td>Business travel</td>
      <td>Business</td>
      <td>3605.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4</td>
      <td>3</td>
      <td>4.0</td>
      <td>4</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>satisfied</td>
    </tr>
    <tr>
      <th>104909</th>
      <td>Female</td>
      <td>disloyal</td>
      <td>0.0</td>
      <td>Business travel</td>
      <td>Business</td>
      <td>862.0</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>5</td>
      <td>5</td>
      <td>5.0</td>
      <td>1</td>
      <td>5.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>neutral</td>
    </tr>
  </tbody>
</table>
<p>50000 rows × 16 columns</p>
</div>



### Number of columns and rows


```python
df_Business.shape
```




    (50000, 16)



The dataset has **50,000** rows and **16** columns.

### Column names and data type


```python
df_Business.dtypes
```




    Gender                                object
    Customer Type                         object
    Age                                  float64
    Type of Travel                        object
    Class                                 object
    Flight Distance                      float64
    Inflight wifi service                float64
    Departure/Arrival time convenient    float64
    Ease of Online booking                object
    Food and drink                        object
    Inflight entertainment               float64
    Baggage handling                      object
    Cleanliness                          float64
    Departure Delay in Minutes           float64
    Arrival Delay in Minutes             float64
    Satisfaction                          object
    dtype: object



### Unique values in each column

 To improve readability, I used the '.astype(int)' command to display numeric 'float' values without decimals by converting the data into integers. 


```python
for col in df_Business:
    if df_Business[col].dtypes == object:
        print(col, ":", df_Business[col].unique())
    else :
        print(col, ":", df_Business[col].unique().astype(int))
    print("\n")
```

    Gender : ['Male' 'Female' 'F' 'M' nan]
    
    
    Customer Type : ['Loyal Customer' 'Disloyal Customer' nan 'disloyal' 'Loyal']
    
    
    Age : [         69          41          52          21          57          45
              39          33          10          55          59          53
              56          47          54          25          51          60
              27          43          20          19          63          50
     -2147483648          38          46          85          26          42
              30          24          22          17          48          40
              16          32          35          44          49          65
              67          66          64           7          37          36
              15          62          34           0          68          70
              58          11          74          13          80          75
               9          61          71          12          73          72
              18          79           8          76          14          77
              78         180         185]
    
    
    Type of Travel : ['Personal Travel' 'Business travel' 'Business' nan]
    
    
    Class : ['Business']
    
    
    Flight Distance : [  -5421       0      56 ...    4983 2820000 9000000]
    
    
    Inflight wifi service : [          3           1           2           4           0           5
     -2147483648]
    
    
    Departure/Arrival time convenient : [          2           1           4           0           5           3
     -2147483648]
    
    
    Ease of Online booking : ['3' nan '4' '0' '5' 'Unknown' '2' '1']
    
    
    Food and drink : ['5' nan '1' '3' 'Great' '4' '2' '0']
    
    
    Inflight entertainment : [          1 -2147483648           2           3           4           5
              10           0]
    
    
    Baggage handling : ['1' nan '2' '3' '5' '4' 'A']
    
    
    Cleanliness : [          3 -2147483648           4           2           5           0]
    
    
    Departure Delay in Minutes : [         14 -2147483648           0          -2          10          36
               9          35           7          11          61          20
             116          26          13          50          71          27
              25           6          15          32         113           1
               3          46          42          21          43         114
              34          23           8           5          49          76
              37          29         134          28          19          57
              64           2         455         123         122           4
             101          31          39         140          89          40
              18          24          22         111         163          93
              16          30          47          63          17          84
              65          45          60          77          12          38
             161          80          74         156          33         105
              95          88         126         195          94          48
             106         249          78          96          72         104
             107         172         136          44          98          87
              91         330          58          70         125         135
             324          68          53         120          55          59
              97         149          52         118          56          82
             108         152          62         210         157          54
             147          83         138         142         128          41
             124         200         221          81          85         110
              73         262          66         265          86         167
              51         158         206         168         121         242
             199          69          67         209         141         162
              99          75         170         183         148         402
             180         253         151         127         252         112
             328         184         191          79         204         255
             282         117         240         143         159         187
             132         100         164         238         298         185
             276          92         171         129         119         201
             174         692         146         178         115         154
              90         153         137         197         175         130
             144         196         258         227         102         203
             359         244         190         109         326         321
             150         351         245         272         181         211
             216         347         131         145         383         921
             160         166         186         165         205         336
             193         173         430         235         182         179
             188         274         401         194         353         219
             303         297         103         241         212         192
             214         233         207         139         435         133
             610         279         310         208         306         224
             368         248         155         259         266         338
             267         291         218         318         215         271
             293         213         269         285         309         452
             322         232         423         394         292         176
             223         286         275         333         169         246
             273         239         480         381         302         859
             226         290         503         189         236         177
             342         198         307         202         264         370
            1017         465         281         463         225         933
             317         410         407         294         357         287
             930         304         348         340         444         277
             429         250         319         312         313         388
             268         415         454         392        1305         229
             220         243         450         350         222         334
             299         217         308         344         320         501
             438         352         358         256         260         341
             331         289         371         228         853]
    
    
    Arrival Delay in Minutes : [          0 -2147483648           6          21          12          16
               8          49          36          65          10         107
             103           9          18          43         -45          66
              25          13          24         105          14           7
              57          29          39         120          63          27
               3          22          28           1          59           2
              70           4          30          19          15         129
              44          52         210         124         119           5
              42          33          98          34          31          48
             156          84          20          46          53         121
             110          17         157          87          38          40
              99          32          55          69          41          83
              67          96          60          45          78          26
              23          37         171          71          81         168
              80          35          89          11          85         200
              73          93          64         104          50         248
             122         117          97         163         135         146
             140          68          72         128         348          77
             134          94         133         323         115          74
              58          91         177         125         138         206
             100          54         259          75         161         137
             131         233         217         130         142         101
             154         256         112          47          62         260
             194         209         118          92         228          79
             150         188          76         153         139          90
             246          95          61         203         143         158
             174         187          56          51         389         106
             257         169         224         195         275          88
             109         330         180         173          86         113
             136         202         255         305         225          82
             148         116         186         126         204         287
             181         147         176         293         208         159
             127         702         185         167         102         123
             165         182         295         152         191         164
             218         262         268         108         379         193
             240         366         192         172         360         336
             298         230         267         149         114         132
             141         183         196         231         359         229
             386         924         213         266         145         216
             151         235         327         178         160         232
             438         243         170         263         404         284
             249         198         302         290         184         227
             212         162         223         470         155         593
             309         299         307         357         261         219
             304         199         292         175         301         254
             111         258         253         270         265         285
             250         444         403         407         334         331
             247         264         234         221         277         380
             189         251         308         238         252         471
             436         319         860         278         317         507
             166         385         205         222        1011         493
             503         281         337         920         144         179
             324         237         272         435         409         283
             362         289         952         288         190         300
             321         341         201         434         392         417
             271         320         236         410         502         381
            1280         211         297         244         388         242
             318         370         207         383         313         226
             197         393         500         322         335         274
             214         220         245         279         296         339
             316         823]
    
    
    Satisfaction : ['Dissatisfied' 'neutral' 'satisfied' 'dissatisfied' nan]
    
    


### Number of missing values in each column


```python
df_Business.isnull().sum()
```




    Gender                               1761
    Customer Type                        1761
    Age                                  2627
    Type of Travel                       1761
    Class                                   0
    Flight Distance                      3396
    Inflight wifi service                1761
    Departure/Arrival time convenient    1761
    Ease of Online booking               7295
    Food and drink                       7295
    Inflight entertainment               6351
    Baggage handling                     6351
    Cleanliness                          6351
    Departure Delay in Minutes           6351
    Arrival Delay in Minutes             7902
    Satisfaction                         2160
    dtype: int64



### Summary statistics for each column
For readability, I restrict numerical values to 2 decimal points.


```python
df_Business.describe().round(2)
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
      <th>Age</th>
      <th>Flight Distance</th>
      <th>Inflight wifi service</th>
      <th>Departure/Arrival time convenient</th>
      <th>Inflight entertainment</th>
      <th>Cleanliness</th>
      <th>Departure Delay in Minutes</th>
      <th>Arrival Delay in Minutes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>47373.00</td>
      <td>46604.00</td>
      <td>48239.00</td>
      <td>48239.00</td>
      <td>43649.00</td>
      <td>43649.00</td>
      <td>43649.0</td>
      <td>42098.00</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>41.85</td>
      <td>1934.74</td>
      <td>2.77</td>
      <td>2.91</td>
      <td>3.84</td>
      <td>3.74</td>
      <td>13.6</td>
      <td>13.06</td>
    </tr>
    <tr>
      <th>std</th>
      <td>13.73</td>
      <td>43693.44</td>
      <td>1.42</td>
      <td>1.50</td>
      <td>1.23</td>
      <td>0.99</td>
      <td>36.6</td>
      <td>38.12</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.00</td>
      <td>-5421.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>-2.0</td>
      <td>-45.00</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>34.00</td>
      <td>594.00</td>
      <td>2.00</td>
      <td>2.00</td>
      <td>3.00</td>
      <td>3.00</td>
      <td>0.0</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>43.00</td>
      <td>1597.00</td>
      <td>3.00</td>
      <td>3.00</td>
      <td>4.00</td>
      <td>4.00</td>
      <td>0.0</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>52.00</td>
      <td>2565.00</td>
      <td>4.00</td>
      <td>4.00</td>
      <td>5.00</td>
      <td>5.00</td>
      <td>10.0</td>
      <td>11.00</td>
    </tr>
    <tr>
      <th>max</th>
      <td>185.00</td>
      <td>9000000.00</td>
      <td>5.00</td>
      <td>5.00</td>
      <td>10.00</td>
      <td>5.00</td>
      <td>1305.0</td>
      <td>1280.00</td>
    </tr>
  </tbody>
</table>
</div>



To get summary statistics for columns with 'object' data type, I use the 'include=object' command


```python
df_Business.describe(include=object)
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
      <th>Gender</th>
      <th>Customer Type</th>
      <th>Type of Travel</th>
      <th>Class</th>
      <th>Ease of Online booking</th>
      <th>Food and drink</th>
      <th>Baggage handling</th>
      <th>Satisfaction</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>48239</td>
      <td>48239</td>
      <td>48239</td>
      <td>50000</td>
      <td>42705</td>
      <td>42705</td>
      <td>43649</td>
      <td>47840</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>4</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>7</td>
      <td>7</td>
      <td>6</td>
      <td>4</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Female</td>
      <td>Loyal Customer</td>
      <td>Business travel</td>
      <td>Business</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>satisfied</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>23001</td>
      <td>40770</td>
      <td>44063</td>
      <td>50000</td>
      <td>8635</td>
      <td>10890</td>
      <td>17056</td>
      <td>30400</td>
    </tr>
  </tbody>
</table>
</div>



### Frequency distribution for unique values in each column


```python
for col in df_Business:
    print(df_Business[col].value_counts())
    print("\n")
```

    Female    23001
    Male      22830
    F          1361
    M          1047
    Name: Gender, dtype: int64
    
    
    Loyal Customer       40770
    Disloyal Customer     6636
    disloyal               831
    Loyal                    2
    Name: Customer Type, dtype: int64
    
    
    39.0     1935
    40.0     1677
    41.0     1533
    42.0     1514
    44.0     1506
             ... 
    73.0       22
    78.0       17
    85.0       14
    180.0       1
    185.0       1
    Name: Age, Length: 74, dtype: int64
    
    
    Business travel    44063
    Business            2122
    Personal Travel     2054
    Name: Type of Travel, dtype: int64
    
    
    Business    50000
    Name: Class, dtype: int64
    
    
    337.0        225
    2475.0       187
    2586.0       148
    1744.0       133
    404.0        133
                ... 
    1285.0         1
    1287.0         1
    1288.0         1
    1289.0         1
    9000000.0      1
    Name: Flight Distance, Length: 3573, dtype: int64
    
    
    2.0    10784
    3.0    10593
    4.0     9140
    1.0     8808
    5.0     7025
    0.0     1889
    Name: Inflight wifi service, dtype: int64
    
    
    4.0    9993
    3.0    9220
    2.0    9183
    5.0    9173
    1.0    8546
    0.0    2124
    Name: Departure/Arrival time convenient, dtype: int64
    
    
    3          8635
    4          8627
    2          8444
    5          7713
    1          7042
    0          1747
    Unknown     497
    Name: Ease of Online booking, dtype: int64
    
    
    4        10890
    5        10296
    3         9710
    2         9567
    1         1722
    Great      497
    0           23
    Name: Food and drink, dtype: int64
    
    
    4.0     15884
    5.0     13508
    3.0      6772
    2.0      5714
    1.0      1451
    10.0      314
    0.0         6
    Name: Inflight entertainment, dtype: int64
    
    
    4    17056
    5    14375
    3     5841
    2     3783
    1     1920
    A      674
    Name: Baggage handling, dtype: int64
    
    
    4.0    14132
    3.0    12473
    5.0    11704
    2.0     5336
    0.0        4
    Name: Cleanliness, dtype: int64
    
    
    0.0      24351
    10.0      1401
    1.0       1228
    2.0        917
    3.0        802
             ...  
    692.0        1
    309.0        1
    285.0        1
    269.0        1
    853.0        1
    Name: Departure Delay in Minutes, Length: 358, dtype: int64
    
    
    0.0      24229
    1.0        885
    2.0        833
    4.0        774
    3.0        772
             ...  
    436.0        1
    379.0        1
    252.0        1
    238.0        1
    823.0        1
    Name: Arrival Delay in Minutes, Length: 361, dtype: int64
    
    
    satisfied       30400
    Dissatisfied     7795
    neutral          6123
    dissatisfied     3522
    Name: Satisfaction, dtype: int64
    
    


### Number of fully duplicated rows


```python
df_Business.duplicated().sum()
```




    3064



### List of issues:

* **Issue 1**: The dataset has float and object values. The columns 'Ease of Online Booking', 'Food and Drink' and 'Baggage Handling' are ratings and should be integer values instead of objects. The columns 'Inflight wifi service', 'Departure/Arrival time convenient','Inflight Entertainment' and 'Cleanliness' should be integer values but are stored as float. <br><br>

* **Issue 2**: There are formatting issues in the 'Gender', 'Customer Type', 'Type of Travel','Satisfaction', "Ease of online booking', 'Food and Drink' and 'Baggage Handling' columns.  For example, in the 'Gender' column, there are 1047 'M' entries and 22830 'Male' entries, which should be consolidated under one term. In the 'Baggage Handling' column, there are 674 'A' entries which should be represented numerically as a rating.  <br><br>

* **Issue 4**: Some columns have extreme and unrealistic outliers. In the 'Flight Distance' column, we see extreme outliers of 9000000 and -5421. In the 'Entertainment' column, the maximum value is 10, even though the rating ranges from 1 to 5. <br><br>

* **Issue 5**: There are null values in all columns other than the 'Class' column. <br> <br>

* **Issue 6**: There are 3064 duplicated rows in the dataset.


## Data Cleansing:

### Drop Class column


```python
df_Business.drop(["Class"], axis=1, inplace=True)
df_Business = df_Business.reset_index(drop=True)
```

    C:\Users\patki\AppData\Local\Temp\ipykernel_11256\366919401.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df_Business.drop(["Class"], axis=1, inplace=True)


### Rename Columns


```python
df_Business.rename(columns = lambda x: x.lower().replace(' ','_'), inplace= True)
df_Business.columns
```




    Index(['gender', 'customer_type', 'age', 'type_of_travel', 'flight_distance',
           'inflight_wifi_service', 'departure/arrival_time_convenient',
           'ease_of_online_booking', 'food_and_drink', 'inflight_entertainment',
           'baggage_handling', 'cleanliness', 'departure_delay_in_minutes',
           'arrival_delay_in_minutes', 'satisfaction'],
          dtype='object')



### Modify improperly formatted data

We will fix the formatting in the columns:
* gender - Replace 'M' with 'Male', and 'F' with 'Female'. 
* customer_type - Replace 'disloyal' with 'Disloyal Customer', and 'Loyal' with 'Loyal Customer'.
* type_of_travel - Replace 'Business travel' and 'Business' with 'Business Travel'
* satisfaction - Replace 'Dissatisfied' with 'dissatisfied'.
* ease_of_online_booking - Replace 'unkown' with null value
* food_and_drink - Replace 'Great' with the rating of 5
* baggage_handling - Replace 'A' with the rating of 5. 


```python
df_Business['gender'] = df_Business['gender'].replace(['M', 'F'],['Male', 'Female'])
df_Business['customer_type']=df_Business['customer_type'].replace(['disloyal', 'Loyal'],['Disloyal Customer','Loyal Customer'])
df_Business['type_of_travel']=df_Business['type_of_travel'].replace(['Business travel','Business'], ['Business Travel','Business Travel'])
df_Business['satisfaction']=df_Business['satisfaction'].replace('Dissatisfied','dissatisfied')
df_Business['ease_of_online_booking'] = df_Business['ease_of_online_booking'].replace('Unknown', np.nan)
df_Business['food_and_drink'] = df_Business['food_and_drink'].replace('Great', '5')
df_Business['baggage_handling'] = df_Business['baggage_handling'].replace('A', '5')
```

### Remove fully duplicated observations


```python
df_Business.drop_duplicates(inplace=True)
df_Business.duplicated().sum()
```




    0



### Handle missing values

First, we check how many null values remain after duplicated rows are removed


```python
df_Business.isnull().sum()
```




    gender                                  2
    customer_type                           2
    age                                   863
    type_of_travel                          2
    flight_distance                       832
    inflight_wifi_service                   2
    departure/arrival_time_convenient       2
    ease_of_online_booking               5174
    food_and_drink                       4747
    inflight_entertainment               4442
    baggage_handling                     4442
    cleanliness                          4442
    departure_delay_in_minutes           4442
    arrival_delay_in_minutes             5282
    satisfaction                         2131
    dtype: int64



A significant amount of null values still remain. I decide to delete any row in the dataset that has a null value.


```python
df_Business.dropna(axis=0, how='any', inplace=True)
df_Business.isnull().sum()
```




    gender                               0
    customer_type                        0
    age                                  0
    type_of_travel                       0
    flight_distance                      0
    inflight_wifi_service                0
    departure/arrival_time_convenient    0
    ease_of_online_booking               0
    food_and_drink                       0
    inflight_entertainment               0
    baggage_handling                     0
    cleanliness                          0
    departure_delay_in_minutes           0
    arrival_delay_in_minutes             0
    satisfaction                         0
    dtype: int64



### Handling outliers

* In the 'inflight_entertainment' column, there are 314 entries with the value 10. Since the rating is on a scale from 1 to 5, I will replace these values with a 5. 
* In the 'flight_distance' column, I will replace the -5421 value with +5421, and delete the rows with values greater than 10,000. This is because the longest possible distance for commercial flights currently is 9572 miles. 


```python
df_Business['inflight_entertainment']=df_Business['inflight_entertainment'].replace(10,5)
df_Business['flight_distance']=df_Business['flight_distance'].replace(-5421,5421)
df_Business = df_Business[df_Business['flight_distance'] < 10000 ]
```

### Fixing Data Types

* Change columns - 'inflight_wifi_service', 'departure/arrival_time_convenient', 'inflight_entertainment' and 'cleanliness' from float to integer.

* Change columns - 'ease_of_online_booking', 'food_and_drink'and 'baggage_handling' from object to integer. 


```python
df_Business[['inflight_wifi_service', 'departure/arrival_time_convenient', 'inflight_entertainment', 'cleanliness', 'ease_of_online_booking', 'food_and_drink', 'baggage_handling']]=df_Business[['inflight_wifi_service', 'departure/arrival_time_convenient', 'inflight_entertainment', 'cleanliness', 'ease_of_online_booking', 'food_and_drink', 'baggage_handling']].astype(int)
```


```python
df_Business.shape
```




    (39703, 15)



## Comparing Data Characteristics before and after Data Cleansing

The following changes can be observed:<br>
* Number of rows reduced from **50,000** to **39,703**. 
* The 'class' column was deleted, reducing number of columns from **16** to **15**. 
* All column names were made lowercase and spaces were replaced by underscore. 
* The columns 'inflight_wifi_service', 'departure/arrival_time_convenient', 'inflight_entertainment' and 'cleanliness' were changed from float to integer.
* The columns 'ease_of_online_booking', 'food_and_drink'and 'baggage_handling' were changed from object to integer.
* Formatting issues in 7 columns were corrected. For example - 'Unknown' entries in the 'ease_of_online_booking' column were changed to null values, 'M' entries in the 'gender' column were replaced with 'Male', and ratings provided as 'Great' or 'A' were replaced with the value 5. 
* There were **3064** duplicated rows that were deleted.
* Each column in the original dataset had more than 1000 null values. All rows with null values were deleted. 
* Outliers in the 'flight_distance' column were corrected or deleted. 



## Before Cleansing:

### Column names and datatype

`Gender                                object
Customer Type                         object
Age                                  float64
Type of Travel                        object
Class                                 object
Flight Distance                      float64
Inflight wifi service                float64
Departure/Arrival time convenient    float64
Ease of Online booking                object
Food and drink                        object
Inflight entertainment               float64
Baggage handling                      object
Cleanliness                          float64
Departure Delay in Minutes           float64
Arrival Delay in Minutes             float64
Satisfaction                          object
dtype: object`

### Unique Values for each Column

`Gender : ['Male' 'Female' 'F' 'M' nan]


Customer Type : ['Loyal Customer' 'Disloyal Customer' nan 'disloyal' 'Loyal']


Age : [         69          41          52          21          57          45
          39          33          10          55          59          53
          56          47          54          25          51          60
          27          43          20          19          63          50
 -2147483648          38          46          85          26          42
          30          24          22          17          48          40
          16          32          35          44          49          65
          67          66          64           7          37          36
          15          62          34           0          68          70
          58          11          74          13          80          75
           9          61          71          12          73          72
          18          79           8          76          14          77
          78         180         185]


Type of Travel : ['Personal Travel' 'Business travel' 'Business' nan]


Class : ['Business']


Flight Distance : [  -5421       0      56 ...    4983 2820000 9000000]


Inflight wifi service : [          3           1           2           4           0           5
 -2147483648]


Departure/Arrival time convenient : [          2           1           4           0           5           3
 -2147483648]


Ease of Online booking : ['3' nan '4' '0' '5' 'Unknown' '2' '1']


Food and drink : ['5' nan '1' '3' 'Great' '4' '2' '0']


Inflight entertainment : [          1 -2147483648           2           3           4           5
          10           0]


Baggage handling : ['1' nan '2' '3' '5' '4' 'A']


Cleanliness : [          3 -2147483648           4           2           5           0]


Departure Delay in Minutes : [         14 -2147483648           0          -2          10          36
           9          35           7          11          61          20
         116          26          13          50          71          27
          25           6          15          32         113           1
           3          46          42          21          43         114
          34          23           8           5          49          76
          37          29         134          28          19          57
          64           2         455         123         122           4
         101          31          39         140          89          40
          18          24          22         111         163          93
          16          30          47          63          17          84
          65          45          60          77          12          38
         161          80          74         156          33         105
          95          88         126         195          94          48
         106         249          78          96          72         104
         107         172         136          44          98          87
          91         330          58          70         125         135
         324          68          53         120          55          59
          97         149          52         118          56          82
         108         152          62         210         157          54
         147          83         138         142         128          41
         124         200         221          81          85         110
          73         262          66         265          86         167
          51         158         206         168         121         242
         199          69          67         209         141         162
          99          75         170         183         148         402
         180         253         151         127         252         112
         328         184         191          79         204         255
         282         117         240         143         159         187
         132         100         164         238         298         185
         276          92         171         129         119         201
         174         692         146         178         115         154
          90         153         137         197         175         130
         144         196         258         227         102         203
         359         244         190         109         326         321
         150         351         245         272         181         211
         216         347         131         145         383         921
         160         166         186         165         205         336
         193         173         430         235         182         179
         188         274         401         194         353         219
         303         297         103         241         212         192
         214         233         207         139         435         133
         610         279         310         208         306         224
         368         248         155         259         266         338
         267         291         218         318         215         271
         293         213         269         285         309         452
         322         232         423         394         292         176
         223         286         275         333         169         246
         273         239         480         381         302         859
         226         290         503         189         236         177
         342         198         307         202         264         370
        1017         465         281         463         225         933
         317         410         407         294         357         287
         930         304         348         340         444         277
         429         250         319         312         313         388
         268         415         454         392        1305         229
         220         243         450         350         222         334
         299         217         308         344         320         501
         438         352         358         256         260         341
         331         289         371         228         853]


Arrival Delay in Minutes : [          0 -2147483648           6          21          12          16
           8          49          36          65          10         107
         103           9          18          43         -45          66
          25          13          24         105          14           7
          57          29          39         120          63          27
           3          22          28           1          59           2
          70           4          30          19          15         129
          44          52         210         124         119           5
          42          33          98          34          31          48
         156          84          20          46          53         121
         110          17         157          87          38          40
          99          32          55          69          41          83
          67          96          60          45          78          26
          23          37         171          71          81         168
          80          35          89          11          85         200
          73          93          64         104          50         248
         122         117          97         163         135         146
         140          68          72         128         348          77
         134          94         133         323         115          74
          58          91         177         125         138         206
         100          54         259          75         161         137
         131         233         217         130         142         101
         154         256         112          47          62         260
         194         209         118          92         228          79
         150         188          76         153         139          90
         246          95          61         203         143         158
         174         187          56          51         389         106
         257         169         224         195         275          88
         109         330         180         173          86         113
         136         202         255         305         225          82
         148         116         186         126         204         287
         181         147         176         293         208         159
         127         702         185         167         102         123
         165         182         295         152         191         164
         218         262         268         108         379         193
         240         366         192         172         360         336
         298         230         267         149         114         132
         141         183         196         231         359         229
         386         924         213         266         145         216
         151         235         327         178         160         232
         438         243         170         263         404         284
         249         198         302         290         184         227
         212         162         223         470         155         593
         309         299         307         357         261         219
         304         199         292         175         301         254
         111         258         253         270         265         285
         250         444         403         407         334         331
         247         264         234         221         277         380
         189         251         308         238         252         471
         436         319         860         278         317         507
         166         385         205         222        1011         493
         503         281         337         920         144         179
         324         237         272         435         409         283
         362         289         952         288         190         300
         321         341         201         434         392         417
         271         320         236         410         502         381
        1280         211         297         244         388         242
         318         370         207         383         313         226
         197         393         500         322         335         274
         214         220         245         279         296         339
         316         823]


Satisfaction : ['Dissatisfied' 'neutral' 'satisfied' 'dissatisfied' nan]`

### Number of missing values in each column

`Gender                               1761
Customer Type                        1761
Age                                  2627
Type of Travel                       1761
Class                                   0
Flight Distance                      3396
Inflight wifi service                1761
Departure/Arrival time convenient    1761
Ease of Online booking               7295
Food and drink                       7295
Inflight entertainment               6351
Baggage handling                     6351
Cleanliness                          6351
Departure Delay in Minutes           6351
Arrival Delay in Minutes             7902
Satisfaction                         2160
dtype: int64`

## After Cleansing:

### Column names and datatype

`gender                                object
customer_type                         object
age                                  float64
type_of_travel                        object
flight_distance                      float64
inflight_wifi_service                  int32
departure/arrival_time_convenient      int32
ease_of_online_booking                 int32
food_and_drink                         int32
inflight_entertainment                 int32
baggage_handling                       int32
cleanliness                            int32
departure_delay_in_minutes           float64
arrival_delay_in_minutes             float64
satisfaction                          object
dtype: object`

### Unique values for each column

`gender : ['Male' 'Female']


customer_type : ['Loyal Customer' 'Disloyal Customer']


age : [ 69  52  57  45  39  33  55  56  47  54  51  60  27  20  19  63  50  38
  46  85  59  41  26  42  24  22  17  48  16  53  32  35  44  49  65  67
  66  64  21  37  36  15  62  40   0  43  68  70  58  34  11  74  13  80
  75  71  12  61  73  72  79  76  14  77  78 180 185]


type_of_travel : ['Personal Travel' 'Business Travel']


flight_distance : [5421    0   56 ... 4817 4963 4983]


inflight_wifi_service : [3 2 0 4 5 1]


departure/arrival_time_convenient : [2 4 0 5 3 1]


ease_of_online_booking : [3 4 0 5 2 1]


food_and_drink : [5 1 3 4 2 0]


inflight_entertainment : [1 2 3 4 5 0]


baggage_handling : [1 2 3 5 4]


cleanliness : [3 4 2 5 0]


departure_delay_in_minutes : [  14    0   10   36    9   35    7   11   61  116   26   13   50   71
   27   25    6   15   32  113    1    3   46   42   21   43  114   34
   23    8   20    5   49   76   37   29  134   28   19   57   64    2
  123  122    4  101   31   39  140   89   40   18   24   22  111  163
   93   16   30   47   63   17   84   65   45   60   77   38  161   12
   80   74  156   33  105   95   88  126  195   94   48  106  249   78
   96   72  104  107  172  136   44   98   87   91  330   58   70  125
  135  324   68   53  120   55   59   97  149   52  118   56   82  108
  152   62  210   54  147   83  138  142  128   41  124  200  221   85
  110   73  262   66  265   86  167   51  158  206  168  121  242  199
   69   67  141  162   99   75  170  183  148  402  180  253  151  209
  127  252  112  328  184  191  157   79  204  255  282  117  240   81
  143  159  187  132  100  164  238  298  185  276   92  171  129  119
  201  692  146  178  115  154  174   90  153  137  197  175  130  144
  196  258  227  102  203  359  244  190  109  326  321  150  351  245
  272  181  211  216  347  131  145  383  921  160  166  186  165  205
  336  193  173  430  235  182  179  188  274  401  194  219  303  297
  103  241  212  192  214  233  207  139  435  133  610  279  310  208
  306  224  368  248  155  259  266  338  267  291  218  318  215  271
  293  213  269  285  309  452  322  232  423  394  292  176  223  286
  275  333  246  273  239  480  381  302  859  169  226  290  503  189
  236  177  342  198  307  202  264  370 1017  465  281  463  225  933
  317  410  407  294  357  287  930  304  348  340  444  277  429  250
  319  312  313  388  268  415  454  392 1305  229  220  243  450  350
  222  334  299  217  308  353  344  320  501  438  352  358  256  260
  341  331  289  371  228  853]


arrival_delay_in_minutes : [   0    6   21   12   16    8   49   36   65  107  103    9   18   43
  -45   66   25   13   24  105   14    7   57   29   39  120   63   27
    3   22   28    1   59    2   70    4   30   19   15  129   44   52
  210  124  119    5   42   33   98   34   31   48  156   84   20   46
   53  121  110   10   17  157   87   38   40   99   32   55   69   41
   83   67   96   60   45   78   26   23   37  171   71   81  168   80
   35   89   11   85  200   73   93   64  104   50  248  122  117   97
  163  135  146  140   68   72  128  348   77  134   94  133  323  115
   74   58   91  177  125  138  206  100   54  259   75  161  137  131
  233  217  130  142  101  154  256  112   47   62  260  194  209  118
   92  228   79  150  188   76  153  139   90  246   95   61  143  158
  174  187   56   51  389  106  257  169  224  195  275   88  109  330
  203  180  173   86  113  136  202  255  305  225   82  148  116  186
  126  204  287  181  147  176  293  208  159  127  702  185  167  102
  123  165  182  295  152  191  164  218  262  268  108  379  193  240
  366  192  172  360  336  298  230  267  149  114  132  141  183  196
  231  359  229  386  924  213  266  145  216  151  235  327  178  160
  232  438  243  170  263  404  284  249  198  302  290  184  227  212
  162  223  470  155  593  309  299  307  357  261  219  304  199  292
  301  254  111  258  253  270  265  285  250  444  403  407  334  331
  175  247  264  234  221  277  380  189  251  308  238  252  471  436
  319  860  278  317  507  166  385  205  222 1011  493  503  281  337
  920  144  179  324  237  272  435  409  283  362  289  952  288  190
  300  321  341  201  434  392  417  271  320  236  410  502  381 1280
  211  297  244  388  242  318  370  207  383  313  226  197  393  500
  322  335  274  214  220  245  279  296  339  316  823]


satisfaction : ['dissatisfied' 'satisfied' 'neutral']`

### Number of missing values in each column

`gender                               0
customer_type                        0
age                                  0
type_of_travel                       0
flight_distance                      0
inflight_wifi_service                0
departure/arrival_time_convenient    0
ease_of_online_booking               0
food_and_drink                       0
inflight_entertainment               0
baggage_handling                     0
cleanliness                          0
departure_delay_in_minutes           0
arrival_delay_in_minutes             0
satisfaction                         0
dtype: int64`
