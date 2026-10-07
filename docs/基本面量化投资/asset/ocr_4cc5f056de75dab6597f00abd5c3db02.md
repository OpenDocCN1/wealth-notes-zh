基本面量化投资

类似地，多因子模型可以写成方差形式为：

\[ \operatorname{Var}(r_p - r_b) = \operatorname{Var}\left(\sum_i w_{p,i} r_i - \sum_i w_{b,i} r_i\right) \]
\[ = \operatorname{Var}\left[\sum_i (w_{p,i} - w_{b,i})(\beta_{i,1} f_1 + \cdots + \beta_{i,k} f_k + \alpha_i)\right] \]
\[ = \left(\sum_i w_{a,i} \beta_{i,1}\right)^2 \operatorname{Var}(f_1) + \cdots + \left(\sum_i w_{a,i} \beta_{i,k}\right)^2 \operatorname{Var}(f_k) + \]
\[ \left(\sum_i w_{a,i}\right)^2 \operatorname{Var}(\alpha_i) + 2 \sum_m \sum_n \left(\sum_i w_{a,i} \beta_{i,m} \sum_j w_{a,j} \beta_{j,n}\right) \operatorname{Cov}(f_m, f_n) \]
（15-54）

式(15-54)中的每一项也可以代入实际值计算，不过由于计算方差需要的样本点较多，因此在实际应用中，通常使用日度因子暴露度和因子收益数据。根据式(15-54)，我们便能够确定主动风险[Var(r_p - r_b)]的来源。例如，归属于因子 f_1 的风险为 \(\left(\sum_i w_{a,i} \beta_{i,1}\right)^2 \operatorname{Var}(f_1)\)，归属于因子 f_k 的风险为 \(\left(\sum_i w_{a,i} \beta_{i,k}\right)^2 \operatorname{Var}(f_k)\)，特殊风险为 \(\left(\sum_i w_{a,i}\right)^2 \operatorname{Var}(\alpha_i)\)。需要指出的是，在上述计算中，我们实际上忽视了因子间的协方差，如果采用上述方法分解风险，还要添加一个考虑因子协方差的调整项①：

\[ 2 \sum_m \sum_n \left(\sum_i w_{a,i} \beta_{i,m} \sum_j w_{a,j} \beta_{j,n}\right) \operatorname{Cov}(f_m, f_n) \]
（15-55）

表 15-1 总结了使用多因子模型进行业绩归因的主要结果。运用因子归因模型，基金经理可以将主动收益和主动风险的来源一一分解，对收益和风险进行更细致的分析，承担希望承担的风险以获取相应的超额收益，同时尽量避免不希望承担的风险。这一过程不仅适用于投资组合，也适用于个股。此外，在基本面因子模型中，由于因子暴露度提前可知，基金经理还可以在个股层面

① Menchero and Davis(2011)指出，这种调整项的方式不利于准确分解因子来源，并提出 x-sigma-rho 方法，将归属于特定因子的风险表述为因子暴露度(x)、因子方差(sigma)、因子和投资组合收益的相关系数(rho)的乘积。当前 Barra 软件中的业绩归因即使用该方法。