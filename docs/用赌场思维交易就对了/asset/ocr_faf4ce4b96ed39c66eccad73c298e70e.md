藉由 CQG，一目均衡雲圖穿越系統可以編寫如下：

```
```
Long Entry:
Imoku1(@,9)[-1] > Imoku2(@,26)[-1] AND Close(@)[-1] >
Imoku3(@,52)[-27] AND Close(@)[-1] > Imoku4(@,9,26,Sim,1)
[-27]
Long Exit:
Imoku1(@,9)[-1] < Imoku2 (@,26)[-1] OR Close(@)[-1] <
Imoku3(@,52)[-27] OR Close(@)[-1] < Imoku4(@,9,26,Sim,1)
[-27]
Short Entry:
Imoku1(@,9)[-1] < Imoku2(@,26)[-1] AND Close(@)[-1] <
Imoku3(@,52)[-27] AND Close(@)[-1] < Imoku4(@,9,26,Sim,1)
[-27]
Short Exit:
Imoku1(@,9)[-1] > Imoku2(@,26)[-1] OR Close(@)[-1] >
Imoku3(@,52)[-27] OR Close(@)[-1] > Imoku4(@,9,26,Sim,1)
[-27]
```
```

轉換線 / 基準線穿越系統的總淨獲利雖然較佳，勝率也較高，但就整體績效表現來看，該系統不夠穩健，因為最大耗損（Max Draw）實在太高，而且盈虧比率（P:L）明顯較低（請比較表 8.5 與表 8.6）。箇中關鍵就如同撲克賭局常說的，「重點不是你贏了多少把，而是當你贏的時候，彩頭有多少。」