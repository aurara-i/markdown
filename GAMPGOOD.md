1. 利用GAMP-GOOD进行O和N文件的下载

遇见问题：

![](file://C:\Users\admin\Documents\IkMarkdown/.assets/spp.md876.2432123.png)
![](https://github.com/aurara-i/markdown/blob/main/picture/1.png)
因为没有配置第三方库和站点列表的原因，首次运行并不成功，进入到mainDir的目录下，将dataset_Win目录下的thirdparty_Win目录和所有站点列表都拷贝到mainDir目录下。

![](file://C:\Users\admin\Documents\IkMarkdown/.assets/spp.md939.2240338.png)

成功运行：![](file://C:\Users\admin\Documents\IkMarkdown/.assets/spp.md996.4073682.png)

在yaml文件中需要设置保存地址，下载时间和下载文件以及观测数据时间间隔。

* 1. O 文件（Observation File，观测文件）：通常遵循 RINEX（Receiver Independent Exchange Format）标准，文件后缀为 `.o`（RINEX 2.x）或 `.rnx`（RINEX 3.x），例如 `CUT000AUS_R_20210010000_01D_30S.rnx`。
  2. P 文件（Precise File，精密产品文件）就是N文件

  * **核心作用** ：存储精密轨道、钟差等辅助数据，用于提高定位精度。**格式** ：多为 SP3（Standard Product 3）格式（轨道，后缀 `.sp3`）或 RINEX 钟差格式（后缀 `.clk`），例如 `igs21001.sp3`（IGS 精密轨道）、`gfz21001.clk`（GFZ 精密钟差）

cd /opt/zotero
./zotero
