
## Graphical Displays

Categorical variable

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_36_0.png)

(a) Bar Chart (条形图)

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_36_1.png)

(b) Pie Chart (饼图)

## Graphical Displays

Quantitative Variable

- Dot plot (打点图) and Histogram(直方图)

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_38_0.png)

(a) Dot Plot

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_38_1.png)

(b) Histogram

- Histogram can also be used to show relative/percent frequency
- Stem-and-Leaf Display(叶茎图)

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_38_2.png)

## Tabular Display

Two Variables

- Crosstabulation (交叉频数表)

- Example: the number of Finger Lakes homes sold for each style and price for the past two years is shown below.

| Price Range | Home Style |  |  |  | Total |
|---|---|---|---|---|---|
|  | Colonial | Log | Split | A-Frame |  |
| < $250,000 | 18 | 6 | 19 | 12 | 55 |
| ≥ $250,000 | 12 | 14 | 16 | 3 | 45 |
| Total | 30 | 20 | 35 | 15 | 100 |

- Remark: When the crosstabulation involves aggregated data, we should investigate whether a hidden variable could affect the results. (Like Simpson's paradox)

## Summary: Tables & Graphs

Data
Categorical Data
Quantitative Data
Categorical Data
Tabular Displays
Graphical Displays
Tabular Displays
Graphical Displays
• Frequency Dist.
• Rel. Freq. Dist.
• % Freq. Dist.
• Crosstabulation
• Bar Chart
• Pie Chart
• Side-by-Side Bar Chart
• Stacked Bar Chart
• Frequency Dist.
• Rel. Freq. Dist.
• % Freq. Dist.
• Cum. Freq. Dist.
• Cum. Rel. Freq. Dist.
• Cum. % Freq. Dist.
• Crosstabulation
• Dot Plot
• Histogram
• Stem-and-Leaf Display
• Scatter Diagram

## Measures of Location

• **Example:** Seventy efficiency apartments were *randomly sampled* in a college town. The monthly rents are listed below.

| Monthly rents |
|---|
| 525 530 535 535 535 535 535 535 540 540 |
| 540 540 540 545 545 545 545 545 560 560 |
| 550 550 550 550 550 550 550 560 565 565 |
| 565 570 570 572 575 575 575 580 580 580 |
| 580 585 590 590 590 600 600 600 600 600 |
| 610 615 625 625 625 635 640 645 670 670 |
| 675 675 680 680 700 700 700 700 715 715 |

• [How you summarize the above data?](#)

- Mean(均值): \(\bar{x}=\frac{\sum x_i}{n}=\frac{41356}{70}=590.80\)
- Median(中位数): \((575+575)/2=575\)
- Mode(众数): 550
- Geometric mean(几何均值): \(\bar{x}_g=\sqrt[n]{x_1x_2\cdots x_n}\)
- Weighted mean(加权均值): \(\bar{x}_w=\frac{\sum w_i x_i}{\sum w_i}\)
- Trimmed mean(截断均值): the mean of data deleting a percentage of the smallest and largest values

## Measures of Location

- Example: Sample Data

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_43_0.png)

- Percentiles (百位位数): the pth percentile means approximately p% of the observations are less than it.

  - Location of the pth percentile: L_p = p/100 (n + 1) = 80/100 (70 + 1) = 56.8

  - 80th percentile = 635 + 0.8(649 − 635) = 646.2

- Quartiles (四分位数): Q1 = 25th percentiles, Q2 = 50th percentiles = median, Q3 = 75th percentiles

  - L_p = 53.25, Q3 = 625 + 0.25(625 − 625) = 625

注意这里，原来图件有错误，末0.25是末0.75

## Measures of Variability

- Example: Sample Data

| 625 | 630 | 530 | 535 | 535 | 535 | 535 | 535 | 540 | 540 |
| 540 | 540 | 540 | 545 | 545 | 545 | 545 | 545 | 550 | 550 |
| 550 | 550 | 550 | 550 | 550 | 580 | 580 | 580 | 565 | 565 |
| 565 | 570 | 570 | 572 | 575 | 575 | 575 | 580 | 580 | 580 |
| 580 | 585 | 590 | 590 | 590 | 600 | 600 | 600 | 600 | 610 |
| 610 | 615 | 625 | 625 | 625 | 635 | 649 | 650 | 670 | 670 |
| 675 | 675 | 680 | 690 | 700 | 700 | 700 | 700 | 715 | 715 |

- Range: largest value - smallest value = 715 − 525 = 190

- Interquartile (四分位) range (IQR): Q₃ − Q₁ = 80

- Variance: s² = Σ(xᵢ - x̄)² / (n-1) = 2996.16

- Standard Deviation: s = √s² = 54.74

- Coefficient of Variation: [s / x̄ × 100] % = 9.27%

## Measures of Distribution Shape

- **Skewness (偏度：衡量变量抽样概率分布形状对称性)**

$$=\frac{n}{(n-1)(n-2)}\sum\left[\frac{x_i-\bar{x}}{s}\right]^3$$

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_45_0.png)

- **Example:** Apartment Rents

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_45_1.png)
