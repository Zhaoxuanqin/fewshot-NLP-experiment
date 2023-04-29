1. 跑label semantics论文的baseline。
2.把label ratio整合到一个 excel中 
3.整合完后根据relevant tags 补全 task中的label：补全 label 较多的task 的label， 补全出现次数较多的label
4.不shuffle validation set 重新跑ner 任务打印badcase
5.看error case中 需要金融背景的 label， example：RevenueFromContractWithCustomerIncludingAssessedTax，RevenueFromRelatedParties
6.Stock，LineOfCredit，Revenue，Loss，LossContingency，ShareBasedCompensation 再查看error   case，5 case， 10 case， 20 case的变化、
