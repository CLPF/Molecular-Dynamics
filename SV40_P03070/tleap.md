WARNING: The unperturbed charge of the unit: -3.000000 is not zero.
FATAL:  Atom .R<GLU 133>.A<OXT 16> does not have a type.
Failed to generate parameters
Parameter file was not saved.



source leaprc.ff99SB
lit= loadPdb carla.pdb
solvateBox lit TIP3PBOX 12
saveAmberParm lit carla.top carla.crd

Solucion

Agrego a tleap.in

addIons lit Na+ 0
addIons lit Cl- 0