---
layout:     post
title:      模型自动调参--GridSearchCV
subtitle:   R
date:       2020-04-16
author:     QY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Obj-C
    - Runtime
    - iOS
--- 

# 模型自动调参--GridSearchCV
链接：https://zhuanlan.zhihu.com/p/109842190
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

机器学习模型中有许多参数，如何选取参数，如何组合多个参数，以达到模型的最优效果？当然，可以采用for循环的方式。sklearn中提供了一个很方便的方法GridSearchCV，可以实现自动调参，非常适用于小数据集。class sklearn.model_selection.GridSearchCV(estimator, 
                                           param_grid, 
                                           scoring=None, 
                                           fit_params=None, 
                                           n_jobs=1, 
                                           iid=True, 
                                           refit=True, 
                                           cv=None, 
                                           verbose=0, 
                                           pre_dispatch='2*n_jobs', 
                                           error_score='raise', 
                                           return_train_score='warn') estimator：所使用的分类器，比如：estimator=RandomForestClassifier(min_samples_split=100, min_samples_leaf=20, max_depth=8, max_features='sqrt', random_state=10)，并且传入除需要确定最佳的参数之外的其他参数。每个分类器都需要一个scoring参数或者score方法。param_grid：值为字典或列表，即需要最优化的参数的取值，param_grid =param_test1，param_test1 = {'n_estimators':range(10,71,10)}scoring：准确评价标准，默认为None（使用estimator的误差估计函数），这时需要使用score函数；或者如scoring='roc_auc'，根据所选模型不同，评价准则不同。cv：交叉验证参数，默认为Nonerefit：默认为True，程序将会以交叉验证训练集得到的最佳参数，重新对所有可用的训练集与测试集进行，作为最终用于性能评估的最佳模型参数。即在搜索参数结束后，用最佳参数结果再次fit一遍全部数据集。iid:默认True,为True时，默认为各个样本fold概率分布一致，误差估计为所有样本之和，而非各个fold的平均。verbose：日志冗长度，int：冗长度，0：不输出训练过程，1：偶尔输出，>1：对每个子模型都输出。n_jobs: 并行数，int：个数,-1：跟CPU核数一致, 1:默认值。pre_dispatch：指定总共分发的并行任务数。当n_jobs大于1时，数据将在每个运行点进行复制，这可能导致OOM，而设置pre_dispatch参数，则可以预先划分总共的job数量，使数据最多被复制pre_dispatch次，进行预测的常用方法和属性grid.fit()：运行网格搜索grid_scores_：给出不同参数情况下的评价结果best_params_：描述了已取得最佳结果的参数的组合best_score_：成员提供优化过程期间观察到的最好的评分属性方法：grid.fit( train_x, train_y )：运行网格搜索grid_scores_：给出不同参数情况下的评价结果best_params_：描述了已取得最佳结果的参数的组合best_score_：成员提供优化过程期间观察到的最好的评分示例：from sklearn.grid_search import GridSearchCV

classifier = RandomForestRegressor()
#将需要遍历的参数定义为字典
parameters_grid= {'n_estimators':[50,100,500,1000],
          'max_depth':[3,4,5],
          'max_features':[0.5,0.75]，
          'criterion':['entropy','gini']}
model=GridSearchCV(classifier,param_grid=parameters_grid,cv=5)
model.fit(x_train,y_train)
print('model.best_score_:: ',model.best_score_)
print('model.best_params_::  ',model.best_params_)
print('模型最高分：{:.3f}'.format(model.score(x_test,y_test)))
