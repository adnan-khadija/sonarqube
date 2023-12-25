# Installation de SonarQube dans Docker

Ce guide vous aidera à déployer SonarQube  et l'analyse de votre code  à l'aide de Docker, une plateforme de conteneurisation populaire. Suivez ces étapes simples pour démarrer rapidement.

## Prérequis
Assurez-vous d'avoir installé Docker sur votre machine. Vous pouvez le télécharger depuis [le site officiel de Docker](https://www.docker.com/products/docker-desktop/).

1. **Téléchargement de l'image SonarQube**
   Exécutez la commande suivante dans votre terminal pour télécharger l'image officielle de SonarQube depuis Docker Hub :
   
*docker pull sonarqube*

![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/e7a96486-eed6-4e32-ba72-2388f0073303)

2.**Démarrage de SonarQube**

Utilisez la commande suivante pour démarrer un conteneur SonarQube :

*docker run -d --name sonarqube -p 9000:9000 sonarqube*

![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/0bd102f9-bc4a-47e7-b242-44ad71d2ac3d)

Cela lancera SonarQube en arrière-plan, exposant les ports 9000 (interface web).

3.**Accès à l'interface web**

Accédez à http://localhost:9000. 

![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/726c9670-cfa2-440c-9b61-288be346f664)

Les informations de connexion par défaut sont les suivantes :

Nom d'utilisateur : admin

Mot de passe : admin

![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/eaf8f235-a2aa-4141-acea-b1bc2c849bb1)

Vous serez invité à changer votre mot de passe après la première connexion.

![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/2eb3fb09-53af-455f-951c-ed040fce812c)

**Interface de sonarqube:**

![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/29143cdb-249a-45bd-8b59-a2df43f54717)

4.**Creation d'un nouveau projet:**

![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/46a3cec7-8b87-4ebb-ac59-f3420c8042fa)



Par la suit il faut choisir de quelle façon l’on veut analyser notre projet dans notre cas nous allons le faire localement.



![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/66a8918a-7826-4a68-9db9-0c739590f2e9)



**Donner un nom au projet:**


![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/ca96d861-71e1-4a85-96ec-d8c7b7249c37)



**Generation de Token :**



![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/9333270b-173e-40c5-8498-a0d8c67f52c1)


![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/d0e1028e-e301-4741-961c-9181d59bc201)


![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/0561b85d-7a34-4eb3-859d-2351a659f035)


![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/5404904e-9c3c-40cd-a9fc-517c0214f12e)


Pour pouvoir exécuter la commande sans problème dans l’invite de commande essayer de mettre la commande sur une seule ligne.


![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/cad3f5fa-1f6f-45b6-b1b1-e0e9699479dd)


![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/4cc29b37-ebeb-4d3a-93ee-844a5e03841f)


![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/660d62b9-fe58-4615-b9ab-a7852a8fa032)


 **Intégration de JACOCO**
 

Actuellement SonarQube ne donne aucune information sur la couverture des tests et c’est parce qu’il manque un outil ou une dépendance dans 
le projet qui fournit un rapport sur les tests.

![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/94496cf4-f79e-4650-baea-654d1cd423b7)


**Ajout de dependency JACOCO**

*<dependency>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.7</version>
    <scope>test</scope>
</dependency>*



![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/1c27a5dd-0864-48fd-a376-cf279bef766f)


Modifier les properties dans le fichier pom.xml comme suit :


*<properties>
    <java.version>17</java.version>
    <!-- JaCoCo Properties -->
    <jacoco.version>0.8.7</jacoco.version>
    <sonar.java.coveragePlugin>jacoco</sonar.java.coveragePlugin>
    <sonar.dynamicAnalysis>reuseReports</sonar.dynamicAnalysis>
    <sonar.jacoco.reportPath>${project.basedir}/../target/jacoco.exec</sonar.jacoco.reportPath>
    <sonar.language>java</sonar.language>
</properties>*



![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/a871ae23-ec30-4396-bb01-702af16ea72c)


**Ajouter le plugin JACOCO:**

Le plug-in JaCoCo Maven permet d’accéder à l’agent d’exécution JaCoCo,
qui enregistre les données de couverture d’exécution et crée un rapport de couverture de code.


<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>${jacoco.version}</version>
    <executions>
        <execution>
            <id>jacoco-initialize</id>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>jacoco-site</id>
            <phase>package</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>




![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/6f744a4d-9060-427f-a1ae-9a8ebf892b48)



![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/d16fe012-8441-4d05-9ddb-fe5127e2470e)


![image](https://github.com/adnan-khadija/sonarqube/assets/147508009/32dbcece-17ea-4a3e-b7f3-69c2ef5b77ec)










