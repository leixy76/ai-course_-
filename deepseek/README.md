# DeepSeekR1安装

## 一、建立虚拟环境

```shell
# 建立工作目录
mkdir deepseek
# 切换到工作目录
cd deepseek 
# 创建虚拟环境
conda create -n deepseek python=3.12 -y
# 激活虚拟环境
conda activate deepseek
```

## 二、安装依赖库

```shell
# 安装VLLM
pip install vllm==0.6.3.post1 \
-i https://pypi.mirrors.ustc.edu.cn/simple
# 卸载高版本的PyTorch
pip uninstall torch torchaudio torchvision -y
# 安装PyTorch2.4.0
pip install torch==2.4.0 torchaudio==2.4.0 torchvision==0.19.0 -f https://mirrors.tuna.tsinghua.edu.cn/pytorch-wheels/torch_stable.html
# 验证PyTorch（如果显示True则为正常）
python -c "import torch; print(torch.cuda.is_available())"
# 安装WEB页面依赖库
pip install openai==1.52.2 streamlit==1.39.0 streamlit_chat==0.1.1 \
httpx==0.27.2 -i https://pypi.mirrors.ustc.edu.cn/simple
```

## 三、下载模型

```shell
# 获取模型下载脚本
wget https://e.aliendao.cn/model_download.py
# 下载大语言模型DeepSeek-R1-Distill-Qwen-1.5B
python model_download.py --e \
--repo_id deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B \
--token YPY8KHDQ2NAHQ2SG
```

## 四、运行模型

```shell
CUDA_VISIBLE_DEVICES=0 \
vllm serve dataroot/models/deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B \
--max-model-len 8192 --disable-log-stats --enforce-eager \
--host 0.0.0.0 --port 8000 --served-model-name deepseek \
--gpu-memory-utilization 0.6
```

## 五、运行WEB界面服务

```shell
streamlit run chat_bot.py
```

