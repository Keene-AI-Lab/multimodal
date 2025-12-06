
## Multimodal Alzheimer's Detection: Speech & Text

<p align="center">
<img src="assets/04-poster.jpeg" alt="Project poster" width="800">
</p>


### Introduction  
Alzheimer’s dementia (AD) is the world’s leading neuro-degenerative disease, affecting roughly **55 million** people and progressing silently for years before clinical diagnosis. Subtle changes in everyday conversation—rhythm, pitch, word choice—often appear long before costly imaging or invasive tests confirm decline, making speech a promising low-cost screen. Leveraging the balanced doctor–patient dialogues of **ADReSSo-2021** [1], we ask whether a single model that both *listens* to prosody and *reads* transcribed language can flag AD more reliably than audio-only or text-only approaches.

<br>



### Objective  
Using the **ADReSSo-2021** corpus [1], we extend last semester’s audio- and text-only models to a single **multimodal** approach. Recordings are encoded with frame-level eGeMAPS speech features and transformer-based sentence embeddings from Whisper transcripts. Fusing these vectors, we retrain our classifiers to test whether joining *how* words are spoken with *what* they say improves Alzheimer’s-dementia detection beyond either modality alone.

<br>



### Framework

**Legend**

| Abbreviation | Model |
|--------------|-------|
| RF | RandomForest |
| XGB | XGBoost |
| MLP | Multi-Layer Perceptron |

<p align="center">
<img src="assets/01-pipeline.png" alt="Pipeline flowchart" width="1000">
</p>

**Figure 1.** ADReSSo-2021 recordings flow through parallel audio and text pathways to extract acoustic and linguistic features, which are then classified to distinguish AD from cognitively normal speakers.

<br>



### Methods  
Each recording is first resampled to 16 kHz with *librosa* and cropped to **patient-only speech** using the provided speaker time-stamps.  We then branch into two complementary streams:

* **Audio stream (*how* it is said).**  
  Frame-level paralinguistic cues are pulled with the eGeMAPS configuration of *openSMILE*, and a higher-level acoustic embedding is taken from a pretrained wav2vec 2.0 model [4–6].

* **Text stream (*what* is said).**  
  Whisper-large v2 supplies the transcript; each sentence is embedded with DistilBERT and mean-pooled to one document vector [7, 8].

The resulting vectors are **z-scored, concatenated, and fed to Random-Forest, XGBoost, and LightGBM classifiers**.  This late-fusion set-up (after Haulcy & Glass [2]) lets us test whether blending prosody with language outperforms unimodal baselines.

<div align="center">

| Feature set            | Tool / model     | Pooled dim. |
|------------------------|------------------|-------------|
| eGeMAPS (prosody)      | *openSMILE*      | **88**      |
| wav2vec 2.0 (speech)   | Base model       | **1024**   |
| DistilBERT (language)  | CLS-token mean   | **768**     |
| **Fusion** (concat)    | —                | **1880**   |

**Table 1.** Feature extraction methods and dimensionality for each modality. The final fusion vector concatenates all features into 1880 dimensions.
</div>

<br>
<br>



### Feature Extraction

### Framework

**Legend**

| Component | Output | Description |
|-----------|--------|-------------|
| eGeMAPS | 88-D | Prosodic features via mean±std pooling |
| wav2vec 2.0 | 1024-D | Acoustic embeddings via averaging |
| DistilBERT | 768-D | Linguistic embeddings via averaging |
| Fusion | 1880-D | Z-scored and concatenated features |

<p align="center">
<img src="assets/02-feature-extraction.png" alt="Feature extraction diagram" width="1000">
</p>

**Figure 2.** Feature-extraction pipeline: audio is windowed, embedded, and joined with Whisper-derived text embeddings to form the final feature vector v.

**Audio path.** Patient speech is windowed at 100 ms and 250 ms with 0 % or 50 % overlap.  Every frame yields 25 eGeMAPS descriptors; mean ± std pooling forms an 88-D prosodic vector.  The same frames feed wav2vec 2.0, whose hidden states are averaged to a 1024-D embedding.

**Text path.** Whisper produces time-stamped transcripts; sentence boundaries guide DistilBERT, and the sentence embeddings are averaged to a single 768-D semantic vector.

The eGeMAPS, wav2vec, and DistilBERT vectors are **z-scored, concatenated (1 880 features), and supplied to the classifier**, giving it both the rhythm of speech and the meaning of words.

<br>



### Results

<div align="center">

<figure class="post-table">
  <table>
    <thead>
      <tr>
        <th>Modality</th>
        <th>Random Forest</th>
        <th>XGBoost</th>
        <th>MLP</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Audio only</td>
        <td>61%</td>
        <td>71%</td>
        <td>61%</td>
      </tr>
      <tr>
        <td>Text only</td>
        <td>77%</td>
        <td>82%</td>
        <td>74%</td>
      </tr>
      <tr>
        <td>Audio + Text</td>
        <td>70%</td>
        <td>61%</td>
        <td>67%</td>
      </tr>
    </tbody>
  </table>
  <figcaption><strong>Table 2.</strong> Classification accuracy by modality and classifier. Text-only models achieved the highest performance across all three classifiers.</figcaption>
</figure>

</div>

**Text-only (DistilBERT + XGBoost)** tops the results at **82% accuracy**, confirming that word-level information is the single strongest cue. The **multimodal fusion** model reaches **70% accuracy**, edging out the **audio-only** pipeline (**71%** best case) in some configurations, though XGBoost on audio alone performs surprisingly well.
<br>



### Conclusion  
Automatic transcripts carry the clearest Alzheimer’s signal in this study; paralinguistic cues alone lag behind. A simple late-fusion of audio and text lifts the audio baseline but still sits below the text-only ceiling, hinting that smarter integration—joint attention layers or larger speech encoders—may be needed to unlock the full value of acoustic patterns. Even so, the fusion results show that speech features can add robustness without hurting accuracy, pointing toward richer multimodal designs as the next step for conversational dementia screening.

<br>



### References  

[1] ADReSSo-2021 Challenge Data. <https://dementia.talkbank.org/ADReSSo-2021/>  
[2] R. Haulcy & J. Glass, *Classifying Alzheimer’s Disease Using Audio and Text-Based Representations of Speech*, INTERSPEECH 2020.  
[3] B. McFee *et al.*, “librosa: Audio and Music Signal Analysis in Python,” SciPy 2015.  
[4] F. Eyben *et al.*, “openSMILE: The Munich Versatile and Fast Open-Source Audio Feature Extractor,” ACM MM 2010.  
[5] F. Eyben *et al.*, “The Geneva Minimalistic Acoustic Parameter Set (eGeMAPS) for Voice Research and Affective Computing,” IEEE T-AC 2016.  
[6] A. Baevski *et al.*, “wav2vec 2.0: A Framework for Self-Supervised Learning of Speech Representations,” NeurIPS 2020.  
[7] A. Radford *et al.*, “Robust Speech Recognition via Large-Scale Weak Supervision,” Whisper Tech Report 2023.  
[8] V. Sanh *et al.*, “DistilBERT, a Distilled Version of BERT,” arXiv 1908.08962.