```
assets = return_table.columns
n_asset = len(assets)
if n_asset < 2:
    weights = np.array([1.])
    weights_dict = {assets[0]: 1.}
else:
    output = describe(return_table, is_print=False)
    covariance_matrix = matrix(output['covariance_matrix'].as_matrix())
    expected_return = output['annualized_return'].iloc[0, :].as_matrix()
    if abs(risk_aversion) < 0.01:
        max_ret = max(expected_return)
        weights = np.array([1. if expected_return[i] == max_ret else 0. for i in range(n_asset)])
        weights_dict = {asset: weights[i] for i, asset in enumerate(assets)}
    else:
        P = risk_aversion * covariance_matrix
        q = matrix(-expected_return.T)
        if allow_short:
            G = matrix(0., (n_asset, n_asset))
        else:
            G = matrix(np.diag(-1 * np.ones(n_asset)))
        h = matrix(0., (n_asset, 1))
        A = matrix(np.ones(n_asset)).T
        b = matrix([1.0])
        solvers.options['show_progress'] = False
        sol = solvers.qp(P, q, G, h, A, b)
        weights = np.array(sol['x'].T)[0]
        weights_dict = dict(zip(assets, weights))

r = np.dot(weights, output['annualized_return'].iloc[0, :].as_matrix())
v = np.sqrt(np.dot(np.dot(weights, covariance_matrix), weights.T))

if show_details:
    print """
Maximum Utility Portfolio:
Risk Aversion: {}
```