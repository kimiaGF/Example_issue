# Issue with parsing multi-system data 

Of the original 140+ systems, two are included (randomly chosen) as OUTCAR files. 

The code used for parsing the systems and sequentially adding them to a MultiSystem is given in the jupyter notebook. 

The following error is given when the code is run:
```
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
Input In [1], in <module>
     14         print(f)
     15     if len(ls)>0:
---> 16         ms.append(ls)
     18 ms.to_deepmd_raw('deepmd')
     19 ms.to_deepmd_npy('deepmd')

File ~/.conda/envs/MLP/lib/python3.10/site-packages/dpdata/system.py:1158, in MultiSystems.append(self, *systems)
   1156 for system in systems:
   1157     if isinstance(system, System):
-> 1158         self.__append(system)
   1159     elif isinstance(system, MultiSystems):
   1160         for sys in system:

File ~/.conda/envs/MLP/lib/python3.10/site-packages/dpdata/system.py:1168, in MultiSystems.__append(self, system)
   1166 if not system.formula:
   1167     return
-> 1168 self.check_atom_names(system)
   1169 formula = system.formula
   1170 if formula in self.systems:

File ~/.conda/envs/MLP/lib/python3.10/site-packages/dpdata/system.py:1196, in MultiSystems.check_atom_names(self, system)
   1193 if len(new_in_self):
   1194     # Previous atom_name not in this system
   1195     system.add_atom_names(new_in_self)
-> 1196 system.sort_atom_names(type_map=self.atom_names)

File ~/.conda/envs/MLP/lib/python3.10/site-packages/dpdata/system.py:357, in System.sort_atom_names(self, type_map)
    352         self.add_atom_names(new_atoms)
    353     # index that will sort an array by type_map
    354     # a[as[a]] == b[as[b]]  as == argsort
    355     # as[as[b]] == as^{-1}[b]
    356     # a[as[a][as[as[b]]]] = b[as[b][as^{-1}[b]]] = b[id]
--> 357     idx = np.argsort(self.data['atom_names'])[np.argsort(np.argsort(type_map))]
    358 else:
    359     # index that will sort an array by alphabetical order
    360     idx = np.argsort(self.data['atom_names'])

IndexError: index 5 is out of bounds for axis 0 with size 5
```
