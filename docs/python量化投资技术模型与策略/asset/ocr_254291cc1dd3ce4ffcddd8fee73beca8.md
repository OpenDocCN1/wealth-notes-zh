```
data[i][2], # high
 data[i][3], # low
 data[i][4]  # close
]
)
volume.append(
    [
    data[i][0], # the date
    data[i][5]  # the volume
    ]
)

options = {
    'rangeSelector': {
        'selected': 1
    },
    'title': {
        'text': 'AAPL Historical'
    },
    'yAxis': [{
        'labels': {
            'align': 'right',
            'x': -3
        },
        'title': {
            'text': 'OHLC'
        },
        'height': '60%',
        'lineWidth': 2
    }, {
        'labels': {
            'align': 'right',
            'x': -3
        },
        'title': {
            'text': 'Volume'
        },
        'top': '65%',
        'height': '35%',
        'offset': 0,
        'lineWidth': 2
    }],
}

H.add_data_set(ohlc, 'candlestick', 'AAPL', dataGrouping = {
```