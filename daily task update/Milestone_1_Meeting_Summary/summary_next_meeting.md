1. 如果BIO evaluation中，数据库中只出现B 标签与 出现过B,I 标签有差别：比如 MISC 类的BIO scheme
（1）
seqeval = evaluate.load('seqeval')
predictions = [['O', 'O', 'B-MISC', 'I-MISC','O','O','B-LOC', 'O', 'O','O']]
references = [['O', 'O', 'B-MISC', 'O','O','O', 'B-LOC', 'O', 'O','O']]
results = seqeval.compute(predictions=predictions, references=references,mode = 'none')
print(results)
输出结果：
{'LOC': {'precision': 1.0, 'recall': 1.0, 'f1': 1.0, 'number': 1}, 
'MISC': {'precision': 0.0, 'recall': 0.0, 'f1': 0.0, 'number': 1}, 
'overall_precision': 0.5, 'overall_recall': 0.5, 'overall_f1': 0.5, 'overall_accuracy': 0.9}
（2）
seqeval = evaluate.load('seqeval')
predictions = [['O', 'O', 'B-MISC', 'B-MISC','O','O','B-LOC', 'O', 'O','O']]
references = [['O', 'O', 'B-MISC', 'O','O','O', 'B-LOC', 'O', 'O','O']]
results = seqeval.compute(predictions=predictions, references=references,mode = 'none')
print(results)
{'LOC': {'precision': 1.0, 'recall': 1.0, 'f1': 1.0, 'number': 1}, 
'MISC': {'precision': 0.5, 'recall': 1.0, 'f1': 0.6666666666666666, 'number': 1}, 
'overall_precision': 0.6666666666666666, 'overall_recall': 1.0, 'overall_f1': 0.8, 'overall_accuracy': 0.9}
只有在上述情况中，即两个 B-MISC 连续出现在 token span中才会有差别，其他所有情况比如下列情况无差别：
(1)
predictions = [['O', 'O', 'B-MISC', 'I-MISC','O','O','B-MISC', 'I-MISC', 'O','O']]
references = [['O', 'O', 'B-MISC', 'I-MISC','O','O','B-MISC', 'I-MISC', 'O','O']]
(2)
predictions = [['O', 'O', 'B-MISC', 'O','O','B-MISC','O', 'O', 'O','O']]
references = [['O', 'O', 'B-MISC', 'O','O','B-MISC', 'O', 'O', 'O','O']]

2. 论文 https://arxiv.org/pdf/2106.01760.pdf ； title：Template-Based Named Entity Recognition Using BART
Idea：能不能专门根据label 的 definition 和 label的拆分 （比如：PreferredStockSharesAuthorized 可以拆分为
preferred， stocker， shares，authorized ） 设计一个template，是否能提升model performance？
我观察 在下句子中：
tokens：
['The', 'Company', 'is', 'authorized', 'to', 'issue', 'up', 'to', '******one******', '******hundred******', '******million******', 
'shares', 'of', 'preferred', 'stock', ',', 'with', 'a', 'par', 'value', 'of', '$', '0.01', '.']
ner_tags:
['O', 'O', 'O', 'O', 'O', 'O', 'O', 'O', 'B-PreferredStockSharesAuthorized', 'I-PreferredStockSharesAuthorized', 'I-PreferredStockSharesAuthorized',
 'O', 'O', 'O', 'O', 'O', 'O', 'O', 'O', 'O', 'O', 'O', 'O', 'O']
label为PreferredStockSharesAuthorized，而且 preferred， stocker， shares，authorized，这几个词几乎全部出现在 tokens中
而且我们有每个label的definition。