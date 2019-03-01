
# SQL to Pandas 速查表


```python
import pandas as pd
import pymysql
```

## 术语对照表

| SQL |Pandas |
| --- | --- |
|  row| row |
|  column| column |
|  index| index |
|  table| DataFrame |
|  limit | head |


```python
conn = pymysql.connect(host='127.0.0.1', user='root', password='12345678',db='test_db')
```


```python
df = pd.read_sql(sql="select * from student", con=conn)
```


```python
df
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
      <th>id</th>
      <th>name</th>
      <th>age</th>
      <th>sex</th>
      <th>city</th>
      <th>money</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>赵雷</td>
      <td>1990-01-01</td>
      <td>男</td>
      <td>北京</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>钱电</td>
      <td>1990-12-21</td>
      <td>男</td>
      <td>天津</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>孙风</td>
      <td>1990-12-20</td>
      <td>男</td>
      <td>成都</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>李云</td>
      <td>1990-12-06</td>
      <td>男</td>
      <td>北京</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>周梅</td>
      <td>1991-12-01</td>
      <td>女</td>
      <td>成都</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>吴兰</td>
      <td>1992-01-01</td>
      <td>女</td>
      <td>北京</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>郑竹</td>
      <td>1989-01-01</td>
      <td>女</td>
      <td>成都</td>
      <td>200.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>张三</td>
      <td>2017-12-20</td>
      <td>女</td>
      <td>天津</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>李四</td>
      <td>2017-12-25</td>
      <td>女</td>
      <td>西安</td>
      <td>35.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>李四</td>
      <td>2012-06-06</td>
      <td>女</td>
      <td>北京</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>11</td>
      <td>赵六</td>
      <td>2013-06-13</td>
      <td>女</td>
      <td>成都</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12</td>
      <td>孙七</td>
      <td>2014-06-01</td>
      <td>女</td>
      <td>天津</td>
      <td>210.0</td>
    </tr>
  </tbody>
</table>
</div>



**SQL 常用句式**

```sql
SELECT DISTINCT [字段] 
FROM [表] JOIN [bin] ON [连接条件] 
WHERE [过滤条件] 
GROUP BY [字段] 
HAVING [条件] 
ORDER BY [字段] DESC 
LIMIT [个数] OFFSET [个数]
```

## SELECT

**SQL**
```sql
SELECT * FROM student

SELECT id, name, sex FROM student 
```
 
**Pandas**
```python
df

df[['id','name','sex']]
```

### DISTINCT

**SQL**
```sql
SELECT DISTINCT name FROM student 
```
 
**Pandas**
```python
df['name'].unique()
```

### WHERE

**例子: =**

**SQL**
```sql
SELECT * FROM student WHERE sex = '男'
```
 
**Pandas**
```python
df[df['sex'] == ('男')]
```

**例子: in & not in**

**SQL**
```sql
SELECT * FROM student WHERE id IN (2,4,6,8,10)

SELECT * FROM student WHERE id NOT IN (2,4,6,8,10)
```
 
**Pandas**
```python
df[df['id'].isin((2,4,6,8))]

df[~df['id'].isin((2,4,6,8))]
```

**多个条件**

**SQL**
```sql
SELECT * FROM student WHERE sex = '男' and id IN (2,4,6,8,10)
```
 
**Pandas**
```python
df[(df['sex'] == ('男')) & (df['id'].isin((2,4,6,8)))]
```

### LIMIT

**SQL**
```sql
SELECT * FROM student limit 3
```
 
**Pandas**
```python
df.head(3)
```

### SELECT & WHERE & LIMIT

**SQL**
```sql
SELECT * FROM student WHERE sex = '男' LIMIT 3

SELECT id, name, sex FROM student WHERE sex ='男' LIMIT 3
```
 
**Pandas**
```python
df[df['sex'] == ('男')].head(3)

df[df['sex'] == ('男')][['id','name','sex']].head(3)
```

### ORDER BY

**SQL**
```sql
SELECT * FROM student ORDER BY age

SELECT * FROM student ORDER BY age DESC
```
 
**Pandas**
```python
df.sort_values('age')

df.sort_values('age', ascending=False)
```

### GROUP BY 

**GROYP BY & COUNT**

**SQL**
```sql
SELECT city, COUNT(*) FROM student GROUP BY city
```
 
**Pandas**
```python
df.groupby(['city']).size().to_frame('size').reset_index()
```

**GROYP BY & SUM**

**SQL**
```sql
SELECT city, SUM(money) FROM student GROUP BY city
```
 
**Pandas**
```python
df.groupby(['city'])['money'].agg('sum').reset_index()
```

### GROUP BY & ORDER BY & COUNT

**GROUP BY 单字段**

**SQL**
```sql
SELECT city, COUNT(*) FROM student GROUP BY sex ORDER BY city
```
 
**Pandas**
```python
df.groupby(['city']).size().to_frame('size').reset_index().sort_values('city')
```

**GROUP BY 多字段**

**SQL**
```sql
SELECT city, sex, COUNT(*) FROM student GROUP BY city, sex ORDER BY city
```
 
**Pandas**
```python
df.groupby(['city','sex']).size().to_frame('size').reset_index().sort_values('city')
```

### HAVING

**SQL**
```sql
SELECT city, COUNT(*) FROM student GROUP BY city HAVING count(*) > 3
```
 
**Pandas**
```python
df.groupby('city').filter(lambda x:len(x)>3).groupby('city').size().to_frame('size').reset_index()
```


```python
df.groupby('city').filter(lambda x:len(x)>3).groupby('city').size().to_frame('size').reset_index()
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
      <th>city</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>北京</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>成都</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.groupby(['city']).size().to_frame('size').reset_index()
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
      <th>city</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>北京</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>天津</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>成都</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>西安</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
