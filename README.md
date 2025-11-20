# RN-POSTURE-CORRECTOR
# 📘 README – Etapa 3: Analiza și Pregătirea Setului de Date pentru Rețele Neuronale

**Disciplina:** Rețele Neuronale  
**Instituție:** POLITEHNICA București – FIIR  
**Student:**Ionescu David
**Data:** 11/20/2025


Introducere

Acest document descrie activitățile realizate în **Etapa 3**, în care se analizează și se preprocesează setul de date necesar proiectului „Rețele Neuronale". Scopul etapei este pregătirea corectă a datelor pentru instruirea modelului RN, respectând bunele practici privind calitatea, consistența și reproductibilitatea datelor.

##  1. Structura Repository-ului Github (versiunea Etapei 3)

ErgoVision-AI/
├── README.md
├── docs/
│   └── dataset_info.md    # Detalii despre sursa pozelor
├── data/
│   ├── raw/               # Pozele originale, neprocesate (toate la un loc)
│   ├── split/             # Dataset-ul impartit pentru antrenare
│       ├── train/
│       │   ├── corect/    # 70% din pozele "bune"
│       │   └── gresit/    # 70% din pozele "rele"
│       ├── validation/
│       │   ├── corect/    # 15% din pozele "bune"
│       │   └── gresit/    # 15% din pozele "rele"
│       └── test/
│           ├── corect/    # 15% (pentru evaluarea finala)
│           └── gresit/
├── src/
│   ├── preprocessing/     # Script: resize_and_augment.py
│   └── neural_network/    # Modelul CNN (urmeaza in etapa 4)
└── requirements.txt       # tensorflow, pandas, matplotlib, scikit-learn


##  2. Descrierea Setului de Date

### 2.1 Sursa datelor

Origine: Imagini colectate personal (fotografii realizate cu telefonul/webcam-ul) si/sau imagini preluate din surse publice (Google Images) care ilustreaza postura la birou sau ridicarea greutatilor.

Format: Imagini RGB (color). Extensii: .jpg, .png.

Etichetare: Etichetarea s-a realizat manual prin plasarea imaginilor in folderele specifice (corect vs gresit).
### 2.2 Caracteristicile dataset-ului

*Numar total de imagini: [Ex: 500 imagini (250 Corect / 250 Gresit)].

Dimensiuni originale: Variabile (ex: unele sunt 1920x1080, altele 640x480).

Echilibrul claselor: S-a urmarit colectarea unui numar aproximativ egal de imagini pentru ambele clase pentru a evita bias-ul (tendinta retelei de a favoriza o clasa majoritara).
### 2.3 Descrierea fiecărei caracteristici

Spre deosebire de datele tabelare, aici caracteristicile ("features") sunt reprezentate de pixelii imaginii.

    Intrare (Input): Matrice de pixeli (Inaltime x Latime x 3 canale de culoare - RGB).

    Iesire (Target): Clasa binara (0 = Postura Corecta, 1 = Postura Incorecta).

---

##  3. Analiza Exploratorie a Datelor (EDA) – Sintetic

### 3.1 Statistici descriptive aplicate
Variatii de iluminare: Dataset-ul contine atat imagini luminoase (lumina naturala), cat si intunecate (lumina artificiala slaba), pentru a asigura robustetea modelului.

Fundal: Imaginile au fundaluri diverse (birou, perete alb, hala industriala) pentru a forta reteaua sa invete caracteristicile corpului uman, nu elementele de fundal.

Unghiuri: Subiectii sunt fotografiati preponderent din profil si semi-profil, unghiurile cele mai relevante pentru analiza ergonomica a coloanei vertebrale.

### 3.2 Analiza calității datelor

Unele imagini aveau rezolutie prea mica (< 100px) si au fost eliminate in faza de curatare.

Existau duplicate (aceeasi imagine salvata de doua ori), care au fost sterse pentru a nu influenta validarea.

### 3.3 Probleme identificate

* [exemplu] Feature X are 8% valori lipsă
* [exemplu] Distribuția feature Y este puternic neuniformă
* [exemplu] Variabilitate ridicată în clase (class imbalance)

---

##  4. Preprocesarea Datelor

Pentru a putea antrena o retea CNN, imaginile au necesitat o serie de transformari standard.

### 4.1 Curățarea datelor

Retelele neuronale necesita intrari de dimensiune fixa.

    Toate imaginile au fost redimensionate la 224 x 224 pixeli. Aceasta dimensiune a fost aleasa deoarece este standardul pentru majoritatea arhitecturilor moderne (ex: MobileNetV2, VGG16).

### 4.2 Transformarea caracteristicilor

Valorile pixelilor (in intervalul 0-255) au fost impartite la 255.

Rezultat: Valori in intervalul [0, 1]. Acest pas este critic pentru convergenta rapida a algoritmului de optimizare (Gradient Descent).

### 4.3 Structurarea seturilor de date

Deoarece setul de date este relativ mic, se foloseste augmentarea artificiala pentru a genera noi variante ale imaginilor si a preveni Overfitting-ul (suprainvatarea).

    Tehnici aplicate (on-the-fly):

        Rotation: Rotire aleatoare (+/- 10 grade).

        Zoom: Marire aleatoare (0-20%).

        Horizontal Flip: Oglindire orizontala (simulare subiect stangaci/dreptaci).

        Brightness: Variatii de luminozitate.

### 4.4 Salvarea rezultatelor preprocesării

S-a folosit o impartire stratificata:

    Train (70%): Pentru antrenarea propriu-zisa.

    Validation (15%): Pentru monitorizarea performantei in timpul antrenarii si reglarea hiperparametrilor.

    Test (15%): Date complet noi, folosite doar la final pentru nota/evaluarea obiectiva.

##  5. Fișiere Generate în Această Etapă

Structura de foldere data/split/ populata cu imagini sortate si verificate.

docs/dataset_info.md – Documentatie detaliata a surselor.

src/preprocessing/data_check.py – Script simplu pentru verificarea integritatii imaginilor (sa nu fie fisiere corupte).

##  6. Stare Etapă (de completat de student)

[x] Structura repository configurata pentru imagini.

[x] Imagini colectate si sortate manual in foldere (Corect/Gresit).

[x] Analiza calitatii (EDA) realizata – s-au eliminat pozele neclare.

[x] Pipeline de preprocesare (Resize + Rescale) definit.

[x] Seturi train/val/test generate fizic pe disk.

[x] Documentatie actualizata in README.
---






