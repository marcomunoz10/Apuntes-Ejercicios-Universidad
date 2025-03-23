Per eliminar una imatge Docker, segueix aquests passos:

---

### **1. Llista les imatges existents**

Executa aquesta comanda per veure totes les imatges al sistema:

```bash
sudo docker images
```

Apareixerà una sortida semblant a aquesta:

```
REPOSITORY          TAG          IMAGE ID       CREATED          SIZE
<none>              <none>       d5f52de61a11   2 minutes ago    150MB
httpd               latest       494b2b45fd74   3 days ago       138MB
```

- **IMAGE ID** és l'identificador únic de la imatge que necessites per eliminar-la.

---

### **2. Elimina una imatge específica**

Executa aquesta comanda per eliminar la imatge, utilitzant el seu **IMAGE ID**:

```bash
sudo docker rmi <IMAGE_ID>
```

Per exemple:

```bash
sudo docker rmi d5f52de61a11
```

---

### **3. Si la imatge està en ús**

Si la imatge està associada a un contenidor actiu o aturat, hauràs de fer el següent:

#### **a. Identifica el contenidor que usa la imatge**

Llista tots els contenidors, inclosos els aturats:

```bash
sudo docker ps -a
```

#### **b. Atura el contenidor si està actiu**

Si el contenidor està en funcionament, atura’l:

```bash
sudo docker stop <CONTAINER_ID>
```

#### **c. Elimina el contenidor**

Un cop aturat, elimina el contenidor:

```bash
sudo docker rm <CONTAINER_ID>
```

#### **d. Torna a eliminar la imatge**

Ara pots eliminar la imatge:

```bash
sudo docker rmi <IMAGE_ID>
```

---

### **Nota: Forçar l'eliminació**

Si necessites forçar l'eliminació d'una imatge (per exemple, si encara està en ús):

```bash
sudo docker rmi -f <IMAGE_ID>
```

### **Activar un contenidor**

```shell
sudo docker start nom_docker
```


