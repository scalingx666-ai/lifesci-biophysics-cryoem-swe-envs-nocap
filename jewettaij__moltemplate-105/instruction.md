# Issue

**Title:** adding a converter from OPLSSAA-2023 FF files

## Description

Hello everyone,

I'm creating this PR to address issue #104. 

I should have addressed all the requests made in the issue, I also refactored a little bit the code.

Below, I'm reporting some of the considerations we made in the issue, with some comments.

# Duplicated interactions

Regarding the duplicated interactions, the observations from the previous files and the current implementation are summarized here.

**old PRM from TINKER**

Duplicated interactions are commented out (and are found only in the dihedrals section). There is a note about it at the beginning of the "Torsional Parameters" section

**old LT, oplsaa.lt**

It just missed the duplicated dihedral interactions, because they were commented out in the original PRM file, and so they were skipped by the converter

**NEW oplsaa2023 from paper SI (similar to the files provided with BOSS)**

Some duplicates are found in bonds/angles/dihedrals, many of which have the same parameters (particularly in the angles section). Here are some examples of lines with duplicated entries with different parameters:

    CW-C= 481.       1.454        JT-R 2014/04 2-vinyl furans
    CW-C= 549.       1.365
---
    CV-CW-NA     70.       106.3    wlj   "     "  imidazole
    CV-CW-NA     70.       103.6    wlj  6/13               
---
    003   0.850    -0.200     0.200     0.0        CT-CT-CT-CT     hydrocarbon OPLS/2020                
    004   1.100    -0.200     0.200     0.0        CT-CT-CT-CT     butane only OPLS/2020                
    :
    005   2.0      -0.20     0.0       0.0         CT-CT-CT-OH     alcohols         OPLS/2020           
    :
    007  -1.552     0.0      0.000     0.0         CT-CT-CT-OH     polyols  AA
    :
    :
    223   0.0       2.17      0.0       0.0        CA-C!-CW-NA     biphenyl-like                        
    :
    :
    431   0.0       2.0       0.0       0.0        CA-C!-CW-NA     biaryl_7                             

an important note is that the comment is not always clear "as it is", as often it "relies" on the comment(s) above. That's a problem when I convert the file, as reordering the interactions I miss such information (this is clear in the second angle example below...)

**NEW oplsaa2023 LT**
    
Duplicates with the same parameters are just skipped, while "real" duplicates are commented out. The cool thing is that they should always appear near each other in this file, and that was not true in the original file, so one could have easily missed some interaction.
As explained above, the comment is not always helpful in determining which parameters to apply, but it is what it is I think... It's relatively easy to go back to the original FF file to check the comment in its context, having maintained the type-names... 

    bond_coeff @bond:CW_C= 481. 1.454     # JT-R 2014/04 2-vinyl furans
    #bond_coeff @bond:CW_C= 549. 1.365     # 
---
    angle_coeff @angle:CV_CW_NA 70. 106.3    # wlj " " imidazole
    #angle_coeff @angle:CV_CW_NA 70. 103.6    # wlj 6/13
---
    dihedral_coeff @dihedral:CA_C!_CW_NA 0.0 2.17 0.0 0.0        # biphenyl-like
    #dihedral_coeff @dihedral:CA_C!_CW_NA 0.0 2.0 0.0 0.0         # biaryl_7 
    :
    dihedral_coeff @dihedral:CT_CT_CT_CT 0.850 -0.200 0.200 0.0  # hydrocarbon OPLS/2020 
    #dihedral_coeff @dihedral:CT_CT_CT_CT 1.100 -0.200 0.200 0.0  # butane only OPLS/2020 
    :
    dihedral_coeff @dihedral:CT_CT_CT_OH 2.0 -0.20 0.0 0.0       # alcohols OPLS/2020 
    #dihedral_coeff @dihedral:CT_CT_CT_OH -1.552 0.0 0.000 0.0    # polyols  AA

___________________


# Waters

**old PRM from TINKER (inherited by old oplsaa.lt)**

has different atom types for different water models; has also flexible variants

    31 OW TIP3-4-P      31-32 & 39-40           k=600.00     r=0.9572
    32 HW TIP3-4-P      32-31-32 & 40-39-40      k=75.00     a=104.52
    39 OW TIP5P
    40 HW TIP5P

    33  M 4P            32-31-33      k=50.00    a=52.26
                        31-33        k=900.00     r=0.1500

    41 LP TIP5P         39-41        900.00     0.7000
                        41-39-41      50.00     109.47
                        40-39-41      50.00     110.6948

    42 OW SPC           42-43        600.00     1.0000
    43 HW SPC           43-42-43      75.00     109.47

The non-bonded parameters seem Wikipedia-correct, but they are not the parameters LAMMPS suggests when one uses PPPM/Ewald, as we are suggesting to the user (see below).
It seems also correct in the O-H and H-O-H parameters, I don't understand why there are bonded interactions with the LP, as those in lammps are treated directly by the pair_style lj/cut/tip4p/cut for TIP4P, while for TIP5P model just the angle L-O-L and distance O-L should be used if I understand the documentation correctly.


**NEW oplsaa2023 from paper SI (similar to the files provided with BOSS)**

Has just one single OW-HW parameter and OW-LP parameter (in the comment it reports they are for for TIP4F water model), and the same is true for HW-OW-HW and OW-LP-HW... The strange thing is that in the atom type definition section, it has explicitly TIP3-4-5P, their flexible variants, and also SPC types... unfortunately, the relative bonded parameters are completely missing :-)


**NEW oplsaa2023.lt**

I propose to completely skip reading the water-related atom types definitions and bonded definitions (just skipping whenever "water" is found in the comment), and just add by hand the relevant parameters, in this way, we can have direct control over the parameters... the only thing to keep in mind is that we have to identify "two-letter" type definitions that don't clash with the ones already in the FF file... I'm using "three-letter" types, that should do the trick...

The parameters I added are taken from the LAMMPS documentation, as we are suggesting using PPPM and LAMMPS indicates different parameters than the ones in the old oplsaa.lt if one wants to use PPPM or Ewald. See the script for more precise comments on each parameter (I also added a note to the user to read the proper LAMMPS documentation when selecting the water model).
It would be great to also introduce the OPC water model, but I don't really know how if I correctly implemented it here, I based my implementation on a thread in MATSCI [https://matsci.org/t/use-opc-water-model/36419](url)

Please have a careful look at the water parameters (particularly charges and LJ parameters), as I may have made mistakes copying stuff around from Wikipedia/documentation and all forcefield files :-)

___________________


# Impropers

They are still a work in progress, I just added the missing `(opls_imp.py)`, but I didn't check if they are correctly detected and applied.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5364f44e7aee`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.