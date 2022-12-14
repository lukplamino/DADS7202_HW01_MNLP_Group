# DADS7202_Group Assignment 1 (Group_MNLP)
> **`Which one is better for structured data, traditional ML or MLP?`**

<img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/Screenshot%202022-08-29%20174351.png" alt="drawing" style="width:400px;"/>

## ✨Highlight
- The best traditional machine learning is RandomForrestClassifier with 75.6% accuracy which beat MLP around +3.8% 
- Comparing to some other traditional models i.e. Logistic Regression and KNN, MLP has higher accuracy rate but MLP runtime is over 14 times longer than the traditional one
- For traditional machine learning, when select best feature with KBestSelection, number of features affect on accuracy significantanly ranging from 68% (k=20) to 75% (k=73) whereas slight impact on MLP accuracy rate (71% - 72%)


## Table of Contents
[Code.ipynp](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/%5BMNLP_Team%5D_7202_HW1_Final_Version.ipynb)
 - [1. Introduction🎯](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#1-introduction)
 - [2. Data📑](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#2-data)
 - [3. Network architecture📦 and Training🔮](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#3-network-architecture-and-training)
 - [4. Results📈](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#4-results)
 - [5. Discussion💭](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#5-discussion)
 - [6. Conclusion📊](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#6-conclusion)
 - [7. References🌐](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#7-references)
 - [Citing](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#citing)
 - [👥 Members, Percent Contribution and Responsibility](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#-members-percent-contribution-and-responsibility)
 - [🖇️End Credit ](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#%EF%B8%8Fend-credit)


## 1. Introduction🎯 

**`Binary classification`**:

This project aims to compare performance of **`traditional ML models`** and  a **`self-designed MLP network model`** by training  models that can predict if a driver will accept a coupon recommended to his/her in different driving scenarios🚗. (1: Accept coupons, 0: Deny coupons)

## 2. Data📑
#### 📍Data source: 
[In-vehicle coupon recommendation Data Set](https://archive.ics.uci.edu/ml/datasets/in-vehicle+coupon+recommendation)

This data was collected via a survey on Amazon Mechanical Turk. The survey describes different driving scenarios including the destination, current time, weather, passenger, etc., and then ask the person whether he will accept the coupon if he is the driver. 

For more information about the dataset, please refer to the paper:
Wang, Tong, Cynthia Rudin, Finale Doshi-Velez, Yimin Liu, Erica Klampfl, and Perry MacNeille. 'A bayesian framework for learning rule sets for interpretable classification.' The Journal of Machine Learning Research 18, no. 1 (2017): 2357-2393.

#### 🔎Exploratory Data Analysis(EDA): 

#### Data preparation and pre-processing:
To get data ready for model:
 - We managed the difference types of data by converting nominal data into object and ordinal data into integer with order from smallest (0) to greatest.
 - We dealt with missing value by 1) drop the column (`car`) since only 1% data available and 2) drop NULL in the residual features because about 1% is missing and distribution does not change after drop out  
 - Lastly, (`Direction_same`) removed as it shares the same information with (`direction_opp`) column

#### 🔨How to solve imbalance data:
We found some features experience imbalance problem since it is dominated by only one class (`toCoupon_GEQ5min`: All '1') or one of the class contributes to over 80% (`toCoupon_GEQ25min`)
Consequently, we drop those columns out. And responsible result ('Y') seems be fine without imbalance (60/40)

All in all, data set is ...
**Devide `21 Attributes` into 3 groups** 

**Group I. Persona attributes**

 1. **`Age`**: (<21, 21-25, 26-30, 31-35, 36-40, 41-45, 46-50, >50)
 2. **`Gender`**: (Female, Male)
 3. **`MaritalStatus`**: (Unmarried partner, Single, Married partner, Divorced, Widowed)
 4. **`Has_Children`**: (1: Has, 0: Doesn't have)
 5. **`Education`**:  (Some college - no degree, Bachelors degree, Associates degree, High School Graduate, Graduate degree (Masters or Doctorate), Some High School)\
 6. **`Occupation`**: (Unemployed, Architecture & Engineering, Student,Education&Training&Library, Healthcare Support, Healthcare Practitioners & Technical, Sales & Related, Management, Arts Design,Arts Design Entertainment Sports & Media, Computer & Mathematical, Entertainment Sports & Media, Computer & Mathematical,Life Physical Social Science, Personal Care & Service, Community & Social Services, Office & Administrative Support, Construction & Extraction, Legal, Retired,Installation Maintenance & Repair, Transportation & Material Moving,Business & Financial, Protective Service,Food Preparation & Serving Related, Production Occupations,Building & Grounds Cleaning & Maintenance, Farming Fishing & Forestry)
 7. **`Income`**: ( Less than $12500, $12500 - $24999, $25000 - $37499, $37500 - $49999, $50000 - $62499, $62500 - $74999,  $75000 - $87499, $87500 - $99999, $100000 or More)
 8. **`Bar`**: How many times do you go to a bar every month? (never, less1, 1-3, 4-8, gt8, nan)
 9. **`CoffeeHouse`**: How many times do you go to a coffee house every month? (never, less1, 1-3, 4-8, gt8, nan)
 10. **`CarryAway`**: How many times do you get take-away food every month? (never, less1, 1-3, 4-8, gt8, nan)
 11. **`RestaurantLessThan20`**: How many times do you go to a restaurant with an average expense per person of less than $20 every month? (never, less1, 1-3, 4-8, gt8, nan)
 12. **`Restaurant20To50`**: How many times do you go to a restaurant with average expense per person of $20 - $50 every month? (never, less1, 1-3, 4-8, gt8, nan)


**Group II. Coupon attributes**

 13. **`Coupon`**: The coupon for...(Restaurant(<$20), Restaurant($20-$50, Coffee House, Carry out & Take away, Bar)
 14. **`Expiration`**: The coupon expires in 1 day or in 2 hours (1d, 2h)
 
**Group III. Other attributes**

 15. **`Destination`**: (No Urgent Place, Home, Work)
 16. **`Passanger`**: Who are the passengers in the car? (Alone, Friend(s), Kid(s), Partner)
 17. **`Weather`**: (Sunny, Rainy, Snowy)
 18. **`Temperature`**: (30, 50, 80)
 19. **`Time`**: (7AM, 10AM, 2PM, 6PM, 10PM)
 20. **`toCoupon_GEQ15min`**: Driving distance to the restaurant/bar for using the coupon is greater than 15 minutes (1,0)
 21. **`Direction_same`**: Whether the restaurant/bar is in the same direction as your current destination (1,0)

#### ✂️Data splitting (train/val/test):
- `random_state` = 88, 
- `test_size` = 0.25
- **`Train Shape`**: (9059, 73)
- **`Test Shape`**: (3020, 73)


[🔝](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#highlight)

## 3. Network architecture📦 and Training🔮
We experiment on each hyperparameter with the following default hyperparameter (change each hyperparameter and keep the default for others) and evaluate the result using **`model accuracy`**  on test set

- **`Random state`**: [88, 99, 100, 110]
- **`Number of Hidden layer`**: min value = 3, max value = 5
- **`Number of Units in Hidden layer`**: [32, 64, 128, 256, 512, 1024]
- **`Activation function in Hidden layer`**: [relu, tanh, sigmoid]
- **`Dropout`**: [0.2, 0.25, 0.3]
- **`Learning rate`**: [0.001, 0.0001, 0.00001, 0.00025]
- **`Activation function in Output layer`**: [softmax, sigmoid]
- **`Loss function`**: BinaryCrossentropy
- **`Optimizer`**: [Adam, Nadam, Adamax]
- **`Batch size`**: [64, 128, 256, 512]
- **`Epoch`**: [100, 200, 300, 400, 500, 600, 800, 1,000, 1,300, 1,500, 2,000]


<img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/Train_models.png" alt="drawing" style="width:1500px;"/>

### Re-train model from the best model (Row 8)
- **`Random state`**: 88
- **`Number of Hidden layer`**: 3
- **`Number of Units in Hidden layer`**: [32, 64, 128]
- **`Activation function in Hidden layer`**: tanh
- **`Dropout`**: 0.25
- **`Learning rate`**: 0.0001
- **`Activation function in Output layer`**: sigmoid
- **`Loss function`**: BinaryCrossentropy
- **`Optimizer`**: Adam
- **`Batch size`**: 128
- **`Epoch`**: 200

<img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/Model_Summary.png" alt="drawing" style="width:500px;"/>

[🔝](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#highlight)

## 4. Results📈
 
### Traditional Machine Learning (ML)
For training traditional machine learning, we picked **`Random Forest`**, **`Logistic Regression`**, **`Decision Tree`**, **`K Nearest Neighbor`**, and **`linear Support Vector Machine`** for trianing data to compare the results of them. We chose those models due to their simplicity and fast of training.

<img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/Traditional_model.png" alt="drawing" style="width:600px;"/>

From the table, we can see that the **`Random Forest with Weighted Averages`** has the highest accuracy at **`75.6%`**.

### Multilayer Perceptron (MLP)

From the experiment, We re-train model with Hyperparameter and find model with highest accuracy, less loss and not over-fit. 

**Model performance**

We train the model with initial random weights in the first round and more 4 rounds without random seed to calculate mean±SD of accuracy as the average of the model performance  
In each round, accuracy of validate and test sets are not significantly different. That proves the model is good fit.
<img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/MLP_mean-SD.png" alt="drawing" style="width:500px;"/>

**Accuracy and Loss on Train vs Validate sets**

<img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/Train_Val.png" alt="drawing" style="width:500px;"/>

### Compare performance of Traditional Machine Learning (ML) and Multilayer Perceptron (MLP)
- All of the `Traditional Machine Learning (ML)`, K Nearest Neighbor and Logistic Regression have lower accuracy than `Multilayer Perceptron (MLP)`.
- Random Forest, Decision Tree and linear Support Vector Machine have better accuracy than `Multilayer Perceptron (MLP)`.
- Comparing `Multilayer Perceptron (MLP)` to Random Forest (the hightest accuracy), Random Forest have better accuracy than Multilayer Perceptron (MLP) around **`3.8%`**

<img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/Compare.png" alt="drawing" style="width:500px;"/>

#### Compare Runtime of Traditional Machine Learning (ML) and Multilayer Perceptron (MLP)
- The whole runtime of Traditional Machine Learning (ML) (Model no.1,2,4,5) is around 1-10 sec while the whole runtime of Multilayer Perceptron (MLP) is longer than Traditional Machine Learning (ML) by 14 times.
<img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/runtime.png" alt="drawing" style="width:400px;"/>


[🔝](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#highlight)


## 5. Discussion💭
 - Learning Rate ค่อนข้างเป็นตัวแปรที่ทำให้ผลของโมเดลแตกต่างกัน 
 <img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/lr.png" alt="drawing" style="width:250px;"/>
 อย่างไรก็ตามการที่ใช้ learning rate ต่ำลงไปเรื่อยๆ ก็อาจทำให้ accuracy ลดลงด้วยเช่นกัน
  
 - Number of epoch แปลผลตรงต่อความแม่นยำ (accuracy) ของโมเดล 
 <img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/Epoch.png" alt="drawing" style="width:250px;"/>
 แต่การที่จำนวน epoch มากขึ้นอาจทำให้โมเดลมีความ overfit เพิ่มขึ้น ตามกราฟด้านล่างนี้
 <img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/Epoch100_500.png" alt="drawing" style="width:600px;"/> 
 
 - การเพิ่ม number of layers เพียงอย่างเดียว อาจไม่ช่วยเพิ่มความแม่นยำใน MLP Model เสมอ จากการสุ่มสร้างโมเดลหลังจากปรับ number of layers = 3 เป็น number of layers = 4 (ภายใต้ parameter อื่นคงที่) ทำให้ accuracy ลดลง
 
 - ทดลองเปรียบเทียบ Optimizer ระหว่าง Adam และ Nadam บน MLP Model โดยกำหนดค่า parameter อื่นๆเท่ากัน พบว่าความแม่นยำบน validation data set แตกต่างกันน้อยมาก ที่ประมาณ 0.1%
 
 - เมื่อเพิ่ม Dense ใน Hidden layer (เช่น 64 >>> 512)  ส่งผลให้ Accuracy ของ Model เพิ่มขึ้น แต่จะเกิด Overfit เพิ่มขึ้นด้วยเช่นเดียวกัน
 
 - ถ้าใช้ Activation function in Output layer เป็น softmax Accuracy ของ Model จะน้อยกว่า แบบ sigmoid เนื่องจาก
   - sigmoid เหมาะสำหรับ Binary Classification
   - softmax เหมาะสำหรับ Multi-Classification



[🔝](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#highlight)

## 6. Conclusion📊
- จากการทดลอง train model ทั้ง Traditional Machine Learning (ML) และ Multilayer Perceptron (MLP) บน data set ชุดนี้ สามารถสรุปได้ ดังนี้
- Traditional Machine Learning (ML) ที่ให้ Accuracy สูงสุดคือ Random Forest โดยมี accuracy อยู่ที่ 75.6%
- ขณะที่ค่า mean±SD accuracy ของ Multilayer Perceptron (MLP) อยู่ที่ 71.78% ± 0.25% ซึ่ง Random Forest มีความแม่นยำสูงกว่าประมาณ 3.8%
- อย่างไรก็ตาม ก็ยังมีบาง Model ที่ให้ค่า accuracy ที่ต่ำกว่า MLP เช่น Logistic Regression (68%) และ K Nearest Neighbor (67.8%)
- ดังนั้น จึงสรุปได้ว่า สำหรับข้อมูลชุดนี้ ไม่ว่าจะเป็น Traditional Machine Learning (ML) หรือ Multilayer Perceptron (MLP) ก็ให้ผล accuracy ที่ไม่ต่างกันมากนัก

- ในเรื่องของเวลาที่ใช่ในการ train model ถ้าเทียบเวลาต่อรอบ (iteration or Epoch) พบว่า MLP ใช้เวลา 0.7 วินาที ต่อ 1 Epoch ขณะที่ Logistic Regression กับ K Nearest Neighbor ใช้เวลาเพียงเสี้ยววิ ต่อ 1 รอบ (0.0002 - 0.006 วินาที) ซึ่งถือว่า MLP ใช้เวลาและทรัพยากรค่อนข้างเยอะกว่าหลายเท่า 
- แต่ถ้าเราเทียบเวลาของ MLP กับ Random Forest และ SVM ซึ่งจะใช้เวลาต่อรอบค่อนข้างนานกว่า MLP 
- อย่างไรก็ตาม ถ้าเรารวมเวลาที่ใช้ในการ tune parameter ด้วย ก็จะพบว่า MLP ใช้เวลานานกว่า ML หลายเท่า แต่ให้ค่า accuracy ที่ไม่ต่างกันมากนัก เพราะ MLP มี parameter ที่มากกว่า และมีความเป็น Black-box ที่เราไม่สามารถอธิบายวิธีการของโมเดลได้ ทำให้ต้องใช้เวลานานในการทดลองใช้ parameter หลายๆแบบ

<img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/conclude.png" alt="drawing" style="width:500px;"/>

[🔝](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#highlight)

## 7. References🌐

### Library
- **`Pipeline`**
- **`SimpleImputer`**
- **`StandardScaler`**
- **`OneHotEncoder`**
- [**`ColumnTransformer`**](https://scikit-learn.org/stable/auto_examples/compose/plot_column_transformer_mixed_types.html)
- **`SelectKBest`**
- **`precision_recall_fscore_support`**
- **`LogisticRegression`**
- **`SVC`**
- **`KNeighborsClassifier`**
- **`RandomForestClassifier`**

### Version
<img src="https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/images/Version.png" alt="drawing" style="width:400px;"/>

### References
- _ZHENGHAO XIAO. (2021, July 3)_
[**Classification on Categorical Data Part 1**](https://www.kaggle.com/code/iyet1killer/classification-on-categorical-data-part-1-sklearn#Model-Training): Sklearn. Kaggle. 
- _Natdanai, T., Wuthipoom, K., Nuj , L., Krisana, P., Songpol, B., Phawit, B_. (2022, February 6)
[**Powered by The Deep Sleeping Crew**](https://github.com/robinoud/BADS7604_HW3_Deep-Learning?fbclid=IwAR2dfuoK7UWRjvps-sSetnGYrIjDQ6ZzNirOOvJcstEQ30aVtTYlMeuwv0c). Github.


[🔝](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#highlight)

## Citing: 
[Bib.file](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group/blob/main/Citing_MNLP.bib)

```
@Misc{MNLP,
   AUTHOR          = {Navapol San. , Pakawut Kam. , Supisara Poo. , Kantima Tec.},
   TITLE           = {Model : MLP vs. Traditional ML},
   YEAR            = {2022},
   howpublished    = "\url{https://github.com/lukplamino/DADS7202_HW01_MNLP_Group.git}"
}
```



[🔝](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#highlight)

## 👥 Members, Percent Contribution and Responsibility 
|No  |ID                |Name                              |% Contribution |Responsibility                             |
|:---:|:---:             |---                               |:---:            |:---|
|1.  |**`6410422002`**  |[Navapol San.](https://www.kaggle.com/navapol)                      |   **`25%`**     |**`Explore data`** **`Prepare dataset`** **`Experiment with traditional ML`** **`Experiment with MLP `**  
|2.  |**`6410422003`**  |[Pakawut Kam.](https://www.kaggle.com/ppakawut)                     |   **`25%`**     |**`Explore data`** **`Prepare dataset`** **`Experiment with traditional ML`** **`Experiment with MLP `** |
|3.  |**`6410422024`**  |[Supisara Poo.](https://www.kaggle.com/supisarapo)                     |   **`25%`**     |**`Explore data`** **`Prepare dataset`** **`Experiment with MLP `** **`Evaluate and conclude result`**  |
|4.  |**`6410422027`**  |[Kantima Tec.](https://www.kaggle.com/kantimatec)                     |   **`25%`**     |**`Explore data`** **`Prepare dataset`** **`Experiment with traditional ML`** **`Evaluate and conclude result`** |

[🔝](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#highlight)

## 🖇️End Credit 
This project is a part of **`DADS7202 Deep Learning`**

Term: 1 Year of education: 2022

🎓Master of Science Program in **`Data Analytics and Data Science`** (DADS)

🏫National Institute of Development Administration (**`NIDA`**)

<img src="https://img.shields.io/badge/Colab-F9AB00?style=for-the-badge&logo=googlecolab&color=525252"/> 



[🔝](https://github.com/lukplamino/DADS7202_HW01_MNLP_Group#highlight)
