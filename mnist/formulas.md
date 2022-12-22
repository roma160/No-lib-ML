$$ F(x) = y $$

$ o^{(l)} $ $ r^{(l)}$

$$
\begin{matrix}
\{ & o^{(0)}, & o^{(1)},\;\; \dots,& o^{(L-1)} & \} \\
& \uparrow && \downarrow \\
& x && y
\end{matrix}
$$

### 0
$ o^{(l)} = W\cdot(o^{(l-1)}) $

$ o^{(l)} = W\cdot(W\cdot(W\cdot(o^{(l-3)}))) = W^3\cdot o^{(l-3)}$

### 1
$ o^{(l)} = W\cdot(o^{(l-1)}) + b^{(l-1)} $

$ o^{(l)} = W\cdot\left(W\cdot\left(W\cdot(o^{(l-3)}) + b^{(l-3)}\right) + b^{(l-2)}\right) + b^{(l-1)} $

$ o^{(l)} = W^3\cdot o^{(l-3)} + W^2\cdot b^{(l-3)} + W\cdot b^{(l-2)} + b^{(l-1)} $

### 2

$ o^{(l)} = f\left( W\cdot(o^{(l-1)}) + b^{(l-1)} \right) $

$ o^{(l)} = f\left(W\cdot f\left(W\cdot o^{(l-2)} + b^{(l-2)}\right) + b^{(l-1)}\right)$

$@$

$$
F: \begin{cases}
o^{(0)} = x; \\
r^{(l)} = W^{(l-1)} \;@\; o^{(l-1)} + b^{(l-1)}; \\
o^{(l)} = A_f^{(l)}(r^{(l)}); \\
y = o^{(L-1)};
\end{cases}
$$

$$
\overbrace{\begin{matrix}
o^{(0)}, & o^{(1)}, & \dots,& o^{(L-2)} & o^{(L-1)} \\
W^{(0)}, & W^{(1)}, & \dots,& W^{(L-2)} & \cdot\\
\cdot & b^{(0)}, & \dots,& b^{(L-3)}, & b^{(L-2)}\\
\cdot & r^{(1)}, & \dots,& r^{(L-2)}, & r^{(L-1)}
\end{matrix}}^{L}
$$

$$ \displaystyle
W^{(l)}_{m\leftarrow n} = 
\begin{pmatrix}
    w^{(l)}_{11} & w^{(l)}_{12} & \dots & w^{(l)}_{1n} \\
    w^{(l)}_{21} & w^{(l)}_{22} & \dots & w^{(l)}_{2n} \\
        \vdots & \vdots & \ddots & \vdots \\
    w^{(l)}_{m1} & w^{(l)}_{m2} & \dots & w^{(l)}_{mn} \\
\end{pmatrix},
$$
Where  $ w^{(l)}_{ij} $ 

## Propagations

$$ C(p) = C = \sum_i{\left(y_i^{(output)} - y_i^{(actual)}\right)^2} = \lVert y^{(output)} - y^{(actual)} \rVert $$

### dC/dy

$$ \displaystyle
\frac{\partial C}{\partial y_i} = \frac{\partial\sum_j{(y_j^{(o)} - y_j^{(a)})^2}}{\partial y_i} 
    = \frac{\partial(y_0^{(o)} - y_0^{(a)})^2}{\partial y_i} + \frac{\partial(y_1^{(o)} - y_1^{(a)})^2}{\partial y_i} + \dots + \frac{\partial(y_i^{(o)} - y_i^{(a)})^2}{\partial y_i} + \dots
$$

$$ \displaystyle
\frac{\partial C}{\partial y_i} = 0 + 0 + \dots + \frac{\partial(y_i^{(o)} - y_i^{(a)})^2}{\partial y_i} + \dots
$$

$$ \displaystyle
\frac{\partial C}{\partial y_i} = 2(y_i^{(o)} - y_i^{(a)});
$$

$$ \displaystyle \bf{
\frac{dC}{dy} = 2(y^{(o)} - y^{(a)});
}
$$

### dout(l)/dout(l-1)

Actually, $ y_i $ is an output of last neuron layer, so, we have already found $ \displaystyle \frac{\partial C}{\partial o^{(L-1)}} = \frac{\partial C}{\partial y} = 2(y^{(o)} - y^{(a)}); $. But, to know the derivatives with respect to other neurons and their weights, we need to find a way to jump to previous layer. Lets recall our recurrent formula:

$$ \displaystyle
o^{(l)}_i = A_f^{(l)}(r^{(l)}_i) = A_f^{(l)}\left(W^{(l-1)}_{i*} \;@\; o^{(l-1)} + b^{(l-1)}_i\right) = 
    A_f^{(l)}\left(\sum_k{\left(W^{(l-1)}_{ik} o^{(l-1)}_k\right)} + b^{(l-1)}_i\right)
$$

$$ \displaystyle
W^{(l)}_{m\leftarrow n} = 
\begin{pmatrix}
    w^{(l)}_{11} & w^{(l)}_{12} & \dots & w^{(l)}_{1n} \\
    w^{(l)}_{21} & w^{(l)}_{22} & \dots & w^{(l)}_{2n} \\
        \vdots & \vdots & \ddots & \vdots \\
    {\bf w^{(l)}_{i1}} & {\bf w^{(l)}_{i2}} & \dots & {\bf w^{(l)}_{in}} \\
        \vdots & \vdots & \ddots & \vdots \\
    w^{(l)}_{m1} & w^{(l)}_{m2} & \dots & w^{(l)}_{mn} \\
\end{pmatrix}
$$


After the expansion, we could try to compute the derivative:

$$ \;\;\;\;\displaystyle
\frac{\partial o^{(l)}_i}{\partial o^{(l-1)}_j} = 
    {A_f^{(l)}}'\left(\sum_k{\left(W^{(l-1)}_{ik} o^{(l-1)}_k\right)} + b^{(l-1)}_i\right) \cdot \frac{\partial \left(\sum_k{\left(W^{(l-1)}_{ik} o^{(l-1)}_k\right)} + b^{(l-1)}_i\right)}{\partial o^{(l-1)}_j}
$$


Recalling that $ A_f'^{(l)} $ is by definition $ A_d^{(l)} $, and the previous sum derivative expansion (in section dC/dy), we would get:

$$ \;\;\;\;\displaystyle
\frac{\partial o^{(l)}_i}{\partial o^{(l-1)}_j} = A_d^{(l)}(r^{(l)}_i) \cdot \frac{\partial \left(\sum_k{\left(W^{(l-1)}_{ik} o^{(l-1)}_k\right)}\right)}{\partial o^{(l-1)}_j}
$$

$$ \;\;\;\;\displaystyle
\frac{\partial o^{(l)}_i}{\partial o^{(l-1)}_j} = A_d^{(l)}(r^{(l)}_i) \cdot \left( 
    \frac{\partial \left(W^{(l-1)}_{i0} o^{(l-1)}_0\right)}{\partial o^{(l-1)}_j} + 
    \frac{\partial \left(W^{(l-1)}_{i1} o^{(l-1)}_1\right)}{\partial o^{(l-1)}_j} + \dots +
    \frac{\partial \left(W^{(l-1)}_{ij} o^{(l-1)}_j\right)}{\partial o^{(l-1)}_j} + \dots
\right)
$$

$$ \;\;\;\;\displaystyle
\frac{\partial o^{(l)}_i}{\partial o^{(l-1)}_j} = A_d^{(l)}(r^{(l)}_i) \cdot \left( 
    0 + 0 + \dots +
    \frac{\partial \left(W^{(l-1)}_{ij} o^{(l-1)}_j\right)}{\partial o^{(l-1)}_j} + \dots
\right)
$$

$$ \;\;\;\;\displaystyle
\frac{\partial o^{(l)}_i}{\partial o^{(l-1)}_j} = A_d^{(l)}(r^{(l)}_i) \cdot W^{(l-1)}_{ij}
$$

$$ \;\;\;\;\displaystyle
\frac{\partial o^{(l)}}{\partial o^{(l-1)}_j} =
A_d^{(l)}(r^{(l)}) \cdot W^{(l-1)}_{*j}
\leftarrow \begin{pmatrix} v_1 \\ v_2 \\ \vdots \end{pmatrix}
$$

That gives us an opportunity, to connect $ o^{(l-1)}_j $ and $ C $ :

$$ \displaystyle
\frac{\partial C}{\partial o^{(l-1)}_j} = 
    \sum_k{ \left(
        \frac{\partial C}{\partial o^{(l)}_k} \cdot \frac{\partial o^{(l)}_k}{\partial o^{(l-1)}_j}
    \right) } = 
    \left(\frac{\partial C}{\partial o^{(l)}}\right)^T @ \left(\frac{\partial o^{(l)}}{\partial o^{(l-1)}_j}\right)
    \leftarrow \begin{pmatrix} u_1, u_2, \dots \end{pmatrix} @ \begin{pmatrix} v_1 \\ v_2 \\ \vdots \end{pmatrix}
$$

$$ \displaystyle
\frac{\partial C}{\partial o^{(l-1)}_j} = 
    \left(\frac{\partial C}{\partial o^{(l)}}\right)^T @ \left(\frac{\partial o^{(l)}}{\partial o^{(l-1)}_j}\right) = 
    \left(\left(\frac{\partial o^{(l)}}{\partial o^{(l-1)}_j}\right)^T @ \left(\frac{\partial C}{\partial o^{(l)}}\right)\right)^T=
    \left(\frac{\partial o^{(l)}}{\partial o^{(l-1)}_j}\right)^T @ \left(\frac{\partial C}{\partial o^{(l)}}\right)
$$

$$ \displaystyle
\frac{\partial C}{\partial o^{(l-1)}_j} = 
    \left(\frac{\partial o^{(l)}}{\partial o^{(l-1)}_j}\right)^T @ \left(\frac{\partial C}{\partial o^{(l)}}\right)=
    \left( A_d^{(l)}(r^{(l)}) \cdot W^{(l-1)}_{*j} \right)^T @ \left(\frac{\partial C}{\partial o^{(l)}}\right)
$$

$$ \displaystyle \bf{
\frac{\partial C}{\partial o^{(l-1)}} =
    \left( A_d^{(l)}(r^{(l)}) \cdot W^{(l-1)} \right)^T @ \left(\frac{\partial C}{\partial o^{(l)}}\right)
}
$$

### dC/dW

$$ \displaystyle
o^{(l)}_i = A_f^{(l)}(r^{(l)}_i) = 
    A_f^{(l)}\left(\sum_k{\left(W^{(l-1)}_{ik} o^{(l-1)}_k\right)} + b^{(l-1)}_i\right)
$$

$$ \displaystyle
\frac{\partial o^{(l)}_i}{\partial W^{(l-1)}_{ij}} =
A_d^{(l)}\left(\sum_k{W^{(l-1)}_{ik} o^{(l-1)}_k} + b^{(l-1)}_i\right) \cdot \frac{\partial \left(\sum_k{W^{(l-1)}_{ik} o^{(l-1)}_k} + b^{(l-1)}_i\right)}{\partial W^{(l-1)}_{ij}}
$$

$$ \displaystyle
\frac{\partial o^{(l)}_i}{\partial W^{(l-1)}_{ij}} =
A_d^{(l)}(r^{(l)}_i) \cdot o^{(l-1)}_j
$$

$$ \displaystyle
\frac{\partial C}{\partial W^{(l-1)}_{ij}} = 
\frac{\partial C}{\partial o^{(l)}_i} \cdot \frac{\partial o^{(l)}_i}{\partial W^{(l-1)}_{ij}} =
\frac{\partial C}{\partial o^{(l)}_i} \cdot \left(A_d^{(l)}(r^{(l)}_i) \cdot o^{(l-1)}_j \right)
$$

$$ \displaystyle
\frac{\partial C}{\partial W^{(l-1)}_{i*}} = 
\frac{\partial C}{\partial o^{(l)}_i} \cdot \left(A_d^{(l)}(r^{(l)}_i) \cdot o^{(l-1)} \right)^T =
\left(\frac{\partial C}{\partial o^{(l)}_i} \cdot A_d^{(l)}(r^{(l)}_i)\right) \cdot \left(o^{(l-1)}\right)^T
$$

$$ \displaystyle \bf{
\frac{\partial C}{\partial W^{(l-1)}} =
\left(\frac{\partial C}{\partial o^{(l)}} \cdot A_d^{(l)}(r^{(l)})\right) @ \left(o^{(l-1)}\right)^T}
$$

### dC/db

$$ \displaystyle
o^{(l)}_i = A_f^{(l)}(r^{(l)}_i) = 
    A_f^{(l)}\left(\sum_k{\left(W^{(l-1)}_{ik} o^{(l-1)}_k\right)} + b^{(l-1)}_i\right)
$$

$$ \displaystyle
\frac{\partial o^{(l)}_i}{\partial b^{(l-1)}_{i}} =
A_d^{(l)}\left(\sum_k{W^{(l-1)}_{ik} o^{(l-1)}_k} + b^{(l-1)}_i\right) \cdot 
\frac{\partial \left(\sum_k{W^{(l-1)}_{ik} o^{(l-1)}_k} + b^{(l-1)}_i\right)}{\partial b^{(l-1)}_{i}}
$$

$$ \displaystyle
\frac{\partial o^{(l)}_i}{\partial b^{(l-1)}_{i}} =
A_d^{(l)}(r^{(l)}_i)
$$

$$ \displaystyle
\frac{\partial C}{\partial b^{(l-1)}_{i}} = 
\frac{\partial C}{\partial o^{(l)}_i} \cdot \frac{\partial o^{(l)}_i}{\partial b^{(l-1)}_{i}} =
\frac{\partial C}{\partial o^{(l)}_i} \cdot A_d^{(l)}(r^{(l)}_i)
$$

$$ \displaystyle \bf{
\frac{\partial C}{\partial b^{(l-1)}} =
\frac{\partial C}{\partial o^{(l)}} \cdot A_d^{(l)}(r^{(l)})}
$$