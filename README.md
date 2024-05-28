## Extraction de texte à partir d'images et visualisation en utilisant deux méthodes différentes (EasyOCR et Keras-OCR)

Ce projet démontre comment lire, afficher et extraire du texte à partir d'images en utilisant OpenCV, Matplotlib et EasyOCR. en utilisant **Google Colab**

### Requirements

Avant d'exécuter le code, assurez-vous d'avoir installé les packages Python suivants :

- OpenCV
- Matplotlib
- EasyOCR
- Pandas

Vous pouvez installer ces packages en utilisant pip:

```sh
pip install pip install opencv-contrib-python matplotlib easyocr pandas
```

Ou vous pouvez installer toutes les dépendances en utilisant requirements.txt :

```sh
pip install -r requirements.txt
```

### Résultats

- **EasyOCR:**
  A extrait environ 60 % du texte de l'image.
  
- **Keras-OCR:**
  A extrait près de 90 % du texte de la même image.



## Extraction de tableau depuis une image en préservant le format du tableau en utilisant img2table

Ce projet vise à extraire du tableau à partir d'une image tout en préservant le format original du tableau. Voici un résumé du processus :

1. **Installation des dépendances** :
   Les dépendances nécessaires sont installées pour le projet. Les packages utilisés sont importés depuis :
   - `img2table.document`
   - `img2table.ocr`
   - `pandas`
     Vous pouvez installer ces packages en utilisant pip:

    ```sh
    pip install pip install img2table[easyocr]
    ```
    ```sh
    pip install pip install img2table[paddle]
    ```

3. **Extraction de texte avec EasyOCR** :
   - Une image est initialisée avec la source de l'image.
   - EasyOCR est utilisé pour l'extraction de texte en spécifiant la langue française.
   - Les tableaux sont extraits avec EasyOCR en détectant les tableaux sans bordure.
   - Les résultats sont stockés et converti en DataFrame.
  
     **Remarque**:
     cette methode n'arrive pas à differentier 5 et S au niveau de header

4. **Extraction de texte avec PaddleOCR** :
   - Une autre méthode d'extraction de texte est utilisée avec PaddleOCR en spécifiant la langue française et des paramètres supplémentaires.
   - Les tableaux sont extraits avec PaddleOCR.
   - Les résultats sont stockés et converti en DataFrame.

     **Remarque**:
     cette methode n'arrive pas à a lire le header(S) il le considère comme une valeur None 

5. **Formater le tableau pour que ça correspond a ce qui est affiché et le convertir en json format** :
   
  ###### Traitement des Deux Premières Lignes de la Première Colonne
  - La fonction `process_first_two_rows(df)` traite les deux premières lignes de la première colonne du DataFrame.
  - Pour chaque ligne, elle extrait le premier élément de la cellule s'il existe, sinon elle ajoute 'None' à la liste des éléments traités.
    ```python
        def process_first_two_rows(df):
            processed_elements = []
            for i in range(3):
                cell = df.iloc[i, 0]
                if pd.isna(cell):
                    processed_elements.append(None)
                else:
                    elements = cell.split('\n')
                    # Take the first element
                    processed_elements.append(elements[0])
            return processed_elements

  ###### Traitement des Lignes Restantes de la Première Colonne
  - La fonction `process_remaining_rows(df)` traite les lignes restantes de la première colonne du DataFrame.
  - Elle extrait les éléments suivants de chaque cellule à partir de la troisième ligne, en les ajoutant à la liste des éléments traités.
  - Si une ligne ne contient pas suffisamment d'éléments, elle ajoute 'None' à la liste des éléments traités.
```python
    def process_remaining_rows(df):
    processed_elements = []
    for i in range(3, len(df.iloc[:, 0])):
        cell = df.iloc[i, 0]
        if pd.isna(cell):
            processed_elements.append(None)
        else:
            elements = cell.split('\n')
            # Remove the first element
            elements = elements[1:]
            if i - 3 < len(elements):
                processed_elements.append(elements[i - 3])
            else:
                processed_elements.append(None)
    return processed_elements



