<body style="font-family: Arial, sans-serif; line-height: 1.6; color: #222; background-color: #fdfdfd;">

<h1 align="center">💧 Water Quality Risk Prediction using IoT & Machine Learning</h1>

<p align="center">
  <a href="https://colab.research.google.com/drive/1mzeq9Z5rPUNwQh1aKHJKDJvRDGqNzmlc?usp=sharing" target="_blank">
    <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab" />
  </a>
</p>

<h3 align="center">🚀 Smart Water Monitoring with AI-driven Classification (Safe / Watch / Critical)</h3>

<p align="center">
An <b>IoT-integrated Machine Learning system</b> for real-time <b>Water Quality Risk Prediction</b>.<br>
Analyzes sensor data such as <b>pH, Turbidity, Temperature, DO, BOD, Ammonia, Nitrite, H₂S, and CO₂</b><br>
to classify water quality into <b>Safe</b>, <b>Watch</b>, or <b>Critical</b> categories.<br><br>
⚡ Built with <b>Python, Scikit-Learn, XGBoost, Gradio</b>, and IoT integration (ESP32/NodeMCU).
</p>

<hr>

<h2>📘 Table of Contents</h2>
<ul>
  <li>1️⃣ <a href="#intro">Introduction</a></li>
  <li>2️⃣ <a href="#arch">System Architecture</a></li>
  <li>3️⃣ <a href="#dataset">Dataset</a></li>
  <li>4️⃣ <a href="#method">Methodology</a></li>
  <li>5️⃣ <a href="#train">Model Training & Evaluation</a></li>
  <li>6️⃣ <a href="#deploy">Deployment</a></li>
  <li>7️⃣ <a href="#results">Results</a></li>
  <li>8️⃣ <a href="#install">Installation & Usage</a></li>
  <li>9️⃣ <a href="#future">Future Work</a></li>
  <li>🔟 <a href="#refs">References</a></li>
</ul>

<hr>

<h2 id="intro">1️⃣ Introduction</h2>
<p>
Water is one of Earth’s most critical natural resources — yet <b>less than 1%</b> is easily accessible for human consumption. Rapid industrialization, pollution, and climate change have degraded global water quality. Traditional laboratory testing is <b>slow and reactive</b>.
</p>

<p>
This system leverages <b>Machine Learning (ML)</b> and <b>IoT sensors</b> to enable <b>real-time monitoring and early contamination warnings</b>.
It predicts water safety levels across three classes:
</p>

<ul>
  <li>🟢 <b>Safe</b> – All parameters within WHO/BIS limits</li>
  <li>🟠 <b>Watch</b> – Slight deviations; treatable risk</li>
  <li>🔴 <b>Critical</b> – Severe contamination; unsafe</li>
</ul>

<hr>

<h2 id="arch">2️⃣ System Architecture</h2>
<p><b>IoT sensors</b> gather data ➜ <b>ESP32/NodeMCU</b> sends readings to the cloud ➜ <b>ML ensemble</b> predicts quality ➜ <b>Gradio dashboard</b> displays live results.</p>

<pre style="background:#f4f4f4;padding:10px;border-left:4px solid #06d6a0;">
IoT Sensors ➜ ESP32/NodeMCU ➜ Cloud Database ➜ ML Model ➜ Gradio Dashboard
</pre>

<ul>
<li><b>IoT Layer:</b> pH, DO, BOD, Temp, TDS, etc.</li>
<li><b>Cloud Layer:</b> Data transmission & storage</li>
<li><b>ML Layer:</b> Ensemble Random Forest + XGBoost classifier</li>
<li><b>Interface:</b> Gradio UI for real-time prediction & visualization</li>
</ul>

<hr>

<h2 id="dataset">3️⃣ Dataset</h2>
<p>
Dataset includes IoT readings, manual lab samples, and open-source repositories (Mendeley, Kaggle).
</p>

<table border="1" cellpadding="6" cellspacing="0">
<tr><th>Parameter</th><th>Description</th><th>Unit</th></tr>
<tr><td>pH</td><td>Acidity/Alkalinity</td><td>-</td></tr>
<tr><td>Turbidity</td><td>Clarity/Suspended solids</td><td>NTU/cm</td></tr>
<tr><td>DO</td><td>Dissolved Oxygen</td><td>mg/L</td></tr>
<tr><td>BOD</td><td>Biochemical Oxygen Demand</td><td>mg/L</td></tr>
<tr><td>Temp</td><td>Temperature</td><td>°C</td></tr>
<tr><td>CO₂</td><td>Dissolved Carbon Dioxide</td><td>ppm</td></tr>
<tr><td>H₂S</td><td>Hydrogen Sulfide</td><td>mg/L</td></tr>
<tr><td>Nitrite</td><td>Nitrite Concentration</td><td>mg/L</td></tr>
<tr><td>Ammonia</td><td>Ammonia Content</td><td>mg/L</td></tr>
</table>

<p><b>Labels:</b> Safe / Watch / Critical based on WHO & BIS (IS 10500:2012)</p>

<hr>

<h2 id="method">4️⃣ Methodology</h2>

<ol>
<li><b>Data Cleaning:</b> Removed unrealistic values:
<pre style="background:#f4f4f4;padding:10px;border-left:4px solid #118ab2;">
filters = [
    ("DO(mg/L)", (1, 12)),
    ("BOD (mg/L)", (0.5, 8)),
    ("Temp", (5, 40))
]
</pre></li>

<li><b>Missing Data:</b> Handled using mean/median imputation.</li>
<li><b>Feature Engineering:</b> Derived <code>DO_to_BOD</code> and <code>Nitrite_H2S</code>.</li>
<li><b>Normalization:</b> Applied scaling for consistent feature impact.</li>
<li><b>Label Encoding:</b> 
<pre style="background:#f4f4f4;padding:10px;border-left:4px solid #118ab2;">
Label Mapping: {'CRITICAL': 0, 'SAFE': 1, 'WATCH': 2}
</pre></li>
<li><b>Model Training:</b> Random Forest ensemble tuned via grid search.</li>
<li><b>Calibration:</b> Isotonic calibration for probability reliability.</li>
</ol>

<hr>

<h2 id="train">5️⃣ Model Training & Evaluation</h2>

<h3>✅ Algorithms Tested</h3>
<ul>
<li>Logistic Regression</li>
<li>Support Vector Machine (SVM)</li>
<li><b>Random Forest Classifier (Best)</b></li>
</ul>

<table border="1" cellpadding="6" cellspacing="0">
<tr><th>Model</th><th>Accuracy</th><th>Precision</th><th>Recall</th><th>F1-score</th></tr>
<tr><td>Logistic Regression</td><td>88.6%</td><td>0.87</td><td>0.88</td><td>0.87</td></tr>
<tr><td>SVM</td><td>93.2%</td><td>0.92</td><td>0.93</td><td>0.92</td></tr>
<tr style="background:#e0ffe0;"><td><b>Random Forest</b></td><td><b>99.3%</b></td><td><b>0.99</b></td><td><b>0.99</b></td><td><b>0.99</b></td></tr>
</table>

<h4>📊 Final Evaluation:</h4>
<pre style="background:#f4f4f4;padding:10px;border-left:4px solid #06d6a0;">
✅ Final Accuracy: 0.9930

Classification Report:
               precision    recall  f1-score   support
           0 (CRITICAL)    0.99      0.98      0.99       302
           1 (SAFE)        0.99      1.00      1.00       350
           2 (WATCH)       0.99      0.99      0.99       350

Accuracy: 0.9930 | Macro Avg: 0.99 | Weighted Avg: 0.99
</pre>

<h4>🧩 Confusion Matrix:</h4>
<pre style="background:#f4f4f4;padding:10px;border-left:4px solid #118ab2;">
[[297   3   2]
 [  0 350   0]
 [  2   0 348]]
</pre>

<hr>

<h2 id="deploy">6️⃣ Deployment</h2>
<p>
The model was integrated with a <b>Gradio Web Interface</b> that allows real-time prediction and visualization.
</p>

<pre style="background:#f4f4f4;padding:10px;border-left:4px solid #06d6a0;">
python app_gradio.py
</pre>

<ul>
<li>Enter water parameter values via sliders</li>
<li>Model returns predicted category (Safe/Watch/Critical)</li>
<li>Bar chart displays calibrated class probabilities</li>
</ul>

<hr>

<h2 id="results">7️⃣ Results</h2>
<p>
Model performance demonstrates strong precision and recall across all classes.
</p>

<ul>
<li>Top influencing features: <b>Turbidity, BOD, DO, Nitrite, Ammonia</b></li>
<li>Overfit Ratio ≈ 1.0 → Excellent generalization</li>
<li>Calibrated probabilities improve real-world interpretability</li>
</ul>

<hr>

<h2 id="install">8️⃣ Installation & Usage</h2>

<h3>🔧 Requirements</h3>
<pre style="background:#f4f4f4;padding:10px;border-left:4px solid #118ab2;">
pip install -r requirements.txt
</pre>

<h3>🧩 Dependencies</h3>
<pre style="background:#f4f4f4;padding:10px;border-left:4px solid #118ab2;">
pandas
numpy
matplotlib
scikit-learn
xgboost
joblib
gradio
</pre>

<h3>🧠 Train Model</h3>
<pre style="background:#f4f4f4;padding:10px;border-left:4px solid #06d6a0;">
python train_model.py
</pre>

<h3>🌐 Launch Web App</h3>
<pre style="background:#f4f4f4;padding:10px;border-left:4px solid #06d6a0;">
python app_gradio.py
</pre>

<hr>

<h2 id="future">9️⃣ Future Work</h2>
<ul>
<li>🌍 Integrate live IoT streaming (MQTT/AWS IoT Core)</li>
<li>🧠 Incorporate deep learning (LSTM, CNN hybrids)</li>
<li>🧪 Include microbial indicators (E. coli, coliform)</li>
<li>📱 Develop mobile dashboard for rural water checks</li>
<li>⚡ REST API deployment via FastAPI</li>
</ul>

<hr>

<h2 id="refs">🔟 References</h2>
<ul>
<li>Sharma et al. (2020) – Random Forest for potability prediction</li>
<li>Chen et al. (2021) – IoT-Edge integrated framework</li>
<li>Patel & Singh (2022) – LSTM-based contamination forecast</li>
<li>Rahman et al. (2023) – ML+GIS hybrid mapping</li>
<li>Zulkifli et al. (2022) – Review of 946 studies</li>
</ul>

<hr>

<h2>👨‍💻 Contributors</h2>
<a href="www.linkedin.com/in/sarvesh-sivasankaran">Sarvesh S</a></br>
<a href="https://www.linkedin.com/in/sherin-katherina-d/">Sherin Katherina D</a></br>
<a href="https://www.linkedin.com/in/feminna-g/">Feminna G</a></br>

<h2>🏁 License</h2>
<p>This project is licensed under the <b>MIT License</b>. You are free to use, modify, and distribute with attribution.</p>

<hr>

<h2 align="center" style="color:#118ab2;">🌊 “Predict. Prevent. Protect.”</h2>
<p align="center"><i>An intelligent AI-driven approach for sustainable water management.</i></p>

</body>
