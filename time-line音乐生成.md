# time-line音乐生成

噪音敏感性任务。





符号生成远远好于非符号，

因为符号生成至少不会引入过量噪声。而在生成过程中非常容易引入噪声，特别是当提示词未命中训练集时，幻觉现象尤其严重。

musecoco的思路： 详细描述->attribute->生成音乐 



chatgpt的尝试：详细故事描述->attribute(这部分没有问题)

attribute->生成音乐（目前存在，短片段音乐尚可）

音乐片段相结合（目前直接套用上面的模型效果较差，并没有很好解决办法）

如musegen，其原先生成的音乐片段再一次生成后，噪声明显增多，且原有风格丢失



riffusion：类stable-diffusion: 

生成效果一般，和文生图一样，引入噪声较多，个人认为降噪器或者音乐后处理过滤器应该有用。



musegen:facebook自己的编码器，audioec，在之前的论文中有写到。

compressed discrete music representation

audio - tranformer架构



musebert:

不开源训练好的模型，transformer-encoder架构，



muselang:

符号音乐生成语言，目前不具备文本转音乐的能力，

独特的音乐编码形式，把乐谱转成独特类字符串格式。个人感觉其直接生成效果还行，如果把模型加大，效果应该会好很多。





符号和非符号生成目前

最新都是基于

主要在于编码方式，后续的tranformer无非是大小不同，结构大差不差。 

编码-》transformer架构。





​	细分领域-> time-line: 迁移学习：把多股音乐相结合的方法，既要保证连贯，又要保证细节不丢失。-》目前似乎不存在该方式，style-gan在这里应该有用。

现有思路：

​	大模型生成较好的提示词，输入完整故事链，输出attribute,生成片段音乐，再进行音乐融合。