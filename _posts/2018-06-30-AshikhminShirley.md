---
layout:     post
title:      Ashikhmin-Shirley 光照模型
subtitle:   
date:       2018-06-30
author:     JPeng
header-img: img/Ashikhmin-Shirley01.png
catalog: true
tags:
    - LightModel
---


# Ashikhmin-Shirley
  各向异性模型。用来表现一些特殊的金属材质。相比其他的模型，可以更好的控制高光的形状。虽然不是基于物理的模型，但是使用了Fresnel函数来获得更真实的反射系数。同时Diffuse项也脱离了Labertian模型，保证了能量守恒，效果还是很不错的，不过计算代价确实很大。如果要用到游戏里面，肯定要做一些简化。
$$
\begin{aligned}
&J_{s}=\frac{\sqrt{\left(n_{u}+1\right)\left(n_{v}+1\right)}}{8 \pi} \frac{N \cdot H \frac{n u(H \cdot T)^{2}+n v(H \cdot B)^{2}}{1-(H \cdot N)^{2}}}{(V \cdot H) \cdot \max (N \cdot L, N \cdot V)} F_{r}\left((V \cdot H), C_{s}\right) \\
&J_{d}=\frac{28}{23} C_{d}\left(1-C_{s}\right)\left(1-\left(1-\frac{N \cdot V}{2}\right)^{5}\right)\left(1-\left(1-\frac{N \cdot L}{2}\right)^{5}\right)
\end{aligned}
$$
下面是原始论文里面的不同的nu,nv值的效果：
![Ashikhmin-Shirley03](https://mypicgo-1256286372.cos.ap-guangzhou.myqcloud.com/img/Ashikhmin-Shirley03.png)
当nu=nv时，A.S模型就退化成了各向同性的模型，下面是原始论文里面的效果比较：![Ashikhmin-Shirley04](D:\gitDemo\liujiapeng550.github.io\img\Ashikhmin-Shirley04.png)



$$
J_{s}=\frac{n+1}{8 \pi} \frac{(N \cdot H)^{n}}{V \cdot H \cdot \max (N \cdot L, N \cdot V)} F_{r}\left(V \cdot H, C_{s}\right)
$$
注意Diffuse项现在是和视点相关的，Fresnel的加入使得在不同的角度，Diffuse和Specular的比例有所区别，Fresnel采用的是Schlick的简化函数：
$$
F_{r}\left(u, \rho_{s}\right)=\rho_{s}+\left(1-\rho_{s}\right)(1-u)^{5}
$$

加入Fresnel的效果，看下面这张原始论文的图就很直观了：
![Ashikhmin-Shirley07](D:\gitDemo\liujiapeng550.github.io\img\Ashikhmin-Shirley07.png)
下面是我的代码实现与论文中的现象相同 
Nu= 10；Nv= 10；            Nu = 10 ； Nv= 100        Nu = 10 ； Nv= 1000          Nu = 10 ； Nv= 10000

![Ashikhmin-Shirley14](D:\gitDemo\liujiapeng550.github.io\img\Ashikhmin-Shirley14.png)

![Ashikhmin-Shirley08](D:\gitDemo\liujiapeng550.github.io\img\Ashikhmin-Shirley08.png)

![Ashikhmin-Shirley09](D:\gitDemo\liujiapeng550.github.io\img\Ashikhmin-Shirley09.png)

![Ashikhmin-Shirley10](D:\gitDemo\liujiapeng550.github.io\img\Ashikhmin-Shirley10.png)

![Ashikhmin-Shirley11](D:\gitDemo\liujiapeng550.github.io\img\Ashikhmin-Shirley11.png)

![Ashikhmin-Shirley12](D:\gitDemo\liujiapeng550.github.io\img\Ashikhmin-Shirley12.png)

![Ashikhmin-Shirley13](D:\gitDemo\liujiapeng550.github.io\img\Ashikhmin-Shirley13.png)
