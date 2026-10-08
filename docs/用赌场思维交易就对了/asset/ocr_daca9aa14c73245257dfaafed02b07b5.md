運用CQG語言編寫RSI趨勢系統的方法如下：

```
Long Entry:
RSI(@,9)[-1] > 65
Short Entry:
RSI(@,9)[-1] < 35
Long Exit:
LoLevel(@,3)[-1]
Short Exit:
HiLevel(@,3)[-1]
```

最後，繼續採用前述RSI 趨勢系統，但採用更緊密的停止策略，停止設定在前一期（而不是前 3 期）高價或低價位置。如此一來，部位平均持有期間將由原來的 6 天縮短爲 3 天（請參考圖 3.5）。

運用CQG語言編寫RSI短期趨勢系統的方法如下：

```
Long Entry:
RSI(@,9)[-1] > 65
Short Entry:
RSI(@,9)[-1] < 35
Long Exit:
LoLevel(@,1)[-1]
Short Exit:
HiLevel(@,1) [-1]
```