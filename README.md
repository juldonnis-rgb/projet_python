# projet_python

import pandas as pd
import matplotlib.pyplot as plt
print(f"Version pandas: {pd.__version__}")      # bien vérifier qu'il y a une version de pandas que jupyter reconnaît
df = pd.read_csv(r"animes.csv")                 # clean la data (j'ai galeré à trouver la bonne syntaxe pour trouver un fichier sur windows et il fallait aussi que je drag and drop le fichier afin de ne pas écrire tout le chemin d'accès)
print("✅ Fichier chargé avec succès !")
df.head                                         
# Taille du tableau
lignes, colonnes = df.shape

print(f"\n📊 Notre tableau contient :")         # j'ai rajouter "\n" pour que ce soit plus beau à lire(#\n)
print(f"   ➜ {lignes} animés")
print(f"   ➜ {colonnes} Types d'informations") 
# Afficher toutes les colonnes
print(f"\nTypes d'informations :")               #\n
print("-" * 40)


for i, col in enumerate(df.columns, 1):
    print(f"  {i}. {col}")
    # Mettre les noms d'animés en majuscules
noms_majuscules = df['Anime'].str.upper()

print(f"\nListes d'animés :")                    #\n
print(noms_majuscules.head(73).to_string())
# Sélectionner le nom, la note et le studio
colonnes = ['Anime', 'Note_Globale', 'Studio','Genre_Tags','Source','Nb_Episodes','Status','Date_Pub','Note_Meilleur_Ep','Meilleur_Ep_Titre','Comm_Meilleur_Ep','Note_Pire_Ep','Pire_Ep_Titre','Comm_Pire_Ep','Comm_Saison_1','Comm_Saison_2','Comm_Saison_3']
selection = df[colonnes]


# Trier par Note_Globale (du plus haut au plus bas)
df_trie = df.sort_values('Note_Globale', ascending=False)

print(f"\n🏆 Top 10 des animés par note :")         #\n
print(df_trie[['Anime', 'Note_Globale', 'Studio']].head(10).to_string(index=False))
# Moyenne des notes
note_moyenne = df['Note_Globale'].mean()

print(f"\n📊 Note moyenne des animés : {note_moyenne:.2f} / 10")

# Moyenne du nombre d'épisodes
episodes_moyen = df['Nb_Episodes'].mean()
print(f"📺 Nombre moyen d'épisodes : {episodes_moyen:.0f}")
# Note la plus haute et la plus basse
note_max = df['Note_Globale'].max()
note_min = df['Note_Globale'].min()


# Quel animé a la meilleure note ?
meilleur = df[df['Note_Globale'] == note_max]['Anime'].values[0]
pire = df[df['Note_Globale'] == note_min]['Anime'].values[0]
print(f"\n⭐ L'animé le mieux noté est {meilleur}, avec {note_max} de note") 
print(f"🏆 L'animé le moins bien noté est {pire}, avec {note_min} de note") #ici au contraire j'ai enlevé le "\n" car il m'a semblé mieux de réunir ces deux informations liées

# Compter les animés par statut
print(f"\n📊 Répartition par statut :")
print(df['Status'].value_counts())

print("\n" + "="*40 + "\n")

# Top 5 des studios avec le plus d'animés
print("\n🏢 Top 5 des studios :")
print(df['Studio'].value_counts().head(10))
# Histogramme des notes
plt.figure(figsize=(10, 5))

plt.hist(df['Note_Globale'], bins=10, color='#6366f1', edgecolor='white')

# Ajouter la moyenne
moyenne = df['Note_Globale'].mean()
plt.axvline(moyenne, color='red', linestyle='--', linewidth=2, label=f'Moyenne: {moyenne:.1f}')

plt.title("Distribution des notes d'animés", fontsize=14, fontweight='bold') #message d'erreur n'empêchant pas l'éxecution du programme mais j'ai compris que la cause était l'émoji en demandant à l'IA
plt.xlabel("Note")
plt.ylabel("Nombre d'animés")
plt.legend()
plt.show()

print("💡 La ligne rouge montre la moyenne !")
 # Compter les animés par statut
statuts = df['Status'].value_counts()

# Créer le graphique
plt.figure(figsize=(8, 5))
couleurs = ['#22c55e', '#6366f1', '#f59e0b', '#ef4444']
bars = plt.bar(statuts.index, statuts.values, color=couleurs[:len(statuts)])

# Ajouter les valeurs sur les barres
for bar, val in zip(bars, statuts.values):
    plt.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 1, 
            str(val), ha='center', fontweight='bold', fontsize=12)

plt.title(" Nombre d'animés par statut", fontsize=14, fontweight='bold') #message d'erreur n'empêchant pas l'éxecution du programme mais j'ai compris que la cause était l'émoji en demandant à l'IA
plt.ylabel("Nombre d'animés")
plt.show()
