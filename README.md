# pymol_structure_rendering on HPC
Transfer the pymol session file on the HPC directory through this code on local terminal:
scp -r /path/to/pymol/session/file/ username@hpcloginid:/path/to/hpc/directory/

#Login to HPC terminal and do this and prepare the PyMOL Script #Create the .pml script file
touch render_script.pml
vi render_script.pml

#Edit.pml file with the following script
# 1.1 general settings
bg_color white
set ray_trace_mode, 1
set antialias, 2
set ray_shadows, 1
set ambient, 0.3

set reflect, 1

set shininess, 10

set spec_reflect, 1

set spec_power, 200

set light_count, 8
# 1.2 Image Resolution
ray 3000, 3000
# 1.3 Protein Representation
hide everything

show cartoon, all

show sticks, organic

set cartoon_fancy_helices, 1

set cartoon_highlight_color, grey70

set cartoon_side_chain_helper, on
# 1.4 Surface Representation
show surface, all

set transparency, 0.2, all
# 1.5 Colors by Secondary Structure and Chain
color marine, ss h

color yellow, ss s

color salmon, ss l

color cyan, chain A

color magenta, chain B
# 1.6 Highlight Ligands
show sticks, organic

color green, organic
# 1.7 Highlight Important Residues
select important_residues, resi 123+456

show spheres, important_residues

color red, important_residues
# 1.8 Labeling Important Residues
label resi 123 and name CA, "Important Residue"

set label_font_id, 7

set label_size, 14

color black, labels
# 1.9 Lighting and Shadows
set spec_reflect, 1

set spec_power, 200

set shininess, 10

set light_count, 8
# 1.10 Depth Cueing
set depth_cue, 1

set fog_start, 0.4
# 1.11 Rendering
ray

png output_image.png

# 2. Create a Batch Submission Script
Next, create a batch submission script for your HPC scheduler (e.g., SLURM, PBS, or LSF). Here’s an example for SLURM:
# 2.1 
#!/bin/bash
#SBATCH --job-name=pymol_render

#SBATCH --output=pymol_render.out

#SBATCH --error=pymol_render.err

#SBATCH --time=01:00:00

#SBATCH --partition=standard

#SBATCH --nodes=1

#SBATCH --ntasks=1

#SBATCH --cpus-per-task=16

#SBATCH --mem=32G

# 2.2 Load PyMOL module if available
module load pymol

# 2.3 Run PyMOL in headless mode
pymol -cq render_script.pml

# 3. Submit the Job to the HPC Cluster
Submit the job to the HPC scheduler using the appropriate command. For SLURM, use:

sbatch pymol_hpc_job.sh

**Additional Tips**
Resource Allocation: Adjust the --cpus-per-task and --mem parameters based on your job requirements and the available resources on your HPC cluster.
Job Monitoring: Monitor the job status using commands like squeue (for SLURM) or qstat (for PBS).
Output Files: Ensure that the output image file (e.g., output_image.png) is saved in a directory where you have write permissions.
Example Workflow
Prepare PyMOL Script: Save your PyMOL commands in render_script.pml.
Create Batch Script: Save the SLURM batch script as pymol_hpc_job.sh.
Submit Job: Submit the batch script using sbatch pymol_hpc_job.sh.
Monitor Job: Check job status and output files once the job is complete.
