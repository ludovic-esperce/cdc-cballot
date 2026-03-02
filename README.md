# Vote numérique des délégués - Cballot

## Descriptif du projet

Le vote des délégués pour la formation approche et cette année l'Afpa souhaite mettre en place le vote numérique pour simplifier le processus de vote.

L'objectif est de développer une application web comportant deux modules :

- une module application présentant une interface d'administration permettant à une personne habilitée de créer des sessions de formation, y associer des stagiaires et organiser une séance de vote ;
- un module permettant aux stagiaires d'une formation de choisir un binôme de délégué.e.s qui se présentent, ceci pour une séance de vote pré-organisée.

Le vote devra être anonyme (rien de l'identité des votants ne devra être stocké en BDD sur le long terme, il n'y aura que leurs choix).

Le nom du projet est libre, il vous est proposé "Cballot" pour "Class ballot" mais toute proposition peut être acceptée.

---

## Détails des fonctionnalités

Cette partie détaille :
1. Les rôles des utilisateurs 
2. **"Organisation de scrutin"** : module d'organisation du vote
3. **"Isoloir"** : module de vote pour les stagiaires 

### Rôles des utilisateurs

Dans le cadre du projet Cballot, deux rôles principaux sont définis pour les utilisateurs :
- Stagiaires votants
    - participants aux sessions de formation ;
    - accèdent à l'interface de vote via un code unique ;
    - ne peuvent voter qu'une seule fois.

- Organisateur de scrutin (formateur / responsable de formation / assistant de formation)
    - responsables de l'organisation et de la gestion des scrutins ;
    - créer des sessions de formation ;
    - ajouter et supprimer des stagiaires ;
    - planifier des séances de vote ;
    - gérer les binômes de candidats.

### Module "organisation de scrutin"

#### Création des sessions de formation

L'administrateur doit pouvoir créer une nouvelle session de formation en spécifiant les détails suivants :
- nom de la session
- date de début et de fin
- liste des stagiaires participants

> [!NOTE]
> Une session de formation est rattachée à une formation pré-existante.
>
> Par exemple, la formation CDA Brest se déroulant du 24/04/2026 au 13/02/2027 est une session de la formation **CDA**.

L'utilisateur administrateur devra donc pouvoir créer des formations (en plus des sessions qui leur sont associées).

Pour chaque session de formation créée, l'administrateur pourra ajouter des stagiaires votants. Seules les adresses email sont **stockée temporairement** dans la base de données afin de permettre aux votants de déposer leur voix.

#### Organisation d'un scrutin

**Planification d'un scrutin**

L'administrateur doit pouvoir planifier une séance de vote pour **une session de formation** en spécifiant :
- date et heure du vote ;
- liste des binômes candidats.

**Gestion des binômes candidats**
L'administrateur doit pouvoir ajouter, modifier et supprimer des binômes de candidats pour un scrutin.

Un binôme de candidats est composé d'un **délégué principal** et d'un **suppléant**.

### Module "isoloir"

Cette partie détaille le fonctionnement du module de vote dédié aux stagiaires.

**Interface de vote**
Un email de demande de participation au vote est envoyée à chaque votant **1 heure** avant le début du vote.

Cet email devra notamment contenir la liste des binômes qui se présentent.

Les votants pourronta accéder à une interface de vote où ils pourront voter pour un binôme.

Il devra être possible d'acéder à cette interface de vote via une URL envoyée par email.

**Code unique et anonymat du vote**
Le vote doit être anonyme, seul le choix du votant doit rester stocké en base de données.

Afin de comptabiliser les votes un numéro unique tel qu'un [UUID](https://www.postgresql.org/docs/current/datatype-uuid.html) pourra être généré pour chaque stagiaire votant.

> [!CAUTION]
> Pour des raisons d'anonymat le numéro unique des votants ne devra pas rester en base de données et devra être supprimé une fois le vote effectué.
>
> Les adresses email des votants doivent également être supprimées de la base de données.
>
> Il est seulement utiliser pour permettre à un stagiaire de voter et assure qu'une personne ne **vote pas plusieurs fois**.

**Confirmation de vote**
Après avoir voté, les votants recevront une confirmation de leur vote par email.

Cette confirmation ne contiendra aucune information permettant d'identifier le votant, conformément aux exigences d'anonymat.

---

## Livrables

- Analyse fonctionnelle :
    - diagramme de cas d'utilisation ;
    - diagramme de séquence représentant le processus de vote ;
    - zoning/wireframe/haute fidélité des écrans à implémenter.
- Spécifications techniques :
    - MCD / MLD / MPD de la BDD relationnelle ;
    - liste des **endpoints** de la Web API à concevoir.
- Code source / scripts :
    - script de création de base de données ;
    - fichiers de configuration Docker permettant de déployer une base de données.

> [!NOTE]
> Dans un premier temps il ne vous est pas demandé de développer l'interface graphique. 

> [!CAUTION]
> Pour des raisons de sécurité il faudra veiller à ce que la base de données suivent les recommandations de sécurité.
>
> Dans cette perspective, il vous faudra mettre en place un ensemble d'éléments de base de données tels que :
> - un utilisateur au droits restreints ;
> - des triggers ou des procédures stockées.

---

# Conseils de développement

## Information sur le diagramme de séquence

Pour apprendre à concevoir des diagrammes de séquence vous pourrez consulter l'excellent cours de Rémy Manu disponible [ici](http://remy-manu.no-ip.biz/UML/Cours/coursUML5.pdf).

Il vous est conseiller de lire principalement les parties suivantes :
- 1-Rôle du diagramme de séquence
- 2-Représentation du diagramme de séquence
- 6-Exemple de diagramme de séquence : distributeur automatique de billets

Il vous sera possible de construire le diagramme de séquence demandé avec les connaissances acquises grâce au cours.

## Envoi d'email

Vous pourrez mettre en place [Spring Email](https://www.baeldung.com/spring-email) afin d'envoyer des emails en utilisant Spring Boot.

Pour se faire il vous faudra utiliser un [serveur SMTP](https://fr.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol). Le plus facile est d'utiliser un service existant, par exemple :
- [Google](https://medium.com/tuanhdotnet/tips-for-sending-mail-from-a-spring-boot-application-using-google-as-mail-server-fcf5ab042594)
- [MailSender](https://www.mailersend.com/n)
