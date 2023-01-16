##### 1. operations on strings.

###### 1.1  concatenate.

```python 
a = 'hello'
b = 'world'
c = 123

d = a + b                    # 'helloworld'
e = a + b + str(c)           # 'helloworld123'
```

###### 1.2  length.

```python
len(a)              # 5
len('这是中文测试')   # 6
```

###### 1.3  sub string.

```python
str = 'hello,world'
str[0]       # 'h'
str[1:3]     # 'el'
str[:4]      # 'hell'
```

###### 1.4  split.

```python
'1 2 3 4 5'.split()        # ['1', '2', '3', '4', '5']
'1,2,3,4,5'.split(',')     # ['1', '2', '3', '4', '5']
```

###### 1.5  join.

```python
'#'.join(['1', '2', '3', '4', '5'])         # '1#2#3#4#5'
```

###### 1.6  count, find, index, rindex.

```python
str = 'miku'
num = str.count('m')       # 1
num = str.count('mi')      # 1
num = str.count('a')       # 0

pos = str.find('m')        # 0
pos = str.find('mi')       # 0
pos = str.find('a')        # -1
pos = str.find('mi', 2)    # search begin from pos 2, of course, no result.

index = str.index('m')     # 0
index = str.index('mi')    # 0
index = str.index('a')     # ValueError: substring not found

index = str.rindex('k')    # 2
```

###### 1.7  startswith, endswith.

```python
str.startswith('mi')      # True
str.endswith('ku')        # True
```

###### 1.8  strip.

```python
'   hello   '.strip()       # 'hello'
'###hello###'.strip('#')    # 'hello'
'###hello###'.lstrip('#')   # 'hello###'
'###hello###'.rstrip('#')   # '###hello'
```

###### 1.9  format.

```python
"This is %s's number %d" % ('yang', 39)               # "This is yang's number 39"
'This is %s\'s number %d' % ('yang', 39)              # "This is yang's number 39"
'This is {}\'s number {}'.format('yang', 39)          # "This is yang's number 39"

'{:,.2f}'.format(312397.2495)   # Separate with commas and keep 2 decimal places
```

###### 1.10  encode, decode.

```python
str = '这是中文'
str.encode()          # b'\xe8\xbf\x99\xe6\x98\xaf\xe4\xb8\xad\xe6\x96\x87', default is utf8.
str.encode('utf8')    # b'\xe8\xbf\x99\xe6\x98\xaf\xe4\xb8\xad\xe6\x96\x87'
str.encode('gbk')     # b'\xd5\xe2\xca\xc7\xd6\xd0\xce\xc4'

len(str.encode('utf8'))    # 12
len(str.encode('gbk'))     # 8

str.encode('utf8').decode('utf8')    # '这是中文'
str.encode('gbk').decode('gbk')      # '这是中文'
str.encode('gbk').decode('utf8')     # UnicodeDecodeError: 'utf-8' codec can't decode byte 0xd5 in position 0: invalid continuation byte
```

###### 1.11  regex.

```python
import re

str = '123456'
other = '12345'

re.match(r'\d{5}', str)       # <re.Match object; span=(0, 5), match='12345'>
re.match(r'\d{6}', str)       # <re.Match object; span=(0, 6), match='123456'>

re.match(r'\d{5}$', str)      # no output.
re.match(r'\d{5}$', other)    # <re.Match object; span=(0, 5), match='12345'>
re.match(r'\d{6}$', str)      # <re.Match object; span=(0, 6), match='123456'>

is_phone_number = '13856724133'
re.match(r'1[^012]\d{9}$', is_phone_number)  # <re.Match object; span=(0, 11), match='13856724133'>
```



##### 2.  operations on files.

###### 2.1  read only mode.

```python
file = open('draft.txt', 'r')
print(file.readable())        # True
print(file.writable())        # False
file.close()
```

###### 2.2  read only mode, open file in utf8.

```python
import codecs

file = codecs.open('draft.txt', 'r', 'utf8')
file.readlines()
file.close()

# or you could write like this, file will be closed automatically:
with open('draft.txt', 'r', encoding='utf8') as file:
    file.readlines()
```

###### 2.3  append to files.

```python
with open('draft.txt', 'a+', encoding='utf8') as file:
    data = ['测试utf8，汉语保存']
    file.writelines(data)
```

###### 2.4  file info.

```python
import os
import time

def timestamp_format(timestamp):
    lt = time.localtime(timestamp)
    return time.strftime("%Y-%m-%d %H:%M:%S", lt);

file = 'draft.txt'
os.path.exists(file)    
os.path.isabs(file)     
timestamp_format(os.path.getatime(file))    # access time.
timestamp_format(os.path.getctime(file))    # create time.
timestamp_format(os.path.getmtime(file))    # mofify time.
os.path.getsize(file)                       # unit: bytes.
os.path.isdir(file)    
os.path.isfile(file)   
```



##### 3. operations on directory.

###### 3.1  get/change current path.

```python
import os

os.getcwd()         
os.chdir('D:/')     # change current path into D:/
```

###### 3.2  create directory.

```python
import os

dir_name = 'E:/this_is_a_test'

if not os.path.exists(dir_name):
    os.mkdir(dir_name)
else:
    os.rmdir(dir_name)
```

###### 3.3  delete a directory recursively.

```python
import shutil

shutil.rmtree(dir_name)
```

###### 3.3  traverse.

```python
imort os

dir_name = 'E:/hatsune miku'

# way 1: os.walk()
for root, dirs, files in os.walk(dir_name):
    for name in dirs:
        print(os.path.join(root, name))
    for name in files:
        print(os.path.join(root, name))
        
# way 2: os.scandir()
def traverse(path_name):
    for item in os.scandir(path_name):
        if item.is_dir():
            traverse(item.path)
        else:
            print(item.path)
            
if __name__ == '__main__':
    traverse('E:/hatsune miku')
```



##### 4.  operations on database. 

```python
# This example uses mysql, so you should install pymysql first.
import pymysql

conn = pymysql.connect(host='localhost', port=3306, user='root', password='193168', database='emp')
cursor = conn.cursor()

sql = 'CREATE TABLE `test_user`(id INT PRIMARY KEY AUTO_INCREMENT, name CHAR(20) NOT NULL)'
res = cursor.execute(sql)
print(res)
sql = 'DROP TABLE IF EXISTS `test_user`'
res = cursor.execute(sql)
print(res)

cursor.close()
conn.close()
```



##### 5.  socket programming.

###### 5.1  a simple socket communication.

```python
# server.
import socket

address = ('localhost', 8039)
sock = socket.socket()    # default is IPV4，TCP, that is socket.AF_INET, socket.SOCK_STREAM.
sock.bind(address)
sock.listen(5)

conn, client_address = sock.accept()
print(client_address)

data = conn.recv(1024).decode()      # default is utf8.
print(data)
conn.sendall('Hello, This is server'.encode())

conn.close()
sock.close()



# client.
import socket

address = ('localhost', 8039)
sock = socket.socket()
sock.connect(address)

sock.sendall('Hello, This is client'.encode())
data = sock.recv(1024).decode()
print(data)
sock.close()
```

###### 5.2  socketserver.

```python
import socketserver

class MyHandler(socketserver.BaseRequestHandler):
    def handle(self):
        data = self.request.recv(1024).decode()
        print('Message from client %s: %s' % (self.client_address, data))
        self.request.send('server received successfully.'.encode())
        self.request.close()

address = ('localhost', 8039)
server = socketserver.ThreadingTCPServer(address, MyHandler)
print("server start.")
server.serve_forever()
```



##### 6.  multi thread.

###### 6.1  create thread.

```python
import threading

def task(num):
    print('current thread {} is running.'.format(num))
    
for i in range(10):
    t = threading.Thread(target = task, args = (i, ))
    t.start()
```

###### 6.2  thread wait.

```python
import threading
import time

def task():
    print('wait here.')
    time.sleep(3)
    print('reruns.')
    
t = threading.Thread(target = task)
t.start()
print('main thread start.')

# You can force the main thread to wait for the child thread to finish executing, use t.join().
```



##### 7.  random.

```python
import random

random.randint(1, 100)        # numbers in [1, 100].
random.randrange(1, 101, 2)   # odd numbers in [1, 101).
random.randrange(2, 101, 2)   # even numbers in [2, 101).

random.random()               # a random floating-point number in [0.0, 1.0).
```

```python
import random

list = ['a', 'b', 'c', 'd']
random.choice(list)           # randomly fetch an item in the array.
random.shuffle(list)          # randomly shuffle the array order.
random.sample(list, 2)        # randomly fetch 2 items in the array.
random.sample(list, 3)        # randomly fetch 3 items in the array.
```

```python
# generate a random length string.
import string
import random

def gen_random_str(length):
    return random.choices(string.ascii_letters + string.digits, k = length)

str = gen_random_str(10)
```



##### 8.  time library.

```python
import time

time.time()                             # 1673844681.2612522
time.ctime()                            # Mon Jan 16 12:51:21 2023
time.strftime('%Y-%m-%d %H:%M:%S')      # 2023-01-16 12:51:21
```

```python
import time

st = time.localtime()

# time.struct_time(tm_year=2023, tm_mon=1, tm_mday=16, tm_hour=12, tm_min=53, tm_sec=40, tm_wday=0, tm_yday=16, tm_isdst=0)
```

```python
import time

time.sleep(1.39)  
```

```python
import time

time.mktime(time.localtime())                             # 1673844890.0

time.strftime('%Y-%m-%d %H:%M:%S', time.localtime())      # 2023-01-16 12:55:05

str = '2022-09-18 12:58:39'
time.strptime(str, '%Y-%m-%d %H:%M:%S')                   # time.struct_time(tm_year=2022, tm_mon=9, tm_mday=18, tm_hour=12, tm_min=58, tm_sec=39, tm_wday=6, tm_yday=261, tm_isdst=-1)
```



##### 9.  datetime library.

```python
import datetime
import os

# only year-month-day.
datetime.date.today()        # datetime.date(2023, 1, 16) 

# get a file's access time.
file = 'draft.txt'
atime = os.path.getatime(file)
d = datetime.datetime.fromtimestamp(atime)
print('{:%Y-%m-%d %H:%M:%S}'.format(d))
```



##### 10.  logging library.

```python
import logging

logging.basicConfig(format = '%(asctime)s | %(filename)s | %(lineno)s | %(message)s', level = logging.DEBUG)

name = 'abc'
number = 39

logging.debug('logging msg : {}'.format(name))
logging.info('logging msg : {}'.format(number))
```

