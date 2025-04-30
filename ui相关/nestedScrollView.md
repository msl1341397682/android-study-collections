# 背景
android的down，move，up的事件分发机制，一旦一个子view消费了事件，那么这个事件就无法再通知给父view了，对于滑动嵌套联动的场景就难以处理，所有提供两个接口来实现父子view之间的联动
- NestedScrollingParent：父view实现
- NestedScrollingChild：子view实现
# 详细流程
- touchDown时机
- 子view向上遍历找到第一个实现了NestedScrollingParent的父view，通信确保进行联动
- touchMove时机
- 滑动前先通知父view，父view消费完之后
- 子view消费剩下的delta
- 子view消费之后再把剩下的距离通知父view
- 整个过程同步进行
