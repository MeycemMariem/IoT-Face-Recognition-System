EN : This README file is presented in English first, followed by a French translation at the end.

FR : La première partie de ce fichier README est rédigée en anglais, tandis que la version française se trouve à la fin.

# IoT Face Recognition System (EN)
The system aims to identify people through cameras installed at different locations in a work environment and to mark the location of these people at a given time.

The system defines two units:

* Multimedia Unit: Cameras + Motion Sensors + PC
* Processing Unit: Raspberry Pi 3
  
Once the motion sensors installed near the entrances of the different zones detect movement, the constantly connected camera will take a capture of the affected area.

This photo will be sent to the processing unit using the **MQTT** protocol, where it first goes through a facial detection test where we check if the received image actually contains a person.

Facial detection is done using a **TensorFlow coco-ssd** node that returns the nature of the objects detected in the photo.
In the case where the returned value is indeed of type "person", a **Python** script `/processing_unity/treat_image.py` using the **OpenCv** library is executed.

**NOTE:** This module's training data comes from a Kaggle dataset, which you can access [here]([https://](https://www.kaggle.com/datasets/kasikrit/att-database-of-faces)). To make the model recognize you, add a folder with your own *pgm* images (like the existing ones). Convert your photos online if needed.

The script trains a Machine Learning model on a dataset of preserved photos in order to be able to process the captured photo and discern the faces it contains, returning the IDs of the recognized faces as output.

The flow deployed in this way in the Processing Unit allows to retrieve:

* ID: ID of the recognized student
* Date: the date of the capture
* Cam #: Camera number that took the capture from which we can find the location.

These three pieces of information will eventually be sent to the Tomcat web server via a **http POST** request to the URI:
`http://<Server@IP>:3031/HelloWorl14.05.v1/api/h/savePlace?ID={{ID}}&Time={{Time}}&Cam={{Cam}}`

This repository contains the scripts and **Node Red** flows necessary to implement this system. Below, we define its hierarchy.

IoT Face Recognition System  
├───media_unit  
│   └───media_unit_flow.json  
├───processing_unit  
│   ├───processing_unit_flow.json  
│   └───processing_scripts  
│       ├───faces_dataset  
│       ├───taken_images  
│       ├───process_image.py  
│       └───train.py  
└───README.md

# Système IoT de reconnaissance faciale (FR)

Ce système a pour objectif d’identifier des personnes à travers des caméras installées aux différents emplacements d'un environnement de travail et de marquer l'emplacement de ces personnes à un temps donné.

Le système définit deux unités :
* Unité multimédia : Caméras + Capteurs de mouvement + PC
* Unité de traitement : Raspberry Pi 3
  
Une fois les capteurs de mouvement installés près des entrées des différentes zones détectent de mouvement, la caméra constamment branchée prendra une capture de la zone concernée.

Cette photo sera envoyée à l’*unité de traitement* à l’aide du protocole **MQTT**, où elle passe au début par un test de détection de visages où on vérifie si l’image reçue contient pour de vrai une personne.

La détection faciale se fait grâce à un nœud **TensorFlow coco-ssd** qui retourne la nature des objets détectés dans la photo.
Dans le cas où la valeur retournée est bien de type « person », un script **Python** `/processing_unity/treat_image.py` utilisant la libraire **OpenCv** est exécuté.

Les données d'entraînement de ce module proviennent d'un jeu de données Kaggle, que vous pouvez consulter [ici](https://www.kaggle.com/datasets/kasikrit/att-database-of-faces). Pour que le modèle reconnaisse votre visage, ajoutez un dossier contenant vos propres images au format pgm (comme ceux existants). Convertissez vos photos en ligne si nécessaire.

Le script fait entrainer un modèle de Machine Learning sur un dataset des photos préservées pour pouvoir traiter la photo capturée et discerner les visages qu’elle contient, retournant en tant qu’output les identifiants des visages reconnus.

Le flow déployé ainsi dans l'Unité de traitement permet de récupérer :
* Identifiant : id de l’étudiant reconnu
* Date : la date de la capture
* N°Cam : Numéro de la caméra qui a prise la capture à partir duquel on peut retrouver l’emplacement.

Ces trois informations seront à la fin envoyées au serveur web Tomcat via une requête **http POST** vers l’URI :
`http://<Server@IP>:3031/HelloWorl14.05.v1/api/h/savePlace?ID={{ID}}&Time={{Time}}&Cam={{Cam}}`

Cette repository contient les scripts et les flows **Node Red** nécessaires à la réalisation de ce système. Ci-dessous, nous définissons son arborescence.

IoT Face Recognition System  
├───media_unit  
│   └───media_unit_flow.json  
├───processing_unit  
│   ├───processing_unit_flow.json  
│   └───processing_scripts  
│       ├───faces_dataset  
│       ├───taken_images  
│       ├───process_image.py   
│       └───train.py  
└───README.md
