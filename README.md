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