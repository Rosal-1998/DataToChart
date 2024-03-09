# Echart 学习记录

### 配置项(options)

	1. title:标题
	2. legend: / 图例组件
	3. xAxis:x轴
      	1. gridIndex: 所在grid的索引。grid可理解为网格
      	2. position: x轴说明的位置（top/bottom）
      	3. type:类型
         	1. value:数值轴，连续数据
         	2. category:类目轴。为该类型时类目数据可自动从 series.data 或 dataset.source 中取，或者可通过 xAxis.data 设置类目数据。
         	3. time:时间轴
         	4. log:对数轴
      	4. name:名称，还可以定义名称的位置和样式，分别由nameLocation、nameTextStyle标明
      	5. data:类别数据。如果没有设置type，但是有这个参数，就默认type是category。
	4. yAxis:y轴,大部分情况和x轴说明一样
      	1. position:y轴说明的位置：left/right
	5. series:系列！重点
      	1. 关键字：
         	1. type：图标的类型
         	2. name：tooltip显示的类别？
         	3. data：数据
      	2. bar:柱状图
         	1. 基础柱状图
         	2. 多类型柱状图：series里面多个{}
         	3. 堆叠柱状图：stack
         	4. 动态排序柱状图
				