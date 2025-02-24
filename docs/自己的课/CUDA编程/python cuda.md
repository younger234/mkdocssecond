# python cuda 编程相关

!!! note  "关于 python cuda 编程的一些CSDN网站"
    [python + cuda 编程(一)](https://blog.csdn.net/skyli114/article/details/127094165?spm=1001.2101.3001.6650.12&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-12-127094165-blog-145222540.235%5Ev43%5Epc_blog_bottom_relevance_base7&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-12-127094165-blog-145222540.235%5Ev43%5Epc_blog_bottom_relevance_base7&utm_relevant_index=16)
    [python + cuda 编程(二)](https://blog.csdn.net/skyli114/article/details/127095089?spm=1001.2014.3001.5502)


Python 中的 CUDA 编程通常使用 `Numba` 或 `PyCUDA` 等库来实现。`Numba` 是一个支持 CUDA 的 JIT 编译器，而 `PyCUDA` 则提供了更底层的 CUDA 接口。下面我将通过几个简单的例子来讲解如何使用 `Numba` 和 `PyCUDA` 进行 CUDA 编程。

### 1. 使用 Numba 进行 CUDA 编程

#### 安装 Numba
首先，你需要安装 `numba` 库：
```bash
pip install numba
```

#### 示例 1: 向量加法
```python
import numpy as np
from numba import cuda

# 定义 CUDA 核函数
@cuda.jit
def vector_add(a, b, c):
    i = cuda.grid(1)
    if i < len(c):
        c[i] = a[i] + b[i]

# 初始化数据
N = 100000
a = np.random.rand(N).astype(np.float32)
b = np.random.rand(N).astype(np.float32)
c = np.zeros(N, dtype=np.float32)

# 配置线程块和网格大小
threads_per_block = 256
blocks_per_grid = (N + threads_per_block - 1) // threads_per_block

# 将数据拷贝到设备
a_device = cuda.to_device(a)
b_device = cuda.to_device(b)
c_device = cuda.device_array(N, dtype=np.float32)

# 启动核函数
vector_add[blocks_per_grid, threads_per_block](a_device, b_device, c_device)

# 将结果拷贝回主机
c = c_device.copy_to_host()

# 验证结果
assert np.allclose(c, a + b)
print("向量加法成功！")
```

#### 代码解释：
1. `@cuda.jit` 装饰器用于定义 CUDA 核函数。
2. `cuda.grid(1)` 获取当前线程的全局索引。
3. `cuda.to_device()` 将数据从主机（CPU）拷贝到设备（GPU）。
4. `cuda.device_array()` 在设备上分配内存。
5. `vector_add[blocks_per_grid, threads_per_block]` 启动核函数，指定网格和线程块的大小。
6. `copy_to_host()` 将结果从设备拷贝回主机。

### 2. 使用 PyCUDA 进行 CUDA 编程

#### 安装 PyCUDA
首先，你需要安装 `pycuda` 库：
```bash
pip install pycuda
```

#### 示例 2: 矩阵乘法
```python
import pycuda.driver as cuda
import pycuda.autoinit
from pycuda.compiler import SourceModule
import numpy as np

# 定义 CUDA 核函数
mod = SourceModule("""
    __global__ void matrix_mul(float *a, float *b, float *c, int n) {
        int row = blockIdx.y * blockDim.y + threadIdx.y;
        int col = blockIdx.x * blockDim.x + threadIdx.x;
        float sum = 0.0f;
        if (row < n && col < n) {
            for (int k = 0; k < n; k++) {
                sum += a[row * n + k] * b[k * n + col];
            }
            c[row * n + col] = sum;
        }
    }
""")

# 初始化数据
N = 32
a = np.random.randn(N, N).astype(np.float32)
b = np.random.randn(N, N).astype(np.float32)
c = np.zeros((N, N), dtype=np.float32)

# 将数据拷贝到设备
a_gpu = cuda.mem_alloc(a.nbytes)
b_gpu = cuda.mem_alloc(b.nbytes)
c_gpu = cuda.mem_alloc(c.nbytes)

cuda.memcpy_htod(a_gpu, a)
cuda.memcpy_htod(b_gpu, b)

# 获取核函数
matrix_mul = mod.get_function("matrix_mul")

# 配置线程块和网格大小
block_size = (16, 16, 1)
grid_size = (int(np.ceil(N / block_size[0])), int(np.ceil(N / block_size[1])))

# 启动核函数
matrix_mul(a_gpu, b_gpu, c_gpu, np.int32(N), block=block_size, grid=grid_size)

# 将结果拷贝回主机
cuda.memcpy_dtoh(c, c_gpu)

# 验证结果
expected = np.dot(a, b)
assert np.allclose(c, expected, atol=1e-5)
print("矩阵乘法成功！")
```

#### 代码解释：
1. `SourceModule` 用于编译 CUDA 核函数。
2. `cuda.mem_alloc()` 在设备上分配内存。
3. `cuda.memcpy_htod()` 和 `cuda.memcpy_dtoh()` 分别用于将数据从主机拷贝到设备和从设备拷贝回主机。
4. `mod.get_function()` 获取编译后的核函数。
5. `block` 和 `grid` 参数用于配置线程块和网格的大小。

### 总结
- `Numba` 提供了更高级的抽象，适合快速开发和原型设计。
- `PyCUDA` 提供了更底层的控制，适合需要精细调优的场景。

通过这两个例子，你可以初步了解如何在 Python 中使用 CUDA 进行并行计算。根据你的需求选择合适的库和工具，可以大大提高计算效率。