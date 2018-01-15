
#  Algorithm


### Constant
- 값에 대한 키 또는 인덱스를 알고 있을 경우 
- minsu_exam_result = {"kor":95, "math":40}


### O(log n) : logarithmic 
- 배열에서 값을 접근할 때 앞 또는 뒤에서 접근 선택이 가능할 경우
 animals = ['cat' , 'dog' , 'fox' , 'giraffe' , 'hippo' , 'koyote' , ..] 

### O(n): linear 
- 자료의 수와 시도횟수가 1대1 관계인 경우 
``` javascript
result = 0 
for i in range(1,100+1): result += i print(result)
```
### O(n^2 ): quadratic
 자료의 참조를 이중으로 하게 될 경우. 
 ``` javascript
result = 0 
for i in range(1,10+1): for j in range(1,j+1):
 result += j		
```
### bubble sort
<iframe width="560" height="315" src="https://www.youtube.com/embed/Cq7SMsQBEUw" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>  
    
 가장 안좋다. 양쪽에 최소 최대 값을 정렬
### selection sort
<iframe width="560" height="315" src="https://www.youtube.com/embed/92BfuxHn2XE" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

 제일 적은 값을 찾아 일일이 전부 정렬한다.
### insertion sort
<iframe width="560" height="315" src="https://www.youtube.com/embed/8oJS1BMKE64" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

정렬하면서 앞으로 나아간다.

### merge sort
<iframe width="560" height="315" src="https://www.youtube.com/embed/ZRPoEKHXTJg" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

![](https://upload.wikimedia.org/wikipedia/commons/c/cc/Merge-sort-example-300px.gif)
 두개씩 비교해 정렬한다.
### heap sort
<iframe width="560" height="315" src="https://www.youtube.com/embed/_bkow6IykGM" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

![](https://upload.wikimedia.org/wikipedia/commons/4/4d/Heapsort-example.gif)
 데이터를 힙에 넣고 최대값부터 출력한다.
### Quick sort !!!
<iframe width="560" height="315" src="https://www.youtube.com/embed/8hEyhs3OV1w" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

![](https://upload.wikimedia.org/wikipedia/commons/9/9c/Quicksort-example.gif)
피벗을 이용해서 정렬함. 평균적으로 가장 빠르다.


 
# Data Structure
데이터를 잘 정렬해서 사용자가 효율적을으로 사용하게끔 하는것


### Tree -Document Object Model
 트리의 자식이 많으면 많을수록 렌더링이 늦어진다.
### Stack !!!
데이터를 두가지 기능으로 추상화시키는것. **Last In, First Out**
- push: 순서대로 넣는다.
- pop: 최근에 넣은 것 부터 빼낸다.


### Queue !!!
들어가는 순서대로 나온다. **First In First Out**
- Enqueue: 순서대로 넣는것
- Dequeue: 순서대로 뺴는것 

### Linked List
다음 데이터를 가르키는 연속적인 주소들로 이루어져 있다.

### Tree
 상하구조가 뚜렷한 (hierarchical) 자료구조를 표현한다. 
 - 메인트리: 전체
 - 엣지: 사이사이의 선
 - 서브트리: 메인트리에 속해있는 작은트리?

