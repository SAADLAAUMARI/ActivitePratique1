# ActivitePratique1
# Principe de l’Inversion de Contrôle et Injection des dépendances

Dans cette étape, nous allons mettre en place l'inversion de contrôle (IoC) et l'injection de dépendances (DI) en séparant les aspects métier des aspects techniques du code. Cela implique d'effectuer l'injection de dépendances en favorisant un couplage faible et en reliant les classes aux interfaces plutôt qu'aux implémentations directes.

## Architecture d'application 
![image](https://user-images.githubusercontent.com/48890714/230694406-2793f4a6-fe1d-458f-b922-4b6b8b41fadb.png)

## Architerctures d'application

Il existe deux types d'architectures d'applications :

- **Architecture Monolithique** : Cette approche consiste à développer une seule application avec une seule technologie, regroupant toutes les fonctionnalités dans un seul bloc. C'est un modèle traditionnel de développement de logiciels, conçu comme une entité unifiée autonome et déployée sur un serveur d'application. Les avantages d'une telle architecture incluent un déploiement facile, un développement simplifié avec une seule base de code, de bonnes performances, des tests simplifiés et un débogage facile. Cependant, les inconvénients comprennent la centralisation de tous les besoins fonctionnels, la nécessité de tester les régressions à chaque modification, la lenteur des mises en production, des performances potentiellement limitées, une évolutivité difficile, une flexibilité réduite et des obstacles à l'adoption de nouvelles technologies.

- **Microservices** : Cette approche repose sur une architecture où les fonctionnalités sont décomposées en une série de services déployables indépendamment. Chaque service possède sa propre logique métier et sa propre base de données, avec un objectif spécifique. Les avantages des microservices incluent l'agilité, une évolutivité flexible, le déploiement continu, une administration et des tests facilités, le déploiement indépendant, une flexibilité technologique, une fiabilité accrue et une adaptation aux méthodes de développement telles que le TDD (Test Driven Development) et les méthodes agiles. Cependant, les inconvénients comprennent une complexité accrue, des coûts d'infrastructure plus élevés, des frais organisationnels supplémentaires, des défis de débogage, un manque de standardisation et une responsabilité moins claire.
    
    
## Exigences d'un projet informatique

Chaque projet informatique présente deux types d'exigences :

- **Exigences Fonctionnelles** : Ce sont les besoins fonctionnels, les attentes des utilisateurs finaux ou les besoins métiers de l'entreprise.

- **Exigences Techniques** : Ce sont les besoins techniques qui peuvent être résumés comme suit :

    - **Performance** : Cette catégorie concerne le temps de réponse de l'application et le problème de montée en charge, qui peut être résolu par la scalabilité. La scalabilité peut prendre deux formes :
    
    - **Scalabilité Horizontale** : Elle consiste à équilibrer la charge et à tolérer les pannes en démarrant plusieurs instances de l'application sur différentes machines pour répondre au problème de montée en charge. Un serveur de répartition de charge (Load Balancer) est utilisé pour équilibrer la charge des requêtes reçues des clients.
        
    - **Scalabilité Verticale** : Il s'agit de la technique qui consiste à augmenter les ressources de la machine où l'application est exécutée. Par ressources, on entend la RAM, le disque dur, le processeur/CPU, etc. L'application crée un thread pour chaque requête d'un client.
        
    - **Maintenance** : L'application doit être évolutive dans le temps, mais en respectant la règle selon laquelle "une application doit être fermée à la modification et ouverte à l’extension", car les modifications peuvent entraîner des problèmes de régression.
    
    - **Sécurité** : Les failles de sécurité constituent l'un des problèmes critiques en développement informatique. Il est essentiel de prêter une attention particulière à la persistance des données et à la gestion des transactions.
    
    - **Versions** : Web / Mobile / Desktop.

##Inversion de contrôle
L'inversion de contrôle (IoC) est un principe fondamental qui consiste à séparer les aspects métier des aspects techniques. Les développeurs se concentrent uniquement sur la partie logique du code tandis que le framework prend en charge les aspects techniques. Ce concept s'appuie sur l'architecture de la programmation orientée aspect (AOP).

La programmation orientée aspect (AOP) vient en complément de la programmation orientée objet (POO) en proposant une approche différente de la structuration des programmes. Alors que la classe est l'unité clé de modularité en POO, en AOP, c'est l'aspect qui devient cette unité. Les aspects permettent de modulariser des préoccupations transversales telles que la gestion des transactions, qui peuvent s'appliquer à plusieurs types et objets.

##Injection des dépendances
L'injection des dépendances se divise en deux types :

```Couplage fort``` : Les classes dépendent directement d'autres classes, ce qui rend les modifications difficiles car chaque changement nécessite une modification du code source.
```Couplage faible``` : Les classes dépendent des interfaces plutôt que des implémentations concrètes, facilitant ainsi l'adaptation à des modifications ultérieures.

##Spring IoC

Il est préférable de gérer la liaison des composants d'un programme à l'aide d'un framework, ce qui permet de modifier les composants ou leur comportement plus facilement. Spring IoC consiste à déclarer dans un fichier XML quelles sont les différentes classes à instancier et à établir les dépendances entre ces instances. Pour ajouter une nouvelle implémentation dans l'application, il suffit de la déclarer dans ce fichier XML.

Il existe deux méthodes pour configurer Spring IoC :

```XML``` : Spring lit le fichier de configuration XML et gère les injections de dépendances en conséquence.
```Annotations``` : Les annotations sont ajoutées aux classes pour indiquer à Spring de les instancier au démarrage de l'application, ainsi que pour spécifier les dépendances entre les différentes instances de classes déjà déclarées.

## Architecture de probléme
## La couche DAO 
Cette couche interroge à la base de données.
- Step 1 : Création d'interface contient la métode qu'on aura besoin dans la couche DAO
``` java
public interface IDao {
     double getdate();
}
```
- Step 2 : Création de classe d'implémentation de interface DAO, dans la quelle on va redéfini la méthode 'getData'.

``` java 
public class DaoImpl implements IDao{
    @Override
    public double getData() {
        System.out.println("Version Base de données");
        double temp=Math.random()*40;
        return temp;
    }
}
```
Dans le cas de modifications, on ajoute un autre classe qui va implémenter l'interface DAO sans besoin de modifier le code source. à ce stade la, on applique la régle ```l’application doit être fermée à la modification et ouverte à l’extension```.
## La couche metier
Cette couche continet les besoins fonctionnels de l'application.
- Step 1 : Création d'interface contient la métode qu'on aura besoin dans la couche Metier, cette méthode va faire le traitement de besoin fonctionnel du utilisateur.
``` java
public interface Imetier {
    double calcul();
}
```
- Step 2 : Création de classe d'implémentation de interface Metier, dans la quelle on va redéfini la méthode 'calcul', avec la déclaration d'un objet de type d'interface 'DAO' en utilisant un couplage faible.

``` java 
public class MetierImpl implements Imetier {
    private IDao dao=null; //coublage faible
    @Override
    public double calcul() {
        double tmp=dao.getdate();
        double resultat =tmp*420 / Math.tan(tmp*Math.PI);
        return resultat;
    }
   /*
    Inejecter dans la variable dao  un object une classe qui implémente l'interface IDao
    */
    public void setDao(IDao dao) {
        this.dao = dao;
    }
}
```
## La couche présentation 
Cette couche contient la partie présentation ( US user interface).
La communication entre la classe Metier et DAO, besoin de faire l'injection des dépendance. avec deux méthode : 
### Instanciation Statique
Dans la couche présentation, on crée une classe ```Presenatation.java``` ou on appliquera une injection statique des dépendances. il joue le rôle de Factory Class, elle génère les dépendances.

``` java
public class Presentation {
    public static void main(String[] args) throws  Exception{
        /*
         Injection des dépendances par instanciation statique => new  (couplage forte)
         */
        IDao dao=new DaoImpl();
        MetierImpl metier = new MetierImpl();
        metier.setDao(dao);
        System.out.println("Résultat :"+metier.calcul());
        
    }
}
```
### Instatnciation Dynamique
Si en veux réspecter la régle ```l’application doit être fermée à la modification et ouverte à l’extension```, avec la métode statique dans le cas d'une nouvelle extension nous obligons à modifer le code. par contre avec la méthode dynamiqye ne faisant seulement l'injection des dépendances sans changement de code. un simple modification au niveau de fichier ```config.txt```

``` java
public class Pres2 {
    public static void main(String[] args) throws FileNotFoundException, ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchMethodException, InvocationTargetException {
        Scanner scanner= new Scanner(new File("config.txt"));
        String daoClassName=scanner.nextLine();
        Class cDao=Class.forName(daoClassName);
        IDao dao=(IDao) cDao.newInstance();
        System.out.println(dao.getData());

        String metierClassName=scanner.nextLine();
        Class cMetier=Class.forName(metierClassName);
        IMetier metier=(IMetier) cMetier.newInstance();

        Method method= cMetier.getMethod("setDao", IDao.class);
        method.invoke(metier,dao);

        System.out.println("Résultat=>"+metier.calcul());
    }
}
```

## Le dossier ext 
Ce dossier contient la partie d'extension de l'application.
à la maintenace on a respecté la régle d'or ```l’application doit être fermée à la modification et ouverte à l’extension``` et on a ajouté un autre dossier pour déclarer des nouvelles implémentations de l'interface IDao.
``` java
public class DaoImplVWS implements IDao {
    @Override
    public double getData() {
        System.out.println("Version web");
        return 90;
    }
}
```
une autre implémentation de l'interface IDao dans la partie d'extension :
``` java
public class MetierImpl implements IMetier {
    private IDao dao;
    @Override
    public double calcul() {
        double tmp=dao.getData();
        double res=tmp*540/Math.cos(tmp*Math.PI);
        return res;
    }

    public void setDao(IDao dao) {
        this.dao = dao;
    }
}
```
## Fichier config.txt
Ce fichier contient la configuration pour l'injection des dépendances dynamiquement.
il contient les noms des différentes implémentatios déja déclarés qu'on va l'utiliser pour l'injection des dépandances d'une façon dynamique sans passer par l'instanciation des objets en utilisant mot cle ```new```.

``` txt 
dao.DaoImpl
metier.MetierImpl
```
