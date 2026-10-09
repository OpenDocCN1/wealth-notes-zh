```
```python
    ''' 准备 (滞后) 特征数据
    '''

    self.data['r'] = np.log(self.data / self.data.shift(1))
    self.data.dropna(inplace=True)
    self.data['s'] = self.data[self.symbol].rolling(self.window).mean()
    self.data['m'] = self.data['r'].rolling(self.window).mean()
    self.data['v'] = self.data['r'].rolling(self.window).std()
    self.data.dropna(inplace=True)
    self.data_ = (self.data - self.env.mu) / self.env.std


    def _resample_data(self):
        ''' 对数据进行重采样以适配交易区间长度
        '''

        self.data = self.tick_data.resample(self.granularity,
                                            label='right').last().ffill().iloc[:-1]
        self.data = pd.DataFrame(self.data['mid'])
        self.data.columns = [self.symbol]
        self.data.index = self.data.index.tz_localize(None)


    def _get_state(self):
        ''' 返回金融市场的 (当前) 状态
        '''

        state = self.data[self.env.features].iloc[-self.env.lags:]
        return np.reshape(state.values, [1, self.env.lags * self.env.n_features])


    def report_trade(self, time, side, order):
        ''' 返回交易和订单明细
        '''

        self.trades += 1
        pl = float(order['pl'])
        self.pl.append(pl)
        cpl = sum(self.pl)
        print('\n' + 71 * '-')
        print(f'{time} | *** GOING {side} ({self.trades}) ***')
        print(f'{time} | PROFIT/LOSS={pl:.2f} | CUMULATIVE={cpl:.2f}')
        print(71 * '-')
        if self.verbose:
            pprint(order)
            print(71 * '-')


    def on_success(self, time, bid, ask):
        ''' 包含主要交易逻辑
        '''

        df = pd.DataFrame({'ask': ask, 'bid': bid, 'mid': (bid + ask) / 2},
                          index=[pd.Timestamp(time)])
        self.tick_data = self.tick_data.append(df)
        self._resample_data()
        if len(self.data) > self.min_length:
            self.min_length += 1
            self._prepare_data()
            state = self._get_state()

            prediction = np.argmax(self.agent.model.predict(state)[:, 0])
            position = 1 if prediction == 1 else -1
            if self.position in [0, -1] and position == 1:
                order = self.create_order(self.symbol,
                                          units=(1 - self.position) * self.units,
                                          sl_distance=self.sl_distance,
                                          tsl_distance=self.tsl_distance,
                                          tp_price=self.tp_price,
                                          ```
```