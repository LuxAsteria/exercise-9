# exercise-9

##PS:All pictures in the assignment are done by myself

###Abstract
In this assignmet, we are going to gain insight into a rather interesting problem--the movement of a billiard ball. Some pictures depict the trajactories are shown in this homework, as well as some *vpython simulations*. Also, I've done a simulation to picture the Lorenz attractor, just for fun.

###Background
Exercise 3.30
> Investigate the Lyapnov exponent of the stadium billiard for several values of alpha. You can do this qualitatively by examining the behavior for only on of initial conditions for each value of alpha you consider, or more quatitatively averaging over a range of initial conditions or each value of alpha.

Exercise 3.31
> Study the behavior for other types of tables. One interesting possibility is a square table with a circular interior wall located either in the center, or slightly off-center. Another possibility is an elliptical table.

###Exercise

### Picture of VISUALIZATION OF The Movement

![img](https://github.com/LuxAsteria/test3/blob/master/billiard%20ball.gif)

####Analyse

![img](https://github.com/LuxAsteria/test3/blob/master/屏幕快照%202016-11-20%20下午9.23.07.png)

Here, I'm going to give the explanation of how to calculate teh trajectories of the billiard ball in a 'round' table.
As is shown in the picture, it's obvious that the relationship between the new angle after colliding and the former angle between x axis and the vector of velocity is that:
<img src="http://latex.codecogs.com/gif.latex?\gamma=\pi-\beta+2\times\alpha" alt="" title="" />
 and <img src="http://latex.codecogs.com/gif.latex?\alpha" alt="" title="" /> could be enumerated by:
 <img src="http://latex.codecogs.com/gif.latex?\alpha=arctan(\frac{y}{x})" alt="" title="" />
 
####level 1:calculate the round and eclipse table
 
Here are the pictures :

![img](https://github.com/LuxAsteria/test3/blob/master/apha%3D0.01.png)

![img](https://github.com/LuxAsteria/test3/blob/master/alpha%3D0.1.png)

>Here's the code

```
def cac(alpha,v,theta_v,r):
    dt=0.01
    vx=[v*cos(theta_v)]
    vy=[v*sin(theta_v)]
    theta=[theta_v]
    theta_s=[0]
    t=[0]
    x=[1]
    y=[0]
    while t[-1]<1000:
        if -alpha*r<=x[-1]<=alpha*r:
            if y[-1]<=-r or y[-1]>=r:
                vx.append(vx[-1])
                vy.append(-vy[-1])
                theta.append(arctan(float(vy[-1]/vx[-1])))
            else:
                vx.append(vx[-1])
                vy.append(vy[-1])
                theta.append(theta[-1])
        if x[-1]<-alpha*r:
            if y[-1]**2+(x[-1]+alpha*r)**2>=r**2:
                theta_new=arctan(float(y[-1]/(x[-1]+r*alpha)))
                theta.append(pi-theta[-1]+2*theta_new)
                vx.append(v*cos(theta[-1]))
                vy.append(v*sin(theta[-1]))
            else:
                vx.append(vx[-1])
                vy.append(vy[-1])
                theta.append(theta[-1])
        if x[-1]>alpha*r:
            if y[-1]**2+(x[-1]-alpha*r)**2>=r**2:
                theta_new=arctan(float(y[-1]/(x[-1]-alpha*r)))
                theta.append(pi-theta[-1]+2*theta_new)
                vx.append(v*cos(theta[-1]))
                vy.append(v*sin(theta[-1]))
            else:
                vx.append(vx[-1])
                vy.append(vy[-1])
                theta.append(theta[-1])
        x.append(x[-1]+vx[-1]*dt)
        y.append(y[-1]+vy[-1]*dt)
        theta_s.append(arctan(y[-1]/x[-1]))
        t.append(t[-1]+dt)
    return x,y,vx,vy

def circle(alpha,r):
    if alpha==0:
        x1=y1=np.arange(-r,r,0.1)
        x1,y1=np.meshgrid(x1,y1)
        plt.contour(x1,y1,x1**2+y1**2,[r**2])
    else:
        lis=list(np.arange(r*alpha,r,0.1))
        lis2=list(np.arange(-r,-r*alpha,0.1))
        lis3=list(np.arange(-r*alpha,r*alpha,0.1))
        y1=[]
        x1=[]
        for i in lis:
            y1.append(sqrt(r**2-i**2))
            y1.append(-sqrt(r**2-i**2))
            x1.append(i)
            x1.append(i)
        for j in lis2:
            y1.append(sqrt(r**2-j**2))
            y1.append(-sqrt(r**2-j**2))
            x1.append(j)
            x1.append(j)
        for k in lis3:
            y1.append(-r)
            y1.append(r)
            x1.append(k)
            x1.append(k)
    return x1,y1
```

It's obvious that the difference between alpha will definitely influent the movement of the ball. 
I also drew the pictures which embody the seperation of the ball in that space.

![img](https://github.com/LuxAsteria/test3/blob/master/v_vx.png)

>Here's the code

```
vx_y0=cac(0.01,sqrt(3),pi/4,10)[1]
x_y0=cac(0.01,sqrt(3),pi/4,10)[0]
fig=plt.figure()
plt.subplot(132)
plt.scatter(x_y0,vx_y0,s=0.1)
plt.title('alpha=0.01')
plt.xlabel('x')
plt.ylabel('vx')

vx_y1=cac(0,sqrt(3),pi/4,10)[1]
x_y1=cac(0,sqrt(3),pi/4,10)[0]
plt.subplot(131)
plt.title('alpha=0')
plt.xlabel('x')
plt.ylabel('y')
plt.scatter(x_y1,vx_y1,s=0.1)

vx_y2=cac(0.1,sqrt(3),pi/4,10)[1]
x_y2=cac(0.1,sqrt(3),pi/4,10)[0]
plt.subplot(133)
plt.xlabel('x')
plt.ylabel('y')
plt.title('alpha=0.1')
plt.scatter(x_y2,vx_y2,s=0.1)
```

And the next is the calculation of the Lyapunove exponent.

####Level 2: Lyapnov exponent(change with alpha)

Here are the pictures:

![img](https://github.com/LuxAsteria/test3/blob/master/屏幕快照%202016-11-16%20下午7.03.54.png)

![img](https://github.com/LuxAsteria/test3/blob/master/屏幕快照%202016-11-16%20下午7.03.02.png)

![img](https://github.com/LuxAsteria/test3/blob/master/屏幕快照%202016-11-16%20下午7.03.29.png)

Here we can see apparently, when alpha grows larger, the movement will become more irregular, which means it gradually goes into a chaos.

####Level 3: something more about the Lorentz attractor

Because I'm interested in the Lorenz attractor, I also do the visualization of the formation of Lorenz attractor.

![img](https://github.com/LuxAsteria/test3/blob/master/lorentz.gif)

###Conclusion
This assignment is mainly about the simulation of the billiard ball. And from the picture and visualized simulation, it's conspicuous that as the constant--alpha changing, the movement of the ball will be undoubtedly affected, which will be shown in the more and more 'widespread' trajectories in the space.

###Acknowledge
[1] ZhiHu, Zhen lei

[2] Vpython tutorial

##PS:All Rights Reserved

