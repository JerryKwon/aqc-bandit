# Incremental anomaly detection using contextual bandit and decision tree

<div style="text-align: right"> AI53301 Fall 2022 Advanced Quality Control </div>
<div style="text-align: right"> AIGS 20215029 Youngin Kwon </div>


## Contents

* [:triangular_flag_on_post: Objective](#\:triangular_flag_on_post\:-Objective)
* [:floppy_disk: Dataset](#\:floppy_disk\:-Dataset)
* [:earth_asia: Environment](#\:earth_asia\:-Environment)
* [:zap: How to run](#\:zap\:-How-to-run)
* [:bar_chart: Result](#\:bar-chart\:-Result)
* [:checkered_flag: Conclusion](#\:checkered_flag\:-Conclusion)
* [:clipboard: References](#\:clipboard\:-References)



## :triangular_flag_on_post: Objective

Apply incremental learning approach by combining contextual multi-armed bandit(CMAB) and decision tree for injection molding data as real industrial dataset.



## :floppy_disk: Dataset

EN: Injection modeling of wind shield side molding for Hyundai Avante(Elantra) CN7 and Genesis G80(RG3).

KO: 사출성형기 AI 데이터셋

https://www.kamp-ai.kr/front/dataset/AiDataDetail.jsp?AI_SEARCH=&page=1&DATASET_SEQ=4&EQUIP_SEL=&FILE_TYPE_SEL=&GUBUN_SEL=&WDATE_SEL=



## :earth_asia: Environment

* OS: Windows
* CPU: Intel i5-10600k
* GPU: NVIDIA GeForce GTX 1660 Super(6GB) 



## :open_file_folder: Directory configuration

```
├── imgs: images for ipynb source files
├── aqc_bandit.yaml: Anaconda environment configuration file 
├── (Download from link) moldset_labeled.csv: Datasets of injection modeling for labeled observation
├── (Download from link) unlabeled_data.csv: Datasets of injection modeling for unlabeled observation
├── baseline_molding.ipynb: Source code of baseline implementation codes for learning and evaluation
├── bandit_molding.ipynb: Source code of proposed combined method codes for learning and evaluation
├── NN_cn7.h5: Trained NN model for CN7 data
├── NN_rg3.h5: Trained NN model for RG3 data
├── (Download from link) list_launchers_cn7.pkl: Trained proposing(CMAB + tree) model for CN7 data
├── (Download from link) list_launchers_rg3.pkl: Trained proposing(CMAB + tree) model for RG3 data
└── README.md: Instructions of project
```



## :zap: How to run

1. Download supplementary material, data and trained proposed model by below link

    https://drive.google.com/drive/folders/179jpaa5bSIgAWT64D8fmpEDsIXovEwiO?usp=sharing
   
2. Locate data(.csv) and source code(.ipynb) files in same directory

   * data(.csv)
     * moldset_labeled.csv
     * unlabeled_data.csv
   * source code(.ipynb)
     * baseline_molding:  baseline implementation codes for learning and evaluation
     * bandit_molding: proposed combined method codes for learning and evaluation

3. Create environment by importing anaconda environment configuration file(.yaml)

   conda env create -f aqc_bandit.yaml

4. Add kernel for created environment

   python -m ipykernel install --user --name aqc_env --display-name "aqc_env"

5. Run each entire source code(.ipynb) for baseline models and proposed method

   * baseline_molding.ipynb
     * Support Vector Machine Classifier(SVC)
     * Deep Neural Network(DNN)
   * bandit_molding.ipynb



## :bar_chart: Result

* Overall Result

    | Metric |      | CN7  |      |      | RG3 |      |
    | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
    |        | SVM | DNN | CMAB<br />(K=6, m=1) | SVM | DNN | CMAB<br />(K=4, m=3) |
    | Accuracy | 0.98 | 0.98 | 0.79 | 0.98 | 0.98 | 0.98 |
    | Precision | 0.00 | 0.00 | 0.07 | 0.00 | 0.00 | 0.0 |
    | Recall | 0.00 | 0.00 | 0.88 | 0.00 | 0.00 | 0.0 |
    | Elapsed Time(s) | 0.01 | 659 | 430 | 0.01 | 632 | 219 |

* Confusion Matrices for data(CN7 | RG3) and models(SVM | DNN | CMAB)

  * CN7

    * SVM

      |                    | (Prediction) True | (Prediction) False |
      | :---: | :---: | :---: |
      | **(Actual) True**  | 420 | 0 |
      | **(Actual) False** | 8 | 0 |

    * DNN

      |                    | (Prediction) True | (Prediction) False |
      | :---: | :---: | :---: |
      | **(Actual) True**  | 418 | 2 |
      | **(Actual) False** | 3 | 5 |
  
    * CMAB

        |      K=6, m=1      | (Prediction) True | (Prediction) False |
        | :----------------: | :---------------: | :----------------: |
        | **(Actual) True**  |        331        |         89         |
        | **(Actual) False** |         1         |         7          |
  
  * RG3
  
    * SVM
    
      |                    | (Prediction) True | (Prediction) False |
      | :----------------: | :---------------: | :----------------: |
      | **(Actual) True**  |        347        |         0          |
      | **(Actual) False** |         8         |         0          |
    
    * DNN
    
      |                    | (Prediction) True | (Prediction) False |
      | :----------------: | :---------------: | :----------------: |
      | **(Actual) True**  |        347        |         0          |
      | **(Actual) False** |         8         |         0          |
    
    * CMAB
    
      |      K=4, m=3      | (Prediction) True | (Prediction) False |
      | :----------------: | :---------------: | :----------------: |
      | **(Actual) True**  |        347        |         0          |
      | **(Actual) False** |         8         |         0          |



## :checkered_flag: ​Conclusion

In our paper, we refer the importance of incremental anomaly detection method for real industrial field where concept drift occurs by testing injection molding dataset. So, we propose LinUCB-based anomaly detection with incremental regression tree model. The proposed model implies efficient approach which spends much less elapsed time, although it does not stable by serveral iterations. There are further ways to develop this model by swapping newly generated node with existing nodes in train step and replacing FIMT-DD based tree learner with other tree models.



## :clipboard: References

[1] Kaize Ding, Jundong Li, and Huan Liu. Interactive anomaly detection on attributed networks.
In Proceedings of the twelfth ACM international conference on web search and data mining,
pages 357–365, 2019.

[2] Dennis Soemers, Tim Brys, Kurt Driessens, Mark Winands, and Ann Nowé. Adapting to
concept drift in credit card transaction data streams using contextual bandits and decision trees.
In Proceedings of the AAAI Conference on Artificial Intelligence, volume 32, 2018.

[3] Elena Ikonomovska, Joao Gama, and Sašo Džeroski. Learning model trees from evolving data
streams. Data mining and knowledge discovery, 23(1):128–168, 2011.

[4] Wonyoung Kim, Gi-soo Kim, and Myunghee Cho Paik. Doubly robust thompson sampling with
linear payoffs. Advances in Neural Information Processing Systems, 34:15830–15840, 2021.

[5] Varun Chandola, Arindam Banerjee, and Vipin Kumar. Anomaly detection: A survey. ACM
computing surveys (CSUR), 41(3):1–58, 2009.

[6] Charu C Aggarwal. An introduction to outlier analysis. In Outlier analysis, pages 1–34.
Springer, 2017.

[7] Leo Breiman, Jerome H Friedman, Richard A Olshen, and Charles J Stone. Classification and
regression trees. Routledge, 2017.

[8] Herbert Robbins. Some aspects of the sequential design of experiments. Bulletin of the American
Mathematical Society, 58(5):527–535, 1952.

[9] Tze Leung Lai, Herbert Robbins, et al. Asymptotically efficient adaptive allocation rules.
Advances in applied mathematics, 6(1):4–22, 1985.

[10] Djallel Bouneffouf, Irina Rish, and Charu Aggarwal. Survey on applications of multi-armed
and contextual bandits. In 2020 IEEE Congress on Evolutionary Computation (CEC), pages
1–8. IEEE, 2020.

[11] Lihong Li, Wei Chu, John Langford, and Robert E Schapire. A contextual-bandit approach to
personalized news article recommendation. In Proceedings of the 19th international conference
on World wide web, pages 661–670, 2010.

[12] Wei Chu, Lihong Li, Lev Reyzin, and Robert Schapire. Contextual bandits with linear payoff
functions. In Proceedings of the Fourteenth International Conference on Artificial Intelligence
and Statistics, pages 208–214. JMLR Workshop and Conference Proceedings, 2011.

 