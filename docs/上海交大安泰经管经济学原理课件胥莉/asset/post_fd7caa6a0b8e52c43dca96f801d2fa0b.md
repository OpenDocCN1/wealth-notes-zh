
## 沉没成本的无关性

- **沉没成本：** 已经发生而且无法收回的成本
- 沉没成本应该与决策无关：无论你做怎样的决策，你都必须支付它们
- 固定成本是一种沉没成本：无论企业决定生产或停止营业，都必须支付固定成本
- 因此，固定成本的大小不影响做是否停止营业的决策

## 完全竞争市场的短期均衡

- 完全竞争企业短期均衡时的盈利状况
  - P>AC： Π>0盈利
  - P=AC： Π=0利润为零
  - P<AC： Π<0亏损
    - AVC<P<AC： 亏损但会继续生产，因可收回全部VC及部分FC
    - P<AVC： 亏损并停产，因连VC都无法得到全部补偿
      - P=AVC： 短期停产点

## 完全竞争市场的短期均衡

### 完全竞争厂商短期获超额利润

![](img/518b44ff56ad05e168220626515b534d_774_0.png)

P>AC：获得超额利润

## 完全竞争市场的短期均衡

### 完全竞争厂商短期亏损但继续生产

![](img/518b44ff56ad05e168220626515b534d_776_0.png)

AVC<P<AC：亏损但继续生产

## 完全竞争市场的短期均衡

### 完全竞争厂商短期亏损并停产

![](img/518b44ff56ad05e168220626515b534d_777_0.png)

P<AVC：亏损并停产

## 企业的短期停止营业决策

- 如果生产能得到的收益小于生产的可变成本，企业就停止营业。
  - 如果 TR < VC，停止营业。
  - 如果 TR/Q < VC/Q，停止营业。
  - 如果 P < AVC，停止营业。

## 完全竞争市场中企业短期供给曲线

- 供给曲线是每一个价格水平上企业愿意而且能够提供的产品的数量的轨迹。
- 它表示短期内企业最有利可图（或者亏损最小）的产量水平与产品价格之间的关系。
- 供给曲线上每一点均为对应市场价格的利润最大或亏损最小的产量点，即均衡产量点。
- 在完全竞争市场中，随着价格的变动，企业实现利润最大化要满足MC=MR=P，即均衡的产量点总是在边际成本曲线上变动。
- 企业的短期供给曲线是位于AVC曲线最低点以上部分的MC曲线（P>AVC）。
- 注意：竞争性企业的供给曲线和短期边际成本SMC曲线并不完全相同。

## 竞争企业的短期供给曲线

### 企业短期供给曲线

成本

如果 **P > AVC**，短期内继续生产

如果 **P < AVC**，停止营业

如果 **P > AVC**，继续生产可以获得利润

MC

ATC

AVC

产量

0

![](img/518b44ff56ad05e168220626515b534d_780_0.png)

## 完全竞争企业的短期供给曲线

![](img/518b44ff56ad05e168220626515b534d_781_0.png)

![](img/518b44ff56ad05e168220626515b534d_781_1.png)

位于平均变动成本（AVC）曲线最低点以上的短期边际成本曲线

## 完全竞争市场中行业的短期供给曲线

- 一个行业（市场）的供给曲线由该行业内所有企业的短期供给曲线的水平加总而得到。
- 行业的短期供给曲线是向右上方倾斜的，表示市场的产品价格和市场的短期供给量成同方向变动。
- 行业的短期供给曲线上与每一个价格水平相对应的供给量可以使全体企业在该价格水平获得最大利润或最小亏损的最优产量。

## 完全竞争市场的短期供给曲线

市场短期供给曲线是各企业短期供给曲线水平加总

![](img/518b44ff56ad05e168220626515b534d_783_0.png)

在完全竞争市场的情况下厂商的需求曲线是一条平行于x轴的线，如果需求一定的话按需生产就好了，为什么还要讨论增减产量？这里竞争的理解

## 企业进入或退出一个市场的长期决策

在长期中，如果从生产中得到的收益小于生产的总成本,企业就退出。

- 如果 TR < TC，就退出。
- 如果 TR/Q < TC/Q，就退出。
- 如果 P < ATC，就退出。

如果一种行动将有利可图，企业就将进入一个市场。

- 如果 TR > TC，就进入。
- 如果 TR/Q > TC/Q，就进入。
- 如果 P > ATC，就进入。

## Whole Slide Image Analysis with Cell Type Detection and Localization

This project focuses on developing a deep learning solution for whole slide image (WSI) analysis. The system detects and localizes different cell types in large pathology images, providing valuable assistance for pathologists in diagnosis and research. Using state-of-the-art object detection models, the solution processes high-resolution whole slide images and identifies various cell categories including inflammatory cells, epithelial cells, and other biological structures.

### Dataset Description

The dataset consists of 50 whole slide images (WSIs) of tissue samples, collected from multiple medical centers. Each WSI is annotated with bounding boxes for different cell types. The slides are scanned at 40x magnification, resulting in images of approximately 100,000 x 100,000 pixels. For training efficiency, the slides are divided into smaller patches of 512x512 pixels.

### Cell Types and Distribution

| Cell Type | Number of Annotations | Percentage |
|-----------|----------------------|------------|
| Inflammatory cells | 45,230 | 62.5% |
| Epithelial cells | 18,450 | 25.5% |
| Stromal cells | 6,820 | 9.4% |
| Other cells | 1,880 | 2.6% |

### Methodology

- 1. **Patch Extraction**: Each WSI is split into 512x512 patches with 25% overlap to ensure complete cell coverage.
- 2. **Model Architecture**: We employ Faster R-CNN with ResNet-50 backbone, pre-trained on ImageNet.
- 3. **Data Augmentation**: Random flips, rotations (90°, 180°, 270°), and color jitter are applied to increase robustness.
- 4. **Training Strategy**: The model is trained using SGD optimizer with momentum 0.9, initial learning rate 0.001, and cosine annealing scheduler.
- 5. **Post-processing**: Non-maximum suppression (NMS) with IoU threshold 0.5 is applied to remove duplicate detections across overlapping patches.
