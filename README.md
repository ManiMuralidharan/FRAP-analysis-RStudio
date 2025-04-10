# FRAP-analysis
# Author : Muralidharan Mani
# Affiliation : University of Wisconsin-Madison
This R code performs a comprehensive analysis of FRAP (Fluorescence Recovery After Photobleaching) data, including data normalization, curve fitting, half-life calculation, R-squared computation, and plotting. Here's a detailed breakdown:

1. Load Required Libraries:

ggplot2: Used for creating high-quality plots.
nls2: Used for non-linear least squares fitting, especially when initial parameter guesses are challenging.
2. Set Working Directory:

setwd("C:/Users/mmani3/Desktop/FRAP"): Sets the working directory to the specified path, where the input CSV files are located. Important: You must change this to the actual path where your data files are stored.
3. Load Data:

Controldata <- read.csv("Controlwithref.csv", header = FALSE): Reads the "Controlwithref.csv" file into the Controldata data frame. header = FALSE indicates that the CSV file does not have a header row.
Referencedata <- read.csv("Referencewithref.csv", header = FALSE): Reads the "Referencewithref.csv" file into the Referencedata data frame.
4. Create Time Vector:

num_frames <- nrow(Controldata): Gets the number of rows (frames) from the Controldata data frame.
time <- seq(0, (num_frames - 1) * 0.995, by = 0.995): Creates a time vector, assuming a frame interval of 0.995 seconds. You should adjust this interval if necessary.
5. Normalize Data (doubleNormalize Function):

This function takes the raw data as input and performs a double normalization.
It assumes that the input CSV files have paired columns: "bleach" data and "reference" data.
It subtracts the mean of the reference data from the bleach data to correct for background.
It divides the background-corrected bleach data by the mean of the reference data.
It normalizes the result by the average of the first 10 data points.
It handles cases where the reference mean is zero, and outputs a warning instead of error.
6. Fit Data and Compute Half-Lives (fitAndGetHalfLife Function):

This function fits a single exponential model to the normalized data for each ROI.
It uses nls for non-linear least squares fitting, with improved initial parameter guesses and robust fitting settings.
It calculates the half-life using the fitted tau parameter (t = ln(2) * tau).
It computes the R-squared value to assess the goodness of fit.
It uses tryCatch to capture and handle fitting errors.
It returns a list containing the fitted models, half-lives, fitted parameters, and goodness-of-fit information.
7. Run Fitting:

Controlresults <- fitAndGetHalfLife(time, Controlnormalized): Runs the fitting function for the Control data.
Referenceresults <- fitAndGetHalfLife(time, Referencenormalized): Runs the fitting function for the Reference data.
The results are stored in lists.
8. Display and Save Half-Life Results:

Prints the calculated half-lives for both Control and Reference data.
Saves the half-life results to CSV files ("Control_half_lives.csv" and "Reference_half_lives.csv").
9. Display and Save R-Squared Values:

Calculates and prints the R-squared values for both Control and Reference fits.
Saves the R-squared values to CSV files ("Control_R_squared.csv" and "Reference_R_squared.csv").
10. Save Fitted Parameters:

Saves the fitted parameters to CSV files ("Control_fitted_params.csv" and "Reference_fitted_params.csv").
11. Compute Mean and SD for Plotting:

Calculates the mean and standard deviation of the normalized data for each time point.
Handles NA values during calculations.
12. Convert to Data Frame for ggplot2:

Creates a data frame suitable for plotting with ggplot2.
13. Plot Mean Recovery Curves with Error Bars:

Uses ggplot2 to create a plot of the mean recovery curves with error bars representing the standard deviation.
Adds labels, titles, and a legend.
Prints the plot to the console.
14. Save Plot:

Saves the plot as a PNG file ("Mean_FRAP_Recovery.png").
Key Improvements and Considerations:

Robust Fitting: The code uses nls with the "port" algorithm, which is more robust for challenging fits.
Improved Initial Parameter Guesses: The initial parameter guesses for the nls fitting are improved to increase the likelihood of convergence.
Error Handling: The tryCatch block handles fitting errors gracefully.
NA Handling: The code handles NA values during calculations.
Clear Output: The code provides clear output and saves the results to CSV files.
ggplot2 Plotting: The code uses ggplot2 for high-quality plotting.
Working Directory: Make sure to set the correct working directory.
Time Interval: Verify and adjust the time interval as needed.
Data Structure: The code assumes that the CSV files have paired columns (bleach and reference).
This R script is well-structured and handles common challenges in FRAP data analysis.
