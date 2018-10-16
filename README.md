# ss5108963jjjk
readme_lab2
## 作法說明 
-3.1 log transform & powerlaw transform
   logtransform: S=C*log(1+r),迭代法得c=0.1803(intensity range)
   powerlaw transform : S=C*(1+E).^Y Y=0.7,1.7,2.5,3.6,4.5 ;C=0.03468
## 分析以及討論
   Log transform 有降低亮區對比度的效果,Power law transform 有增強亮區對比度的效果;選定的Gamma值增加會讓圖片較暗但是清析;
   但是增加到一定程度會使圖片過暗而無法分辨.
   
-3.2 Histogram equalization 
## 作法說明
   ImageHist()做法: call function in another function.
   先得知各個pixel的intensity在[0~255]之間的frequency分布,有PDF之後做CDF階梯,
   再將CDF除以row或column數目後,呈上最大像素值(L=255),得到新的histogram。
   histEqualization()做法: 1.讀原圖,做原圖imhist同等功能  2. 讀入imagehist()  
   3.做HE影像與功能(histogram)  4. 將步驟1.~3.皆畫出figures.
## 分析以及討論
    第一次寫完出現Salt and Pepper Noise(fig2-a), 情況嚴重,可能發生隨機錯誤;中位數濾波可以較有效去除此問題,加入後出現新影像,有效改善影像品質。中位數     濾波其實是排序濾波（rank-order filtering）的一種特殊形式, (fig2-b)是簡易示意圖。但是中位數濾波執行速度慢,造成Busy;因此最後使用歧異點方法。
- 3.3 and 3.4 spatial filter and laplacian filter
## 作法說明  
  spatialFiltering(): 2-D convolution,使用mask在圖片中決定中心點,蓋上遮罩後得到edge強化(mask後影像大部分只有邊緣),寫法:Nested function.
  laplacianFiltering() :在此使用scale,詢問使用者想要的scale如a.2圖scale=3;
  印出結果圖片。b.1~3.是利用Laplacian of Gaussian濾波,以9*9mask實作,邊緣效果過多。
## 分析以及討論
  拉普拉斯濾波器是一種空間二階導數的運算子，它對於影像中快速變化的區域(包含edge)具有很大的強化作用，因此常結合Zero Crossing Detection(如下圖)作為邊緣   偵測的工具。
  Laplacian濾波器有以下三種型態
  [0 1 0;1 -4 1;0 1 0]
  [1 1 1;1 -8 1;1 1 1]        k1 mask
  [-1 2 -1;2 -4 2;-1 2 -1]
  上述三種mask在執行後會的到高度相似的結果圖.
  銳化濾波
  [0 -1 0;-1 5 -1;0 1 0]     k2 mask
  [-1 -1 -1;-1 9 -1;-1 -1 -1] k3 mask
  k2遮罩比原本laplacian mask對比效果更強,但是scale需在0~1之間,否則過亮無法辨識
  ;k1遮罩比原本mask邊緣*5效果更強(細節更突出);k3遮罩則是最突出邊緣*9,masked後可獲得最多細節.但是scale還是不能大,會有過亮問題。
  Laplacian濾波器的使用也採標準的迴旋積方法。由於它對高頻訊號的高度敏感，同時也會強化高頻雜訊，所以應用拉普拉斯濾波器前常先進行平滑化濾波(如高斯平滑   濾波)，以便降低雜訊干擾。但高斯平滑濾波+Laplacian濾波需要雙重迴旋積運算，十分耗費計算資源，因此多數採用LoG (Laplacian of Gaussian)濾波器。
## LoG注意事項
The LoG operator calculates the second spatial derivative of an image. This means that in areas where the image has a constant intensity (i.e. where the intensity gradient is zero), the LoG response will be zero. In the vicinity of a change in intensity, however, the LoG response will be positive on the darker side, and negative on the lighter side. This means that at a reasonably sharp edge between two regions of uniform but different intensities, the LoG response will be:
•	zero at a long distance from the edge,
•	positive just to one side of the edge,
•	negative just to the other side of the edge,
•	zero at some point in between, on the edge itself.
## 附錄: 各種平滑濾波器的比較,與處理lab2之3.2部分胡椒鹽雜訊有關,以及lab1 的bilinear.

                    適用影像           特色
nearest             graylevel         執行快,影像易模糊
median filter       graylevel         影像不模糊,執行速度慢
zoom in/out         binary            適合二值化影像的小區塊雜訊去除或填補
MAX/min filter      graylevel         適合灰階影像的小區塊雜訊去除或填補
peak/valley filter  graylevel         淡化灰階影像的小區塊灰階與周邊灰階的差異
  
