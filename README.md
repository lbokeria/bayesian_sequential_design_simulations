
# Before running the simulations:

First, please open the .Rproj file. Having the project started makes sure the working directory is set correctly.

Then, please make sure Rstudio packages are up-to-date. 
Finally, please make sure to install the "pacman" package using install.packages('pacman').


# Main sequence of steps to run simulations:
1_simulation_parameters.R: specify various types of simulations you'd like to run. You can either run on the cluster or locally.

The rest of the scripts can be run on your local machine, after you have synced the simulation results from the cluster to your local machine. Running these scripts on the login nodes will generally be slower. 

2_move_slurm_output.R: slurm will dump the results in the main working directory. To avoid cluttering the space, this script will move the specified folder to the ./data folder. Subsequent scripts will look for data there. 

3_preprocess_output.R: slurm outputs the results in a list format. This script will bind it into a nice dataframe, and save the dataframe into specified subfolder of the ./analysis_results/ folder. This dataframe will have each row represent one "step" in every simulation, i.e. one addition of a batch of participants and checking of the Bayes factor.

4_summary_stats_many_altNs.R: This script takes the dataframe generated by the 3_preprocess_output.R script. It will calculate the probabilities of supporting H1/H0/undecided, depending on various maxN limits.

5_plot_results.R: Warning: this script will NOT work at all on login nodes. Perform this step on your local PC. This script will take the power_table.RData file produced by 4_summary_stats_many_altNs.R script. For every unique combination of factors you specified in the initial job, it will produce two plots: (1) percentage of simulations supporting H1/H0/undecided, and (2) mean and median number of participants needed to run to achieve a certain "power".
The code optionally allows you to create these plots only for certain combination of the factors, instead of all of them. Otherwise, sometimes, if there are way too many combinations, you'll get lost in the plethora of plots that are produced.

6_post_slurm_wrapper.R: This script is a wrapper to run scripts 2 through 5, so you don't have to run them separately. It cannot run the 1st script, because the 2nd script must wait until slurm is done with the simulations.


# Folders:

data:
Where the raw data output from slurm is copied to and left untouched.

scripts:
Folder for all the scripts. Has a ./utils subfolder that contains some helper functions for various purposes.

analysis_output:
For preprocessing and other stats results from various analysis. Contains an example analyzed output in the "results_1" folder, where we simulated all the combination of the following factors: minN=24, batchSize=12, maxN=600, BF10 criteria of 6 or 10, BF01 criteria of 6 or 10, one-tailed and two-tailed, paired and unpaired t-test.


# Authors:

- Alex Quent
- Rik Henson
- Levan Bokeria
- Andrea Greve

