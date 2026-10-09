c['twitter']['api_secret_key']),
retry=True)

```
In [41]: l = t.statuses.home_timeline(count=5)
```

```
In [42]: for e in l:
    print(e['text'])

The Bank of England is effectively subsidizing polluting industries in its pandemic rescue program, a think tank sa...
Coo1 shared task: mining scientific contributions (by SeeTedTalk @SoerenAuer and Jennifer D'Souza)
Twelve people were hospitalized in Wyoming on Monday after a hot air balloon crash, officials said.

Three hot air...
Company announcement: Revolot launches Open Banking for its 400,000 Italian...
#fintech
```

```
In [43]: l = t.statuses.user_timeline(screen_name='dyjh', count=5)
```

```
In [44]: for e in l:
    print(e['text'])

#Python for #AlgoTrading (focus on the process) &amp; #AI in #Finance (focus on prediction methods) will complement eac...
Currently putting finishing touches on #AI in #Finance (@ReillyMedia). Book going into production shortly.
Chinatown Is Coming Back, One Noodle at a Time
Alt data industry balloons as hedge funds strive for edge via @FT |
"We remain of the view that alternative d...
He just follow me on Twitter (or LinkedIn). Then you will notice for sure when it is out.
```

+   1. 连接到 Twitter API。
2. 从我的主时间线检索并打印 5 条（最近的）推文。
3. 从用户时间线检索并打印 5 条（最近的）推文。

Twitter API 还允许搜索，根据搜索可以检索和处理最近的推文。

```
In [45]: d = t.search.tweets(q='#Python', count=7)
In [46]: for e in d['statuses']:
    print(e['text'])

RT @kirkDBorne: #AI is Reshaping Programming - Tips on How to Stay on Top:
Courses:
1: #MachineLearning - Jupyte...
RT @reuvenlerner: Today, a #Python student's code didn't print:

x = 5
if x == 5:
    print('yes!')
```