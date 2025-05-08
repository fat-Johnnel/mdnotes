## 3.4.2 多维连续情形 📚  
设多维连续型随机变量为$(X_1,X_2,\ldots,X_n)$，$g$为$n$元函数，若  
$$Y = g(X_1,X_2,\ldots,X_n),$$
一般先求$Y$的分布函数，若$Y$为连续型随机变量，则对其分布函数求导得到概率密度。  

### 和的分布 📈  
设$(X,Y)$的联合密度为$f(x,y)$，$Z = X + Y$。积分区域如图3.7所示（直线$x + y = z$左下方为积分区域）。  
1. **分布函数**：  
 $$F_Z(z) = \iint_{x+y \leq z} f(x,y) \,dxdy = \int_{-\infty}^{+\infty} \left( \int_{-\infty}^{z-x} f(x,y) \,dy \right) dx$$
   或等价表示为：  
 $$F_Z(z) = \int_{-\infty}^{+\infty} \left( \int_{-\infty}^{z-y} f(x,y) \,dx \right) dy$$

2. **概率密度**：  
 $$f_Z(z) = F'_Z(z) = \int_{-\infty}^{+\infty} f(x,z-x) \,dx = \int_{-\infty}^{+\infty} f(z-y,y) \,dy$$

3. **独立情形下的卷积公式**
若$X$与$Y$独立，则：  
$$f_Z(z) = \int_{-\infty}^{+\infty} f_X(x)f_Y(z-x) \,dx = \int_{-\infty}^{+\infty} f_X(z-y)f_Y(y) \,dy$$
即$Z$的概率密度为$f_X$与$f_Y$的**卷积**。  
#### 商的分布 📉  
设$(X,Y)$的联合密度为$f(x,y)$，定义$Z = \frac{X}{Y}$，其分布函数推导如下：  

1. **分布函数**（积分区域如图3.8所示）：  
  $$F_Z(z) = \iint_{\frac{x}{y} \leq z} f(x,y) \,dxdy$$ 
   将积分拆分为$y > 0$和$y < 0$两部分：  
  $$F_Z(z) = \int_{0}^{+\infty} \int_{-\infty}^{yz} f(x,y) \,dxdy + \int_{-\infty}^{0} \int_{yz}^{+\infty} f(x,y) \,dxdy$$ 

2. **概率密度**（对分布函数求导）：  
  $$f_Z(z) = \int_{-\infty}^{+\infty} |y| f(yz, y) \,dy$$ 
  $$\text{（公式3.18）}$$ 

#### 最大最小值分布 📊  
设$X_1, X_2, \ldots, X_n$为独立同分布随机变量，分布函数为$F(x)$，概率密度为$f(x)$，定义：  
- **最大值**：$Y = \max\{X_1, X_2, \ldots, X_n\}$ 
- **最小值**：$Z = \min\{X_1, X_2, \ldots, X_n\}$ 
1. **最大值分布**：  
   - **分布函数**：  
    $$F_Y(y) = P(Y \leq y) = [F(y)]^n$$ 
   - **概率密度**：  
    $$f_Y(y) = n [F(y)]^{n-1} f(y) \quad \text{（公式3.19）}$$ 

2. **最小值分布**：  
   - **分布函数**：  
    $$F_Z(z) = P(Z \leq z) = 1 - [1 - F(z)]^n$$ 
   - **概率密度**：  
    $$f_Z(z) = n [1 - F(z)]^{n-1} f(z) \quad \text{（公式3.20）}$$ 

---