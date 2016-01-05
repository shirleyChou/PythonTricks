作者：萧井陌

链接：http://zhuanlan.zhihu.com/xiao-jing-mo/20353331

来源：知乎

---

``` python
# users.json
"""
[   
    {
        "type": "message",
        "user": "UAOI192",
        "text": "你好",
        "ts": "1444916384.000042"
    },
    {
        "type": "message",
        "user": "UAOI102",
        "text": "世界",
        "ts": "1444916420.000044"
    }
]
"""

# messages.json
"""
[
   {
        "id": "UAOI102",
        "name": "bot1",
        "profile": {
            "title": "shui",
            "first_name": "",
            "last_name": "ju",
            "real_name": "ju",
            "email": "abc@qq.com"
        },
    },
    {
        "id": "UAOI192",
        "name": "bot2",
        "profile": {
            "title": "",
            "first_name": "",
            "last_name": "机",
            "real_name": "机",
            "email": "def@gmail.com",
        },
    }
]
"""

# ————————————
# 原始版本
# ————————————
import json
import time
with open('/Users/hehehe/PycharmProjects/chat/users.json', 'r') as f:
    data = json.load(f)
ddic=dict()
for i in range(28):
    ddic[data[i]['id']]= data[i]['name']
for key in ddic.keys():
    print (key,ddic[key])
with open('/Users/hehehe/PycharmProjects/chat/2015-10-15.json', 'r') as f1:
    d2 = json.load(f1)
for i in range(len(d2)):
    d2[i]['user']=ddic[d2[i]['user']]
    print (time.ctime(float(d2[i]['ts'])),':')
    print (d2[i]['user'],':',d2[i]['text'])


# ————————————
# 格式化后的版本
# 请参考 PEP8 编码规范
# ————————————
import json
import time


with open('/Users/hehehe/PycharmProjects/chat/users.json', 'r') as f:
    data = json.load(f)

ddic = dict()

for i in range(28):
    ddic[data[i]['id']] = data[i]['name']

for key in ddic.keys():
    print(key, ddic[key])

with open('/Users/hehehe/PycharmProjects/chat/2015-10-15.json', 'r') as f1:
    d2 = json.load(f1)

for i in range(len(d2)):
    d2[i]['user'] = ddic[d2[i]['user']]
    print(time.ctime(float(d2[i]['ts'])), ':')
    print(d2[i]['user'], ':', d2[i]['text'])


# ————————————
# 取更合适的变量名
# 使用变量简化语句和逻辑，消除重复
# ————————————
import json
import time


user_file = '/Users/hehehe/PycharmProjects/chat/users.json'
with open(user_file, 'r') as f:
    user_dict_list = json.load(f)

username_dict = {}

for i in range(28):
    user = user_dict_list[i]
    user_id = user['id']
    username = user['name']
    username_dict[user_id] = username

for key in username_dict.keys():
    print(key, username_dict[key])

message_file = '/Users/hehehe/PycharmProjects/chat/2015-10-15.json'
with open(message_file, 'r') as f1:
    message_dict_list = json.load(f1)

for i in range(len(message_dict_list)):
    message = message_dict_list[i]
    message['user'] = username_dict[message['user']]
    timestamp = float(message['ts'])
    formatted_time = time.ctime(timestamp)
    print(formatted_time, ':')
    print(message['user'], ':', message['text'])


# ————————————
# 用更合适的库函数和语句
# ————————————
import json
import time


user_file = '/Users/hehehe/PycharmProjects/chat/users.json'
with open(user_file, 'r') as f:
    user_dict_list = json.load(f)

username_dict = {}

# for i in range(28):
    # user = user_dict_list[i]
    # 经过上一步的变量提取，这里的改写只需要做一点微小的改动而不影响后续代码
for user in user_dict_list:
    user_id = user['id']
    username = user['name']
    username_dict[user_id] = username

for key, value in username_dict.items():
    print(key, value)

message_file = '/Users/hehehe/PycharmProjects/chat/2015-10-15.json'
with open(message_file, 'r') as f1:
    message_dict_list = json.load(f1)

# for i in range(len(message_dict_list)):
    # message = message_dict_list[i]
    # 经过上一步的变量提取，这里的改写只需要做一点微小的改动而不影响后续代码
for message in message_dict_list:
    message['user'] = username_dict[message['user']]
    timestamp = float(message['ts'])
    formatted_time = time.ctime(timestamp)
    print(formatted_time, ':')
    print(message['user'], ':', message['text'])


# ————————————
# 使用函数划分逻辑和代码
# ————————————
import json
import time


# 下面这四个注释看上去很奇怪是因为我是先注释了原来的代码功能块再提取为函数的
# load users
def users_dict_from_file(user_file):
    with open(user_file, 'r') as f:
        user_dict_list = json.load(f)
        username_dict = {}
        for user in user_dict_list:
            user_id = user['id']
            username = user['name']
            username_dict[user_id] = username
    return username_dict


# print users
def print_users(users_dict):
    for key, value in users_dict.items():
        print(key, value)


# load messages
def messages_from_file(message_file):
    with open(message_file, 'r') as f:
        message_dict_list = json.load(f)
        return message_dict_list


# print messages
def print_messages(message_dict_list, username_dict):
    for message in message_dict_list:
        username = username_dict[message['user']]
        timestamp = float(message['ts'])
        formatted_time = time.ctime(timestamp)
        print(formatted_time, ':')
        print(username, ':', message['text'])


# run code
user_file = '/Users/hehehe/PycharmProjects/chat/users.json'
message_file = '/Users/hehehe/PycharmProjects/chat/2015-10-15.json'

username_dict = users_dict_from_file(user_file)
print_users(username_dict)
messages = messages_from_file(message_file)
print_messages(messages, username_dict)


# ————————————
# 我写程序的思路
# ————————————

# model classes
class GuaModel(object):
    def __init__(self, json_dict):
        super(Model, self).__init__()
        self._obj_from_json(json_dict)

    def __repr__(self):
        # properties = json.dumps(self.__dict__, sort_keys=True, indent=2)
        # return "<{0}: {1}>".format(self.__class__.__name__, properties)
        class_name = self.__class__.__name__
        properties = ('{0} = {1}'.format(k, v) for k, v in self.__dict__.items())
        return '<{0}: \n  {1}\n>'.format(class_name, '\n  '.join(properties))

    def key_mapper(self):
        """Force subclasses to implement this method"""
        raise NotImplementedError

    def _obj_from_json(self, json_obj):
        """Private method"""
        mapper = self.key_mapper()
        print('mapper', mapper)
        for k, v in mapper.items():
            try:
                paths = v.split('.')
                v = json_obj
                for p in paths:
                    v = v[p]
            except Exception as e:
                v = None
            setattr(self, k, v)


# define models
class User(GuaModel):
    def key_mapper(self):
        mapper = {
            'id': 'id',
            'name': 'name',
            'real_name': 'profile.real_name',
        }
        return mapper


class Message(GuaModel):
    def key_mapper(self):
        mapper = {
            'user_id': 'user',
            'text': 'text',
            'timestamp': 'ts',
        }
        return mapper

    def formatted_time(self):
        timestamp = float(self.timestamp)
        ft = time.ctime(timestamp)
        return ft
        

def users_dict_from_file(user_file):
    with open(user_file, 'r') as f:
        users = [User(u) for u in json.load(f)]

        username_dict = {}
        for u in users:
            username_dict[u.id] = u.name
        return username_dict


# print users
def print_users(users):
    for u in users
        print(u)


# load messages
def messages_from_file(message_file):
    with open(message_file, 'r') as f:
        messages = [Message(m) for m in json.load(f)]
        return messages


# print messages
def print_messages(messages, username_dict):
    for m in messages:
        user = username_dict[m.id]
        content = '{} :\n{} : {}'.format(m.formatted_time(), user.name, m.text)
        print(content)
```

