---
title: PyTorch模型导出为ONNX格式
description: 利用torch.onnx.export，将PyTorch模型导出为.onnx格式。
author: yu
date: 2025-04-12 20:42:32 +0800
categories: [AI, PyTorch]
tags: [AI, PyTorch]
---


## 前言

假设您已经训练得到了PyTorch模型文件（.pt文件）。下面需要将PyTorch模型导出为ONNX格式（.onnx文件）。

`torch.onnx.export` 是 PyTorch 提供的模型导出工具，用于将训练好的模型转换为 **ONNX（Open Neural Network Exchange，开放神经网络交换）** 格式，以便在其他框架（如 TensorRT、OpenVINO、ONNX Runtime）中部署。

## torch.onnx.export 参数详解

### 核心参数

#### `model`（要导出的模型）
- **作用**：传入 PyTorch 模型（nn.Module 实例）。
- **示例**：
  ```python
  weight = "best.pt"  # 训练好的模型（模型参数文件）
  model = MyModel(weight=weight)
  # model.eval()  # 可选，设置为 eval 模式（避免 Dropout/BatchNorm 影响）
  ```

#### `args`（模型输入示例）
- **作用**：
  - 提供模型的输入示例（Tensor 或 Tuple），用于追踪计算图。
  - 即迭代训练时，传给模型的张量，形状为 `(batch_size, 样本形状)`。
- **要求**：
  - 输入张量的`形状`和`数据类型`应与实际推理时一致。
  - 如果模型有多个输入，需传入 `tuple`，如 (x1, x2)。
- **示例**：
  ```python
  # 示例输入（batch=1, 3通道, 224x224图像）
  dummy_input = torch.randn(1, 3, 224, 224)
  ```

#### `f`（输出文件路径）
- **作用**：指定导出的 ONNX 文件路径（如 "./model.onnx"）。

### 关键可选参数

#### export_params（是否导出模型参数）
- **作用**：若为 `True`（**默认**），导出模型权重；若为`False`，导出无权重模型（需额外加载参数）。
- **适用场景**：分离模型结构和参数时使用。

#### `input_names` 和 `output_names`（输入/输出名称）
- **作用**：分别为输入和输出张量指定名称（便于后续部署时识别），默认为None。
- **格式**：输入/输出节点可能多个，因此这两个参数均为字符串列表`list[str]`。
- **示例**：
  ```python
  torch.onnx.export(
      ...,
      input_names=["input"],      # 输入名称（如用于 TensorRT）
      # input_names=["input1", "input2"],      # 多个输入
      output_names=["output"],     # 输出名称
  )
  ```

#### `dynamic_axes`（动态维度支持）
- **作用**：指定哪些维度是动态的（如可变 batch size 或序列长度），默认为None。
- **格式**：`dict`，键为 **input_names / output_names** 设置的 **输入 / 输出名称**，值为动态维度的索引。
- **示例**：
  ```python
  torch.onnx.export(
      ...,
      dynamic_axes={
          "input": {0: "batch"},   # 第0维（batch）动态
          "output": {0: "batch"},   # 输出同样支持动态batch
      },
  )
  ```
  dynamic_axes 中的维度名称（如 `"batch"`）可以自定义为任意字符串，只要符合 Python 变量命名规则即可。名称本身仅作为**可读性标识**，不影响模型的数学行为或部署。

#### opset_version（ONNX算子集版本）
- **作用**：指定导出的 ONNX 算子版本（不同版本支持的操作不同），默认为None。
- **推荐**：使用较新版本（如 opset_version=17），但需确保部署环境支持。
- **示例**：
  ```python
  torch.onnx.export(..., opset_version=17)
  ```

#### do_constant_folding（常量折叠优化）
- **作用**：若为 `True`（默认），则优化计算图中的常量（减少冗余计算）。

#### verbose（是否打印详细信息）
- **作用**：默认为False，不打印详细信息；若为 True，导出时打印计算图信息（调试用）。

#### training（导出时的运行模式）
- **作用**：枚举类型。用于控制模型在导出时的运行模式（训练模式或推理模式），这对包含 Dropout、BatchNorm 等行为随模式变化的层尤为重要。
- **可选值**：
  - `TrainingMode.EVAL`（默认） - 以推理模式导出模型。
  - `TrainingMode.PRESERVE` - 如果**model.training**为False，则以推理模式导出；为True以训练模式导出。
  - `TrainingMode.TRAINING` - 以训练模式导出模型，禁用可能干扰训练的优化。

#### operator_export_type
- **作用**：控制算子导出的模式，决定如何将 PyTorch 算子映射到 ONNX 算子。枚举类型。
- **可选值**：
  - `OperatorExportTypes.ONNX`（默认）：将 PyTorch 算子导出为标准 ONNX 算子。
  - `OperatorExportTypes.ONNX_FALLTHROUGH`。
  - `OperatorExportTypes.ONNX_ATEN`。
  - `OperatorExportTypes.ONNX_ATEN_FALLBACK`。

#### keep_initializers_as_inputs
- **作用**：控制是否将模型的初始化参数（如权重、偏置）作为 ONNX 图的输入节点。
  - None（默认）：
    - 如果 operator_export_type=ONNX，初始化为常量（不作为输入）。
    - 如果 operator_export_type=ONNX_ATEN，初始化为输入节点。
  - False：强制将初始化参数内联为常量。
  - True：强制将初始化参数作为输入节点（便于后续修改权重）。

#### custom_opsets
- **作用**：默认为None，指定自定义的 ONNX 算子集版本，覆盖全局 `opset_version` 的设置。
- **格式**：字典：{"opset_domain": version}，例如 {"custom_domain": 1}。
- **适用场景**：模型中使用了一些非标准 ONNX 算子（如自定义算子），需要指定其版本。

#### export_modules_as_functions
- **作用**：控制是否将 PyTorch 的子模块（nn.Module）导出为 ONNX 的 Function 节点。
  - False（默认）：将子模块展开为具体的计算图。
  - True：将子模块封装为 ONNX 的 Function，保持模块化结构。

## 导出为onnx

请看如下示例。

**模型有一个输入**：
```python
import torch
from model import MyModel

# 1. 加载预训练模型并设置为eval模式
weight = "./best_params.pt"
device = "cuda:0"

model = MyModel(weights=weight, device=device)
model.eval()

# 2. 生成示例输入（batch_size=1, 单通道, 1x4096序列）
batch_size = 1  # 批大小
sample_shape = [1, 4096]  # 样本形状shape 1x4096  序列长度(数据点数)为4096
dummy = torch.Tensor(batch_size, sample_shape[0], sample_shape[1]).to(device)
# dummy = torch.randn(batch_size, sample_shape[0], sample_shape[1]).to(device) # 这个也可以

# 3. 导出ONNX模型
torch.onnx.export(
    model,
    (dummy,),  # 一个输入
    # dummy,  # 一个输入，可以不使用tuple
    f="./best_params.onnx",
    input_names=["input"],
    output_names=["output"],
    opset_version=15,
    dynamic_axes={
        "input": {0: "batch_size", 2: "length"},  # 第0维（batch_size）动态，第2维（序列长度）动态
        "output": {0: "batch_size"}  # 输出同样支持动态batch_size
    },
)
```

**模型有多个输入**：
```python
import torch
from model import MyModel

# 1. 加载预训练模型并设置为eval模式
weight = "./best_params.pt"
device = "cuda:0"

model = MyModel(weights=weight, device=device)
model.eval()

# 2. 生成示例输入（batch_size=1, 3通道, 224x224图像）
batch_size = 1  # 批大小
sample_shape = [3, 224, 224]  # 样本形状shape 3x224x224
dummy = torch.Tensor(batch_size, sample_shape[0], sample_shape[1], sample_shape[2]).to(device)
dummy2 = torch.randn(batch_size, sample_shape[0], sample_shape[1], sample_shape[2]).to(device)

# 3. 导出ONNX模型
torch.onnx.export(
    model,
    (dummy,dummy2),  # 模型有多个输入，如两个
    f="./best_params.onnx",
    input_names=["input", "input2"],
    output_names=["output"],
    opset_version=15,
    dynamic_axes={
        "input": {0: "batch"},  # 第0维（batch）动态
        "input2": {0: "batch"},  # 第0维（batch）动态
        "output": {0: "batch"}  # 输出同样支持动态batch
    },
)
```
