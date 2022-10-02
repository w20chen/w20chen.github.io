---
layout:     	post
title: Data Structure in Python
subtitle: 	
date:      	2021-06-28
author:    	Chen
header-img: img/home-bg.jpg
catalog:   true
tags:
    - CS
---
### list
phones = ["apple","huawei","xiaomi"]   #create a list<br>
<br>
phones[0]   #get the value of phones[0]<br>
phones.index("huawei")<br>
phones.count("huawei")<br>
len(phones)<br>
<br>
phones.append("xiaomi")           #add an element at the end of the list<br>
phones.insert(1,"xiaomi")<br>
phones.extend(["oppo","vivo"])    #extend the list<br>
<br>
phones[1]="oppo"   #change the value of phones[1]<br>
<br>
phones.pop()       #delete the last element by default<br>
phones.pop(0)      #delete phones[0]<br>
phones.remove("apple")<br>
phones.clear()     #delete all<br>
del phones[0]<br>
del phones[1:3]    #delete phones[1], phones[2]<br>
phones[-500:]      #last 500 elements in the list<br>
<br>
#reserve a list<br>
phones.reverse()<br>
phones[::-1]<br>
<br>
#sort<br>
phones.sort()<br>
sorted(phones)<br>
<br>
#unpacking<br>
phones=["apple","huawei","xiaomi"]<br>
x, y, z=phones   
#x=='apple', y=='huawei', z=='xiaomi'<br>
#remove duplicates<br>
list_ = list(set(list_))  # the element should be hashable

### tuple
atuple=(1,2,3,4)        #create a tuple<br>
atuple[0]               #get the value<br>
atuple = tuple(alist)   #turn a list into a tuple<br>
#You cannot change a tuple when it has been defined

### dict
<strong>a hash table</strong><br>
#create<br>
**dict_ = {}**     # empty dictionary<br>
**set_ = set()**   # empty set<br>
profile = dict(name = "someone", age = 19)<br>
profile = {"name": "someone", "age": 19}<br>
<br>
profile["name"]   #'someone'<br>
profile["age"]   #19<br>
profile.get("gender","male")   #return "male" if there is no "gender" key<br>
<br>
#add<br>
profile["newKey"]="newKeyVal"<br>
<br>
#alter<br>
profile["name"]="newName"<br>
<br>
#delete<br>
profile.pop("age")<br>
del profile["age"]<br>
<br>
"name" in profile #True<br>
"name" not in profile #False

### set
#create<br>
**dict_ = {}**     # empty dictionary<br>
**set_ = set()**   # empty set<br>
aset = {"huawei","xiaomi"}<br>
#add<br>
aset.add("apple")<br>
#delete<br>
aset.remove("xiaomi")<br>
aset.discard("xiao")      #It's still correct even if this element doesn't exist<br>
aset.pop()                #delete an element by random<br>
aset.clear()              #delete all<br>
<br>
len(aset)<br>
aset.union(bset)          #or aset | bset<br>
aset.difference(bset)     #or aset - bset<br>
aset.intersection(bset)   #or aset & bset<br>
<br>
"huawei" in aset<br>
"huawei" not in aset<br>
<br>
aset.isdisjoint(bset)<br>
aset.issubset(bset)

### stack
FILO (first in last out)
<br>
lst = []<br>
lst.append("a")<br>
lst.append("b")<br>
lst.append("c")<br>
print(lst.pop())<br>
print(lst.pop())<br>
print(lst.pop())

### queue
FIFO<br>
import queue<br>
q = queue.Queue()<br>
q.put("a")<br>
q.put("b")<br>
q.put("c")<br>
print(q.get())<br>
print(q.get())<br>
print(q.get())


### deque
from collections import deque<br>
d = deque()<br>
d.append("a")<br>
d.appendleft("b")<br>
d.append("c")<br>
d.append("d")<br>
d.appendleft("e")<br>
print(d.pop())<br>
print(d.popleft()) <br>
print(d.pop())  <br>
print(d.popleft())<br>
print(d.popleft())

### heap
import heapq<br>
heapq.heappush(heap, x)<br>
heapq.heappop(heap)<br>
heapq.heapify(heap)<br>
heapq.heapreplace(heap, x)<br>
heapq.nlargest(n, iter)<br>
heapq.nsmallest(n, iter)
