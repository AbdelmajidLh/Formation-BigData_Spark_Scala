# Certains messages d'erreur et comment les résoudre sur votre machine linux

Erreur :
La commande n'a pas pu être trouvée car « /usr/bin:/bin » n'est pas incluse dans la variable d'environnement PATH.
ls : commande introuvable
```bash
export PATH=$PATH:/usr/bin:/bin
```
