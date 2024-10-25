# Non-dilute random alloys

## Foreword

In this project, we will study the effect of chemical short-range order (CSRO) on lattice parameters, lattice distortion (LD), unstable stacking fault energies (USFEs), and/or melting point of non-dilute random alloys, with a focus on the following 21 multi-principal element alloys (MPEAs): CoCrNi, HfTiZr, HfNbTa, HfNbTi, HfTaTi, MoNbTa, MoNbTi, MoNbV, MoNbW, MoTaW, MoTaTi, MoTaV, MoVW, NbTaTi, NbTaV, NbTaW, NbTiV, NbTiZr, NbVW, TaTiZr, and TaVW, each of which contains three elements in equal molar.

As summarized in [another GitHub repository](https://github.com/shuozhixu/MSMSE_2025), there are two methods to build a CSRO structure. In this project, the NPT method will be used to generate the CSRO structures; the embedded-atom method (EAM) potential will be employed for the interatomic interactions, unless stated otherwise. The EAM potential file, `HfMoNbTaTiVWZr_Mubassira2025.eam.alloy`, can be found in this GitHub repository.

The purpose of this project is to answer the following four questions

- [1] Does the segregation and/or local ordering of the same atomic pair vary across different MPEAs?
- [2] How does the CSRO affect lattice parameter, LD, USFE, and/or melting point across MPEAs? What is the relationship between properties of the MPEAs and those of individual elements?

for those, we will investigate all 21 ternaries. These last 19 alloys were chosen for their stable body-centered cubic (BCC) structures.

The third question is:

- [3] Are the answers to the questions [1,2] dependent on the interatomic potential?

for that, we will investigate MoNbTa, MoNbV, NbTaV, and NbVW using [a moment tensor potential (MTP)](https://github.com/ucsdlxg/MoNbTaVW-ML-interatomic-potential-and-CRSS-ML-model). This is described in [another GitHub repository](https://github.com/shuozhixu/JAP-MTP_2025). We will also investigate NbTaTi, NbTaV, and NbTiV using [a modified embedded-atom method (MEAM) potential](https://doi.org/10.1016/j.commatsci.2024.112886), as described in [a third GitHub repository](https://github.com/shuozhixu/JAP-MEAM_2025).

The last question we aim to answer is:

- [4] Are the answers to the questions [1,2] dependent on the number of constituent elements in an MPEA?

for that, we will investigate three binaries (NbTa, NbTi, NbV), one quaternary (NbTaTiV), and one quinary (NbTaTiVZr) using the MEAM potential, as described in [the third GitHub repository](https://github.com/shuozhixu/JAP-MEAM_2025).

## LAMMPS

Following [another project](https://github.com/shuozhixu/MSMSE_2025), we can build LAMMPS with MANYBODY and MC packages and submit jobs on both [OSCER](http://www.ou.edu/oscer.html) and [Bridges-2](https://www.psc.edu/resources/bridges-2).

## CSRO structure

All CSRO structures will be built at an annealing temperature of 300 K.

In [another project](https://github.com/shuozhixu/MSMSE_2025), the CSRO structures in CoCrNi, MoNbTa, and HfTiZr were built using the NPT method. Hence, we can use them directly.

In what follows, we focus on the remaining 18 ternaries (all BCC).

### NbTiZr

Here, we take NbTiZr as an example. First, modify the file `lmp_mcnpt.in` in the `MoNbTa/csro/` directory in [a previous GitHub repository](https://github.com/shuozhixu/MSMSE_2025). Take NbTiZr as an example:

- Replace all `Mo`, `Nb`, and `Ta` with `Nb`, `Ti`, and `Zr`, respectively; the replacement should be case-sensitive, e.g., don’t replace `mo` in the word `thermo` with another element
- Change line 28 to
	`pair_coeff * * HfMoNbTaTiVWZr_Mubassira2025.eam.alloy Nb Ti Zr`

Submit the job using `lmp_mcnpt.in` and `HfMoNbTaTiVWZr_Mubassira2025.eam.alloy`. Once the simulation is finished, you will find two files, among others. The first one is `data.NbTiZr_CSRO`, which will be used to calculate the lattice parameter, USFE, LD, and melting point (to be described later). The second file `cn.out`, which can be used to calculate the Warren-Cowley (WC) parameters following [another project](https://github.com/shuozhixu/MSMSE_2025).

### Other 17 ternaries

Follow the steps above to build CSRO structures for the other 17 BCC ternaries and calculate the WC parameters.

## Lattice parameter

Lattice parameters of random and CSRO CoCrNi and MoNbTa were calculated in [a previous paper](https://doi.org/10.3390/modelling5010019). However, both CSRO structures were generated by the SGC method there. Here we will calculate the lattice parameter of the all 21 ternaries, in both random and CSRO structures. The lattice parameter of the 20 random ternaries (except HfTiZr), in units of &#8491;, can be found in the file `random-a0.txt` in this GitHub repository.

### NbTiZr with CSRO

Here, we take NbTiZr as an example. Its lattice parameter can be calculated by

	(lx/(10*sqrt(6.))+ly/(46*sqrt(3.)/2.)+lz/(14*sqrt(2.)))/3.
	
where

	lx = xhi - xlo
	ly = yhi - ylo
	lz = zhi - zlo

where `xhi`, `xlo`, `yhi`, `ylo`, `zhi`, and `zlo` can be found in the first few lines of the data file `data.NbTiZr_CSRO`.

Let's denote the lattice parameter as $a_0$.

### Other 18 BCC ternaries with CSRO

Follow the steps above to calculate the lattice parameter of the other 18 BCC ternaries with CSRO.

### CoCrNi with CSRO

Following the step in [a previous project](https://github.com/shuozhixu/Modelling_2024) to calculate the lattice parameter of the CSRO CoCrNi generatd by the NPT method.

### Random HfTiZr

Run a LAMMPS simulation using `lmp_0K.in`, `data.HfTiZr_random`, and `HfMoNbTaTiVWZr_Mubassira2025.eam.alloy`. The first two files can be found in the `HfTiZr/` directory in this GitHub repository. The simulation will generate a file `data.HfTiZr_min` upon finished.

HfTiZr has two lattice parameters, $a$ and $c$.

$c$ can be calculated by

	lx/16.
	
where

	lx = xhi - xlo

where `xhi` and `xlo` can be found in the first few lines of the data file `data.HfTiZr_min`.

$a$ can be estimated in OVITO by looking into the _x_ axis and measuring the size of the hexagon.

### HfTiZr with CSRO

Repeat the simulation above, but using the data file `data.HfTiZr_CSRO` generated using the NPT method in [another project](https://github.com/shuozhixu/MSMSE_2025).

## LD

In the literature, there are at least four ways to quantify the average LD in atomic-level simulations. The first one is based on the difference in the lattice parameters among all constituent elements, see [Zhang et al.](https://doi.org/10.1002/adem.200700240); the second one adopts the idea of "[local lattice strain](https://doi.org/10.1016/j.scriptamat.2020.06.030)"; the third one uses the full width at half maximum (FWHM) of the radial distribution function, see [Jian et al.](http://dx.doi.org/10.1016/j.actamat.2020.08.044); the last one is the root mean squared atomic displacement (RMSAD), proposed by [Song et al.](https://doi.org/10.1103/PhysRevMaterials.1.023404) and defined as the average displacement of relaxed atoms from their ideal positions in the undistorted crystal lattice. All four ways take into account the chemical compositions and their nominal molar ratios, while the last three, sometimes referred to as "local LD", additionally consider the distribution of atoms. As a result, the first method yields the same LD for two atomistic structures with the same composition but differing CSROs.

To investigate the effect of CSRO on LD, researchers have employed the last two LD definitions. [Jian et al.](http://dx.doi.org/10.1016/j.actamat.2020.08.044) and [Chen et al.](https://doi.org/10.1016/j.actamat.2024.119910), by studying CoCrNi and NbTiZr respectively, found that the FWHM is smaller in the CSRO structure than in the random structure for the same material. In the meantime, in five out of six BCC multi-principal element alloys, [Wang et al.](https://doi.org/10.1038/s41524-024-01330-6) showed that RMSAD becomes larger in CSRO structures than in random structures. Therefore, it seems that the effect of CSRO on LD depends on how LD is defined. Our goal is to address this mystery by comparing the two definitions systematically. In what follows, we will consider all 21 ternaries. Note that two ternaries --- CoCrNi and HfTiZr --- do not have a BCC structure.

### A preparatory step

First, we prepare energy minimized structures. For the CSRO structures, the data files obtained in the "CSRO structure" section are already energy minimized and can be used directly. For the random structures, we need to prepare the energy minimized data files ourselves. Take MoNbTa as an example: the first step is to build a non energy minimized structure using the file `MoNbTa/random/atomsk_MoNbTa.sh` in [another project](https://github.com/shuozhixu/Modelling_2024); the second step is to modify the file `lmp_0K.in` in the "Random HfTiZr" section above; the third step is to run the LAMMPS simulation, generating a new data file which is the energy minimized data file for random MoNbTa.

### FWHM

Calculations of FWHM can follow Figure 2(c) of [Jian et al.](http://dx.doi.org/10.1016/j.actamat.2020.08.044) and Figure 3(f) of [Chen et al.](https://doi.org/10.1016/j.actamat.2024.119910). Specifically, we can take these steps:

- Load the energy minimized structures (either random or CSRO) into OVITO
- Apply "[Coordination analysis](https://www.ovito.org/manual/reference/pipelines/modifiers/coordination_analysis.html)". Let the "Cutoff radius" be 4 and the "number of histogram bins" be 400
- Click "Show in data inspector", and a panel will pop up on the left
- Click the icon above the disk icon to switch to the "Table view"
- Click the disk icon to save the radial distribution function into a txt file, where the _x_ axis has a unit of Angstrom while the _y_ axis is unitless
- Identify the maximum _g_(_r_) value, i.e., the largest value in the second column of the txt file, denoted as _g_<sub>max</sub> (most likely _g_<sub>max</sub> is at the first peak)
- Calculate the "half maximum", i.e., 0.5 _g_<sub>max</sub>
- Calculate the "full width" corresponding to 0.5 _g_<sub>max</sub>

### RMSAD

Two files needed for RMSAD calculations can be found in the `rmsad/` directory in this GitHub repository. Take the CSRO structure of MoNbTa as an example:

- Run a LAMMPS simulation with `lmp_relax2unrelax.in`, `data.MoNbTa_CSRO`, and the appropriate interatomic potential file, yielding two dump files: `dump.relax` and `dump.unrelax`. The first dump file contains the same relaxed atomic positions as `data.MoNbTa_CSRO` except that atoms are sorted by their IDs in the former. The second dump file contains unrelaxed atomic positions where atoms are also sorted.
- Follow Equation 3 of [Song et al.](https://doi.org/10.1103/PhysRevMaterials.1.023404) to calculate the RMSAD. That equation is implemented in the python code `RMSAD.py`. Run it together with the two dump files.

## USFE

[A recent work](https://doi.org/10.1038/s41524-023-01046-z) calculated the USFEs in MoNbTi and NbTaTi. In that work, an MTP (not the same MTP used in this project) was used, and the NPT method was employed to build the CSRO structures. It was found that the CSRO increases the USFEs in both alloys, see [Supplementary Figure 10](https://static-content.springer.com/esm/art%3A10.1038%2Fs41524-023-01046-z/MediaObjects/41524_2023_1046_MOESM1_ESM.pdf). Relatedly, while most work found that the critical resolved shear stress increases with CSRO, [Liu and Curtin](https://doi.org/10.1016/j.actamat.2023.119471) found the opposite in a Mo-Nb binary.

Here, to show the effect of CSRO on USFE, we calculate the USFE of all 19 BCC ternaries, in both random and CSRO structures. The USFE of the 19 random ternaries, in units of mJ/m<sup>2</sup>, can be found in the file `random-usfe.txt` in this GitHub repository. Therefore, we only need to calculate the USFE of all 19 CSRO BCC ternaries.

It may be wise to [run those high-throughput simulations automatically](https://github.com/RichardBrinlee/USFE25_high_throughput).

### NbTiZr

Here, we take CSRO NbTiZr as an example.

#### Plane 1

The simulation requires files 
`lmp_gsfe.in`, `data.NbTiZr_CSRO`, and `HfMoNbTaTiVWZr_Mubassira2025.eam.alloy`. The first file can be found in the `gsfe/` directory in this GitHub repository.

Modify `lmp_gsfe.in`:

- line 18, replace the number `3.3` with $a_0$

Then run the simulation. Once it is finished, we will find a new file `gsfe_ori`. Run

	sh gsfe_curve.sh

which would yield a new file `gsfe`. The first column is the displacement along the $\left<111\right>$ direction while the second column is the generalized stacking fault energy (GSFE) value, in units of mJ/m<sup>2</sup>. The USFE is the peak GSFE value.

#### Other planes

According to [this paper](http://dx.doi.org/10.1016/j.intermet.2020.106844), in an alloy, multiple GSFE curves should be calculated. Hence, we need to make one more change to `lmp_gsfe.in`:

- line 48, replace the number `1` with `2`

Then run the simulation and obtain another USFE value.

We can then replace that number with `3`, `4`, `5`, ..., `20`, respectively. It follows that we run 18 more simulations. Eventually, we obtain 20 USFE values in total. We then calculate the mean USFE value for NbTiZr.

### Other BCC structures

Follow the steps above to calculate the mean USFEs of the other 18 CSRO BCC ternaries.

## Melting point

The melting point of a material is calculated using the [liquid-solid coexistence method](https://doi.org/10.1063/5.0101548). The input file `lmp_mp.in` can be found in the directory `melting_point/` in this GitHub repository. Value of `liquidt`, set in line 6, may be lowered if a material is found to break up during a simulation.

### CoCrNi

The data file for random CoCrNi is from [this paper](http://dx.doi.org/10.1016/j.actamat.2020.08.044), while that for CSRO CoCrNi built using the NPT method is from [another project](https://github.com/shuozhixu/MSMSE_2025).

#### Random CoCrNi

Run the simulation with files `lmp_mp.in`, `data.CoCrNi_gsfe_random`, and `CoCrNi.lammps.eam`. The first file can be in the directory `melting_point/` in this GitHub repository.

Once it is finished, we will find a file `avePE.out`. Plot its 2nd and 4th column as the _x_ and _y_ axes, respectively. It should look like one of the curves in Figure 2(b) of [this paper](http://dx.doi.org/10.1098/rspa.2017.0084).

In line 7 of `lmp_mp.in`, the trial melting point is set as 1200 K.

- If 1200 K is lower than the actual melting point, the curve we plot will decrease. Then increase the last number in line 7 and rerun the simulation.
- If 1200 K is higher than the actual melting point, the curve we plot will increase. Then decrease the last number in line 7 and rerun the simulation.

Repeat the steps above until we find the actual melting point, which should lead to a curve that neither increases nor decreases.

Note:

- The trial melting point cannot be equal to or higher than `liquidt`.
- Save the data for each curve because eventually we will need to plot multiple curves like those in Figure 2(b) of [this paper](http://dx.doi.org/10.1098/rspa.2017.0084).

#### CoCrNi with CSRO

- Use the data file `data.CoCrNi_CSRO` instead
- Use the correct data file name in line 18 of the file `lmp_mp.in`

#### Pure metals

Run the atomsk script, `atomsk_Ni.sh`, which can be found in `CoCrNi/pure_metals/` in this GitHub repository, to build a Ni structure named `data.Ni`, by

	sh atomsk_Ni.sh

Then use the data file and the same potential file to calculate its melting point.

Then follow the same procedure to calculate the melting points of Co and Cr. Use `atomsk_Co.sh` and `atomsk_Cr.sh`.

In each case, remember to modify `lmp_mp.in` accordingly.

Note: In pure metals, Co has an HCP lattice while Cr has a BCC lattice, and so the two `atomsk_*.sh` files build corresponding lattices. However, Co and Cr take the FCC lattice in CoCrNi. We then use `atomsk_Co_FCC.sh` and `atomsk_Cr_FCC.sh` to generate data files `data.Co_FCC` and `data.Cr_FCC`, respectively, and recalculate the melting points. Finally, for the same metal, we take the average melting points of the two lattices as the melting point of this metal.

### MoNbTa

The data file for random MoNbTa is from [this paper](https://doi.org/10.3390/modelling5010019), while that for CSRO CoCrNi built using the NPT method is from [another project](https://github.com/shuozhixu/MSMSE_2025).

#### Random MoNbTa

Run the simulations with files `lmp_mp.in`, `data.MoNbTa_random`, and `HfMoNbTaTiVWZr_Mubassira2025.eam.alloy` to calculate its melting point. The second file can be found in the directory `MoNbTa/` in this GitHub repository.

Make the following two changes in the file `lmp_mp.in`:

- line 18. Use the correct data file name
- line 24. Change it to `pair_coeff * * HfMoNbTaTiVWZr_Mubassira2025.eam.alloy Mo Nb Ta`

#### MoNbTa with CSRO

Use files `lmp_mp.in`, `data.MoNbTa_CSRO`, and `HfMoNbTaTiVWZr_Mubassira2025.eam.alloy` to calculate its melting point.

Remember to modify `lmp_mp.in` accordingly.

#### Pure metals

Run the atomsk script, `atomsk_Mo.sh`, which can be found in `MoNbTa/pure_metals/` in this GitHub repository, to build a Mo structure named `data.Mo`, by

	sh atomsk_Mo.sh

Then use the data file and the same potential file to calculate its melting point.

Follow the same procedure to calculate the melting points of Nb and Ta. Use `atomsk_Nb.sh` and `atomsk_Ta.sh`.

In each case, modify `lmp_mp.in` accordingly.

### HfTiZr

Data files for both random and CSRO HfTiZr are from [another project](https://github.com/shuozhixu/MSMSE_2025).

#### Random HfTiZr

Run the simulations with files `lmp_mp.in`, `data.HfTiZr_random`, and `HfMoNbTaTiVWZr_Mubassira2025.eam.alloy` to calculate its melting point.

Remember to modify `lmp_mp.in` accordingly.

#### HfTiZr with CSRO

Note: the CSRO structure was annealed at 300 K.

Use files `lmp_mp.in`, `data.HfTiZr_CSRO`, and `HfMoNbTaTiVWZr_Mubassira2025.eam.alloy` to calculate its melting point.

Remember to modify `lmp_mp.in` accordingly.

#### Pure metals

Run the atomsk script, `atomsk_Hf.sh`, which can be found in `HfTiZr/pure_metals/` in this GitHub repository, to build a structure named `data.Hf`, by

	sh atomsk_Hf.sh

Then use the data file and the same potential file to calculate its melting point.

Follow the same procedure to calculate the melting points of Ti and Zr. Use `atomsk_Ti.sh` and `atomsk_Zr.sh`.

In each case, modify `lmp_mp.in` accordingly.

## Reference

If you use any files from this GitHub repository, please cite

- Shuozhi Xu, Wu-Rong Jian, Irene J. Beyerlein, [Ideal simple shear strengths of two HfNbTaTi-based quinary refractory multi-principal element alloys](http://dx.doi.org/10.1063/5.0116898), APL Mater. 10 (2022) 111107
