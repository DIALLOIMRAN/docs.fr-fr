---
title: "Custom Binding Security | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: a6383dff-4308-46d2-bc6d-acd4e18b4b8d
caps.latest.revision: 30
author: "BrucePerlerMS"
ms.author: "bruceper"
manager: "mbaldwin"
caps.handback.revision: 30
---
# Custom Binding Security
L'exemple suivant illustre comment configurer la sécurité à l'aide d'une liaison personnalisée.Il indique également comment utiliser une liaison personnalisée afin d'activer la sécurité de niveau message à l'aide d'un transport sécurisé.Cette configuration est utile lorsqu'un transport sécurisé est requis pour la transmission des messages entre le client et le service et que ces messages doivent en même temps bénéficier d'une sécurité de niveau message.Cette configuration n'est pas prise en charge par les liaisons fournies par le système.  
  
 Cet exemple comporte un programme de console client \(EXE\) et un programme de console service \(EXE\).Le service implémente un contrat duplex.Le contrat est défini par l'interface `ICalculatorDuplex`, laquelle expose les opérations mathématiques suivantes : addition, soustraction, multiplication et division.L'interface `ICalculatorDuplex` permet au client d'effectuer des opérations mathématiques, en calculant le résultat de l'exécution sur une session.Indépendamment de l'interface précédente, le service peut retourner ses propres résultats sur l'interface `ICalculatorDuplexCallback`.Un contrat duplex requiert une session, un contexte devant être établi pour mettre en relation l'ensemble des messages échangés entre le client et le service.Une liaison personnalisée et sécurisée prenant en charge la communication duplex est définie.  
  
> [!NOTE]
>  La procédure d'installation ainsi que les instructions de génération relatives à cet exemple figurent en fin de rubrique.  
  
 La configuration du service définit une liaison personnalisée prenant en charge les éléments suivants :  
  
-   Communication TCP protégée à l'aide du protocole TLS\/SSL.  
  
-   Sécurité de message Windows.  
  
 La configuration de la liaison personnalisée active le transport sécurisé en activant simultanément la sécurité de niveau message.Lors de la configuration d'une liaison personnalisée, l'ordre de définition de ses éléments est déterminant, chacun de ces éléments représentant une couche de la pile des canaux \(consultez [Liaisons personnalisées](../../../../docs/framework/wcf/extending/custom-bindings.md)\).La liaison personnalisée est définie dans les fichiers de configuration du service et du client, tel qu'illustré dans l'exemple de configuration suivant.  
  
```  
<bindings>  
  <!-- Configure a custom binding. -->  
  <customBinding>  
    <binding name="Binding1">  
      <security authenticationMode="SecureConversation"  
                 requireSecurityContextCancellation="true">  
      </security>  
      <textMessageEncoding messageVersion="Soap12WSAddressing10" writeEncoding="utf-8"/>  
      <sslStreamSecurity requireClientCertificate="false"/>  
      <tcpTransport/>  
    </binding>  
  </customBinding>  
</bindings>  
```  
  
 La liaison personnalisée utilise un certificat de service afin d'authentifier le service au niveau du transport et de protéger les messages pendant leur transmission entre le client et le service.Ces deux tâches sont effectuées par l'élément de liaison `sslStreamSecurity`.Le certificat du service est configuré à l'aide d'un comportement de service, tel qu'illustré dans l'exemple de configuration suivant.  
  
```  
<behaviors>  
      <serviceBehaviors>  
        <behavior name="CalculatorServiceBehavior">  
          <serviceMetadata />  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <serviceCredentials>  
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName"/>  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  
```  
  
 En outre, la liaison personnalisée utilise la sécurité de niveau message avec le type d'informations d'identification Windows, c'est\-à\-dire le type par défaut.Cette tâche est effectuée par l'élément de liaison `security`.Le client et le service sont tous deux authentifiés à l'aide de la sécurité au niveau du message si le mécanisme d'authentification Kerberos est disponible.Cela se produit à condition toutefois que l'exemple soit exécuté dans l'environnement Active Directory.Si le mécanisme d'authentification Kerberos n'est pas disponible, l'authentification NTLM est utilisée.NTLM authentifie le client au service mais n'authentifie pas le service au client.L'élément de liaison `security` est configuré pour utiliser un type `authenticationType``SecureConversation`, ce qui entraîne la création d'une session de sécurité à la fois au niveau du client et du service.Ceci est nécessaire pour permettre au contrat duplex du service de fonctionner.  
  
 Lorsque vous exécutez l'exemple, les demandes et réponses d'opération s'affichent dans la fenêtre de console cliente.Appuyez sur ENTER dans la fenêtre du client pour l'arrêter.  
  
```  
Press <ENTER> to terminate client.  
Result(100)  
Result(50)  
Result(882.5)  
Result(441.25)  
Equation(0 + 100 - 50 * 17.65 / 2 = 441.25)  
  
```  
  
 Lorsque vous exécutez l'exemple, vous pouvez consulter les messages retournés au client sur l'interface de rappel envoyée depuis le service.Tous les résultats intermédiaires sont affichés, suivis de l'équation en entier une fois toutes les opérations terminées.Appuyez sur ENTER pour arrêter le client.  
  
 Le fichier Setup.bat inclus vous permet de configurer le client et le serveur à l'aide du certificat de service requis pour exécuter les applications hébergées nécessitant une sécurité basée sur les certificats.Ce fichier de commandes doit être modifié pour fonctionner sur plusieurs ordinateurs ou sans hébergement.  
  
 Les informations suivantes fournissent une vue d'ensemble des différentes sections des fichiers de commandes applicables à cet exemple afin que vous puissiez les modifier en fonction de la configuration requise :  
  
-   Création du certificat de serveur  
  
     Les lignes suivantes du fichier Setup.bat créent le certificat de serveur à utiliser.La variable `%SERVER_NAME%` représente le nom du serveur.Modifiez cette variable pour spécifier votre propre nom de serveur.Ce fichier de commandes affecte par défaut la valeur localhost au nom du serveur.  
  
     Le certificat est stocké dans le magasin CurrentUser pour les services hébergés par le Web.  
  
    ```  
    echo ************  
    echo Server cert setup starting  
    echo %SERVER_NAME%  
    echo ************  
    echo making server cert  
    echo ************  
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe  
  
    ```  
  
-   Installation du certificat de serveur dans le magasin de certificats approuvés du client.  
  
     Les lignes suivantes du fichier Setup.bat copient le certificat de serveur dans le magasin de personnes de confiance du client.Cette étape est obligatoire parce que les certificats générés par Makecert.exe ne sont pas implicitement approuvés par le système client.Si vous disposez déjà d'un certificat associé à un certificat racine approuvé du client, par exemple d'un certificat émis par Microsoft, il n'est pas nécessaire d'ajouter le certificat du serveur au magasin de certificats du client.  
  
    ```  
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
    > [!NOTE]
    >  Le fichier de commandes Setup.bat est conçu pour s'exécuter à partir d'une invite de commandes de Visual Studio 2010.La variable d'environnement du Kit de développement MS SDK doit pointer vers le répertoire d'installation du Kit de développement SDK.Cette variable est définie automatiquement dans une invite de commandes de Visual Studio 2010.  
  
### Pour configurer, générer et exécuter l'exemple  
  
1.  Assurez\-vous d'avoir effectué la procédure indiquée à la section [Procédure d'installation unique pour les exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2.  Pour générer l'édition C\# ou Visual Basic .NET de la solution, conformez\-vous aux instructions figurant dans [Génération des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3.  Pour exécuter l'exemple dans une configuration à un ou plusieurs ordinateurs, conformez\-vous aux instructions figurant dans [Exécution des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
### Pour exécuter l'exemple sur le même ordinateur  
  
1.  Ouvrez une fenêtre d'invite de commandes de Visual Studio avec des privilèges d'administrateur et exécutez Setup.bat à partir du dossier d'installation de l'exemple.Tous les certificats requis pour l'exécution de l'exemple sont ainsi installés.  
  
    > [!NOTE]
    >  Le fichier de commandes Setup.bat est conçu pour être exécuté à partir d'une invite de commandes de [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)].La variable d'environnement PATH définie dans l'invite de commandes [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] pointe vers le répertoire qui contient les exécutables requis par le script Setup.bat.  
  
2.  Lancez Service.exe à partir de \\service\\bin.  
  
3.  Lancez Client.exe à partir de \\client\\bin.L'activité du client s'affiche sur son application de console.  
  
4.  Si le client et le service ne parviennent pas à communiquer, consultez la rubrique [Troubleshooting Tips](http://msdn.microsoft.com/fr-fr/8787c877-5e96-42da-8214-fa737a38f10b).  
  
### Pour exécuter l'exemple sur plusieurs ordinateurs  
  
1.  Sur l'ordinateur de service :  
  
    1.  Créez un répertoire virtuel nommé servicemodelsamples sur l'ordinateur de service.  
  
    2.  Copiez les fichiers programme du service de \\inetpub\\wwwroot\\servicemodelsamples dans le répertoire virtuel sur l'ordinateur de service.Assurez\-vous de copier les fichiers dans le sous\-répertoire \\bin.  
  
    3.  Copiez les fichiers Setup.bat et Cleanup.bat sur l'ordinateur de service.  
  
    4.  Ouvrez une fenêtre d'invite de commandes de Visual Studio avec des privilèges d'administrateur et exécutez `setup.bat service`.L'exécution de cette commande crée un certificat de service dont le nom du sujet correspond au nom de l'ordinateur sur lequel le fichier de commandes a été exécuté.  
  
        > [!NOTE]
        >  Le fichier de commandes Setup.bat est conçu pour s'exécuter à partir d'une invite de commandes de Visual Studio 2010.La variable d'environnement PATH doit pointer vers le répertoire d'installation du Kit de développement SDK.Cette variable est définie automatiquement dans une invite de commandes de Visual Studio 2010.  
  
    5.  Modifiez le [\<serviceCertificate\>](../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) du fichier Service.exe.config en fonction du nom du sujet du certificat généré précédemment.  
  
    6.  Exécutez Service.exe à partir d'une invite de commandes.  
  
2.  Sur l'ordinateur client :  
  
    1.  Copiez les fichiers programme du client à partir du dossier \\client\\bin\\ sur l'ordinateur client.Copiez également le fichier Cleanup.bat.  
  
    2.  Exécutez Cleanup.bat pour supprimer tous les anciens certificats d'exemples précédents.  
  
    3.  Exportez le certificat du service en ouvrant une invite de commandes de Visual Studio avec des privilèges d'administrateur et en exécutant la commande suivante sur l'ordinateur de service \(remplacez `%SERVER_NAME%` par le nom complet de l'ordinateur où le service s'exécute\) :  
  
        ```  
        certmgr -put -r LocalMachine -s My -c -n %SERVER_NAME% %SERVER_NAME%.cer  
        ```  
  
    4.  Copiez le fichier % SERVER\_NAME%.cer sur l'ordinateur client \(remplacez %SERVER\_NAME% par le nom complet de l'ordinateur où le service s'exécute\).  
  
    5.  Importez le certificat du service en ouvrant une invite de commandes de Visual Studio avec des privilèges d'administrateur et en exécutant la commande suivante sur l'ordinateur client \(remplacez %SERVER\_NAME% par le nom complet de l'ordinateur où le service s'exécute\) :  
  
        ```  
        certmgr.exe -add -c %SERVER_NAME%.cer -s -r CurrentUser TrustedPeople  
        ```  
  
         Les étapes c, d et e ne sont pas requises lorsque le certificat est publié par un émetteur approuvé.  
  
    6.  Modifiez le fichier App.config du client comme suit :  
  
        ```  
        <client>  
            <endpoint name="default"  
                address="net.tcp://ReplaceThisWithServiceMachineName:8000/ServiceModelSamples/Service"   
                binding="customBinding"   
                bindingConfiguration="Binding1"   
                contract="Microsoft.ServiceModel.Samples.ICalculatorDuplex"  
        behaviorConfiguration="CalculatorClientBehavior" />  
        </client>  
  
        ```  
  
    7.  Si le service s'exécute dans un environnement de domaine sous un compte autre que NetworkService ou LocalSystem, vous devrez peut\-être modifier l'identité de son point de terminaison dans le fichier App.config du client afin de lui affecter une identité UPN ou SPN adaptée au compte utilisé pour l'exécuter.Pour plus d'informations sur l'identité des points de terminaison, consultez la rubrique [Identité du service et authentification](../../../../docs/framework/wcf/feature-details/service-identity-and-authentication.md).  
  
    8.  Exécutez Client.exe à partir d'une invite de commandes.  
  
### Pour procéder au nettoyage après exécution de l'exemple  
  
-   Exécutez Cleanup.bat dans le dossier exemples une fois que vous avez terminé d'exécuter l'exemple.  
  
## Voir aussi