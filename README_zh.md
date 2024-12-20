[中文版](README_zh.md)  
[English](README.md)

# ComfyUI-Workflow-Sanbu

<div align="center">
<img src="title.jpg">
<b>散步的 ComfyUI 工作流合集 | Sanbu's ComfyUI Workflow Collection</b>
</div>

<br><br>

个人用 Comfyui 工作流，转载部分会注明原作者及其对应仓库出处。
如有帮助欢迎 star，加快合集更新。

## 快速开始

ComfyUI 下载：git clone <https://github.com/comfyanonymous/ComfyUI.git>

Comfyui 管理器下载：cd custom_nodes && git clone <https://github.com/ltdrdata/ComfyUI-Manager.git>

Comfyui 汉化：cd custom_nodes && git clone <https://github.com/AIGODLIKE/AIGODLIKE-COMFYUI-TRANSLATION.git>  （随后选中页面 setting，AGL，选择中文

- **使用时，将工作流图片拖入 Comfyui 页面即可使用。**
- 运行如果遇到问题，优先更新 Comfyui 查看是否能够解决，不能解决则提交 issue

##  目录 ✨

- [一、基础](#一基础)
  - [基础操作](#基础操作)
  - [图像生成](#图像生成)
    - [SD1.5](#sd15)
    - [SDXL](#sdxl)
    - [Flux](#flux)
    - [SD3.5](#sd35)
- [二、图像标签获取](#二图像标签获取)
- [三、图像放大](#三图像放大)
- [四、图像抠图](#四图像抠图)
- [五、局部修复](#五局部修复)
- [六、图像参考 / 风格转换](#六图像参考--风格转换)
- [其他特殊工作流](#其他特殊工作流)

**其他推荐插件：**

Comfyui 页面查看硬件资源、显卡占用 <https://github.com/crystian/ComfyUI-Crystools>

### 一、基础

#### 基础操作

<div align="center">
<img src="workflow/1-basic/workflow_basic_image_operation.png">
<b>图片基础操作</b>
</div>

- 图片加载后对图片进行 batch 和 crop 操作
- 使用转接点可以无限延展接线

<div align="center">
<img src="workflow/1-basic/workflow_basic_image_operation_mask.png">
<b>mask 基础操作</b>
</div>

- 在图片结点右键，选择遮罩编辑器（MaskEditor）即可进行 mask 绘制；或传入带透明通道图像使用。

<div align="center">
<img src="workflow/1-basic/workflow_basic_image_operation_SetGetNode.png">
<b>节点复用设置</b>
</div>

- 安装复用节点插件 https://github.com/kijai/ComfyUI-KJNodes
- 创建 SetNode 节点，可以传入任意工作流的输出作为备用，根据重命名结果可操作
- 创建 GetNode 节点，可根据重命名结果获得 SetNode 节点的输出

#### 图像生成

包含普通生成图像工作流，以及条件控制（控制网络）图像生成工作流。

##### **SD1.5**

测试模型下载：<https://www.liblib.art/modelinfo/1fd281cf6bcf01b95033c03b471d8fd8>

<div align="center">
<img src="workflow/1-basic/workflow_SD1.5_txt2img_img2img.png">
<b>SD1.5 文生图与图生图工作流</b>
</div>

- 文生图(txt2img): 通过文字提示词生成图像
- 图生图(img2img): 通过参考图片和文字提示词转换成潜在编码，利用潜在编码生成新的图像
- 使用时更换 checkpoint 为任意 SD1.5 模型

<div align="center">
<img src="workflow/1-basic/workflow_SD1.5_txt2img_region_condition.png">
<b>SD1.5 条件 latent 分区域生成</b>
</div>

- 使用 Conditioning (Set Area) 结点控制不同区域生成时的条件，根据不同区域的需求得到最后的生成图像


<div align="center">
<img src="workflow/1-basic/workflow_SD1.5_controlnet_txt2img.png">
<b>SD1.5 控制网络 多个综合</b>
</div>

- 下载控制网络权重，可获取全部模型：

```python
from modelscope import snapshot_download
model_dir = snapshot_download('AI-ModelScope/ControlNet-v1-1', cache_dir='./ComfyUI/models/controlnet/')
print('安装完成')
```

<div align="center">
<img src="workflow/1-basic/workflow_SD1.5_controlnet_multi_openpose_txt2img.png">
<b>SD1.5 控制网络 openpose 多人物</b>
</div>

- 可以很轻松使用多个人物的 openpose 创建稳定的人物姿态，测试图片可以在[该地址获取](https://comfyanonymous.github.io/ComfyUI_examples/controlnet/pose_worship.png)


##### **SDXL**

测试模型下载：<https://www.liblib.art/modelinfo/506c46c91b294710940bd4b183f3ecd7>

<div align="center">
<img src="workflow/1-basic/workflow_SDXL_txt2img_img2img.png">
<b>SDXL 文生图与图生图工作流</b>
</div>

- 文生图(txt2img): 通过文字提示词生成图像
- 图生图(img2img): 通过参考图片和文字提示词转换成潜在编码，利用潜在编码生成新的图像
- 使用时更换 checkpoint 为任意 SDXL 模型

turbo 模型

lightning 模型

##### **Flux**

flux models 中存放文件夹对应：

| 下载地址 | 文件夹 |
| --- | --- |
| <https://www.modelscope.cn/models/livehouse/flux1-dev-fp8/resolve/master/flux1-dev-fp8.safetensors> | checkpoint |
| <https://www.modelscope.cn/models/AI-ModelScope/flux-fp8/resolve/master/flux1-dev-fp8-e4m3fn.safetensors> | unet |
| <https://www.modelscope.cn/models/AI-ModelScope/flux-fp8/resolve/master/flux1-schnell-fp8-e4m3fn.safetensors> | unet |
| <https://www.modelscope.cn/models/SilentAfr/flux_clip/resolve/master/clip_l.safetensors> | clip |
| <https://www.modelscope.cn/models/mapjack/Flux_1_fp18/resolve/master/t5xxl_fp8_e4m3fn.safetensors> | clip |
| <https://www.modelscope.cn/models/AI-ModelScope/FLUX.1-dev/resolve/master/ae.safetensors> | vae |

<div align="center">
<img src="workflow/1-basic/workflow_flux-dev-full_txt2img.png">
<b>Flux 统一dev fp8 文生图工作流</b>
</div>

- 包含了clip和vae的多合一模型

Flux dev 和 schnell 都没有负面提示，因此CFG 应该设置为 1.0，意味着忽略负面提示。

<div align="center">
<img src="workflow/1-basic/workflow_flux-dev_txt2img.png">
<b>Flux dev fp8 文生图工作流</b>
</div>

- clip和unet部分分开下载的模型  

<div align="center">
<img src="workflow/1-basic/workflow_flux-schnell_txt2img.png">
<b>Flux schnell fp8 文生图工作流</b>
</div>

##### **SD3.5**

模型下载地址 <https://www.modelscope.cn/models/cutemodel/comfyui-sd3.5-medium>

<div align="center">
<img src="workflow/1-basic/workflow_SD3.5_txt2img.png">
<b>sd3.5 fp8 文生图工作流</b>
</div>

- 可以只使用一个文本编码器，或者使用三个文本编码器

### 二、图像标签获取

<div align="center">
<img src="workflow/2-caption/workflow_wd14_txt2img.png">
<b>wd14 tagger 标签获取工作流</b>
</div>

<div align="center">
<img src="workflow/2-caption/workflow_florence_txt2img.png">
<b>florence caption 标签获取工作流</b>
</div>

### 三、图像放大

- 模型文件参考下载 <https://www.modelscope.cn/models/cutemodel/Resolution-model/files>
- 模型文件放在 models/upscale_models 目录下

<div align="center">
<img src="workflow/3-upscale/workflow_upscale_txt2img.png">
<b>超分辨率图像放大</b>
</div>

<div align="center">
<img src="workflow/3-upscale/workflow_upscale_resize_latent_txt2img.png">
<b>resize latent缩放 图像放大</b>
</div>

### 四、图像抠图

<div align="center">
<img src="workflow/4-segment/workflow_clipseg.png">
<b>ClipSeg 分割</b>
</div>
- 运用 clipseg , 我们可以用自然语言分割预期区域,此处支持输出软硬边缘的 mask
- 可以直接 clone 仓库到 custom_nodes 处,直接运行即可使用: https://github.com/sanbuphy/ComfyUI-CLIPSEG

SAM

### 五、局部修复与扩展

局部重绘

局部修复

面部眼部修复

图像扩展

### 六、图像参考 / 风格转换

sd1.5 ipadapter 参考

sdxl ipadapter 参考

flux redux 参考

### 其他特殊工作流

#### BizyAir

电脑资源不够，可以利用 BizyAir 组件进行0资源生图体验：

<https://github.com/siliconflow/BizyAir>

FLUX 文生图与图生图工作流

captiopn 工作流

### Reference

感谢以下作者网站提供的灵感

Comfyui 官方教程： <https://comfyanonymous.github.io/ComfyUI_examples/>