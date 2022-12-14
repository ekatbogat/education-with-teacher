# Название Проекта " Обучение с учителем" 

Из «Бета-Банка» стали уходить клиенты. Каждый месяц. Немного, но заметно. Банковские маркетологи посчитали: сохранять текущих клиентов дешевле, чем привлекать новых. В нашем распоряжении исторические данные о поведении клиентов и расторжении договоров с банком.

- **Цель**

Спрогнозировать, уйдёт клиент из банка в ближайшее время или нет.

- **Задачи**

Построить модель с предельно большим значением _F1_-меры. Довести метрику до 0.59. Проверить _F1_-меру на тестовой выборке самостоятельно. Дополнительно измерить _AUC-ROC_, сравнить её значение с _F1_-мерой.

Источник данных: [https://www.kaggle.com/barelydedicated/bank-customer-churn-modeling](https://www.kaggle.com/barelydedicated/bank-customer-churn-modeling)

**Итак, изучив полученные данные,обработали их и подготовили к исследованию. Произвели преобразование категориальных признаков. Изучили целевой признак,
удалили ненужные столбцы. Масштабировали количественные переменные. Изучили корреляцию, заметили, что наибольшая корреляция наблюдается у целевого признака с возрастом, балансом на счёте и активностью клиента. Наблюдается дисбаланс классов. Разделили выборку на три части: тренировочную, валидационную (для подбора гиперпараметров) и тестовую (на которой будем тестировать модель). Обучили модель без учёта дисбаланса. Получили результаты:**

_- Логистическая регрессия (Logistic Regression)_

- F1_LR: 0.29
- ROC_auc_LR: 0.58

_- Модель 'Дерево решений' (Decision Tree Classifier) Лучшее значение max_depth:6, при котором:_

- F1_best_DTC = 0.52
- ROC_auc_DTC: 0.67

_- Модель "Случайный лес" (RandomForestClassifier)Лучшее значение best n_estimators = 71 ; best max_depth = 16 при которых:_

- F1_best_RFC = 0.56
- ROC_auc_RFC: 0.70

**Борьба с дисбалансом**

При применении гиперпараметра class_weight = 'balanced' для борьбы с дисбалансом классов модели показали:

_- Модель логистической регрессии показала результат, который значительно лучше, чем без данного гиперпараметра._

- f1 0.46
- roc_auc 0.69

_- Модель 'Дерево решений' (Decision Tree Classifier) при лучшем значение max_depth:7_

- F1: 0.53
- ROC_auc: 0.66.

_- Модель случайного леса лучшее значение best n_estimators = 31 ; best max_depth = 16 показала результат:_

- f1 0,58
- roc_auc 0,74.

**Таким образом лучший результат на текущих обучающих данных с применением гиперпараметра class_weight = 'balanced' получены также моделью "Случайный лес".**

**Увеличение выборки (upsampling)**

_- Модель логистической регрессии_

- F1_LR_up: 0.46
- ROC_auc_LR_up: 0.70

_- Модель 'Дерево решений'_

- F1_DT_up: 0.53
- ROC_auc_DT_up: 0.66

_- Модель случайный лес_

- F1: 0.58
- ROC_auc_RF_up: 0.74

**Таким образом лучший результат на текущих обучающих данных, при увеличении выборки, получены также моделью "Случайный лес". Результат с увеличением выборки почти такой же, как и при применении гиперпараметра class_weight = 'balanced'.**

**Уменьшение выборки (Downsampling)**

_- Модель логистической регрессии_

- F1: 0.46
- ROC_auc_LR_down: 0.69

_- Модель 'Дерево решений'_

- F1_DT_down: 0.52
- ROC_auc_DT_down: 0.68

_- Модель случайный лес_

- F1_RF_down: 0.56
- ROC_auc_RF_down: 0.75

**Таким образом, модели показали почти такой же результат метрик, при уменьшении выборки, как и при увеличении (Upsampling).**

**Порог классификации**

_- Модель логистической регрессии_

- Самое высокое значение F1-score = 0.46 наблюдается при пороге 0,30.

_- Модель 'Дерево решений'_

- Самое высокое значение F1-score = 0.55 наблюдается при пороге 0,35.

_- Модель случайный лес_

- Самое высокое значение F1-score = 0.59 наблюдается при пороге 0,30.

**Изменение порога классификации показал лучший результат, по сравнению с остальными способами борьбы с дисбалансом.**

**GridSearchCV**

_-Логистическая регрессия_

- F1: 0.46
- AUC_Roc: 0.75

_- Модель дерево решений_

- F1: 0.45
- AUC_roc: 0.66

_- Модель случайный лес_

- F1: 0.57
- AUC: 0.82

**Таким образом, лучший результат метрик модель Случайный лес показала при подборе параметров, методом GridSearchCV. Порог оценки F1 больше 0,59 балла:**

- F1-мера Test set: 0.61
- ROC_auc Test set: 0.74
