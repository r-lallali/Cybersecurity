# US 9 :

Dans cette partie nous nous intéresserons à l'ID de la clé USB dont on nous a fourni le dump. Tout d'abord, nous récupérons plusieurs fichiers en binaire illisibles directement dont un qui nous intéresse particulièrement, le fichier SYSTEM.

Afin de déchiffrer ces fichiers, nous utilisons une librairie Python qui se nomme Registry, elle nous permet de convertir du binaire en texte.

Grâce au code ci-dessous, nous récupérons donc le contenu de SYSTEM :

```python
from Registry import Registry

reg = Registry.Registry("SYSTEM")

def dump_key(key, depth=0):
    path = key.path()
    print(f"{path}")
    for val in key.values():
        try:
            print(f"{path}\\{val.name()} = {val.value()}")
        except Exception:
            print(f"{path}\\{val.name()} = [erreur lecture]")
    for subkey in key.subkeys():
        dump_key(subkey, depth + 1)

dump_key(reg.root())
```

Une fois le contenu converti et ouvert, on fait face à plusieurs milliers de lignes d'informations, ici nous devons chercher le registre **SYSTEM/ControlSet001/Enum/USBSTOR** qui correspond aux copies de la configuration actuelle et plus précisément le chemin **Enum/USBSTOR** correspond aux derniers périphériques de stockage connectés à l'ordinateur.

En extrayant via une recherche dans le fichier ou par une requête grâce à **RegRipper** :

```
perl rip.pl -r SYSTEM -p usbstor
```

On obtient alors les informations suivantes sur le périphérique de stockage USB :

```
Launching usbstor v.20200515
usbstor v.20200515
(System) Get USBStor key info

USBStor
ControlSet001\Enum\USBStor

Disk&Ven_SanDisk&Prod_Cruzer_Blade&Rev_1.00 [2020-02-03 12:12:32]
  S/N: 4C530000281008116284&0 [2020-02-03 12:12:32Z]
  Device Parameters LastWrite: [2020-02-03 12:12:32Z]
  Properties LastWrite       : [2020-02-03 12:12:42Z]
    FriendlyName          : SanDisk Cruzer Blade USB Device
    First InstallDate     : 2020-02-03 12:12:32Z
    InstallDate           : 2020-02-03 12:12:32Z
    Last Arrival          : 2020-02-03 12:44:21Z
    Last Removal          : 2020-02-03 12:45:00Z
```

Ainsi son UID correspond ici à S/N soit **4C530000281008116284.**

Si on avait cherché directement dans le fichier on aurait lu directement :

```
ROOT\ControlSet001\Enum\USBSTOR
ROOT\ControlSet001\Enum\USBSTOR\Disk&Ven_SanDisk&Prod_Cruzer_Blade&Rev_1.00
ROOT\ControlSet001\Enum\USBSTOR\Disk&Ven_SanDisk&Prod_Cruzer_Blade&Rev_1.00\4C530000281008116284&0
ROOT\...\4C530000281008116284&0\DeviceDesc = @disk.inf,%disk_devdesc%;Disk drive
ROOT\...\4C530000281008116284&0\Capabilities = 16
ROOT\...\4C530000281008116284&0\Address = 6
ROOT\...\4C530000281008116284&0\ContainerID = {3899d6a0-755e-5c4f-9aec-4655aee0f935}
ROOT\...\4C530000281008116284&0\HardwareID = ['USBSTOR\DiskSanDisk_Cruzer_Blade____1.00',
                                             'USBSTOR\DiskSanDisk_Cruzer_Blade____',
                                             'USBSTOR\DiskSanDisk_', 'USBSTOR\GenDisk', 'GenDisk']
ROOT\...\4C530000281008116284&0\CompatibleIDs = ['USBSTOR\Disk', 'USBSTOR\RAW', 'GenDisk']
ROOT\...\4C530000281008116284&0\ClassGUID = {4d36e967-e325-11ce-bfc1-08002be10318}
ROOT\...\4C530000281008116284&0\Service = disk
ROOT\...\4C530000281008116284&0\Mfg = @disk.inf,%genmanufacturer%;(Standard disk drives)
ROOT\...\4C530000281008116284&0\FriendlyName = SanDisk Cruzer Blade USB Device
ROOT\...\4C530000281008116284&0\ConfigFlags = 0
ROOT\...\4C530000281008116284&0\Device Parameters\Partmgr\DiskId = {635b1203-467e-11ea-ba75-000c295e7ac0}
```
