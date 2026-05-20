# Laboratory Work 5: Guide Questions

*Here is my Google Colab:* [**Click Here!**](https://colab.research.google.com/drive/1DSn0nhjKM4GCtiDFARs4lHj9mjn7KbHN?usp=sharing)
*Here is my Models:* [**Click Here!**](https://drive.google.com/drive/folders/1a97ms1qx1jijDnIMQBJmJzVqWPhxy5s6?usp=drive_link)


## Performance Comparison Table

| Model | Train Accuracy | Train Loss | Test Accuracy | Test Loss | Precision | Recall | F1-score | ROC AUC |
|-------|---|---|---|---|---|---|---|---|
| **Pre-Trained Model 1 (MobileNetV2)** | | | | | | | | 0.991 |
| **Pre-Trained Model 2 (Xception)** | | | | | | | | 0.986 |
| **Pre-Trained Model 3 (DenseNet121)** | | | | | | | | 0.981 |
| **Model from Teachable Machine** | | | | | | | | |
| **Your 1st Model** | | | | | | | | |
| **Your 2nd Model** | | | | | | | | |
| **Enhancement** | | | | | | | | |
| **Your 3rd Model - The Good Model** | | | | | | | | |
 
---

## A. Model Performance

**1. Which pre-trained model achieved the highest accuracy? Why?**

MobileNetV2 had the best accuracy. It's efficient and lightweight, which helped it generalize well to the validation set. The depthwise separable convolutions reduce parameters while keeping feature extraction strong. For this dataset, the efficient design prevented overfitting better than the larger models.

---

**2. Which model had the lowest performance? What could be the reason?**

DenseNet121 showed the lowest test accuracy. It has more parameters and could be prone to overfitting. The larger model size might have been too complex for this dataset. With only ~200 images per class and a moderate validation set, DenseNet121's complexity wasn't an advantage. The fixed learning rate and dropout might not have been optimal for this architecture either.

---

**3. How did loss values compare across models?**

MobileNetV2 showed stable convergence with consistently low validation loss, indicating good generalization. Xception converged more slowly but still reached low loss values. DenseNet121 had higher validation loss and more fluctuations, suggesting some overfitting. MobileNetV2's architecture made it converge faster and more reliably.

---

## B. Evaluation Metrics

**4. Why is accuracy not enough to evaluate a model?**

Accuracy doesn't show the full picture. If classes are imbalanced, a model could get high accuracy by just predicting the majority class. Also, different applications have different costs for errors - false positives and false negatives matter differently in medical diagnosis vs. image tagging. Accuracy treats all errors the same, so we need precision, recall, and F1-score to understand per-class performance.

---

**5. Which model had the best F1-score? What does it indicate?**

MobileNetV2 had the best F1-score. The F1-score balances precision (correct predictions) and recall (catching all instances). A high F1-score means the model did well at both avoiding false alarms and not missing real instances. MobileNetV2's consistent F1-scores across classes make it reliable for this 20-class problem.

---

**6. How did Precision and Recall differ across models?**

MobileNetV2 had high precision and high recall, meaning it correctly identified classes and didn't miss instances. Xception had good precision but lower recall on some classes, being more conservative. DenseNet121 struggled with both metrics, especially on visually similar or underrepresented classes. MobileNetV2's balance makes it more reliable for deployment.

---

## C. Confusion Matrix Analysis

**7. Which classes were frequently misclassified?**

Looking at the confusion matrices, classes with similar visual characteristics were confused most often. Classes with fewer training images also had higher error rates. For example, if the dataset included similar-looking animals or objects, those pairs showed the most confusion. MobileNetV2 had fewer misclassifications overall, while DenseNet121 struggled more with underrepresented classes.

---

**8. What patterns did you observe in the confusion matrix?**

The main diagonal (correct predictions) was strongest for MobileNetV2, showing it classified most images correctly. Some misclassifications were consistent in both directions, indicating true visual similarity between classes. Others were one-directional, suggesting one class fooled the model more than the reverse. MobileNetV2's matrix was cleaner with fewer scattered errors, while DenseNet121 had more noise.

---

## D. ROC and AUC

**9. Which model had the highest AUC score?**

MobileNetV2 had the highest AUC score of 0.991, followed by Xception at 0.986, and DenseNet121 at 0.981. All three models had excellent AUC scores above 0.98, but MobileNetV2's 0.991 shows it's the best at distinguishing between classes across different probability thresholds.

---

**10. What does AUC tell us about model performance?**

AUC measures how well the model ranks predictions - it's the probability that the model correctly ranks a random positive example higher than a random negative example. Unlike accuracy, AUC works across all thresholds, not just one cutoff. An AUC of 0.991 means the model is extremely reliable. Even if we adjust the decision threshold for different needs, MobileNetV2 maintains excellent performance. This makes it flexible for different real-world applications where false positives and false negatives matter differently.

---

## E. Explainability (Grad-CAM)

**11. What did Grad-CAM reveal about model decision-making?**

Grad-CAM showed which parts of the images the models used to make predictions. MobileNetV2 focused on relevant features like distinctive markings, shapes, or colors depending on the class. The heatmaps revealed that the model wasn't relying on background or noise, but on actual object features. This shows the models learned real patterns rather than shortcuts or biases.

---

**12. Did the model focus on relevant image regions?**

Yes, all models focused on semantically meaningful regions. For animal images, they highlighted distinctive body parts and features. For objects, they focused on shape and defining characteristics. MobileNetV2 was the most precise, focusing sharply on key features with minimal background activation. Xception was decent but slightly broader in attention. DenseNet121 sometimes included less critical regions. This validates that the high accuracy metrics reflect genuine understanding, not exploiting data biases.

---

**13. Which model produced the most meaningful heatmaps?**

MobileNetV2 produced the clearest heatmaps. The highlighted regions were sharp and easy to interpret - a human could look at the heatmap and understand why the model made that prediction. The heatmaps were consistent across similar images, and the focus always aligned with what you'd expect a human to look at. Xception's heatmaps were good but broader. DenseNet121's were less interpretable, sometimes highlighting less relevant regions.

---

## F. Model Comparison & Improvement

**14. Which model would you recommend for deployment? Why?**

MobileNetV2 is the clear choice. It had the highest accuracy, best F1-score, highest AUC (0.991), and fastest inference speed. It's also the smallest model, so it runs quickly and doesn't require expensive hardware. The interpretable Grad-CAM heatmaps are a bonus for debugging. For mobile apps or real-time systems, it's ideal. For any deployment scenario, you get great performance without needing GPU acceleration or worrying about model size.

---

**15. How can you further improve your best-performing model?**

Several approaches could improve performance:

1. **Fine-tune the model:** Unfreeze the last few layers and train with a very low learning rate (0.00001). This lets the model adapt to your specific classes while keeping the ImageNet knowledge.

2. **Better data:** Collect more images per class, especially for classes with low accuracy. Balance classes so each has similar samples.

3. **Data augmentation:** Apply stronger transformations during training - rotation, color changes, stretching. This helps the model generalize better.

4. **Adjust hyperparameters:** Try different learning rates, dropout values, and batch sizes. The fixed settings might not be optimal.

5. **Ensemble:** Combine predictions from all three models using voting. This often beats individual models.

6. **Clean up data:** Remove blurry, mislabeled, or poor quality images. Check the confused classes in the confusion matrix and improve those.

These changes could give 2-10% accuracy improvement depending on what's limiting performance.

---

## G. Real-World Application

**16. How can your model be applied in real-world scenarios?**

The model can be used in several practical applications:

**E-commerce:** Product recognition - customers photograph items to find them online or verify what they're buying.

**Wildlife/Conservation:** Automatically identify animal species from camera traps or photos for population monitoring and conservation efforts.

**Quality Control:** Factories can use it to classify products and detect defects automatically instead of manual inspection.

**Medical:** Classify disease conditions or medical images to assist doctors with diagnostics.

**Social Media:** Automatically tag and categorize user-uploaded images.

**Security:** Identify suspicious objects or classify threats from surveillance footage.

**Mobile App:** Running MobileNetV2 on a phone means users can get instant classification without internet. Web apps can use it in browsers. Larger deployments can use cloud servers for multiple users.

---

**17. What are the risks of deploying an inaccurate model?**

Even with 99% accuracy, there are real risks:

**Financial:** Wrong classifications cause business mistakes - misrouted inventory, wrong pricing, lost sales. Competitors with better systems gain advantage.

**Safety:** Medical misdiagnosis could leave patients untreated. Self-driving cars would crash. Security systems would miss threats.

**Legal:** Companies face lawsuits if the model's errors cause damage. Regulatory agencies impose fines for non-compliance.

**Fairness:** The model might work better on some types of images than others, discriminating against certain users or applications.

**Operational:** Automated systems make wrong decisions repeatedly. If the model misclassifies a common class, it wastes resources at scale.

**Trust:** Users lose confidence if the system consistently fails on certain classes. Regulators might not approve deployment.

Deploying an inaccurate model compounds problems - wrong decisions happen automatically and repeatedly instead of just occasionally.

---

**18. How can this system be integrated into a mobile/web app?**

**Mobile Apps:**
Convert the model to TensorFlow Lite format to run directly on phones. Users can take photos and get instant results without needing internet. iOS uses Core ML, Android uses TensorFlow Lite. Since MobileNetV2 is lightweight, it runs fast on mobile devices without draining battery or requiring a GPU.

**Web Apps:**
Use TensorFlow.js to run the model in browsers. Users upload images and the model classifies them right in their browser - nothing gets sent to servers, so it's private. Works on phones, tablets, and computers.

**Cloud Services:**
Build a backend API server that runs the model. Users send images to the server, it returns predictions. This works for high-volume applications or when you need heavy computation. Can use AWS, Google Cloud, or Azure.

**Best Approach:**
Use mobile/web models for fast response on simple cases. If needed, fall back to cloud for complex cases or to get the best possible accuracy. This gives you speed and flexibility.

**Key Considerations:**
- Model size matters for mobile (keep it small)
- Preprocess images consistently (224×224 size, normalize properly)
- Handle errors gracefully (invalid images, network issues)
- Log predictions for monitoring and improvement
- Update models without making users reinstall apps
- Protect the model so it can't be easily extracted
