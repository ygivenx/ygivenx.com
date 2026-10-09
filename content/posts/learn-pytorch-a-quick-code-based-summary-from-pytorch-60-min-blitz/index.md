---
title: "Learn PyTorch - a quick code-based summary from PyTorch 60-min Blitz"
date: 2023-08-22
categories: 
  - "data-science"
  - "developer"
---

- [Tensor](#aioseo-tensor)
    - [Standard NumPy like indexing](#aioseo-standard-numpy-like-indexing)
    - [Joining Tensor](#aioseo-joining-tensor)
    - [Multiplying Tensors](#aioseo-multiplying-tensors)
    - [👉🏼 In-place operations](#aioseo-in-place-operations)
    - [Bridge with NumPy](#aioseo-bridge-with-numpy)
    - [NumPy array to Tensor](#aioseo-numpy-array-to-tensor)
- [AutoGrad](#aioseo-autograd)
    - [Training a NN](#aioseo-training-a-nn)
        - [Forward Propagation](#aioseo-forward-propagation)
        - [Backward Propagation](#aioseo-backward-propagation)
        - [Adjusts the parameters proportionate to the error](#aioseo-backward-propagation)
        - [Reference: Optimize using gradient descent 3Blue1Brown](#aioseo-backward-propagation)
    - [Single Training Step in PyTorch](#aioseo-single-training-step-in-pytorch)
- [Differentiation in Autograd](#aioseo-differentiation-in-autograd)

## Tensor

- everything is tensor

- computation happens in compiled c++ code

- over 300 mathematical operations

- default float32

- seed is used to generate replicable data

### Standard NumPy like indexing

```
import torch
tensor = torch.ones(4, 4)
tensor[:, 1] = 0
print(tensor)
"""
tensor([[1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.]])
"""
```

### Joining Tensor

```
# joins tensors along an existing axis
torch.cat([tensor, tensor], dim=1) # combine tensors columnwise

"""
tensor([[1., 0., 1., 1., 1., 0., 1., 1.],
        [1., 0., 1., 1., 1., 0., 1., 1.],
        [1., 0., 1., 1., 1., 0., 1., 1.],
        [1., 0., 1., 1., 1., 0., 1., 1.]])
"""

# torch.stack is subtly different, it joins tensors on a new axis
torch.stack([tensor, tensor], dim=1)
"""
tensor([[[1., 0., 1., 1.],
         [1., 0., 1., 1.]],

        [[1., 0., 1., 1.],
         [1., 0., 1., 1.]],

        [[1., 0., 1., 1.],
         [1., 0., 1., 1.]],

        [[1., 0., 1., 1.],
         [1., 0., 1., 1.]]])
       
torch.Size([4, 2, 4])
"""
```

### Multiplying Tensors

```
# Element wise product

print(tensor.mul(tensor))

# Alternative

print(tensor * tensor)
"""
tensor([[1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.]])
tensor([[1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.]])
"""
# matrix multiplication

print(tensor.matmul(tensor))

# alternative

print(tensor @ tensor)
"""
tensor([[3., 0., 3., 3.],
        [3., 0., 3., 3.],
        [3., 0., 3., 3.],
        [3., 0., 3., 3.]])
tensor([[3., 0., 3., 3.],
        [3., 0., 3., 3.],
        [3., 0., 3., 3.],
        [3., 0., 3., 3.]])
"""
```

### 👉🏼 In-place operations

Operations with **\_** suffix are in place

```
in-place operations save some memory but can be problematic while computing derivatives because of an immediate loss of history. Therefore, it is discouraged.
```

```
print(tensor, "\n")
tensor.add_(5)
print(tensor)

"""
tensor([[1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.]]) 

tensor([[6., 5., 6., 6.],
        [6., 5., 6., 6.],
        [6., 5., 6., 6.],
        [6., 5., 6., 6.]])
"""
```

### Bridge with NumPy

```
t = torch.ones(5)
print(f"t: {t}")
n = t.numpy()
print(f"n: {n}")

"""
t: tensor([1., 1., 1., 1., 1.])
n: [1. 1. 1. 1. 1.]
"""
```

👉🏼 Any change in **tensor** will reflect in the corresponding **NumPy** array

```
t.add_(5)  # tensor

print(n)   # numpy array is updated

"""
[6. 6. 6. 6. 6.]
"""
"""
```

### NumPy array to Tensor

```
a = np.ones(5)

b = torch.from_numpy(n)

print(f"a: {a}")

print(f"b: {b}")

np.add(a, 2, out=a)

print(f"a: {a}")

print(f"b: {b}")

"""
a: [1. 1. 1. 1. 1.]
b: tensor([3., 3., 3., 3., 3.])
a: [3. 3. 3. 3. 3.]
b: tensor([3., 3., 3., 3., 3.])
"""
```

👉🏼 Any change in **NumPy** will reflect in the corresponding **Tensor**

## AutoGrad

Neural Networks are a collection of nested functions that are executed on some input data.

### Training a NN

#### Forward Propagation

Run the input data through each of the functions

#### Backward Propagation

#### Adjusts the parameters proportionate to the error

#### Reference: Optimize using gradient descent [3Blue1Brown](https://www.youtube.com/watch?v=tIeHLnjs5U8)

### Single Training Step in PyTorch

```
import torch
import torchvision

model = torchvision.models.resnet18(pretrained=True)
data = torch.rand(1, 3, 64, 64)
label = torch.rand(1, 1000)

prediction = model(data)  # forward pass

loss = (prediction - label).sum()
loss.backward()  # backward pass

optim = torch.optim.SGD(model.parameters(), lr=1e-2, momentum=0.9)

optim.step()
```

## Differentiation in Autograd

```
a = torch.tensor([2.0, 3.0], requires_grad=True)
b = torch.tensor([6.0, 4.0], requires_grad=True)

Q = 3 * a ** 3 - b ** 2

external_grad = torch.tensor([1, 1]) # this is the gradient of a scalaer function i.e. Q with itself. dQ\dQ = 1
Q.backward(gradient=external_grad)  # since Q is a vector 🤨

a.grad  # 9*a**2

Q.sum().backward()  # instead of external grad we can also aggregate first and then apply backward

a.grad  # 9*a**2
```

Mathematically,

Given a vector valued function - \`$latex vec{y} = f(\\vec{x})$\`, the gradient of \`$latex \\vec{y}$\` with respect to \`$latex \\vec{x}$\` will be a Jacobian matrix J:

\`$$latex J = \\begin {pmatrix} \\frac{\\partial{y}}{\\partial{x\_1}} … \\frac{\\partial{y}}{\\partial{x\_n}} \\end{pmatrix}$$\`
