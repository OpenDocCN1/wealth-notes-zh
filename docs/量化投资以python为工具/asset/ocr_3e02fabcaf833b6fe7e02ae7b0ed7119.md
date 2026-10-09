```
...		elif signal[i]==3:
...				position[i]=0
...			
elif signal[i]==-3:
...				position[i]=0
...
```

```
In [40]: position=pd.Series(position, index=CoSpreadT.index)
In [41]: position.tail()
Out[42]:
Trddt
2015-06-24    0
2015-06-25    0
2015-06-26    0
2015-06-29    0
2015-06-30    0
dtype: float64
```

```
In [53]: def TradeSim(priceX, priceY, position):
...: 			n=len(position)
...: 				size=1000
...: 				shareY=size*position
...: 				shareX=[(-beta)*shareY[0]+priceY[0]/priceX[0]]
...: 				cash=[2000]
...: 				for i in range(1,n):
...: 					shareX.append(shareX[i-1])
...: 					cash.append(cash[i-1])
...: 					if position[i-1]==0 and position[i]==1:
...: 						shareX[i]=(-beta)*shareY[i]+priceY[i]/priceX[i]
...: 						cash[i]=cash[i-1]-(shareY[i]*priceY[i]+shareX[i]*priceX[i])
...: 					elif position[i-1]==0 and position[i]==-1:
...: 						shareX[i]=(-beta)*shareY[i]+priceY[i]/priceX[i]
...: 						cash[i]=cash[i-1]-(shareY[i]*priceY[i]+shareX[i]*priceX[i])
...: 					elif position[i-1]==1 and position[i]==0:
...: 						shareX[i]=0
...: 						cash[i]=cash[i-1]+(shareY[i-1]*priceY[i]+shareX[i-1]*priceX[i])
...: 					elif position[i-1]==-1 and position[i]==0:
...: 						shareX[i]=0
...: 						cash[i]=cash[i-1]+(shareY[i-1]*priceY[i]+shareX[i-1]*priceX[i])
...: 				cash = pd.Series(cash, index=position.index
...: 				shareY=pd.Series(shareY, index=position.index)
...: 				shareX=pd.Series(shareX, index=position.index)
...: 				asset=cash+shareY*priceY+shareX*priceX
...: 				account=pd.DataFrame({'Position':position,'ShareY':shareY, 'ShareX':shareX,
...: 							'Cash':cash,'Asset':asset})
...: 				return(account)
...:
```

```
In [54]: account=TradeSim(PLICCT, SinopecT, position)

In [55]: account.tail()
Out[55]:

								Asset		Cash	Position	ShareX	ShareY
Trddt
2015-06-24		5992.514	5992.514		0				0		0
```