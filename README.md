# Molecular-Dynamics

## Trabajar en el Cluster de UNQ

1. Entrar al cluster de UNQ con: 

```Bash
	ssh -p 2222 cpadilla@207.248.75.21
```

2. Cambiar en el cluster a @1rick: con:

```Bash
	ssh 10.6.0.42
```

Note: Me largó lo siguiente

```Bash
	The authenticity of host '10.6.0.42 (10.6.0.42)' can't be established.
ECDSA key fingerprint is SHA256:ZQXFe6z3isC6lLTr39CvE8w7ySxDUNZ4yrnqdFcWhtg.
Are you sure you want to continue connecting (yes/no)? 
# pongo yes
```

3. Comando tleap

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

4. Corri otra proteina para probar con nuevos permisos de usuario. Largue en @rick: (archivos iniciales: leaprc.ff99SB RsmE.pdb tleap.in)

```Bash
	tleap -s -f tleap.in
```
Corrió y al final escribí "quit"

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
Se crearon los siguientes archivos: leap.log rsme.crd rsme.top 

5. Realizo la Minimización

 - Muevo los archivos `rsme.crd y rsme.top` generados en la carpeta genero a la carpeta min
 - En la carpeta min, modifico el archivo min.sh con los nombres de llos archivos 

 ```Bash
	#de esto
  sander -O -i min.in -o minout -p .op -c .crd -r min.crd
  #a esto
  sander -O -i min.in -o minout -p rsme.top -c rsme.crd -r min.crd
```
