```
There was a typo, namely : after pr... 
RT @GavLaaaaaaaa: Javascript Does Not Need a StringBulider
#programing #softwareengineering #bigdata
#datasclence...
RT @codeFlawGo: It is necessary to publish regular updates on Twitter
#programmer #coder #developer #technolpgy RT @paka_ins: Learning to c...
RT @GavLaaaaaaaa: Javascript Does Not Need a StringBuilder
#programing #softwareengineering #bigdata
#datasclence...
```

① 搜索带有 "Python" 标签的推文，并打印出最迟的 5 条。

还可以收集某个 Twitter 用户的大量推文，并以词云的形式创建摘要（参见图 4-3）。下面的 Python 代码再次使用了 NLP 辅助函数，如 4.7 节中的 Python 代码所示。

```
In [47]: l = t.statuses.user_timeline(screen_name='elonmusk', count=50) ①

In [48]: tl = [e['text'] for e in l] ②

In [49]: tl[:5] ③
Out[49]: ['@Finchvvy @Lindworm @cLeanTechnica True',
         '@Lindworm @cLeanTechnica Highly likely down the road',
         '@cLeanTechnica True fact',
         '@NASA spaceflight Scrubbed for the day. A Raptor turbopump spin start valve
         didn't open, triggering an automatic abo...
         '@erdaycastonault I'm in the Boca control room. Hop attempd in ~33 minutes.' ]

In [50]: wc = nlp.generate_word_cloud(' '.join(tl), 35,
             name='../../Images/ch4/04/musk_twitter_wc.png') ④
```

+   ① 检索用户 elonmusk 的最近 50 条推文。
② 收集 list 对象的文本。
③ 显示最后 5 条推文的摘录。
④ 生成词云摘要并显示它。

![](bbox=[0.15, 0.65, 0.85, 0.88])