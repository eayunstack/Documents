# 测试比较与分析

对测试结果的比较主要有横向比较（不同并发数对测试的影响）和纵向比较（与此前的测试数据作对比）。

根据比较得出结论，并对比较内容进行分析。

## Nova

### 横向比较

![Nova 创建与列出云主机](../pictures/nova-create-and-list.png)
<center><i>Nova 创建与列出云主机</i></center>

![Nova 创建与删除云主机](../pictures/nova-create-and-delete.png)
<center><i>Nova 创建与删除云主机</i></center>

> 由[测试结果](results.md)中的数据以及上图进行横向比较，整体看来：
> * 随着并发数的增加，创建云主机、列出云主机、删除云主机所用的时间呈现上升的趋势；
> * 列出云主机和删除云主机所用时间虽然呈现上升趋势，但变化不大；
> * 随着并发数的增加，创建云主机的成功率有所下降。

### 纵向比较

将本次测试结果与历史数据进行纵向比较：

> **创建与列出云主机**：

|并发数|执行总数|创建云主机(Sec)|列出云主机(Sec)|测试日期|测试版本|
|------|--------|---------------|---------------|--------|--------|
|60|229|119.879|2.488|2015-11-11|EayunStack v1.1|
|90|229|211.73|4.475|2015-11-11|EayunStack v1.1|
|120|229|336.747|5.658|2015-11-11|EayunStack v1.1|
|150|229|406.904|4.966|2015-11-11|EayunStack v1.1|
|2|50|5.473|0.364|2015-06-09|EayunStack v1.0|

> **创建与删除云主机**：

|并发数|执行总数|创建云主机(Sec)|删除云主机(Sec)|测试日期|测试版本|
|------|--------|---------------|---------------|--------|--------|
|60|229|105.674|9.575|2015-11-11|EayunStack v1.1|
|90|229|210.56|13.057|2015-11-11|EayunStack v1.1|
|120|229|329.861|15.146|2015-11-11|EayunStack v1.1|
|150|229|448.142|13.308|2015-11-11|EayunStack v1.1|
|2|10|5.079|2.831|2015-06-09|EayunStack v1.0|

> 由上述数据非常明显地对比到：
> * 由于某些原因，云主机的创建非常慢；
> * 经过分析后，发现了 [Bug #5500](http://192.168.15.2/issues/5500)。
> * Bug #5500 会引起云主机创建的超时，因此这是成功率有所下降的原因。

手动进行清理后，测试结果与清理前的对比如下：

|并发数|执行总数|创建云主机(Sec)|列出云主机(Sec)|测试时间|
|------|--------|---------------|---------------|--------|
|2|50|42.475|0.445|清理前|
|2|50|||清理后|

> 经过对比，结论如下：
> * 虽速度没有达到原先的水平，但速度有所提升；
> * 创建速度比原先慢的原因**可能是 EayunStack v1.1 版本中增加了组件的复杂性**。

## Neutron

### 横向比较

![Neutron 创建与列出各资源](../pictures/neutron-create-and-list.png)
<center><i>Neutron 创建与列出各资源</i></center>

![Neutron 创建与删除云主机](../pictures/neutron-create-and-delete.png)
<center><i>Neutron 创建与删除云主机</i></center>

> 由[测试结果](results.md)中的数据以及上图进行横向比较，整体看来：
> * 随着创建资源的并发数的增加，创建、列出、删除所用的时间均呈现上升趋势，但变化不大；
> * 随着创建资源的并发数的增加，测试的成功率呈现下降的趋势。

对上述对比经过分析后，发现了 [Bug #5504](http://192.168.15.2/issues/5504)。

网络创建的失败导致了与网络相关的其他资源的失败。

### 纵向比较

将本次测试结果与历史数据进行纵向比较：

> **网络资源测试的纵向比较**：

|并发数|执行总数|网络</br>创建(Sec)|网络</br>列出(Sec)|网络</br>删除(Sec)|测试日期|测试版本|
|------|--------|------------------|------------------|------------------|--------|--------|
|1|70|0.326/0.331|0.139|0.227|2015-11-12|EayunStack v1.1|
|3|70|0.341/0.358|0.127|0.239|2015-11-12|EayunStack v1.1|
|21|70|0.698/0.882|0.409|0.52|2015-11-12|EayunStack v1.1|
|45|70|1.616/1.745|0.362|0.659|2015-11-12|EayunStack v1.1|
|2|20|0.26/0.271|0.118|0.185|2015-06-09|EayunStack v1.0|

> **子网资源测试的纵向比较**：

|并发数|执行总数|子网</br>创建(Sec)|子网</br>列出(Sec)|子网</br>删除(Sec)|测试日期|测试版本|
|------|--------|------------------|------------------|------------------|--------|--------|
|1|70|0.221/0.226|0.206|0.195|2015-11-12|EayunStack v1.1|
|3|70|0.245/0.215|0.209|0.199|2015-11-12|EayunStack v1.1|
|21|70|0.580/0.641|0.469|0.479|2015-11-12|EayunStack v1.1|
|45|70|0.838/0.659|0.329|0.425|2015-11-12|EayunStack v1.1|
|5|10|0.228|0.099|-|2015-06-09|EayunStack v1.0|
|5|30|0.169|-|0.158|2015-06-09|EayunStack v1.0|

> **路由资源测试的纵向比较**：

|并发数|执行总数|路由</br>创建(Sec)|路由</br>列出(Sec)|路由</br>删除(Sec)|测试日期|测试版本|
|------|--------|------------------|------------------|------------------|--------|--------|
|1|70|0.190/0.165|0.182|0.320|2015-11-12|EayunStack v1.1|
|3|70|0.225/0.228|0.19|0.392|2015-11-12|EayunStack v1.1|
|21|70|0.511/0.702|0.358|0.998|2015-11-12|EayunStack v1.1|
|45|70|0.679/0.612|0.415|0.801|2015-11-12|EayunStack v1.1|
|5|50|0.145|0.205|-|2015-06-09|EayunStack v1.0|
|5|30|0.170|-|0.343|2015-06-09|EayunStack v1.0|

> **端口资源测试的纵向比较**：

|并发数|执行总数|端口</br>创建(Sec)|端口</br>列出(Sec)|端口</br>删除(Sec)|测试日期|测试版本|
|------|--------|------------------|------------------|------------------|--------|--------|
|1|70|0.268/0.254|0.404|0.269|2015-11-12|EayunStack v1.1|
|3|70|0.286/0.268|0.452|0.278|2015-11-12|EayunStack v1.1|
|21|70|0.796/0.682|0.864|0.712|2015-11-12|EayunStack v1.1|
|45|70|1.493/0.796|1.16|0.705|2015-11-12|EayunStack v1.1|
|5|20|0.231/0.215|0.174|0.236|2015-06-09|EayunStack v1.0|

> ###### 注：
> **列出**一栏中有两个数据(xx/xx)，分别来自 xxx_create_and_list 和 xxx_create_and_delete 两个测试。

> ###### 比较：
> 历史数据中的执行总数与当前测试接近，经过上述纵向对比，当前版本比原先版本的创建、列出、删除所用时间稍有增加，但幅度非常小，可以认为基本一致。

## Glance

### 横向比较

![Glance 创建与列出镜像](../pictures/glance-create-and-list.png)
<center><i>Glance 创建与列出镜像</i></center>

> 由[测试结果](results.md)中的数据进行横向比较，整体看来：
> * 随着并发数的增加，创建镜像和列出镜像所需时间呈现上升趋势；
> * 列出镜像所需时间虽然呈现上升趋势，但变化不大；
> * 而总执行时间变化不大。

### 纵向比较

> #### 说明：
> 由于 Glance 该测试相关的内容没有历史数据，暂不进行纵向比较。

## Cinder

### 横向比较

![Cinder 创建与列出镜像](../pictures/cinder-create-and-list.png)
<center><i>Cinder 创建与列出镜像</i></center>

![Cinder 创建与列出镜像](../pictures/cinder-create-and-delete.png)
<center><i>Cinder 创建与删除镜像</i></center>

> 由[测试结果](results.md)中的数据进行横向比较，整体看来：
> * 创建并列出卷的测试中，并发数对创建、列出卷的时间影响不大；
> * 创建并删除卷的测试中，随着并发数的增加，创建、删除卷的时间呈现上升的趋势；
> * 创建并删除卷的测试中，创建卷所需时间与删除卷所需时间接近。

### 纵向比较

|并发数|执行总数|卷</br>创建(Sec)|卷</br>列出(Sec)|卷</br>删除(Sec)|测试日期|测试版本|
|------|--------|----------------|----------------|----------------|--------|--------|
|60|300|19.726/23.02|5.952|28.07|2015-11-13|EayunStack v1.1|
|90|300|22.501/41.95|6.936|43.224|2015-11-13|EayunStack v1.1|
|120|300|18.244/64.661|5.693|66.596|2015-11-13|EayunStack v1.1|
|150|300|23.23/84.03|7.963|89.162|2015-11-13|EayunStack v1.1|
|1|3|2.642|0.118|-|2015-06-09|EayunStack v1.0|
|2|5|4.998|-|2.891|2015-06-09|EayunStack v1.0|

> #### 说明：
> 由于上述执行总数差别太大，纵向比较**无效**。
