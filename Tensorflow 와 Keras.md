# Tensorflow 와 Keras 

## Tensorflow

* 텐서플로: 수치연산한 결과를 시각적 그래프로 그리는 라이브러리

* edges와 nodes로 구조화된 graph로 프로그램이 구성

  <img src="C:\Users\student\AppData\Roaming\Typora\typora-user-images\image-20200414093334802.png" alt="image-20200414093334802" style="zoom:%;" />

  * 엣지 (edge) : 
  * 노드 (node) : 하나의 연산, 입력값을 받아 결과값 출력 
  * 세션 (session) : 



```python
import tensorflow as tf

a=tf.placeholder("float") # 엣지
b=tf.placeholder("float") # 엣지

y=tf.multiply(a,b) # 노드 
sess=tf.Session() # 그래프 실행을 위해서는 세션이 필요 
print(sess.run(y,feed_dict={a:3,b:2}))

>>>6.0
```

* 플레이스 홀더 (placeholder)
  프로그램 실행 중에 값을 변경할 수 있는 변수를 2개 정의 
  데이터를 담는 버퍼와 같은 것 

https://www.tensorflow.org/

### Tensorflow의 구조 

* 그래프를 빌드하는 부분 

  ```python
  a=tf.placeholder("float") # 엣지
  b=tf.placeholder("float") # 엣지
  
  y=tf.multiply(a,b) # 노드 
  ```

  

* 그래프를 실행하는 부분 

  ```python
  sess=tf.Session() # 그래프 실행을 위해서는 세션이 필요 
  print(sess.run(y,feed_dict={a:3,b:2}))
  ```

* 텐서플로우 실행 구조 
  
  * Session은 fetch와 feed 2가지 방법으로 처리 

![image-20200414101427793](C:\Users\student\AppData\Roaming\Typora\typora-user-images\image-20200414101427793.png)

### Tensorflow의 차원

```python
hello = tf.constant("Hello") # node 
print( hello )

>>> Tensor("Const:0", shape=(), dtype=string)
```

```python
3 # 랭크 :0 tensor, shape: []
[1,2,3] #랭크 :1, shape: [3]
[[1,2,3],[1,2,3]] #랭크 2, shape: [2,3]
[[[1,2,3],[4,5,6]]] # 랭크: 3, shape: [2,1,3] shape: 각차원의 요소의 갯수가 shape 
```



### 연산방법 

```python
a = tf.constant(10)
b = tf.constant(20)
print(a)
print(b)
sess = tf.Session()
# 1) 방법
# print(sess.run(tf.add(a , b)))
# 2) 방법 
c = tf.add(a,b)
print(sess.run(c))

# *** 항상 session 객체를 소환해야 계산이 된다. 
print(sess.run(a))
print(sess.run(b))
print(sess.run([a,b]))

# *** 사용이 끝나면 close 하는 것이 좋다. 
sess.close()

>>> 
Tensor("Const_41:0", shape=(), dtype=int32)
Tensor("Const_42:0", shape=(), dtype=int32)
30
10
20
[10, 20]
```



```python
a = tf.placeholder(dtype= "float") 
a = tf.placeholder(tf.float32)  # 데이터 타입 실수 지정, 위 와 같은의미 
b = tf.placeholder(tf.float32) 
adderNode = a + b
sess = tf.Session()
print(sess.run(adderNode, feed_dict = {a:5 , b:3}))

>>>
8.0
```

```python
a = tf.placeholder(tf.float32)  # 데이터 타입 실수 지정, 위 와 같은의미 
b = tf.placeholder(tf.float32) 

adderNode = a + b
triple = adderNode*b
sess = tf.Session()
print( sess.run(triple, feed_dict = {a:2, b:3}))

>>>
15.0

```

```python
x = tf.placeholder(tf.float32,[None,3])
# print(x)
xdata=[[1,2,3],[4,5,6]]
# xdata2=[[1,2],[4,5]]
sess.run(x, feed_dict={x:xdata})
# sess.run(x, feed_dict={x:xdata2}) # shape이 맞지 않아서 에러발생 
```



### 가설함수 생성 

````python
# H(x) = wx + b 
# w와 b는 variable 
w = tf.Variable( tf.random_normal([2,1]))
w = tf.Variable([[1],[2]])
# hf=tf.matmul(x,w)+b
````

```python
xdata = [[1,2,3],   #2행 3열 
         [4,5,6]]
x=tf.placeholder(tf.float32, [None,3])

w = tf.Variable(tf.random_normal([3,1])) 
b = tf.Variable(tf.random_normal([1]))

hf = tf.matmul(x,w) + b

sess=tf.Session()
# hf = wx+b 
# x shape => (2,3)
# w shape => (3,1) => (2,1) 정의 해보기 
sess.run(tf.global_variables_initializer())
# sess.run(hf , feed_dict = {x:xdata , w:wdata})
print(sess.run(w))
print("="*30)
print(sess.run(b))
print("="*30)
print(sess.run(hf,feed_dict={x:xdata}))

>>>
[[ 0.18470806]
 [-0.20844084]
 [-0.00717347]]
==============================
[-0.00225501]
==============================
[[-0.25594905]
 [-0.3486677 ]]
```

### tensorflow 기반 선형 회귀모델

```python
# 선형회귀모델 
xtrain = [80,95,97] # 모의고사 점수 (3명)
ytrain = [82,90,98] # 수능 점수 (3명)

#모의고사를 70점 받았는데 수능 몇점나올까요~? 

w = tf.Variable(tf.random_normal([1]))
b = tf.Variable(tf.random_normal([1]))

hf = xtrain*w + b

# ********* 실행시 아래 두문장 필수 작성 ************ 
sess = tf.Session()
sess.run(tf.global_variables_initializer())
# ********* 실행시 위 두문장 필수 작성 ************ 
print(sess.run([w, b, hf]))

>>>
[array([-1.366571], dtype=float32), array([-0.8158702], dtype=float32), array([-110.14155, -130.6401 , -133.37326], dtype=float32)]

```

* 모델 생성 

```python
# 모델 
hf = xtrain * w + b
cost = tf.reduce_mean(tf.square(hf - ytrain)) # (예상값 - 실제값 ) 합의 제곱에 대한 평균 

opt = tf.train.GradientDescentOptimizer(0.01) # alpha 값 
train = opt.minimize(cost) # 경사 하강 알고리즘 

# fit the line 
print(sess.run(cost), sess.run(w), sess.run(b))

sess.run(train)

print(sess.run(cost), sess.run(w), sess.run(b)) # 발산됌 ===> 무한대값 계속 하면  nan이 나옴 
# alpha 값 작게 조정 필요 
```



```python
opt = tf.train.GradientDescentOptimizer(0.0001) # alpha 값 
train = opt.minimize(cost) # 경사 하강 알고리즘 

print(sess.run(cost), sess.run(w), sess.run(b)) # => 수렴하는 과정 볼 수 있음 
>>> 7392.3706 [0.04258462] [0.38667476]

sess.run(train)
print(sess.run(cost), sess.run(w), sess.run(b))
>>> 10.858798 [0.9729772] [0.39711803]

```



```python
for step in range(2001):
    sess.run(train)
    if step % 100 == 0:
        print(step, sess.run(cost), sess.run(w),sess.run(b))
        
>>> 0 19951.32 [2.5517848] [-0.77292657]
100 9.569884 [0.99984413] [-0.7872302]
200 9.569175 [0.9998146] [-0.78453606]
300 9.568453 [0.99978507] [-0.78184193]
400 9.567703 [0.9997556] [-0.7791478]
500 9.566993 [0.99972624] [-0.7764537]
600 9.56627 [0.99969673] [-0.77375954]
700 9.565524 [0.9996672] [-0.7710654]
800 9.564814 [0.9996378] [-0.76837295]
900 9.564097 [0.99960816] [-0.7656832]
1000 9.56337 [0.99957883] [-0.762995]
1100 9.562642 [0.9995493] [-0.76030684]
1200 9.561919 [0.9995199] [-0.75761867]
1300 9.561206 [0.99949056] [-0.7549305]
1400 9.560475 [0.99946094] [-0.7522423]
1500 9.559758 [0.99943155] [-0.74955416]
1600 9.5590315 [0.9994022] [-0.746866]
1700 9.558315 [0.9993726] [-0.7441778]
1800 9.557574 [0.9993431] [-0.74148965]
1900 9.556858 [0.9993139] [-0.7388015]
2000 9.556142 [0.99928427] [-0.7361133]
```

```python
# 모의고사 점수가 50점 => 수능점수 ? 
# hf = xtrain * w + b 
yhat = sess.run(w)[0] * 50 + sess.run(b)[0]
print("예상되는 수능 점수는 "+ str(yhat.round(2)) +" 점 입니다.")

>>> 예상되는 수능 점수는 49.23 점 입니다.
```

## 연습문제 

car 데이터에서 

임의의 실린더수 (10,12,16) 가 입력 => hp? 

cost, w, b 출력 

cost 함수 시각화 (w) 축 

```python
# 데이터 읽기 
path = "C:\\Users\\student\\Desktop\\DY\\★ 데이터\\306. carsdata"
cars=pd.read_csv(path + "\\cars.csv")

# 사용할 데이터 칼럼 및 xtrain , ytrain
data=cars[[' cylinders',' hp']]
xtrain = list(data[' cylinders'])
ytrain = list(data[' hp'])

# w, b random 변수 설정 
import tensorflow as tf

w = tf.Variable(tf.random_normal([1]))
b = tf.Variable(tf.random_normal([1]))

# 가설함수 설정 
hf = xtrain*w + b

sess = tf.Session()
sess.run(tf.global_variables_initializer())

cost = tf.reduce_mean(tf.square(hf - ytrain)) # (예상값 - 실제값 ) 합의 제곱에 대한 평균 

opt = tf.train.GradientDescentOptimizer(0.0001) # alpha 값 
train = opt.minimize(cost) # 경사 하강 알고리즘 

# cost , w, b 출력 
sess.run(train)
print(sess.run(cost), sess.run(w), sess.run(b))

>>472.08432 [18.480501] [3.7122276] 

# 임의의 실린더 값 입력했을 때 예측값 출력 
x=float(input())
yhat = sess.run(w)[0] * x  + sess.run(b)[0]

print("입력된 실린더 수는"+str(x)+" 입니다\n"+ "예상되는 hp는 "+ str(yhat.round(2)) +" 입니다.")

>>10
입력된 실린더 수는10.0 입니다
예상되는 hp는 188.63 입니다.


# cost함수 그래프 시각화 
xtrain = list(data[' cylinders'])
ytrain = list(data[' hp'])

W = tf.placeholder(tf.float32)

# 선형모델의 가설
hypothesis = xtrain * W

# 비용함수 
cost = tf.reduce_mean(tf.square(hypothesis - ytrain))

# session 설정 
sess = tf.Session()

# 비용함수 점찍기위한 함수 
W_history = []
cost_history = []

for i in range(-30, 70):
    curr_W = i
    curr_cost = sess.run(cost, feed_dict={W: curr_W})
    W_history.append(curr_W)
    cost_history.append(curr_cost)

# Show the cost function
plt.plot(W_history, cost_history)
plt.show()
```

![img](data:image/png;base64,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)







-------- (4/14) ----------------------------------------------------------------------------------

