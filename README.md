# ModelZoo
Pretrain Chinese model from CLUE 高质量中文预训练模型集合

模型和效果对比，将会在 2020-02-29 晚上 更新到项目中

介绍
---------------------------------------------
本项目是与<a href='https://github.com/CLUEbenchmark/CLUECorpus2020'>CLUECorpus2020</a>的姊妹项目，通过使用前者的预训练语料库和新版的词汇表，来做模型的预训练。详细报告见，技术报告

项目亮点：

1.提供了大模型、小模型和语义相似度模型。大模型取得了与当前中文上效果最佳的模型一致的效果，在一些任务上效果更好。

2.小模型速度比Bert-base提升8倍左右，与albert_tiny速度一致，但效果更佳；

3.语义相似度模型，用于处理语义相似度或句子对问题，有很大概率比直接用预训练模型效果要好；

4.一期支持6个分类和句子对任务，会支持CLUE benchmark所有任务；

模型下载 
---------------------------------------------
| 模型简称 | 语料 | 词汇表|直接下载 | 
| :------- | :--------- | :---------: |  :---------: | 
| **`RoBERTa-large-clue`** | **CLUECorpus2020** | **CLUEVocabulary** |  |  
| **`RoBERTa-tiny-clue`** | **CLUECorpus2020** | **CLUEVocabulary** | **[TensorFlow](https://storage.googleapis.com/cluebenchmark/pretrained_models/RoBERTa-tiny-clue.zip)**  | 
| **`RoBERTa-tiny-pair`** <br/>句子对任务专门模型| **CLUECorpus2020** | **CLUEVocabulary** | **[TensorFlow](https://storage.googleapis.com/cluebenchmark/pretrained_models/RoBERTa-tiny-pair.zip)** | 
| **`RoBERTa-large-pair`** | **CLUECorpus2020** | **CLUEVocabulary** | ** ** | 

（地址稍后更新）

<a href='https://www.cluebenchmarks.com/small_model_classification.html'>效果对比-小模型</a>
---------------------------------------------
| 模型   | Score  | 参数    | AFQMC  | TNEWS'  | IFLYTEK'   | CMNLI   |  
| :----:| :----: | :----: | :----: |:----: |:----: |:----: | 
| <a href="#">BERT-base (baseline)</a>      | 67.57% | 108M |  73.70% | 56.58%  | 60.29% | 79.69% | 
| <a href="#">ALBERT-tiny</a>      | 60.65% | 4M  | 69.92% | 53.35%  | 48.71% | 70.61% |  
| <a href="#">RoBERTa-tiny-clue</a>      | 63.60% | 7.5M  | 69.52% | 54.57%  | 57.31% | 73.10% |  

<a href='https://www.cluebenchmarks.com/classification.html'>效果对比-大模型</a>
---------------------------------------------
| 模型   | Score  | 参数    | AFQMC  | TNEWS'  | IFLYTEK'   | CMNLI   |  
| :----:| :----: | :----: | :----: |:----: |:----: |:----: | 
| <a href="#">BERT-base  (baseline)</a>      | 67.57% | 108M |  73.70% | 56.58%  | 60.29% | 79.69% | 
| <a href="#">RoBERTa-wwm-large</a>      | 69.46% | 325M | **74.44%** | **58.41%**  | 62.77% | 82.20% | 
| <a href="#">RoBERTa-large-clue</a>      | **69.68%** | **290M**  | 74.41% | 58.38%  | **63.58%** | **82.36%** |  

<a href='https://www.cluebenchmarks.com/small_model_classification.html'>效果对比-句子对pair模型</a>
---------------------------------------------
| 模型   | Score  | 参数    | AFQMC  | 
| :----:| :----: | :----: | :----:  |
| <a href="#">RoBERTa-tiny-clue</a>      | 69.52% | 7.5M| 69.52%|
| <a href="#">RoBERTa-tiny-pair</a>      | 69.93%(&#8593;0.41) |7.5M | 69.93 % |
| <a href="#">RoBERTa-large-clue</a>      | 74.00% | 92M| 74.00%|
| <a href="#">RoBERTa-large-pair</a>      | 74.41%(&#8593;0.41) |92M | 74.41% |
 
速度对比
---------------------------------------------
| 模型   | 词汇表  | 词汇表大小    | 参数量  | 训练设备  | 训练速度   | 
| :----:| :----: | :----: | :----: |:----: |:----: | 
| <a href="#">BERT-base</a>      | google_vocab  | 21128 | 102M  | TPU V3-8  | 1k steps/404s | 
| <a href="#">BERT-base</a>      | clue_vocab  | 8021(&#8595;62.04%) | 92M(&#8595;9.80%)  | TPU V3-8  | 1k steps/350s(&#8593;15.43%) | 
| <a href="#">RoBERTa-tiny-clue</a>      | clue_vocab  | 8021(&#8595;62.05%) | 7.5M(&#8595;92.6%)  | TPU V3-8  |1k steps/50s(&#8593;708.0%)  | 

模型结构
---------------------------------------------
    为方便调用，所有模型都保持和Bert-base一致的结构，并可以直接使用Bert加载。
    RoBERTa-xxx-clue.zip
        |- bert_model.ckpt      # 模型权重
        |- bert_model.meta      # 模型meta信息
        |- bert_model.index     # 模型index信息
        |- bert_config.json     # 模型参数
        |- vocab.txt            # 词表
    
一键运行.基线模型与代码 Baseline with codes
---------------------------------------------------------------------
    使用方式：
    1、克隆项目 
       git clone https://github.com/CLUEbenchmark/CLUEPretrainedModels.git
    2、进入到相应的目录
       分类任务  
           例如：
           cd CLUEPretrainedModels/baselines/models/bert
           ###cd CLUEPretrainedModels/baselines/models_pytorch/classifier_pytorch
    3、运行对应任务的脚本(GPU方式): 会自动下载模型和任务数据并开始运行。
       bash run_classifier_xxx.sh
       如运行 bash run_classifier_iflytek.sh 会开始iflytek任务的训练  
    4、tpu使用方式(可选)  
        cd CLUEPretrainedModels/baselines/models/bert/tpu  
        sh run_classifier_tnews.sh即可测试tnews任务（注意更换里面的gs路径和tpu ip）。数据和模型会自动下载和上传。
        
        cd CLUEPretrainedModels/baselines/models/roberta/tpu  
        sh run_classifier_tiny.sh即可运行所有分类任务（注意更换里面的路径,模型地址和tpu ip）  


预训练
---------------------------------------------
add something here...

问题反馈和支持
---------------------------------------------
 可以提交issue，加入讨论群(QQ:836811304)
 
 或发送邮件CLUEbenchmark@163.com

##### Research supported with Cloud TPUs from Google's TensorFlow Research Cloud (TFRC)


TODO LIST:
---------------------------------------------
1. PyTorch版本
2. 在线预测代码


