# Sonar-Mines-vs-Rocks - Instructions for installation and configuration in bash

1. Clone the git repository:
```bash
git clone https://github.com/kajinmo/Sonar-Mines-vs-Rocks.git
cd sonar
```

2. Configure the correct Python version with `pyenv`:
```bash
pyenv install 3.12.3
pyenv local 3.12.3
```

3. Configure `poetry` for Python version 3.12.3 and activate the virtual environment:
```bash
poetry env use 3.12.3
poetry shell
```

4. Install project dependencies:
```bash
poetry install
```

5. Open vscode:
```
code
```

---

# Training Sonar to detect Mines

**About the Dataset**

The dataset is [“Sonar: Mines vs. Rocks”](http://archive.ics.uci.edu/ml/datasets/Connectionist+Bench+%28Sonar%2C+Mines+vs.+Rocks%29) by R. Gorman and T. Sejnowski and is available on the UCI machine learning repository.

This dataset consists of sonar data collected to distinguish between rocks and metal objects, structures such as sea mines on the seafloor.

Unfortunately, the dataset page and the original article from which it was derived provide limited context on how to interpret the data. The article primarily emphasizes the application of machine learning models, which is impressive given its publication year (1988) - but lacks detailed information about the experiment and data interpretation.

I don’t want to just apply machine learning algorithms without understanding the basics of interpreting the dataset, so I have found some materials to help me understand how to interpret the data.

**What is Sonar and how it works?**

Sonar - which stands for Sound Navigation and Ranging, is a technology that uses sound waves for the detection, classification, and location of underwater objects. In commercial applications, sonar is used in fish finders, medical imaging, material inspection, and seismic exploration.

A sonar system emits sound waves, usually in the form of pulses, into the water. These sound waves travel through the water in all directions.

When the sound waves encounter an object in the water, such as a submarine, fish, or the seafloor, they bounce off or reflect back toward the sonar device. The sonar system has a receiver that detects the echoes or reflected sound waves. By analyzing the characteristics of the received echoes, such as their intensity, frequency, and time delay, the sonar system can provide information about the size, shape, and distance of the objects in the water.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aae78508-e730-49b9-b91e-da942c52e3b0/89d1ccfd-3b58-4595-baee-dc45e01e0d8e/Untitled.png)

https://www.fao.org/3/x6602e/x6602e02.htm

**Aquatic Environment: why visualize with sound and not with light?**

Although we are accustomed to using light to visualize objects, sound offers significant advantages in certain situations, especially in environments where light is limited (such as the deep sea) or where penetration into opaque media is necessary (as in medical ultrasound equipment).

In the ocean, visible light is absorbed and scattered by water molecules and other particles. Red, green, and yellow wavelengths are absorbed first, causing colors to appear less vibrant and eventually fade as you go deeper. This results in a monochromatic appearance with reduced contrast and detail.

https://twitter.com/ToK_ScienceTree/status/1758279770473918780

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aae78508-e730-49b9-b91e-da942c52e3b0/1179326b-42a0-4c21-8784-59857e50e810/Untitled.png)

Video showing the underwater color attenuation: https://www.pbslearningmedia.org/resource/buac20-68-sci-ps-colorsunderwater/colors-underwater/

On the other hand, water is a very effective medium for sound propagation. Sound waves travel great distances with minimal energy loss in water. This is why sonar experiments are typically conducted in aquatic environments. Sound provides a more reliable means of detecting, classifying, and locating underwater objects when light is insufficient.

**Fourier Transformation**

When a chirp is emitted from the sonar, echoes from underwater objects are received by the sonar receiver over time. Since these echoes result from the emitted signal bouncing off various objects, multiple sound waves return to the sonar at different times.

To analyze these received signals, Fourier Transformation is used to convert the data from the time domain to the frequency domain. In the image below, you can see how this process works visually.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aae78508-e730-49b9-b91e-da942c52e3b0/a7e21112-e178-4e0b-a082-0152700cd7cf/Untitled.png)

Visualization of Fourier Transformation 

This transformation allows us to examine the energy of the received signals as a function of wave frequencies. By analyzing these frequency components, we can detect and classify underwater objects based on their unique acoustic signatures. Below, we can see the “noise signature” of a metal cylinder and a rock, which are the objects of study in the original and this article.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/aae78508-e730-49b9-b91e-da942c52e3b0/d5b9ffd3-16c3-435c-8784-249d92aa1f7f/Untitled.png)

Amplitude displays of a typical return from the cylinder and the rock as function of time (R. Gorman 1988)

With that concepts in mind, we can finally understand the dataset!

[]

**Dataset Information**

- Contains 208 observations.
- Each observation consists of 60 features in the range of 0.0 to 1.0 (pre-processed and normalized).
- The dataset is already transformed into the frequency domain, meaning each feature represents a frequency energy.
- The ‘target’ feature is the label associated with each record, containing the letter "R" if the object is a rock and "M" if it is a mine (metal cylinder).

Note: In my interpretation, the dataset description appears ambiguous regarding whether the features are represented in the time or frequency domain. Below, we will confirm that it is indeed the latter. However, it is important to note that the input does not consist of raw sonar data or spectrograms but rather spectral envelopes. Regrettably, we lack additional information regarding the frequencies, possibly due to the historical context of the dataset originating from the Cold War era.

Note: regrettably, we lack additional information regarding the frequencies, possibly due to the historical context of the dataset originating from the Cold War era. In my interpretation, the dataset description appears ambiguous regarding whether the features are represented in the time or frequency domain. In fact, in session [__]we will confirm that the latter is the case. It's important to note that the input does not consist of raw sonar data or spectrograms, but rather spectral envelopes. 

https://cainvas.ai-tech.systems/use-cases/sonar-data-recognition-app/

https://www.simonwenkel.com/2018/08/23/revisiting_ml_sonar_mines_vs_rocks.html
