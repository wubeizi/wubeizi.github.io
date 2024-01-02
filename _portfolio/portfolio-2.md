---
title: "Analysis of Nerve Cell Discharge⚡️"
excerpt: "This project is a course on neural modeling and data analysis. <br/><img src='/images/PCA.png'>"
collection: portfolio
---

Recording and analyzing neuronal activity is the foundation of neuroscience research, which is the study of electrophysiology. The basis of electrophysiological research is to extract these activities from the original electrode voltage signals. Spike Sorting precisely separates the different characteristics of different neuron discharge signals from the original voltage signal, and estimates the activity of individual neurons from a set of voltage signals. After Sorting, the AP sequences are classified into individual neural cells for further quantitative analysis to explore the relationship between neural cell discharge, external stimuli, and behavior. In 1981, Hubel and Wiessel pioneered a new field of visual neural coding research using this neural recording method and won the Nobel Prize in Physiology and Medicine.👩‍🔬🔬 <br/>

The code repository for this project is located at: [<b>Analysis-of-nerve-cell-discharge</b>](https://github.com/wubeizi/Analysis-of-nerve-cell-discharge).<br/>

This project uses a data file called spikewave.mat, which includes 5376 spike waveforms and each waveform has a length of 40 sampling points, to complete the following tasks:
- <b>Stack these spike waveforms together, observe and describe their separability</b><br/>
```python
from scipy.interpolate import make_interp_spline
 
#画出波形图
t = np.arange(0, 40) #按顺序生成40个点
for i in range(5376):
    #曲线平滑处理,代码参考https://blog.csdn.net/m0_48300767/article/details/130075597
    m = make_interp_spline(t, spike_data[i])
    xs = np.linspace(0, 40, 500)
    ys = m(xs)
    plt.plot(xs, ys)
plt.title('origin spike')
plt.show()
```
<br/><img src='/images/result1.png'>
- <b>Using PCA to reduce the dimensions of spikes to 2 and 3 dimensions respectively</b>
```python
#降为3维进行分析
from sklearn.decomposition import PCA #主成分分析
transfer = PCA(n_components= 3)
new_data = transfer.fit_transform(spike_data)
 
#波形图
t = np.arange(0, 3) 
for i in range(5376):
    plt.plot(t, new_data[i])
plt.title('3d spike')
plt.show()
 
#散点图  代码参考课件和https://blog.csdn.net/qq_40985985/article/details/119676953
fig = plt.figure()
ax = fig.add_subplot(projection='3d')
ax.scatter(new_data.T[0], new_data.T[1], new_data.T[2])   # 绘制数据点 c: 'r'红色，'y'黄色，等颜色
    
ax.set_title('3d scatter')
ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Z')
 
plt.show()
```
<br/><img src='/images/result2.png'><img src='/images/result2(2).png'>
- <b>Using appropriate clustering methods to classify them into reasonable categories</b>
```python
#利用K-means进行聚类
from sklearn.cluster import KMeans
kms = KMeans(n_clusters=5) #聚类成5类
kms.fit(new_data)
 
centers = kms.cluster_centers_
print(centers)
 
#代码参考https://blog.csdn.net/icefountain/article/details/129181949
y_train = pd.Series(kms.labels_)
y_train.rename('res',inplace=True)
 
result = pd.concat([pd.DataFrame(new_data),y_train],axis=1)
print(result)
 
Category_one = result[result['res'].values == 0]
k1 = result.iloc[Category_one.index]
 
Category_two = result[result['res'].values == 1]
k2 = result.iloc[Category_two.index]
 
Category_three = result[result['res'].values == 2]
k3 =result.iloc[Category_three.index]
 
Category_four = result[result['res'].values == 3]
k4 =result.iloc[Category_four.index]
 
Category_five = result[result['res'].values == 4]
k5 =result.iloc[Category_five.index]
```
<br/><img src='/images/PCA.png'>
- <b>Classify the waveforms after Spike sorting according to nerve cells, overlay them with different colors, observe and describe the differences in their waveforms within and between classes</b>
```python
new_result = np.array(result)
 
t = np.arange(0, 40) #按顺序生成40个点
for i in range(5376):
    if new_result[i][3] == 0:
        m = make_interp_spline(t, spike_data[i])
        xs = np.linspace(0, 40, 500)
        ys = m(xs)
        plot1, = plt.plot(xs, ys, color='blue')
    if new_result[i][3] == 1:
        m = make_interp_spline(t, spike_data[i])
        xs = np.linspace(0, 40, 500)
        ys = m(xs)
        plot2, = plt.plot(xs, ys, color='red')
    if new_result[i][3] == 2:
        m = make_interp_spline(t, spike_data[i])
        xs = np.linspace(0, 40, 500)
        ys = m(xs)
        plot3, = plt.plot(xs, ys, color='green')
    if new_result[i][3] == 3:
        m = make_interp_spline(t, spike_data[i])
        xs = np.linspace(0, 40, 500)
        ys = m(xs)
        plot4, = plt.plot(xs, ys, color='black')
    if new_result[i][3] == 4:
        m = make_interp_spline(t, spike_data[i])
        xs = np.linspace(0, 40, 500)
        ys = m(xs)
        plot5, = plt.plot(xs, ys, color='yellow')
plt.legend([plot1, plot2, plot3, plot4, plot5], ['cluster_one', 'cluster_two', 'cluster_three', 'cluster_four', 'cluster_five'])
plt.title('Spike sorting--3d')
plt.show()
```
<br/><img src='/images/result4.png'>

This project mainly utilizes <font color=blue><b>PCA</b></font> and <font color=blue><b>clustering algorithm</b></font> to reduce and cluster spike waveforms. The reference code is as follows:
- [Line chart smoothing processing](https://blog.csdn.net/m0_48300767/article/details/130075597)
- [Python 3D drawing](https://blog.csdn.net/qq_40985985/article/details/119676953)
- [K-means clustering](https://blog.csdn.net/icefountain/article/details/129181949)