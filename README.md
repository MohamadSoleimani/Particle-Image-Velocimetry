# Particle Image Velocimetry Report

## Introduction
In fluid mechanics, one of the most critical factors in the design and construction of devices is determining the speed distribution within mechanical systems. This information is essential for calculating drag and lift forces, heat transfer, particle scattering, and other phenomena. To achieve this, identifying the velocity field and accurately measuring its values are crucial. Optical velocity measurement is one of the methods available to fulfill this requirement. In this report, the particle image velocimetry (PIV) method is investigated through three tests: jet, natural displacement, and mixing. Velocity measurement using the PIV method was performed using a code written in Python, and the results were compared with those obtained from PIV lab software, a commercial tool. The report also examines the errors and limitations of this method and compares the results based on the selection of interrogation windows and the search for optimal values of these windows.

## Image Reading and Processing
The initial step involves reading black-and-white images with dimensions of 1920x1080 pixels. The images are processed using Python, which creates a matrix corresponding to the image size. An 8-bit camera is used, storing light intensity values ranging from 0 to 255 in the matrix.

## Window Creation for Particle Analysis
The processed images are divided into smaller sections called interrogation windows. Each interrogation window is surrounded by a larger search window to ensure complete overlap at the center. The size of these windows depends on factors like particle size, camera zoom, and resolution. Typically, the interrogation matrix is between 10 and 50 pixels, while the search matrix size should be more than twice the maximum expected particle displacement to ensure accurate tracking.

## Correlation Coefficient Calculation
The particle movement is tracked using correlation coefficients, which range from -1 to 1. A value of 1 indicates a perfect match, while -1 indicates complete non-matching. The formula used is as follows:

$$
R = \frac{\sum_{i}(f_i - \bar{f})(g_i - \bar{g})}{\sqrt{\sum_{i}(f_i - \bar{f})^2}\sqrt{\sum_{i}(g_i - \bar{g})^2}} \tag{1}
$$

Where:
- \( f_i \) and \( g_i \) are the pixel values in the interrogation and search windows.
- \( \bar{f} \) and \( \bar{g} \) are the average pixel values in their respective windows.

In practical experiments, a correlation value of 1 is never achieved due to constant particle motion. The maximum degree of correlation is used to determine particle displacement, with a minimum correlation threshold of 0.5. Below this threshold, displacement is averaged from surrounding matrices.

## Accuracy and Curve Fitting
The initial accuracy of this method is 0.5 pixels. By applying curve fitting and interpolation, accuracy is enhanced to 0.3 pixels. A quadratic curve of the following form is used:

$a \cdot x^2 + b \cdot y^2 + c \cdot x \cdot y + d \cdot x + e \cdot y + f = R_{cormx}[ij] \tag{2}$

This system of equations is solved using the values of five adjacent points to calculate the coefficients:

- $\( a \cdot 0^2 + b \cdot 0^2 + c \cdot 0 \cdot 0 + d \cdot 0 + e \cdot 0 + f = R_{cormx}[ij] \tag{3} \)$
- $\( a \cdot 1^2 + b \cdot 0^2 + c \cdot 1 \cdot 0 + d \cdot 1 + e \cdot 0 + f = R_{cormx}[ij+1] \tag{4} \)$
- $\( a \cdot (-1)^2 + b \cdot 0^2 + c \cdot (-1) \cdot 0 + d \cdot (-1) + e \cdot 0 + f = R_{cormx}[ij-1] \tag{5} \)$
- $\( a \cdot 0^2 + b \cdot 1^2 + c \cdot 0 \cdot 1 + d \cdot 0 + e \cdot 1 + f = R_{cormx}[i+1j] \tag{6} \)$
- $\( a \cdot 0^2 + b \cdot (-1)^2 + c \cdot 0 \cdot (-1) + d \cdot 0 + e \cdot (-1) + f = R_{cormx}[i-1j] \tag{7} \)$

By deriving the quadratic equation, the location of the maximum correlation can be calculated with an accuracy of 0.1 pixels.

## Results

### Free Convection Experiment
The results of the free convection experiment demonstrate the effectiveness of the PIV method in capturing the velocity field within the convection currents. The analysis shows a consistent pattern of particle movement corresponding to the expected flow behavior. The comparison with the commercial PIV lab software indicates a high level of accuracy, with deviations within acceptable limits.

<div align="center">
    <img width="400" height="300" src="Figures/piv_1.png" alt="Free Convection Experiment Results">
    <p><i>Figure 1: Free Convection Velocity Field</i></p>
</div>

### Jet Experiment
In the jet experiment, the PIV method successfully captured the velocity distribution of the jet flow. The results highlight the ability of the PIV method to track rapid changes in particle displacement, particularly in high-speed regions. The comparison with commercial software confirms the reliability of the Python code, with minor discrepancies attributed to the resolution and window size selection.

<div align="center">
    <img width="400" height="300" src="Figures/piv_2.png" alt="Jet Experiment Results">
    <p><i>Figure 2: Jet Velocity Field</i></p>
</div>
