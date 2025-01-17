(简体中文|[English](./README.md))

# 声纹识别
## 介绍
声纹识别是一项用计算机程序自动提取说话人特征的技术。

这个 demo 是一个从给定音频文件提取说话人特征，它可以通过使用 `PaddleSpeech` 的单个命令或 python 中的几行代码来实现。

## 使用方法
### 1. 安装
请看[安装文档](https://github.com/PaddlePaddle/PaddleSpeech/blob/develop/docs/source/install_cn.md)。

你可以从 easy，medium，hard 三中方式中选择一种方式安装。

### 2. 准备输入
这个 demo 的输入应该是一个 WAV 文件（`.wav`），并且采样率必须与模型的采样率相同。

可以下载此 demo 的示例音频：
```bash
# 该音频的内容是数字串 85236145389
wget -c https://paddlespeech.bj.bcebos.com/vector/audio/85236145389.wav
```
### 3. 使用方法
- 命令行 (推荐使用)
  ```bash
  paddlespeech vector --task spk --input 85236145389.wav

  echo -e "demo1 85236145389.wav" > vec.job
  paddlespeech vector --task spk --input vec.job

  echo -e "demo2 85236145389.wav \n demo3 85236145389.wav" | paddlespeech vector --task spk
  ```
  
  使用方法：
  ```bash
  paddlespeech vector --help
  ```
  参数：
  - `input`(必须输入)：用于识别的音频文件。
  - `model`：声纹任务的模型，默认值：`ecapatdnn_voxceleb12`。
  - `sample_rate`：音频采样率，默认值：`16000`。
  - `config`：声纹任务的参数文件，若不设置则使用预训练模型中的默认配置，默认值：`None`。
  - `ckpt_path`：模型参数文件，若不设置则下载预训练模型使用，默认值：`None`。
  - `device`：执行预测的设备，默认值：当前系统下 paddlepaddle 的默认 device。

  输出：
  ```bash
  demo  [ -5.749211     9.505463    -8.200284    -5.2075014    5.3940268
      -3.04878      1.611095    10.127234   -10.534177   -15.821609
      1.2032688   -0.35080156   1.2629458  -12.643498    -2.5758228
    -11.343508     2.3385992   -8.719341    14.213509    15.404744
      -0.39327756   6.338786     2.688887     8.7104025   17.469526
      -8.77959      7.0576906    4.648855    -1.3089896  -23.294737
      8.013747    13.891729    -9.926753     5.655307    -5.9422326
    -22.842539     0.6293588  -18.46266    -10.811862     9.8192625
      3.0070958    3.8072643   -2.3861165    3.0821571  -14.739942
      1.7594414   -0.6485091    4.485623     2.0207152    7.264915
      -6.40137     23.63524      2.9711294  -22.708025     9.93719
      20.354511   -10.324688    -0.700492    -8.783211    -5.27593
      15.999649     3.3004563   12.747926    15.429879     4.7849145
      5.6699696   -2.3826702   10.605882     3.9112158    3.1500628
      15.859915    -2.1832209  -23.908653    -6.4799504   -4.5365124
      -9.224193    14.568347   -10.568833     4.982321    -4.342062
      0.0914714   12.645902    -5.74285     -3.2141201   -2.7173362
      -6.680575     0.4757669   -5.035051    -6.7964664   16.865469
    -11.54324      7.681869     0.44475392   9.708182    -8.932846
      0.4123232   -4.361452     1.3948607    9.511665     0.11667654
      2.9079323    6.049952     9.275183   -18.078873     6.2983274
      -0.7500531   -2.725033    -7.6027865    3.3404543    2.990815
      4.010979    11.000591    -2.8873312    7.1352735  -16.79663
      18.495346   -14.293832     7.89578      2.2714825   22.976387
      -4.875734    -3.0836344   -2.9999814   13.751918     6.448228
    -11.924197     2.171869     2.0423572   -6.173772    10.778437
      25.77281     -4.9495463   14.57806      0.3044315    2.6132357
      -7.591999    -2.076944     9.025118     1.7834753   -3.1799617
      -4.9401326   23.465864     5.1685796   -9.018578     9.037825
      -4.4150195    6.859591   -12.274467    -0.88911164   5.186309
      -3.9988663  -13.638606    -9.925445    -0.06329413  -3.6709652
    -12.397416   -12.719869    -1.395601     2.1150916    5.7381287
      -4.4691963   -3.82819     -0.84233856  -1.1604277  -13.490127
      8.731719   -20.778936   -11.495662     5.8033476   -4.752041
      10.833007    -6.717991     4.504732    13.4244375    1.1306485
      7.3435574    1.400918    14.704036    -9.501399     7.2315617
      -6.417456     1.3333273   11.872697    -0.30664724   8.8845
      6.5569253    4.7948146    0.03662816  -8.704245     6.224871
      -3.2701402  -11.508579  ]
  ```

- Python API
  ```python
  import paddle
  from paddlespeech.cli import VectorExecutor

  vector_executor = VectorExecutor()
  audio_emb = vector_executor(
      model='ecapatdnn_voxceleb12',
      sample_rate=16000,
      config=None,  # Set `config` and `ckpt_path` to None to use pretrained model.
      ckpt_path=None,
      audio_file='./85236145389.wav',
      force_yes=False,
      device=paddle.get_device())
  print('Audio embedding Result: \n{}'.format(audio_emb))
  ```

  输出：
  ```bash
  # Vector Result:
   [ -5.749211     9.505463    -8.200284    -5.2075014    5.3940268
      -3.04878      1.611095    10.127234   -10.534177   -15.821609
      1.2032688   -0.35080156   1.2629458  -12.643498    -2.5758228
    -11.343508     2.3385992   -8.719341    14.213509    15.404744
      -0.39327756   6.338786     2.688887     8.7104025   17.469526
      -8.77959      7.0576906    4.648855    -1.3089896  -23.294737
      8.013747    13.891729    -9.926753     5.655307    -5.9422326
    -22.842539     0.6293588  -18.46266    -10.811862     9.8192625
      3.0070958    3.8072643   -2.3861165    3.0821571  -14.739942
      1.7594414   -0.6485091    4.485623     2.0207152    7.264915
      -6.40137     23.63524      2.9711294  -22.708025     9.93719
      20.354511   -10.324688    -0.700492    -8.783211    -5.27593
      15.999649     3.3004563   12.747926    15.429879     4.7849145
      5.6699696   -2.3826702   10.605882     3.9112158    3.1500628
      15.859915    -2.1832209  -23.908653    -6.4799504   -4.5365124
      -9.224193    14.568347   -10.568833     4.982321    -4.342062
      0.0914714   12.645902    -5.74285     -3.2141201   -2.7173362
      -6.680575     0.4757669   -5.035051    -6.7964664   16.865469
    -11.54324      7.681869     0.44475392   9.708182    -8.932846
      0.4123232   -4.361452     1.3948607    9.511665     0.11667654
      2.9079323    6.049952     9.275183   -18.078873     6.2983274
      -0.7500531   -2.725033    -7.6027865    3.3404543    2.990815
      4.010979    11.000591    -2.8873312    7.1352735  -16.79663
      18.495346   -14.293832     7.89578      2.2714825   22.976387
      -4.875734    -3.0836344   -2.9999814   13.751918     6.448228
    -11.924197     2.171869     2.0423572   -6.173772    10.778437
      25.77281     -4.9495463   14.57806      0.3044315    2.6132357
      -7.591999    -2.076944     9.025118     1.7834753   -3.1799617
      -4.9401326   23.465864     5.1685796   -9.018578     9.037825
      -4.4150195    6.859591   -12.274467    -0.88911164   5.186309
      -3.9988663  -13.638606    -9.925445    -0.06329413  -3.6709652
    -12.397416   -12.719869    -1.395601     2.1150916    5.7381287
      -4.4691963   -3.82819     -0.84233856  -1.1604277  -13.490127
      8.731719   -20.778936   -11.495662     5.8033476   -4.752041
      10.833007    -6.717991     4.504732    13.4244375    1.1306485
      7.3435574    1.400918    14.704036    -9.501399     7.2315617
      -6.417456     1.3333273   11.872697    -0.30664724   8.8845
      6.5569253    4.7948146    0.03662816  -8.704245     6.224871
      -3.2701402  -11.508579  ]
  ```

### 4.预训练模型
以下是 PaddleSpeech 提供的可以被命令行和 python API 使用的预训练模型列表：

| 模型 | 采样率
| :--- | :---: |
| ecapatdnn_voxceleb12 | 16k
