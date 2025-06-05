---
Type:
  - Page
aliases: 
Status: 
tags: 
modifiedDate: 星期一, 五月 26日 2025, 8:52:18 晚上
---
[本科毕业设计](本科毕业设计.md)

参考文章
[基于MindSpore对llama进行lora微调_llama-7b.yaml mindspore-CSDN博客](https://blog.csdn.net/atregret/article/details/133824034)

```python
from mindformers.trainer import Trainer
# 初始化预训练任务
trainer = Trainer(task='text_generation',
                  model='llama_7b',
                  pet_method='lora',
                  train_dataset="./alpaca-fastchat2048.mindrecord")
# 调用finetune接口进行微调
trainer.finetune(finetune_checkpoint="./llama.ckpt")
```
