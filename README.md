# Optimizarea imaginilor
# Scopul lucrării
Scopul lucrării este de a se familiariza cu metodele de optimizare a imaginilor.
# Sarcina
Compararea diferitelor metode de optimizare a imaginilor:

Ștergerea fișierelor temporare și a dependențelor neutilizate

Reducerea numărului de straturi

Utilizarea unei imagini de bază minime

Reambalarea imaginii

Utilizarea tuturor metodelor

# Execuție

Cream un repozitoriu containers09 și il copiam pe computer. În directorul containers09 cream directorul site și plasam în el fișierele site-ului (html, css, js).

![image](https://github.com/user-attachments/assets/6e8bb196-ecba-4d03-b2b5-153c924cd7f7)

![image](https://github.com/user-attachments/assets/618138f3-7b0b-488c-b728-1b39b27b0946)

Pentru teste de optimizare a imaginilor va fi utilizată imaginea creată în bază Dockerfile.raw:

![image](https://github.com/user-attachments/assets/3a66dc55-6750-48f2-b96e-f4f81674dd3b)

Cream în folderul containers09 și construim imaginea cu numele mynginx:raw:

![image](https://github.com/user-attachments/assets/c88db3a8-f3ca-4f3b-bb45-a1dd62117240)

# Eliminarea dependențelor neutilizate și a fișierelor temporare

Eliminam fișierele temporare și dependențele neutilizate în Dockerfile.clean:

![image](https://github.com/user-attachments/assets/4e63e86e-cc9c-474c-9b17-831a48f35389)

Asamblam imaginea cu numele mynginx:clean și verificam dimensiunea:

![image](https://github.com/user-attachments/assets/02d6ba01-af57-43dd-a304-c4e7e7fcb9cc)

![image](https://github.com/user-attachments/assets/a0e3129e-cc33-4c2e-a06b-8cf163ef5973)

# Minimizarea numărului de straturi

Minimizam numărul de straturi în Dockerfile.few:

![image](https://github.com/user-attachments/assets/4b91abdc-f23c-413a-a092-9f15e08574a2)

Construim imaginea cu numele mynginx:few și verificam dimensiunea:

![image](https://github.com/user-attachments/assets/cbcbe556-65f4-43b4-9195-9e0109c864e5)

![image](https://github.com/user-attachments/assets/a3c865f5-c1f7-439d-990e-258987b4104c)

# Utilizarea unei imagini de bază minime

Înlocuim imaginea de bază cu alpine în Dockerfile.alpine:

![image](https://github.com/user-attachments/assets/0a01b525-c993-4d14-9255-62f15e208bf3)

Construim imaginea cu numele mynginx:alpine și verificam dimensiunea:

![image](https://github.com/user-attachments/assets/58c64778-b3c7-49aa-81b7-fe64e3d35bb9)

![image](https://github.com/user-attachments/assets/1c11b129-7e40-46e4-80cf-76a72e37efa2)

# Repachetarea imaginii

Repachetam imaginea mynginx:raw în mynginx:repack:

```
docker container create --name mynginx mynginx:raw
docker container export mynginx | docker image import - mynginx:repack
docker container rm mynginx
docker image list
```
![image](https://github.com/user-attachments/assets/55a2ca3e-76e4-49ee-895f-65de73e6970c)

# Utilizarea tuturor metodelor

Cream imaginea mynginx:minx utilizând toate metodele de optimizare. Pentru aceasta definim următorul Dockerfile.min:

![image](https://github.com/user-attachments/assets/072fd577-e8cb-439c-bafd-d32b01ad197e)

Construim imaginea cu numele mynginx:minx și verificam dimensiunea. Repachetam imaginea mynginx:minx în mynginx:min:

![image](https://github.com/user-attachments/assets/ebe768cf-338d-4bdb-a8f9-b38ec249038e)

# Pornire și testare

Verificați dimensiunea imaginilor:

![image](https://github.com/user-attachments/assets/43d94d05-c4d5-4d48-8030-82b1e65ccf1f)

# Răspunsuri la întrebări:
# Care metoda de optimizare a imaginilor vi se pare cea mai eficientă?

Utilizarea Alpine ca imagine de bază oferă cea mai semnificativă reducere a dimensiunii. Alpine este mult mai mică decât Ubuntu, ceea ce face ca imaginea finală să fie mult mai compactă. Combinarea tuturor metodelor (Alpine + minimizarea straturilor + curățarea) oferă rezultatul optim.

# De ce curățirea cache-ului pachetelor într-un strat separat nu reduce dimensiunea imaginii?

În Docker, fiecare instrucțiune RUN creează un nou strat. Chiar dacă ștergem fișierele într-un strat nou, acestea au fost deja adăugate în straturile anterioare. Dimensiunea totală a imaginii include toate straturile. De aceea, este mai eficient să combinăm instalarea și curățarea într-o singură instrucțiune RUN, astfel încât fișierele cache să nu fie niciodată persistate într-un strat.
    
# Ce este repachetarea imaginii?

Repachetarea imaginii implică exportarea și importarea unui container. Acest proces comprimă toate straturile imaginii într-un singur strat, eliminând potențiale "straturi moarte" și reducând dimensiunea totală. Cu toate acestea, dezavantajul este că pierdem istoria straturilor și capacitatea de a utiliza cache-ul la reconstruirea imaginii.

# Concluzii

Optimizarea imaginilor Docker este esențială pentru reducerea costurilor de stocare, îmbunătățirea vitezei de descărcare și a performanței generale. Cele mai eficiente metode de optimizare sunt:

Utilizarea unei imagini de bază minime (Alpine)
Combinarea comenzilor pentru a reduce numărul de straturi
Curățarea cache-urilor și a fișierelor temporare în aceeași instrucțiune RUN în care au fost create

Aceste tehnici pot reduce semnificativ dimensiunea imaginilor Docker, făcându-le mai eficiente și mai rapide pentru deployment.
