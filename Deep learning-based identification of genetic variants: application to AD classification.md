# Deep learning-based identification of genetic variants: application to AD classification

## Abstract

novel-three step 

* divide the whole genome into nonoverlapping fragments of an optimal size, run CNN to select phenotype-associated fragments
* Sliding Window Association Test(SWAT) : calculate phenotype influence scores(PIS)
* run CNN on all identified SNPs to develop a classification model

ADNI data : N=981, CN = 650 AD = 331

AUC = 0.82

## Introduction

Deep learning : has been used to predict disease by handling medical imaging data

In genetic research,

- molecular phenotypes and potential transcription factor binding sites

- more recently, cature of mutation and analysis of gene regulations

***GWAS***

* Genome-wide association studies
* SNPs : 조사하고자 하는 대상의 유전서열 내 하나의 DNA 염기서열 변이

GWAS problem? : high-dimension low-sample size problem

**New model**

* divide the whole genome into nonoverlapping fragments of an optimal size, run CNN to select phenotype-associated fragments
* Sliding Window Association Test(SWAT) : calculate phenotype influence scores(PIS)
* run CNN on all identified SNPs to develop a classification model

**About AD**

* 베타- 아밀로이드
* 타우 단백질
* APOE 유전자 :  3- 4-배 가량 AD 위험 증가

AD의 진행을 막거나 느리게 하기 위해, 전구 단계에서의 AD 탐지를 위한 biomarker를 확인하는 것이 중요

**Approach**

* genome data for AD (N =981; CN=650,AD=331)
* APOE region
* CNN
* XGBoost and random forest ( gain 3.8% and 9.6% )
* 75.2% accuracy


## Materials and methods

* Study participants : ADNI data 
* Genotyping and imputation
    * Imputation : 관찰되지 않은 Genotype을 통계적 기법에 의해 추론하는 것
    * SNP call rate < 95% , Hardy-Weinberg P value<1x10^-6,MAF<1%, sex inconsistencies, sample call rate <95%
* GWAS : using PLINK
* Fragmentation of whole genome data
    * 10-200 SNPs
    * train-test-validation ( 60:20:20)
    * CNN, LSTM, LSTM-CNN, attention
    * Early stopping : prevent overfitting
    * measurement : ACC

### Deep learning

* ReLU
* Adam

![image](https://user-images.githubusercontent.com/101063108/170886783-f10a1be3-1664-41fa-af3e-2091b29d2885.png)

* backpropagation : upadate weight

![image](https://user-images.githubusercontent.com/101063108/170886792-d3ff131a-a1ef-4bd7-a6d0-ff750ba21ab1.png)

* partial derivative : chain rule
* error Y to the weight of output layer

![image](https://user-images.githubusercontent.com/101063108/170887030-d95f360e-a385-494c-a951-dff9dd262d69.png)

* error Y to the weight of hidden layer

![image](https://user-images.githubusercontent.com/101063108/170887042-dec190df-b14f-49ed-a2d5-71a728b7ebc9.png)

* k th SNP = Sk , PIS

![image](https://user-images.githubusercontent.com/101063108/170887064-4f340ace-841c-4cd3-9731-1c1cbc1fa415.png)

* select top 100 to 10000 SNPs based on the PIS
* CNN : kernel size (5) max pool size (2) fully-connected (64) -softmax
* RNN : difficult
* XGBoost : xgboost package
* RF : number of tree (10), maximum depth (3)

## Results

in first step,

1. Fig 1A.  highest accuracy :  40 SNPs
2. Fig 1B. highest accuracy : CNN and LSTM-CNN
3. computation time : sharply increased compared to CNN
4. LSTM : fragment contains more SNPs

=> 40 SNPs and CNN

![image](https://user-images.githubusercontent.com/101063108/170887552-a1e4d239-fd52-41ad-9ff6-cfce3fb026b0.png)


in second step,

1. SWAT ; Fig 2. 40 windows : PIS value( using P-value, Z-score )

![image](https://user-images.githubusercontent.com/101063108/170887565-c0c100e4-40d8-4e51-8c23-1823d7bbeff2.png)


3. Fig 3. smallest P-value : rs5117 (APOC1 gene) , rs429358 (APOE gene) - SNX14, SNX16,...

![image](https://user-images.githubusercontent.com/101063108/170887571-a4a23cbb-7e5f-44ee-a0ea-07480c38964a.png)


in third step,

1. XGBoost and Random Forest
2. highest mean accuracy of 10 cross validation :  75.02% 
3. AUC : 81.57% ( 4000 SNPs)
4. 6.3% higher (RF : 2000 SNPs), 1.94% higher (XGBoost : 1000 SNPs)
5. number of APOE :  66.7% (8.3% diff)

![image](https://user-images.githubusercontent.com/101063108/170887580-8b23fc85-5ad0-421a-92a4-eead5da7ed23.png)

![image](https://user-images.githubusercontent.com/101063108/170887601-ce0af92a-5a78-44a1-ba18-e156e9fcb274.png)

![image](https://user-images.githubusercontent.com/101063108/170887603-4460daf5-b0e1-4caa-b012-363696e18984.png)


## Discussion

* ADNI cohort (N = 981)
* novel three steps
* most significant genetic loci in APOE/APOC1/TOMM40 genes 
* 75.0 % acc. 

it is important to identify novel genetic loci related to the disease

will investigate use of quantitative endophenotypes


