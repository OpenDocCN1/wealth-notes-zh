# 清空欄列

```
For i = 1 To NoRecords
    TradeResult(i) = Empty
    EquityCurve(i) = 0
Next i
```

# 開始模擬破產機率

```
Number_Of_Trades = 1
Account_Balance = Account_Start
Account_New_High = Account_Start
Account_DrawDown_Percent = 0
Number_of_Losses_Before_Ruin = 0
Fixed_Dollar_Risk = Account_Start / Unit_Of_Money

i = 1
j = 1
x = 0

Do Until Account_DrawDown_Percent >= Ruin_Point_Drawdown
(i - 1) > 200000000 Or x >= 10000
    Check For New Equity High and reset number of losing tra

    If Account_Balance > Account_New_High Then
        Account_New_High = Account_Balance
        Number_of_Losses_Before_Ruin = 0
        Number_of_Trades_Since_Account_High = 0
    End If

Generate random number to see whether a trade wins or los
    Win_or_Loss = Rnd

Check for a Win
    If Win_or_Loss >= (1 - Accuracy) Then
We have a WIN!
    Calculate the profit
    If Money_Mgt_Approach = "Fixed Percentage Risk Mon
        TradeResult(j) = ((Fixed_Percent_Risked * Acco
            * PayOff_Ratio)
    End If

    If Money_Mgt_Approach = "Fixed Dollar Risk Money Mgt
        TradeResult(j) = Fixed_Dollar_Risk * PayOff_Rati
    End If
```