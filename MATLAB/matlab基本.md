### 		基本运算

```matlab
D=sub2ind(S,I,J) //下标转换成序号
S为包含行，列数量的向量，一般用size()函数获得
I为行下标
J为列下标
```

注：以m x n矩阵A为例，矩阵元素A（i，j)的序号就是（j-1）Xm+i

```matlab
[I,J]=ind2sub(S,D)
S为包含行，列数量的向量，一般用size()函数获得
D是序号
```

```
A(:,[2,4])=[]
//删除第二列和第四列所有的元素
```

```
reshape(A,m,n)
矩阵总元素不变，重排成M*N的二维矩阵

A(:)等价于reshape(A,6,1)
```



矩阵的除法运算

```
B/A --> B*inv(A)
A\B --> inv(A)*B

a/5 <--> 5\a
```

注：.* 	./ 	.\ 	.^都是对应元素进行相关运算

```
水仙花题：
>>m=100:999;
>>m1=rem(m,10);
>>m2=rem(fix(m/10),10);
>>m3=fix(m/100);
>>k=find(m==m1.*m1.*m1+m2.*m2.*m2+m3.*m3.*m3); //得出符合条件的序号
k=
	54 271 272 308
>>s=m(k)
s=
	153 370 371 407
```

 

求和和求积：

sum() 求和

prof() 求积



cumsum() 累加和函数

cumprod() 累乘积函数



标准差与相关系数：

std(X):计算向量X的标准差

std(A):计算矩阵A的各列的标准差

std(A,flag,dim):flag取0或1，当flag=0时，计算样本标准差，当flag=1时，计算总体标准差，默认为flag=0,dim=1



相关系数：

corrcoef(A):返回由矩阵A所形成的一个相关系数矩阵，其中，第i行第j列的元素表示原矩阵A中第i列和第j列的相关系数

corrcoef(X,Y):在这里，X Y是向量，与corrcoef([X,Y])的作用一样，用于求X Y向量之间的相关系数



排序：

sort(X) 对向量X按升序排列

[Y,I]=sort(A,dim,mode)       mode有ascend和descend模式，Y是排序后的矩阵，I记录Y中的元素在A中的位置



### 字符串

''在字符串中表示为'

```
字符串倒过来排列
ch='dslfvo9302'
revch=ch(end:-1:1)
```

```
将字符串中的小写字母变成大写字母
k=find(ch>='a'&ch<='z')  
ch(k)=ch(k)-('a'-'A')

统计字符串中小写字母的个数
length(k)
```

```
将字符串里的值当作matlab命令来执行
eval(s)
```

```
s1='MATLAB'
a=abs(s1) //把s1转换成ASCII码的数值矩阵
char(a+32) //把数值矩阵转换为字符串矩阵
```

```
字符串的比较
>>'www0'>='W123'
ans=
	1  1  1  0
```

```
strcmp(s1,s2)
strncmp(s1,s2,n)
strcmpi(s1,s2)
strncmpi(s1,s2,n)

findstr(s1,s2):返回短字符串在长字符串的开始位置
strrep(s1,s2,s3):将字符串s1中的所有子字符串s2替换为字符串s3

```





### 矩阵处理

inv 求逆矩阵



zeros函数 产生全0矩阵

ones函数  产生全1矩阵

eye函数 产生对角线为1的矩阵

rand函数 产生(0,1)区间均匀分布的随机矩阵

randn函数 产生均值为0，方差为1的标准正态分布随机矩阵

zeros(m) m*m矩阵

zeros(m,n) m*n矩阵

zeros(size(A)) 与A同大小矩阵



注：fix(a+(b-a+1)*x) :产生[a,b]区间上随机分布的随机整数  

​		u+px:得到均值为u,方差为p平方的随机数



例：产生5阶两位随机整数矩阵A，再产生均值为0.6 方差为0.1的5阶正态分布随机矩阵B 最后验证

（A+B）I=IA+BI （I为单位矩阵）

```
A=fix(10+(99-10+1)*rand(5));
B=0.6+sqrt(0.1)*randn(5);
C=eye(5);
(A+B)*C==C*A+B*C;
```







#### 特殊矩阵



magic(n) 产生特定n阶的魔方矩阵

vander(V) 生成以向量V为基础的范德蒙矩阵

hilb(n)                                       （希尔伯特矩阵: H(i,j)=1/(i+j-1)）

compan(p)  生成伴随矩阵 其中p是一个多项式的系数向量，高次幂系数在前

pascal(n) 生成帕斯卡矩阵

![](C:\Users\jinyu\AppData\Roaming\Typora\typora-user-images\image-20210221161930259.png)

```
生成多项式x3-2x2-5x+6的伴随矩阵
>>p=[1,-2,-5,6]
>>A=compan(p)
```

#### 矩阵变换

对角矩阵：只有对角线上有非零元素的矩阵

数量矩阵：对角线上的元素相等的对角矩阵



diag(A): 提取主对角线元素

diag(A,k): 提取矩阵A第k条对角线的元素 （主对角线为第0条）

diag(V): 以向量V为主对角线元素，产生对角矩阵

diag(V,k):



triu(A): 提取矩阵A的主对角线及以上的元素

triu(A,k): 提取矩阵A的第k条对角线及以上的元素

//tril 也相对应



（.'） 转置运算符

 （'） 共轭转置（在转置的基础上还要取每个数的复共轭）



fliplr(A): 对矩阵A实施左右翻转

flipud(A): 上下翻转



#### 矩阵求值

求矩阵的最大元素和最小元素

```
--参数为向量时
y=max(X); --返回向量X的最大值存入y,如果X中包含复数元素，则按模取最大值。
[y,k]=max(X); --返回向量X的最大值存入y,最大值元素的序号存入l,如果X中包含复数元素，则按模取最大值

--参数位矩阵时
max(A):返回一个行向量，每个元素都是对应列向量的最大值
[Y,U]=max(A):返回行向量Y和U，Y记录每列的最大值，U向量记录每列最大值元素的行号
max(A,[],dim):dim取1或2.dim取1时，与max(A)一致。dim取2时，行列置换

--
max(max(A)) --求一个矩阵的最大元素
max(A(:)) --将A矩阵堆叠成一个列向量
```











det(A): 求方阵A所对应的行列式的值

rank(A):求矩阵A的秩

（PS：奇数阶的魔方矩阵是满秩矩阵

​             一重偶数阶魔方矩阵秩为n/2+2 n是2的倍数 但非4的倍数

 			双重偶数阶魔方矩阵秩均为3， 即阶数是4的倍数）



矩阵的迹：对角线元素之和，也是矩阵特征值之和

trace(A)



求向量范数：

norm(V)或norm(V,2) 计算向量V的2-范数 （各元素的平方之和的平方根）

norm(V,1) 计算向量V的1-范数（计算各元素的绝对值的和）

norm(V,inf) 计算向量V的无穷-范数 （绝对值最大的元素的绝对值）



求矩阵范数：（调用函数的格式与向量完全相同）

1-范数：矩阵列元素绝对值之和的最大值

2-范数：A'A矩阵的最大特征值的平方根

无穷-范数：所有矩阵行元素绝对值之和的最大值



矩阵的条件数：为A的范数与A的逆矩阵的范数的乘积（条件数越接近于1，矩阵的性能越好，反之）



对应的计算矩阵的条件数的函数：

cond(A,1)

cond(A)或cond(A,2)

cond(A,inf)



例:求2~10阶希尔伯特矩阵的条件数

```matlab
for n=2:10

​		c(n)=cond(hilb(n))

end

format long

c'


```



#### 矩阵的特征值和特征向量

E=eig(A) 求矩阵A的全部特征值 构成向量E

[X,D]=eig(A) 求矩阵A的全部特征值 构成对角阵D 	并产生矩阵X	 X各列是相应的特征向量



#### 稀疏矩阵

A=sparse(S) 将S转化为稀疏存储方式的矩阵A

S=full(A)



sparse(m,n) 生成m*n的所有元素都是0的稀疏矩阵

sparse(u,v,S) 其中u,v,S是3个等长的向量，S是要建立的稀疏存储矩阵的非零元素，u(i),v(i),分别是s(i)的行和列下标



B=spconvert(A) 直接建立稀疏存储矩阵





带状稀疏矩阵:指所有非零元素集中在对角线上的矩阵

[B,d]=spdiags(A) 从带状稀疏矩阵A中提取全部非零对角线元素赋给矩阵B及其这些非零对角线的位置向量d

A=spdiags(B,d,m,n) 产生带状稀疏矩阵的稀疏存储矩阵A 其中m,n为原带状稀疏矩阵的行数和列数，矩阵B的第i列即为原带状稀疏矩阵的第i条非零对角线，向量d为原带状稀疏矩阵所有非零对角线的位置



应用：

```
kf1=[1;1;2;1;0];
k0=[2;4;6;6;1];
k1=[0;3;1;4;2];
B=[kf1,k0,k1];
d=[-1;0;1];
A=spdiags(B,d,5,5);
f=[0;3;2;1;5];
x=A\f
```



#### 矩阵计算求线性方程组



LU分解函数:

[L,U]=lu(A):产生一个上三角阵U和一个变换形式的下三角阵L，使之满足A=LU

注意，这里的矩阵A必须是方阵



[L,U,P]=lu(A):产生一个上三角阵U和一个变换形式的下三角阵L以及一个置换矩阵P，使之满足PA=LU

注意，这里的矩阵A必须是方阵



```
Ax=b
LUx=b
x=U\(L\b)

Ax=b
PAx=Pb
--PA=LU--
LUx=Pb
x=U\(L\P*b)
```





迭代法：

1.雅可比迭代法

2.高斯-赛德尔迭代法



### 函数



函数参数的可调性

```
function fout=test(a,b,c)
if nargin==1
	fout=a;
elseif nargin==2
	fout=a+b;
elseif nargin==3
	fout=(a*b*c)/2;
end
```



声明全局变量： global x



求极限：



```
limit(f,x,a)
--即求函数f关于变量x在a点的极限。若x省略，则采用系统默认的自变量。a默认值为0

limit(f,x,a,'right')
limit(f,x,a,'left')
```



求导：

```
diff(f,x,n)
求函数f关于变量x的n阶导数。（n的默认值为1）



例：求偏导：z=x*exp(y)/y^2;
g=x*exp(y)/y^2;
diff(g,x);
diff(g,y);
```





泰勒展开

```
taylor(fcn,x,x0,'Order',6);%对函数fcn在点x0处，进行6阶泰勒展开；
```



### 图形绘制



```
x=linspace(0,2*pi,100);
plot(x,[xin(x);xin(2*x);sin(3*x)])
legend('sin(x)','sin(2x)','sin(3x)') 
```

​	

```
x=[1,2,3,4];
y=[4,3,2,1];
cx=x+y*i;
plot(cx) //cx=complex(x,y);
```

```
title(...);
title('y=cos{\omega}t');
title('y=e^{axt}')
title('X_{1}{\geq}X_{2}')
title('{\bf y=cos{\omega}t+{\beta}}')

title('y=cos{\omega}t','Color','r')
title('y=cos{\omega}t','FontSize',24)

"\bf" 加粗
"\it" 斜体
"\rm" 正体
```

```
xlable('-2\pi\leq x x\leq 2\pi')

text(-2*pi,0,'-2{\pi}')
text(3,0.28,'\leftarrow sin(x)')

```



坐标控制：

```
axis([xmin,xmax,ymin,ymax,zmin,zmax])
axis equal: 纵、横坐标轴采用等长刻度
axis square: 产生正方形坐标系（默认为矩形）
axis auto:使用默认设置
axis off:取消坐标轴
axis on:显示坐标轴
```



给坐标系加网格和边框：

```
grid on
grid off	//网格

box on 
box off       //边框
```



图形保持：（绘制两个同心圆）

```
t=linspace(0,2*pi,100);
x=sin(t);y=cos(t);
plot(x,y,'b')
hold on;
plot(2*x,2*y,'r--')
grid on
axis([-2.2,2.2,-2.2,2.2])
axis equal
```



图形窗口的分割：

```
subplot(2,2,1);  //把背景板分成2X2的格式，选中第一个为活动区
x=linspace(0,2*pi,60);
y=sin(x);
plot(x,y);
title('sin(x)');
axis([0,2*pi,-1,1]);
```



对数坐标图

```
semilogx(x1,y1,选项1，x2,y2,选项2，...)
semilogy(x1,y1,选项1，x2,y2,选项2，...)
loglog(x1,y1,选项1，x2,y2,选项2，...)
```



极坐标： （绘制心形曲线）

```
t=0:pi/100;2*pi;
r=1-sin(t);
subplot(1,2,1)
polar(t,r)
subplot(1,2,2)
t1=t-pi/2;
r1=1-sin(t1);
polar(t,r1)
```



### 统计图

```
y=[1,2,3,4,5;1,2,1,2,1;5,4,3,2,1];
subplot(1,2,1)
bar(y)
title('Group')
subplot(1,2,2)
bar(y,'stacked')
title('Stack')
```



例：绘制条形图

```
x=[2015,2016,2017];
y=[68,80,115,98,102;
75,88,102,99,110;
81,86,125,105,115];
bar(x,y)
title('Group');
```



绘制直方图

```
y=randn(500,1);
subplot(2,1,1);
hist(y);
title('高斯分布直方图');
subplot(2,1,2);
x=-3:0.2:3;
hist(y,x);
title('指定区间中心点的直方图')
```



绘制极坐标下的直方图

```
y=randn(500,1);
theta=y*pi;
rose(theta)
title('在极坐标下的直方图')
```



面积类图形：



制作扇形图：

```
score=[5,17,23,9,4];
ex=[0,0,0,0,1] //使第五个扇形分离突出
pie(score,ex)
legend('优秀'，'良好'...,'location','eastoutside')
```



散点类图形：



制作散点桃心曲线

```
t=0:pi/50:2*pi;
x=16*sin(t).^3;
y=13*cos(t)-5*cos(2*t)-2*cos(3*t)-cos(4*t);
scatter(x,y,'rd','filled')
```





矢量图形：

```
quiver(x,y,u,v)
//(x,y) (u,v) 分别为矢量起点和终点
```



例:已知向量A，B求A+B，并用矢量图表示

```
A=[4,5];B=[-10,0];C=A+B;
hold on;
quiver(0,0,A(1),A(2));
quiver(0,0,B(1),B(2));
quiver(0,0,C(1),C(2));
text(A(1),A(2),'A');
text(B(1),B(2),'B');
text(C(1),C(2),'C');
axis([-12,6,-1,6])
grid on
```



### 三维曲线



绘制螺旋线

```
t=linspace(0,10*pi,200);
x=sin(t)+t.*cos(t);
y=cos(t)-t.*sin(t);
z=t;
subplot(1,2,1)
plot3(x,y,z)
grid on
subplot(1,2,2)
plot3(x(1:4:200),y(1:4:200),z(1:4:200))
grid on
```



绘制空间曲线：

```
t=0:pi/50:6*pi;
x=cos(t);
y=sin(t);
z=2*t;
plot3(x,y,z,'p')
xlabel('X'),ylabel('Y'),zlabel('Z');
grid on
```



fplot3与之对应



### 三维曲面

[X,Y]=meshgrid(x,y);

其中，参数x,y为向量，储存网格点坐标的X,Y为矩阵。

```matlab
x=2:6;
y=(3:8)';
[X,Y]=meshgrid(x,y);
```

```
//例子

x=2:6;
y=(3:8)';
[X,Y]=meshgrid(x,y);
Z=randn(size(X));
plot3(X,Y,Z);
grid on;
```



用mesh绘制函数

```
t=-2:0.2:2;
[X,Y]=meshgrid(t);
Z=X.*exp(-X.^2-Y.^2);
mesh(X,Y,Z); 
```

meshc meshz surfc surfl 

```matlab
[x,y]=meshgrid(0:0.1:2,1:0.1:3);
z=(x-1).^2+(y-2).^2-1;
subplot(2,2,1);
meshc(x,y,z);title('meshc(x,y,z)')
subplot(2,2,2);
meshz(x,y,z);title('meshz(x,y,z)')
subplot(2,2,3);
surfc(x,y,z);title('surfc(x,y,z)')
subplot(2,2,4);
surfl(x,y,z);title('surfl(x,y,z)')
```



[x,y,z]=cylinder(R,n)  // R为各个等间隔高度的半径 n表示在圆柱圆周上有n个间隔点（默认有20个间隔点）



用cylinder函数分别绘制柱面，花瓶和圆锥面

```
subplot(1,3,1);
[x,y,z]=cylinder;
surf(x,y,z);
subplot(1,3,2);
t=linespace(0,2*pi,40);
[x,y,z]=cylinder(2+cos(t),30);
surf(x,y,z);
subplot(1,3,3);
[x,y,z]=cylinder(0:0.2:2,30);
surf(x,y,z);
```



生成两个互相垂直且直径相等的圆柱面的相交图形

```sql
[x,y,z]=cylinder(1,60);
z=[-1*z(2,:);z(2,:)]; //将柱底面坐标改为-1
surf(x,y,z);
hold on
surf(y,z,x);
axis equal
```



peaks函数：

```
p1=peaks(10); 
p2=peaks; //生成一个49阶方阵
p3=peaks(-3:0.2:3);
[x,y]=meshgrid(-2:0.1:2,0:0.1:5);
p4=peaks(x,y);
```



```matlab
//fsurf fmesh  参数方程有两个自变量
fsurf(funx,funy,funz,uvlims) //uvlims默认为[-5,5,-5,5]
fmesh(funx,funy,funz,uvlims)
```





图形修饰：

```
view(az,el) //az为方位角，el为仰角
view(x,y,z) //为视点在坐标系的位置
view(2) //设置为二维平面观察图形 （即方位角为0度，仰角为90度）
view(3) //设置为三维空间观察图形（视点为默认）
```



色图：

```
surf(peaks);
colormap hot;
```



例：创建一个灰色系列的色图矩阵

```
c=[0,0.2,0.4,0.6,0.8,1]';
camp=[c,c,c];
surf(peaks);
colormap(cmap);
```



三维图形表面的着色：

```
shading faceted --将每个网格片用其高度对应的颜色进行着色，网格线是黑色
shading flat --将每个网格片用同一个颜色进行着色，且网格线也用相应的颜色
shading interp --在网格片内采用颜色插值处理
```



绘制3/4圆

```matlab
t=linspace(0,2*pi,100);
x=sin(t);
y=cos(t);
p=y>0.5;
y(p)=NaN;
plot(x,y);
axis([-1.1,1.1,-1.1,1.1]);
axis square
grid on
```



绕X轴的旋转曲面

```matlab
clear;clc;clf;
x = linspace(0,1,100);
y = x-x.*sin(3*x);
figure(1);
plot(x,y);
xlabel('x');ylabel('f(x)');
axis equal;
[X,Y,Z] = cylinder(y,100);
figure(2);
surf(2*Z,X,Y);
xlabel('X');ylabel('Y');zlabel('Z');
axis equal
```

x=t(1);





### 多项式

```matlab
--多项式乘法
conv(p1,p2):多项式相乘函数

--多项式除法
[Q,r]=deconv(p1,p2):多项式相除函数
其中，Q返回多项式P1除以P2的商式，r返回P1除以P2的余式。这里，Q和r仍是多项式系数向量。
deconv是conv的逆函数，因此有
    P1=conv(Q,P2)+r
    

--多项式求导
p=polyder(P): 
p=polyder(P,Q):  求P*Q的导函数
[p,q]=polyder(P,Q): 求P/Q的导函数，导函数的分子存入P，分母存入q

--多项式的求值
polyval(p,x): 代数多项式求值
	p为多项式系数向量，x可以是标量，向量或矩阵。都是求x元素带入多项式对应的值

polyvalm(p,x): 矩阵多项式求值
	要求x为方阵，以方阵为自变量求多项式的值
	
--多项式求根
roots(p):多项式求根函数
其中，p为多项式的系数向量

--若已知多项式的全部根，则可以用poly函数建立起该多项式，其调用格式为：
	p=poly(x)
```



### 数据插值与函数拟合

```matlab
x=[0,3,5,7,9,11,12,13,14,15];
y=[0,1.2,1.7,2.0,2.1,2.0,1.8,1.2,1.0,1.6];
x1=0:0.1:15;
y1=interp1(x,y,x1,'spline'); --x,y是两个等长的已知向量，x1是一个向量或标量，表示要插值的点
plot(x1,y1);

插值方法：
linear:线性插值，默认方法。将与插值点靠近的两个数据点用直线连接，然后再直线上选取对应插值点的数据。
nearest:最近点插值，选择最近样本点的值作为插值数据
pchip:三次插值
spline:
```



二维函数插值：

```
Z1=interp2(X,Y,Z,X1,Y1,method)
X,Y是两个向量，表示两个参数的采样点，Z是采样点对应的函数值，X1，Y1表示要插值的点
```



曲线拟合：

```
polyfit():多项式拟合函数
函数功能：求得最小二乘拟合多项式函数

P=polyfit(X,Y,m)
[P,S]=polyfit(X,Y,m)
[P,S,mu]=polyfit(X,Y,m)

	根据样本数据X和Y，产生一个m次多项式P及其在采样点误差数据S，mu是一个二元向量，mu(1)是mean(X),
而mu(2)是std(X)
```



最小二乘法

```matlab
function e = fun1(c)
x=0:24;
y=[15,14,14,14,14,15,16,18,20,22,23,25,28,31,32,31,29,27,25,24,22,20,18,17,16];
e=y-(c(1).*x.^2+c(2).*x+c(3))

end


>>c=lsqnonlin(@fun1,[0,0,0])
%c=lsqcurvefit(fun,c,x,y)
```



非线性参数拟合

```
x=[0,0.2,0.4,0.6,0.8,1];
y=[4.0,4.5,5.0,6.1,6.8,7.6];
myfunc=inline('3+beta(1)*x+exp(beta(2)*x)','beta','x');
beta=nlinfit(x,y,myfunc,[0 0]);
a=beta(1)
b=beta(2)
```



```matlab
x=[0,0.2,0.4,0.6,0.8,1];
y=[4.0,4.5,5.0,6.1,6.8,7.6];
myfunc=inline('3+beta(1)*x+exp(beta(2)*x)','beta','x');
beta=nlinfit(x,y,myfunc,[0 0]);
a=beta(1)
b=beta(2)
a =
1.6195
b =
1.1131
```



### 微分积分



数值微分的实现：

```
dx=diff(x): 计算向量x的一阶向前差分
dx=diff(x,n): 计算向量x的n阶向前差分 如:diff(x,2)=diff(diff(x))
dx=diff(A,n,dim): 计算矩阵A的n阶差分 dim=1位默认，按列计算差分 （2的话按行）
```



```matlab
x=[0,sort(2*pi*rand(1,5000)),2*pi];
y=sin(x);
f1=diff(y)./diff(x);
f2=cos(x(1:end-1));
plot(x(1:end-1),fi,x(1:end-1),f2);
d=norm(f1-f2)
```



数值积分:

```
[l,n]=quad(filename,a,b,tol,trace) --基于自适应辛普森方法
[l,n]=quadl(filename,a,b,tol,trace) --基于自适应Gauss-Lobatto方法

filename是被积函数名，a和b分别是定积分的下限和上限，积分限[a,b]必须是有限的，不能为无穷大；tol用来控制积分精度，默认时取tol=10的-6次方;trace控制是否展现积分过程，若取非0则展现积分过程，取0则不展现，默认时取trace=0;返回参数l即定积分的值，n为被积函数的调用次数

```



数值积分的实现：

```
I=integral(filename,a,b) --基于全局自适应积分方法
其中，I是计算得到的积分；filename是被积函数，a,b为上下限，积分限可为无穷大

[I,err]=quadgk(filename,a,b) --基于自适应高斯-克朗罗德方法
其中，err返回近似误差范围，

基于梯形积分法： -- a=x1<x2<...<xn=b
I=trapz(x,y)
```



多重定积分的数值求解

```
I=integral2(filename,a,b,c,d)
I=quad2d(filename,a,b,c,d)
I=dblquad(filename,a,b,c,d,tol)

I=integral3(filename,a,b,c,d,e,f)
I=triplequad(filename,a,b,c,d,e,f,tol)
```



不定积分：

int(f,x) 函数f对变量x的不定积分



定积分：

int(f,x,a,b)





### 非线性方程组与极值计算

求零点：

```
x=fzero(filename,x0)
--filename是待求根方程左端的函数表达式，x0是初始值

x=fsolve(filename,x0,option)
--x为返回的近似解，x0为初值，option为优化参数
```



二分法求零点：

```matlab
clear;
clc;
syms U L;    
f=@(x)x*log(sqrt(x^2-1)+x)-sqrt(x^2-1)-0.6*x; 
U=3;    
L=2;
while U-L>1e-10  
    root=(U+L)/2;   
    if f(root)==0    
        break;   
    end
    if f(root)*f(U)<0    
        L=root;
    else
        U=root;
    end
end
root
```



求极值：

```
无约束最优化问题：

[xmin,fmin]=fminbnd(filename,x1,x2,option) --x1,x2为左右边界
[xmin,fmin]=fminsearch(filename,x0,option) --x0是一个向量，为极值点的初值
[xmin,fmin]=fminunc(filename,x0,option)

有约束最优化问题：


```



常微分方程数值求解函数

```
[t,y]=solver(filename,tspan,y0,option)
t,y分别给出时间向量和相应的数值解，solver为求常微分方程数值解的函数，filename是定义f(t,y)的函数名，该函数必须返回一个列向量；tspan形式为[t0,tf],表示求解区间；y0是初始状态向量,option是可选参数，用于设置求解属性
```

求解函数统一命名格式：odennxx

nn是数字，代表所用方法的阶数；xx是字母，用于标注方法的专门特征

```
f=@(t,y)(y^2-t-2)/4/(t+1);
[t,y]=ode23(f,[0,10],2); //y(0)=2
y1=sqrt(t+1)+1;
plot(t,y,'b:',t,y1,'r')
```

```matlab
fun1.m:

function f = fun1(x)

f=1000-x(1)^2+2*x(2)^2-x(3)^2-x(1)*x(2)-x(1)*x(3);

 

end

 

confun1.m:

function [c,ceq]= confun1(x)

c=[x(1)^2+x(2)^2+x(3)^2-25;1-x(1)^2-x(2)^2-x(3)^2];

ceq=[8*x(1)+14*x(2)+7*x(3)-56];

 

 

end

 

 

脚本文件:

[x,fval]=fmincon(@fun1,[1,1,1],[],[],[],[],[0,0,0],[],@confun1)


```



### 符号



syms n

```
lt() le() ge() ge() eq() ne()
<     <=  >     >=  ==   ~=
```



可用assume函数对符号对象设置值域，函数调用格式为：

```
assume(condition)  --指定变量满足condition
assume(expr,set)   --指定表达式expr属于set

如：
syms x;
assume(x<0);
abs(x)==x
```



因式分解与展开运算

```
factor(s):对s分解因式
expand(s):对s进行展开
collect(s):对s合并同类项
collect(s,v):对s按变量v合并同类项

如：
syms a b;
s=a^3-b^3;
factor(s)
```



其它运算

```
[n,d]=numden(s) 提有理分式的分子分母
c=coeffs(s,x) 提取符号表达式的系数 s为函数 x为自变量
simplify(s) 符号表达式化简

p=sym2poly(s) 符号多项式转换为多项式系数向量 （反之）
```



代数方程符号求解：

```
solve(s):求解符号表达式s的代数方程，求解变量为默认变量
solve(s,v):求解符号表达式s的代数方程，求解变量为v
solve(s1,s2,...,sn,v1,v2,...,vn);

syms x y
[x y]=solve('18*x + y + 6*x*y','3*x^2 + x + y^2/3 + 2*y','x','y')
```



常微分方程符号求解：

Dy表示一阶导 D2y表示二阶导

```
dsolve(e,c,v)
用于求解常微分方程e在初值条件c下的特解。参数v是方程中的自变量，省略是按默认原则处理，若没有c，则求通解。

dsolve(e1,e2,...,en,c1,c2,...,cn,v)

如：
syms x y t;
y=dsolve('Dy-(x^2+y^2)/x^2/2',x)

[x,y]=dsolve('Dx=4*x-2*y','Dy=2*x-y',t)
```

