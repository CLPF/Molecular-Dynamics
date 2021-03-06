# Molecular-Dynamics
https://ambermd.org/tutorials/basic/tutorial0/index.php

## Trabajar en el Cluster de UNQ

1. Entrar al cluster de UNQ con: 

```Bash
	ssh -p 2222 cpadilla@207.248.75.21
```

Nota: puedo cambiar en el cluster a @1rick: con:

```Bash
	ssh 10.6.0.42
```

Me largó lo siguiente

```Bash
	The authenticity of host '10.6.0.42 (10.6.0.42)' can't be established.
ECDSA key fingerprint is SHA256:ZQXFe6z3isC6lLTr39CvE8w7ySxDUNZ4yrnqdFcWhtg.
Are you sure you want to continue connecting (yes/no)? 
  # pongo yes
```
---

2. Bajar la proteina del protein data bank (en este caso parto de la proteina `RsmE.pdb`)

---

3. Genero los archivos de topología y coordenadas con el comando `tleap`

```Bash
	tleap -s -f leap.in
```
Error

```Bash
Opening /shared/amber16/dat/leap/prep/cree.top: Permission denied
Opening /shared/amber16/dat/leap/lib/cree.top: Permission denied
Opening /shared/amber16/dat/leap/parm/cree.top: Permission denied
Opening /shared/amber16/dat/leap/cmd/cree.top: Permission denied
Could not open file cree.top: system error
saveAmberParm: Could not open file: cree.top
```
El error se debia a permisos de usuario

- Largue en @rick::~/genero$ (archivos iniciales: `leaprc.ff99SB RsmE.pdb tleap.in`)

```Bash
	tleap -s -f tleap.in
```
¿Que contiene `tleap.in`?
```Bash
source leaprc.ff99SB 
lit= loadPdb RsmE.pdb
solvateBox lit TIP3PBOX 12
saveAmberParm lit rsme.top rsme.crd
[1]+  Done                    nohup sh heat.sh
```

- Corrió y al final escribí "quit"

```Bash
-I: Adding /shared/amber16/dat/leap/prep to search path.
-I: Adding /shared/amber16/dat/leap/lib to search path.
-I: Adding /shared/amber16/dat/leap/parm to search path.
-I: Adding /shared/amber16/dat/leap/cmd to search path.
-s: Ignoring startup file: leaprc
-f: Source tleap.in.

Welcome to LEaP!
Sourcing: ./tleap.in
----- Source: ./leaprc.ff99SB
----- Source of ./leaprc.ff99SB done
Log file: ./leap.log
Loading parameters: /shared/amber16/dat/leap/parm/parm99.dat
Reading title:
PARM99 for DNA,RNA,AA, organic molecules, TIP3P wat. Polariz.& LP incl.02/04/99
Loading parameters: /shared/amber16/dat/leap/parm/frcmod.ff99SB
Reading force field modification type file (frcmod)
Reading title:
Modification/update of parm99.dat (Hornak & Simmerling)
Loading library: /shared/amber16/dat/leap/lib/all_nucleic94.lib
Loading library: /shared/amber16/dat/leap/lib/all_amino94.lib
Loading library: /shared/amber16/dat/leap/lib/all_aminoct94.lib
Loading library: /shared/amber16/dat/leap/lib/all_aminont94.lib
Loading library: /shared/amber16/dat/leap/lib/ions94.lib
Loading library: /shared/amber16/dat/leap/lib/solvents.lib
Loading PDB file: ./RsmE.pdb
  Added missing heavy atom: .R<ARG 6>.A<NH1 17>
  Added missing heavy atom: .R<ARG 6>.A<NH2 20>
  Added missing heavy atom: .R<ARG 31>.A<NH1 17>
  Added missing heavy atom: .R<ARG 31>.A<NH2 20>
  Added missing heavy atom: .R<ARG 44>.A<NH1 17>
  Added missing heavy atom: .R<ARG 44>.A<NH2 20>
  Added missing heavy atom: .R<TYR 48>.A<OH 14>
  Added missing heavy atom: .R<ARG 50>.A<NH1 17>
  Added missing heavy atom: .R<ARG 50>.A<NH2 20>
  Added missing heavy atom: .R<CASP 59>.A<OXT 13>
  total atoms in file: 439
  Leap added 482 missing atoms according to residue templates:
       10 Heavy
       472 H / lone pairs
  Solute vdw bounding box:              40.376 32.630 38.190
  Total bounding box for atom centers:  64.376 56.630 62.190
  Solvent unit box:                     18.774 18.774 18.774
  Total vdw box size:                   67.185 59.860 65.364 angstroms.
  Volume: 262876.924 A^3
  Total mass 122438.502 amu,  Density 0.773 g/cc
  Added 6441 residues.
Checking Unit.
WARNING: The unperturbed charge of the unit: -1.000000 is not zero.

 -- ignoring the warning.

Building topology.
Building atom parameters.
Building bond parameters.
Building angle parameters.
Building proper torsion parameters.
Building improper torsion parameters.
 total 162 improper torsions applied
Building H-Bond parameters.
Incorporating Non-Bonded adjustments.
Not Marking per-residue atom chain types.
Marking per-residue atom chain types.
  (Residues lacking connect0/connect1 -
   these don't have chain types marked:

        res     total affected

        CASP    1
        NMET    1
        WAT     6441
  )
 (no restraints)
> quit
        Quit
```
Se crearon los siguientes archivos: `leap.log rsme.crd rsme.top`

---

4. Realizo la Minimización en @rick::~/min$

 - Muevo los archivos `rsme.crd y rsme.top` generados en la carpeta "genero" a la carpeta "min". Parto de los siguientes archivos: `min.in  min.sh  rsme.crd  rsme.top  sc`
 - En la carpeta "min", modifico el archivo min.sh con el comando `nano` o `vi` los nombres de los archivos de la siguiente manera

 ```Bash
	#de esto
  sander -O -i min.in -o minout -p .op -c .crd -r min.crd
  #a esto
  sander -O -i min.in -o minout -p rsme.top -c rsme.crd -r min.crd
```
- Largo la minimización con el siguiente comando

 ```Bash
nohup sh ./min.sh &
```
Results
 ```Bash
[1] 54160
nohup: ignoring input and appending output to 'nohup.out'
```
> hacer doble enter
Se generan los archivos: `min.crd mdinfo minout nohup.out` 

- Modifico el archivo sc con los archivos correspondientes `rsme.top min.crd y cambio a min.pdb de salida`

 ```Bash
parm rsme.top
trajin min.crd
strip :WAT
trajout min.pdb
```
- Largo el comando `cpptraj` de analisis de trayectorias. Una vez que corra se genera el pdb minimizado

 ```Bash
cpptraj -i sc
```

Results
 ```Bash
CPPTRAJ: Trajectory Analysis. V17.00
    ___  ___  ___  ___
     | \/ | \/ | \/ |
    _|_/\_|_/\_|_/\_|_

| Date/time: 08/11/21 03:01:30
| Available memory: 34.842 GB

INPUT: Reading input from 'sc'
  [parm rsme.top]
        Reading 'rsme.top' as Amber Topology
        Radius Set: modified Bondi radii (mbondi)
  [trajin min.crd]
        Reading 'min.crd' as Amber NC Restart
  [strip :WAT]
    STRIP: Stripping atoms in mask [:WAT]
  [trajout min.pdb]
        Writing 'min.pdb' as PDB
---------- RUN BEGIN -------------------------------------------------

PARAMETER FILES (1 total):
 0: rsme.top, 20244 atoms, 6500 res, box: Orthogonal, 6442 mol, 6441 solvent

INPUT TRAJECTORIES (1 total):
 0: 'min.crd' is a NetCDF AMBER restart file, Parm rsme.top (Orthogonal box) (reading 1 of 1)
  Coordinate processing will occur on 1 frames.

OUTPUT TRAJECTORIES (1 total):
  'min.pdb' (1 frames) is a PDB file

BEGIN TRAJECTORY PROCESSING:
.....................................................
ACTION SETUP FOR PARM 'rsme.top' (1 actions):
  0: [strip :WAT]
        Stripping 19323 atoms.
        Stripped topology: 921 atoms, 59 res, box: Orthogonal, 1 mol
Warning: No PDB space group specified.
----- min.crd (1-1, 1) -----
100% Complete.

Read 1 frames and processed 1 frames.
TIME: Avg. throughput= 178.6352 frames / second.

ACTION OUTPUT:

RUN TIMING:
TIME:           Init               : 0.0000 s (  0.60%)
TIME:           Trajectory Process : 0.0056 s ( 98.12%)
TIME:           Action Post        : 0.0000 s (  0.00%)
TIME:           Analysis           : 0.0000 s (  0.00%)
TIME:           Data File Write    : 0.0000 s (  0.02%)
TIME:           Other              : 0.0001 s (  0.01%)
TIME:   Run Total 0.0057 s
---------- RUN END ---------------------------------------------------
TIME: Total execution time: 0.1203 seconds.
--------------------------------------------------------------------------------
To cite CPPTRAJ use:
Daniel R. Roe and Thomas E. Cheatham, III, "PTRAJ and CPPTRAJ: Software for
  Processing and Analysis of Molecular Dynamics Trajectory Data". J. Chem.
  Theory Comput., 2013, 9 (7), pp 3084-3095.
  ```
Se generó el archivo `min.pdb`
Ejemplo
![min.pdb](imagen0.bmp)

---

5. Calentamiento
- Para poder llevar a cabo el Calentamiento necesito......, por tal motivo me logeo en
```Bash
su aormazabal
pas: 4321aormazabal
ssh aormazabal@10.6.0.45
cd carla
```
- Paso las carpetas `genero, min y heat` a esta nueva dirección con
```Bash
scp -r cpadilla@10.6.0.41:~/heat .
```
- Corroboro en que CPU puedo trabajar con (El que está a menor temperatura)
```Bash
nvidia-smi
```
- Modifico el nombre de los archivos
Parto de los archivos `heat.0.in  heat.1.in  heat.sh`
```Bash
#!/bin/bash
export CUDA_VISIBLE_DEVICES="0"
taskset -c 2 pmemd.cuda -O -i heat.0.in -o heat.0.out -p ../genero/rmse.top -c ../min/min.crd -r heat0.crd -ref ../min/min.crd
taskset -c 2 pmemd.cuda -O -i heat.1.in -o heat.1.out -p ../genero/rmse.top -c heat0.crd -r heat1.crd
```
- Largo el calentamiento con
```Bash
nohup sh heat.sh &
```
Results
 ```Bash
[1] 26677
nohup: ignoring input and appending output to ‘nohup.out’
```
> hacer doble enter
Se generan los archivos: `heat.0.out  heat.1.out  mdcrd  mdinfo  nohup.out` 