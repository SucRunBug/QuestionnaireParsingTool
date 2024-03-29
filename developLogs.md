## 2023.06.26

最初我在纠结，在收集信息的时候也是让专业人士进行表格的填写，为什么不让专业人士使用问卷填写软件收集信息，然后能更直接的提取和分析信息。

考虑到偏远地区可能连信号都没有，还考虑到隐私的问题，纸质也可能用于备份，我想到了解决这几个困难的方案：
信号没有的话，专业人士上门以纸质的形式收集信息，有信号了再将纸质上的信息录入系统；
隐私问题，我认为许多公司的云存储技术已经非常成熟了，他们甚至会雇佣数据安全和管理专家来维护云端数据库，可以很放心的存入。如果依旧不放心，也可以搭建个人服务器用于存储，不过需要提升网络安全的可靠性和长期维护；
如果说非要纸质进行备份，可能需要专业人员填写了纸质问卷后，再录入系统。

如果上面的解决方案还不行，我将采取以下的思路将填好的纸质信息进行解析：
要获取纸质问卷的信息，要根据图片扫描文件的格式进行处理，我初步的思路是将扫描件中的文字以及手写文字提取出来，再通过现有算法或者大模型处理文字，获取出有效信息。
所以首先需要找到一个将PDF或者图片格式中文字提取的有效算法，之后还可以进行算法的优化。

我先是在GitHub上随便搜索了一下关于“手写文字识别”的内容，发现都是不好入手的项目，因为我之前没有什么关于机器学习项目的部署经验，算法也是些很陌生的，但不能让我一上来就放弃了吧。
我现在还可以参考的是上学期数字图像处理的课本，里面有讲到图像识别的核心技术，可惜没有实践项目。
我还在飞桨平台上找到了一些关于手写文字识别的项目，我先可以通过它们进行入门。

这些项目我也不是太懂，特别是代码部分。执行出结果后我也不知道如何利用识别出的文字，并且我要处理的调查表中有个目前来说很难处理的部分，就是一个框里面打勾还是没打勾。

后来我搞到了百度飞桨的OCR的许多学习资料，或许我能从中找到一些灵感。

## 2023.06.27

目前我的思路还是通过文字识别，然后提取信息，但以目前模型的水准，例如基于PP-OCRv3中英文超轻量预训练模型进行优化手写文字识别模型，准确率只有50%左右，很难达到预期，况且问卷中还有打勾的复选框，感觉很难完成理想的工作。
我还是更建议采用专业人士分别使用纸质和电子文档分别录入，然后纸质作为备份，电子存入私人云空间进行保密存储。

## 2023.07.08

我突然想到了，如果扫描件精度还可以，每次扫描出来的文件，对应的位置都比较准确的话，那么就可以提取指定像素区域，再进行信息识别和存储。如果扫描出的文件出现了对不准的情况，可以采用选取基准点，再根据基准点整体移动图片到准确的位置。

如果上述方案可行，第一步是将扫描件的指定区域裁剪出来，再利用手写数字识别等技术解析信息，复选框使用特征像素检测的方案。如果能成功，可以大量减少填写电子版的速度。

## 2023.07.13

为了保证每一次处理的文件都是标准格式的，可以考虑使用扫描仪或者复印机的扫描格式，我在淘宝上看到的二手扫描仪价格通常在四百多，可以将纸质文件扫描生成jpeg或者pdf格式的文件。

以下内容是Claude的回答：
如果要提取的信息位置是固定的,但不知道具体坐标,可以通过以下思路获取信息位置:

1. 利用模板匹配技术,定义一个空白简历模板,上面标注所有要提取字段的大致区域。

2. 输入实际简历图片,与模板图片进行匹配,定位出文本框的具体坐标。可以使用OpenCV等计算机视觉库的模板匹配功能。

3. 根据匹配的文本框坐标,确定要提取信息的具体位置。如果匹配得分不够理想,可以让用户进行校验修正。

4. 在确定的坐标位置,执行光学字符识别(OCR),得到文本内容。

5. 通过规则或者模型,从OCR结果中解析提取出结构化信息。

6. 重复上述流程,遍历所有要提取的字段。

这种方法通过模板匹配的定位与OCR技术的结合,可以在没有具体坐标的情况下,依然实现对固定区域信息的提取。

## 2024.01.07

灵感记录：手机扫描身份证，把信息提取出来；寄快递扫描地址图片？自动识别并填写地址；医院里扫描纸质文件，获取信息？

这些是不是已经有现成的开源算法，试着复现它们。

或者去网上找一下科普的视频以及相关论文。

主题就是图像识别的原理，是如何将图片中的文字提取出来的。



我的开发阶段：

- 原理探索（论文和网上的科普视频）
- 现有开源代码复现？
- 增加难度（图片不清晰、手写文字识别）

在过程中记得多多记录，体现工作量。



**Google**检索关键词：图像识别（都是识别出具体物品，不符合要求），身份证文字识别（有很多国内互联网做了收费的接口，比如百度、阿里等。不适合了解原理）

**GitHub**检索关键词：身份证识别（比如此[仓库](https://github.com/isee15/Card-Ocr) 使用的是百度开源的PaddleOcr，不过需要在Windows环境中运行，不过环境应该都不是大问题，只要开源就行，目前暂时不打算去跑项目了，毕竟原理都不清楚。）

**bilibili**检索关键词：身份证识别（有那种几行代码教你识别出来的，盲猜是调用了第三方API），文字识别（收藏了一个近2小时的python实战项目，普遍都是关于OCR的，我都不知道啥是OCR）



详细内容我干脆出一篇[文档](./documents/what_is_OCR.md)，用来汇总找到的所有有关OCR的内容。

## 2024.01.08

该文档中介绍了OCR的基本概念，和技术路线中每个步骤有关的相应论文总结，但是论文时间有点老，可能需要阅读最新的论文了。

在此之前，想要感受一下整体过程，我准备复现一下[基于pytorch的文字识别项目](https://www.bilibili.com/list/watchlater?oid=687540358&bvid=BV1gU4y1r7Ku&spm_id_from=333.337.top_right_bar_window_view_later.content.click&p=2)

## 2024.01.09

草草完成了计划书，大致取好了名字《基于OCR的图像识别算法研究》。

花了一点时间快速看完了昨天提到的B站项目，讲解了使用的算法及其原理，通过调试讲解了代码，整体上来说就是完成了文字识别的工作，还缺少进一步的处理。

视频最后提到了拍照搜题的软件，比如我以前用过的小猿搜题，就是OCR与信息检索的融合应用。

基础知识还是匮乏的，比如人家提到的池化、卷积操作，我想着去看李沐的视频，还是直接开始看OCR文章中提到的论文，再通过ChatGPT快速了解，如果不能了解再看《动手学深度学习》，通过那里面的内容来打基础？？？

找论文的方式：Google学术起手（筛选近2-3年）——找到引用数多的，并查看所有版本——绿色字体中间部分为期刊名，查询影响因子（10以上，至少5以上）

## 2024.01.10

注意填写在线表格，内容是毕业设计实施进度，表格链接位于毕设文件夹中的通知内，未上传至GitHub。

最近一次填写是1.15，每隔7天填写一次，填写至2.5后终止21天，直至26号继续循环。

## 2024.01.12

最近一期报告内容就填写“完成了一篇关于OCR基本原理的调研，总结了一篇文章”，并贴出GitHub链接？或者说自己搞懂了传统OCR的过程，读了文章中提到的论文，了解了OCR的技术路线。

再一个就是如果要答辩上学期的实习内容，就用OCR压缩包里的内容，作为一个demo使用。

## 2024.01.21

​	接下来我打算搞定图像预处理相关的内容，我先把我的OCR的科普文章里相关的内容和论文掌握了，再试着读一下最新有关图像预处理的论文，写一个summary，期间要去理解生僻的概念，利用ChatGPT+搜索引擎+《动手学深度学习》。

阅读论文时，主要关注“基于CNN(LeNet-5)的文字识别”

### 重要摘要

​	在传统OCR技术中，图像预处理通常是针对图像的成像问题进行修正。**常见的预处理过程包括：几何变换（透视、扭曲、旋转等）、畸变校正、去除模糊、图像增强和光线校正**等。

​	~~传统方法上采用HoG对图像进行特征提取，然而HoG对于图像模糊、扭曲等问题鲁棒性很差，对于复杂场景泛化能力不佳。~~由于深度学习的飞速发展，现在**普遍使用基于CNN的神经网络作为特征提取手段**。得益于CNN强大的学习能力，配合大量的数据可以增强特征提取的鲁棒性，面临模糊、扭曲、畸变、复杂背景和光线不清等图像问题均可以表现良好的鲁棒性。

### 生词

HoG（可以说一下它的缺点，进而体现创新？？？）

基于CNN的神经网络

特征提取（和图像预处理的关系？？？）



对于生词我打算专门创建一个记录生词概念的文档`concept_definitions.md`，放在documents的目录下。

然后我尴尬地发现，再解释生词的过程中又会出现新的生词。就像是查一个英语的英文释义，又遇到了不会的单词。我想正确的做法应该和学英语一样，别管新出现的生词，不然会无限嵌套下去。

## 2024.03.03

​	订阅了一个月gpt4，我先要确定好我需要问的问题，然后记录它的回答。从而快速地在三月五日完成我的开题报告，可能我需要围绕着我的最终目的重构整个项目。

​	目前还发现一个缺点是，无法查找中文文献。不过一般谁看中文文献啊。
