# Predicting long time contributors with knowledge units of programming languages: an empirical study

## Abstract
Long-time contributors (LTCs) are essential for the sustainability of open source software (OSS) projects, but unfortunately many developers leave early. Predicting potential LTCs early in their tenure allows project maintainers to effectively allocate resources and mentoring to enhance their development and retention. Prior study shows that developers are primarily motivated to join OSS projects by opportunities to learn and enhance their skills at different areas, including the aspects of programming languages. This motivation plays a crucial role in their continued engagement and contributions to projects. Mapping programming language expertise to developers and characterizing projects in terms of how they use programming languages can help identify developers who are more likely to become LTCs. However, prior studies on predicting LTCs do not consider programming language skills. Towards filling this gap, this paper reports an empirical study on the usage of knowledge units (KUs) of the Java programming language to predict LTCs. A KU is a cohesive set of key capabilities that are offered by one or more building blocks of a given programming language. We select 75 real-world actively maintained Java projects from GitHub. Next, we build a prediction model called KULTC, which leverages KU-based features along five different dimensions. To engineer these features, we detect and analyze KUs from the studied 75 Java projects (spanning a total of 353K commits and 168K pull requests) as well as 4,219 other Java projects in which the studied developers previously worked (spanning a total of 1.7M commits). We compare the performance of KULTC with the state-of-the-art model, which we call BAOLTC. Even though KULTC focuses exclusively on the programming language perspective, KULTC achieves a median AUC of at least 0.75 and significantly outperforms BAOLTC. Combining the features of KULTC with the features of BAOLTC results in an enhanced model (KULTC+BAOLTC) that significantly outperforms BAOLTC across different settings with a normalized AUC improvement of 16.5%. Our feature importance analysis with SHAP reveals that developer expertise in the studied project is the most influential feature dimension for predicting LTCs. Finally, we develop a cost-effective model (KULTC_DEV_EXP+BAOLTC) that significantly outperforms BAOLTC. These encouraging results can be helpful to researchers who wish to further study the developers’ engagement/retention to OSS projects or build models for predicting LTCs. Future work in this area should thus (i) consider KULTC as a baseline model and (ii) consider KU-based features in the design of models that predict LTCs.


This repository contains the replication package for our manuscript:
**"Predicting Long-time Contributors with Knowledge Units of Programming Languages: An Empirical Study"**



---

## Table of Contents

1. [Extracting KUs from Java Source Code](#1-extracting-kus-from-java-source-code)  
2. [Generate KU-based Features for Profile Dimensions](#2-generate-ku-based-features-for-profile-dimensions)  
3. [Construct Studied Models (KULTC and Baseline)](#3-construct-studied-models-kultc-and-baseline)  
4. [Hyper-parameter Tuning Analysis](#4-hyper-parameter-tuning-analysis)  
5. [Rank Models Based on Performance (AUC)](#5-rank-models-based-on-performance-auc)  
6. [Model Feature Importance Analysis](#6-model-feature-importance-analysis)  

---

## 1. Extracting KUs from Java Source Code

- **Configure Paths:**  
  Update all paths in: util/ConstantUtil.java
- **Run Main Script:**  
To extract KUs from selected project commits, execute: MasterRepositoryReleaseLevelCommitAnalyzerOtherProject.java

- **Details:**
  - Threaded implementation accelerates KU extraction.
  - Set `numberOfThreads` (default: 30) for concurrent processing.
  - Internally, this invokes:
    ```
    ChildRepositoryReleaseLevelCommitAnalyzerOtherProject.java
    ```
  - Extracted KUs for each file in every commit are saved in the designated location.

---

## 2. Generate KU-based Features for Profile Dimensions

- **Configure Paths:**  
Set paths in `util/ConstantUtil.java`.

- **Run Feature Extraction Scripts:**

| Dimension | Script |
|-----------|--------|
| Studied Project Developer Expertise | `studiedProjects/KUFeatureExtractorMaster.java` |
| Other Project Developer Expertise   | `otherProjects/KUFeatureExtractorMaster.java` |
| Collaborator Expertise              | `collaborator/KUFeatureExtractorMaster.java` |
| Studied Project Characteristics     | `projectdim/ProjectDimKUAnalyzer.java` |
| Other Project Characteristics       | `projectdim/OtherProjDimKUAnalyzer.java` |

- **Output:**  
Each script will generate a `.csv` file containing the KU feature vector for the respective dimension.

---

## 3. Construct Studied Models (KULTC and Baseline)

- **Language:** Python  
- **Required Packages:**  
`sklearn`, `numpy`, `pandas`, `xgboost`, `imblearn`, `lightgbm`, `pickle`, `multiprocessing`
- **Script**
  ltc_KU/ku-model-variation/kultc_feature_variation.py
- **Details:**
- Automatically maps selected features to corresponding models.
- Configurable parameters (e.g., boot limit).
- Uses **AutoSpearman** to remove highly correlated features.
- Builds models and saves results to the specified directory.

---

## 4. Hyper-parameter Tuning Analysis

- **Script:**
  hyper-parameter-analysis/hyper-parameter-tuning-analyzer.py
- **Purpose:**  
  Builds models using varying parameters and classification algorithms to analyze performance.

---

## 5. Rank Models Based on Performance (AUC)

- **Method Used:** Scott-Knott ESD

- **Scripts:**
  - Generate SK-Rank Input:
    ```
    ltc-model-analysis/model-sk-rank-analysis.py
    ```
  - Apply SK-Rank:
    ```
    ku-model-variation/sk-rank-models.py
    ```
---

## 6. Model Feature Importance Analysis

### SHAP Analysis

- **Purpose:**  
  Analyze feature importance using SHAP — a widely used local interpretation technique.

- **Required Package:**  
  `shap`

- **Scripts:**
  - Generate SHAP values:
    ```
    ltc-model-analysis/shap_feature_importance_analysis_trained_all_data_single_model.py
    ```
  - Analyze SHAP results:
    ```
    ku-model-variation/full_model_feature_shap.py
    ```

### Rank Features Using Scott-Knott

- **Method Call:**  
  Run the following function inside the SHAP script:
  ```python
  generate_sk_rank_input_data()

## Cite Our Work
```bibtex
@article{ahasanuzzaman2025predicting,
  title={Predicting long time contributors with knowledge units of programming languages: an empirical study},
  author={Ahasanuzzaman, Md and Oliva, Gustavo A and Hassan, Ahmed E},
  journal={Empirical Software Engineering},
  volume={30},
  number={3},
  pages={99},
  year={2025},
  publisher={Springer}
}
```
