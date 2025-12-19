

# Example Projet : Dématérialisation de courrier entrant  - Monitoring des opérations

> [!NOTE]
> Visuels indicatifs de développement, à titre d'illustration

## Contexte 
Projet de mise en place d'une solution de dématéralisation de courrier entrant pour un cabinet d'audit et de conseil, puis maintenance de la solution.

### Le process en ultra-bref

:incoming_envelope: > scan :page_facing_up: > import :open_file_folder:> indexation :label: :desktop_computer: > traitement business :gear: :briefcase: > export pour archivage :file_cabinet:

### Stack
Windows server-IIS/SQL Server/MS Azure/O365
Hyland OnBase - C# et SQL

## Cherchez la data
La data est partout et capitale dans ce type de projet qui semble centré sur le document mais repose surtout sur une base de données structurée, des workflows bien calibrés, des classes bien organisées :
* Données métier (décrivant pour un objet courrier le client, la mission, le type de document, d'affranchissement, les destinataires, le courrier en lui-même, etc.)
* Données techniques (identifiants de batch, de machines, horadatage, statuts de workflows, de jobs, données de paramétrage, etc.)
* Données issues de référentiels (ici utilisateurs, clients, missions)
qu'il faut:
* identifier
* formater
* contrôler
* organiser dans un modèle de données 
* faire fonctionner
* documenter
* maintenir

<!--
Source - https://stackoverflow.com/a
Posted by bim, modified by community. See post 'Timeline' for change history
Retrieved 2025-12-19, License - CC BY-SA 4.0
-->

<figure>
<p align="center" width="100%">
  <img src="P1_data_model.png" alt="Version de travail Modèle de données P1" style="width:60%">
  <figcaption><h6 align="center">Version de travail du modèle de données</h6></figcaption>
  </p>
</figure>

## :bar_chart: Focus Besoin de reporting 
Au sein de la solution même,
> Mettre à disposition le monitoring de la solution, du point de vue de l'éxécution automatique (imports de documents, workflows de traitements, exports)
mais aussi des actions utlisateurs au sein des workflows fontionnels.

> Monitorer la mise à jour automatisée quotidienne des données utiles issues de 3 référentiels client

### :hammer_and_wrench: Actions mises en place 
> Monitoring de la solution : 
* Utilisation du module Reporting Dashboards du progiciel utilisé OnBase (Hyland)
* Accès via client lourd ou via le client web directement par URL, déjà exploités par les utilisateurs métier pour les workflows fonctionnels comme services et techniques pour les worflows de traitement.
* Droits d'accès aux dashboards selon les groupes utilisateurs et rôles associés

> Monitoring de la mise à jour automatisée quotidienne des données utiles issues de 3 référentiels client :  
* Logs spécifiques créés directement via le script d'import en C#.
* Ces logs sont ensuite exploités comme des objets de la solution et consultables dans une vue dédiée aux administrateurs.
 
#### Liste des rapports dynamiques mis en place

* Actions stats
Statistiques par action utilisateur une fois le document validé (e.g. : Paper version request, PDF export, etc.), par utilisateur et groupe d'utilisateurs
<figure>
<p align="center" width="100%">
  <img src="P1_action_stats.png " alt="Exemple web Actions stats" style="width:60%">
  <figcaption><h6 align="center">Exemple reporting web 'Action stats'</h6></figcaption>
  </p>
</figure>
<figure>
<p align="center" width="100%">
  <img src="P1_action_stats_parameters.png" alt="Exemple web Actions stats-date picker" style="width:40%">
  <figcaption><h6 align="center">Exemple date picker web 'Action stats'</h6></figcaption>
  </p>
</figure>

* Control stats
Statistiques par action utilisateur en phase de contrôle du document (Qualify and Send, or Forward back to mailroom, and reason for forwarding back), by user and user group
Détail raisons de refus/renvoi du document après numérisation
<p align="center" width="100%">
  <img src="P1_back2mr.png" alt="Exemple Nombre de renvois par motif de refus" style="width:60%">
  <figcaption><h6 align="center">Exemple Nombre de renvois par motif de refus</h6></figcaption>
  </p>
</figure>

* __Import stats - month details__ : 
Imports de documents par Service et par mois (importés via scanners tiers)
* __Indexing stats - month details__ : 
Indexation des documents par service, par mois et temps moyen d'indexation
* __Mailroom global stats - day__ : 
Documents importés et indexés par batch et par jour (enveloppes exclues)
* __Mailroom global stats - month__ : 
Documents importés et indexés par batch et par mois (enveloppes exclues)
* __User activity__ :
Connexions par utilisateur et groupe d'utilisateurs par jour
* __Disk Group report__ : 
Espace disque utilisé par chaque service/type sur le srveur de fichiers (NAS).
* __TAX-PAS push AR auto__ : 
Nombre et ID des documents AR exportés automatiquement en PDF 
* __PDF auto Email stats__ : 
Nombre et statut des emails en envoi automatique avec PDF attaché (seulement pour les services éligibles)
* __License usage monitoring__ : 
License pic usage monitoring- par année, mois, jour, utilisateurs uniques

### :dart: Exemples d'améliorations identifiées grâce à ces rapports, et de résolutions d'incidents auxquelles ils ont contribué
* Ajustement de la résolution des scanners pour équilibrer volumes de fichiers et confort d'exploitation du document numérisé par l'utilisateur
* Ajustement des volumes de licenses et prévisions d'accroissement au fil du déploiement
* Identification, analyse et résolution d'une sauvergarde tierce de DB qui interrompait certains jobs
* Réactivité et reprise en cas d'incident réseau quand les envois auto d'emails ou les dépôts de pdf par la solution étaient affectés













