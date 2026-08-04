# Maquette Airtable Herrikoa - notice

Maquette visuelle d'avant-vente produite à partir de `CDC-maquette-airtable-herrikoa (1).md`.

**Livrable** : `maquette-herrikoa.html`, un seul fichier autonome (CSS et JS inclus, aucune dépendance externe, aucun appel réseau). S'ouvre dans n'importe quel navigateur, conçu pour 1440 px de large. Feuille de style d'impression incluse : la page affichée à l'écran est celle qui s'imprime, en paysage A4.

## Structure : 6 interfaces, 16 pages

Les six interfaces sont visibles en permanence dans la barre latérale, chacune avec ses vues en dessous.

**Pilotage**
| Page | Type Airtable |
|---|---|
| Tableau de bord portefeuille | Tableau de bord |
| Tableau de bord prospection | Tableau de bord |
| Tableau de bord contacts | Tableau de bord |

**Participations**
| Page | Type Airtable | Fiche au clic |
|---|---|---|
| Pipe prospects | Kanban (empilage : Statut) | Entreprise |
| Liste des entreprises | Liste | Entreprise |
| Liste des contacts | Liste | Contact |
| Opérations de financement | Liste | Opération |
| AG des entreprises | Liste **ou** Calendrier, au choix | Assemblée générale |

**Actionnaires** : Liste des actionnaires (Liste, fiche Contact au clic)

**Partenaires** : Liste des entreprises partenaires (fiche Organisation) · Liste des contacts partenaires (fiche Contact)

**Institutionnels** : Liste des entreprises institutionnels (fiche Organisation) · Liste des contacts institutionnels (fiche Contact)

**Communication**
| Page | Type Airtable | Fiche au clic |
|---|---|---|
| Liste des publications LinkedIn | Liste | Publication |
| Liste des évènements | Liste | Évènement |
| Calendrier | Calendrier | Publication ou Évènement |

## Le modèle de données

Sept tables du CDC, plus une table ajoutée.

| Table | Origine |
|---|---|
| Entreprises | §4.1 |
| Opérations de financement | §4.2 |
| Contacts | §4.3 |
| Notes de suivi | §4.4 |
| Assemblées générales | §4.5 |
| Publications et évènements | §4.7, fusionnée avec §4.6 |
| **Organisations** | nouvelle |

**Organisations** regroupe les personnes morales qui ne sont pas des participations : actionnaires personnes morales, partenaires et institutionnels. Une seule table avec un champ `Type` à trois valeurs, déclinée en trois vues filtrées - le même principe que vous appliquez déjà aux contacts. Champs : `Nom`, `Type`, `Adresse`, `Code postal`, `Ville`. Si vous préférez trois tables séparées, rien ne change visuellement, seule cette notice est à corriger.

Cette table est bien distincte de **Entreprises** §4.1, qui ne contient que les sociétés du portefeuille et les dossiers en cours d'instruction.

## Les fiches détail

Ce sont des pages **Entrée** : rubriques (Groupes) empilées les unes sous les autres, chaque champ sur sa ligne, libellé à gauche. Une page Entrée Airtable n'a pas de mise en page en colonnes.

| Fiche | Champs | Détail |
|---|---|---|
| Entreprise | 21 | §4.1 à l'identique |
| Opération de financement | 10 | §4.2 à l'identique |
| Contact | 14 | les 11 champs de §4.3 + 3 ajouts validés |
| Organisation | 6 | table nouvelle |
| Note de suivi | 5 | §4.4 à l'identique |
| Assemblée générale | 5 | §4.5 à l'identique |
| Publication | 9 | §4.7 à l'identique |
| Évènement | 13 | §4.7 + les 4 champs de §4.6 |

Les trois champs ajoutés sur Contacts :
- `Statut entreprise` - lookup du statut de l'entreprise liée. C'est lui qui alimente les onglets Prospects / Membres / Anciens membres de la liste des contacts (instruction en cours → Prospect, Portefeuille → Membre, Sorti ou Arrêté → Ancien membre).
- `Organisation` - lien vers la table Organisations, pour un actionnaire investissant en personne morale, un partenaire ou un institutionnel.
- `AG Herrikoa` - sélection Inscrit / Non inscrit, affichée en vert et rouge dans la liste des actionnaires.

Publications et évènements partagent une seule table, mais leurs deux pages Entrée n'affichent chacune que les champs qui les concernent : la fiche évènement ajoute `Invités`, `Inscrits`, `Relances à faire` et `Statut`, que la fiche publication n'affiche pas.

Toutes les sélections déroulantes contiennent toutes les options du CDC, pour vérification. La fiche entreprise porte en plus ses quatre rubriques d'enregistrements liés, alimentées par les vraies données : Lynxter affiche ses 3 opérations et 5 notes, Alki les 2 siennes.

## Points à vérifier ou à arbitrer

**1. Le sélecteur Liste / Calendrier sur la page AG des entreprises.**
Il est maquetté comme demandé, en haut à droite, et il fonctionne. À confirmer côté Airtable : la référence interne décrit `Visualisations` comme un réglage de conception, pas comme un sélecteur offert à l'utilisateur final. Si ce n'est pas disponible, il faudra deux pages.

**2. Les indicateurs de kanban et de liste sont sur les tableaux de bord.**
Le CDC les demandait en haut du Pipe prospects et au-dessus du tableau contacts. Les éléments Numéro n'existent que sur une page Tableau de bord ; ni un Kanban ni une Liste n'en portent. D'où les trois pages de Pilotage.

**3. La colonne « Décision » du kanban n'existe pas.**
Un Kanban n'affiche que les valeurs de son champ d'empilage, et « Décision » n'est pas une valeur du `Statut` du CDC. Les colonnes sont les vraies étapes, jusqu'à Arrêté qui porte les motifs d'arrêt.

**4. La liste d'entrées du tableau de bord est limitée à 3 champs.**
« Entreprises à surveiller » montre donc nom, dernier point de suivi et montant engagé, la couleur portant le niveau de vigilance.

**5. Un champ calculé ajouté : `Date du dernier point de suivi` sur Entreprises.**
Rollup MAX sur Notes de suivi, sans lequel la liste « Entreprises à surveiller » ne peut pas afficher cette colonne demandée par le CDC.

**6. Deux graphiques ajoutés sur les tableaux de bord prospection et contacts.**
Dossiers par étape, origine des dossiers, répartition par type, origine des contacts. Tous dérivés de champs existants, tous natifs (Barre et Donut). Sans eux, ces deux pages n'auraient porté que trois ou quatre chiffres.

**7. Le suivi d'inscription de l'AG Herrikoa.**
Il revient partiellement par la colonne `AG Herrikoa` de la liste des actionnaires. Le détail chiffré du CDC écran 4b (5 840 invités, 312 inscrits, 190 relances) n'est pas affiché : vous avez indiqué de ne pas l'ajouter au tableau de bord.

**8. Défilement horizontal sur les vues larges.**
Six colonnes de kanban, ou une liste à plus de dix colonnes, ne tiennent pas dans 1200 px : la page défile horizontalement, comme Airtable. À l'impression, les colonnes passent à la ligne.

## Ce qui n'apparaît pas, volontairement

Tous les points du CDC §8 sont respectés : aucun scoring ni indicateur de santé, aucune alerte automatique sur les échéances, aucune statistique LinkedIn, aucun graphique sur les écrans de communication, aucune cartographie interactive, aucun écran budgétaire, aucun assistant conversationnel.

Le champ `Coordonnées` figure sur la fiche entreprise comme demandé au CDC §4.1, mais aucune carte n'est affichée.

## Données

Fictives. Les participations réelles nommées au CDC (Alki, Lynxter, Wikicampers, Tekniaero, Don Quichosse, La Boîte Concept, Ma Petite Sponso, Pateco) sont toutes en vigilance « Normal ». Les entreprises en « À surveiller » ou « Difficulté », les prospects, les dossiers arrêtés, les organisations et tous les contacts portent des noms inventés : il n'était pas souhaitable d'afficher une société réelle et identifiable comme étant en difficulté dans un document destiné à circuler.

Les chiffres se recoupent d'un écran à l'autre : 68 participations, 3 420 000 € engagés, 18 dossiers en cours pour 940 000 € sollicités, 185 000 € sur Lynxter (somme de ses trois opérations, identique dans la liste des opérations et dans sa fiche).
