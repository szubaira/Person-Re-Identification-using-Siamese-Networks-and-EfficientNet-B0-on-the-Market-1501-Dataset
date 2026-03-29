## **Person Re-Identification using Siamese Networks and EfficientNet-B0 on the Market-1501 Dataset**

### **Project Overview**
This project addresses the challenge of **Person Re-Identification (Re-ID)**, which involves identifying the same individual across multiple non-overlapping camera views. To solve this, I developed a **Siamese Neural Network** using a pre-trained **EfficientNet-B0** backbone to extract highly discriminative 512-dimensional feature embeddings. Utilizing the **Market-1501 dataset**, the model was trained using a **Triplet Margin Loss** function to minimize the distance between images of the same person (positives) while maximizing the distance from different individuals (negatives). The implementation successfully optimized embedding clusters, providing a scalable solution for automated tracking and security applications.

### **Business Understanding**
**Stakeholders:** Security and surveillance agencies, urban planning departments, and large-scale retail managers.  
**Business Problem:** Manually tracking individuals through fragmented camera networks is labor-intensive and prone to human error. In sectors like retail analytics, understanding customer journeys is vital for optimizing store layouts and marketing strategies. Research indicates that automated Re-ID systems can significantly reduce response times in search-and-rescue or security incidents. However, the system must overcome challenges such as varying lighting conditions, camera angles, and occlusions to be commercially viable.

### **Data Understanding**
The analysis utilized the **Market-1501 dataset**, a benchmark for person re-identification tasks.  
* **Data Composition:** The dataset contains 32,668 annotated bounding boxes of 1,501 identities captured by six different cameras. For the Siamese architecture, the data was structured into **Anchor, Positive, and Negative (APN)** triplets.
* **Timeframe:** The data reflects real-world captures across a university campus, providing high intra-class variability.
* **Data Limitations:** The images are relatively low resolution ($128 \times 64$ or similar aspect ratios), and the dataset is limited by "pose-bias," where individuals may only be seen from specific angles (e.g., only from the back), making frontal identification more difficult.
* **Preprocessing:** Images were normalized and transformed using the `PyTorch` and `Albumentations` libraries to improve model generalization.
<img width="796" height="450" alt="Screen Shot 2026-03-30 at 00 57 11" src="https://github.com/user-attachments/assets/eef471b1-dff7-4c14-9cf7-984330a1c412" />
<img width="795" height="452" alt="Screen Shot 2026-03-30 at 00 58 24" src="https://github.com/user-attachments/assets/81f09103-a147-4dfd-9ea5-aba3700bf2f9" />

### **Modeling and Evaluation**
The project implemented a sophisticated deep learning pipeline:
* **Model Architecture:** A **Siamese Network** leveraging a pre-trained **EfficientNet-B0** (via the `timm` library). The standard classifier was replaced with a custom linear layer to output a 512-dimensional embedding vector.
* **Training Strategy:** Triplet mining was used to feed the network with sets of three images: an Anchor, a Positive (same person), and a Negative (different person).
* **Evaluation Metrics:**
    * **Triplet Margin Loss:** This was the primary objective function, ensuring that the distance between anchor and positive pairs is smaller than the distance between anchor and negative pairs by at least a specified margin.
    * **Validation Loss:** The model performance was monitored via validation loss to prevent overfitting, with the "Best Model" weights saved upon reaching the lowest validation error.
<img width="1560" height="1560" alt="final results" src="https://github.com/user-attachments/assets/c603b3e4-6ef4-4a53-9827-1f96185c12fe" />

### **Conclusion**
The project demonstrates that Siamese Networks with transfer learning (EfficientNet) are highly effective at learning identity-based features rather than just visual textures. 

**Recommendations:**
I recommend deploying this model as a microservice within a centralized surveillance platform to automate the initial "filtering" of potential matches across camera feeds, thereby augmenting human security teams.

**Future Steps:**
1.  **Metric Expansion:** Implement **Mean Average Precision (mAP)** and **Rank-1 Accuracy** to benchmark the model against state-of-the-art Re-ID research.
2.  **Hard Triplet Mining:** Update the data loader to perform "Hard Negative Mining," focusing the training on the most challenging cases to further sharpen the embedding boundaries.
3.  **Cross-Domain Adaptation:** Test the model on different datasets (e.g., DukeMTMC) to evaluate its robustness in diverse environments.
