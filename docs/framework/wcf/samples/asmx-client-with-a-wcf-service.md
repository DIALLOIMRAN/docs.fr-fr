---
title: "ASMX Client with a WCF Service | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 3ea381ee-ac7d-4d62-8c6c-12dc3650879f
caps.latest.revision: 21
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 21
---
# ASMX Client with a WCF Service
Cet exemple montre comment créer un service à l'aide de [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] puis accéder au service depuis un client non\-[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], tel qu'un client ASMX.  
  
> [!NOTE]
>  La procédure d'installation ainsi que les instructions de génération relatives à cet exemple figurent en fin de rubrique.  
  
 Cet exemple se compose d'un programme de console client \(.exe\) et d'une bibliothèque de service \(.dll\) hébergés par les services IIS \(Internet Information Services\).Le service implémente un contrat qui définit un modèle de communication demande\-réponse.Le contrat est défini par l'interface `ICalculator`, qui expose des opérations mathématiques \(`Add`, `Subtract`, `Multiply` et `Divide`\).Le client ASMX adresse des demandes synchrones à une opération mathématique et le service répond avec le résultat.  
  
 Le service implémente un contrat `ICalculator` tel que défini dans le code suivant.  
  
```  
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples"), XmlSerializerFormat]  
public interface ICalculator  
{  
    [OperationContract]  
    double Add(double n1, double n2);  
    [OperationContract]  
    double Subtract(double n1, double n2);  
    [OperationContract]  
    double Multiply(double n1, double n2);  
    [OperationContract]  
    double Divide(double n1, double n2);  
}  
  
```  
  
 Le <xref:System.Runtime.Serialization.DataContractSerializer> et le <xref:System.Xml.Serialization.XmlSerializer> mappent des types CLR à une représentation XML.Le <xref:System.Runtime.Serialization.DataContractSerializer> interprète des représentations XML différemment du XmlSerializer.Les générateurs proxy non\-[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], tels que Wsdl.exe, génèrent une interface plus utilisable avec le XmlSerializer.L'<xref:System.ServiceModel.XmlSerializerFormatAttribute> est appliqué à l'interface `ICalculator` pour garantir que le XmlSerializer est utilisé pour mapper des types CLR au XML.L'implémentation de service calcule puis retourne le résultat approprié.  
  
 Le service expose un point de terminaison unique de communication avec le service, lequel est défini à l'aide du fichier de configuration Web.config.Le point de terminaison se compose d'une adresse, d'une liaison et d'un contrat.Le service expose le point de terminaison à l'adresse de base fournie par l'hôte IIS \(Internet Information Services\).L'attribut `binding` a la valeur basicHttpBinding, qui fournit des communications HTTP à l'aide de SOAP 1.1, qui est conforme à WS\-BasicProfile 1.1, comme le montre l'exemple de configuration suivant.  
  
```  
<services>  
   <service   
       name="Microsoft.ServiceModel.Samples.CalculatorService"  
       behaviorConfiguration="CalculatorServiceBehavior">  
       <!-- This endpoint is exposed at the base address provided by the host: http://localhost/servicemodelsamples/service.svc.  -->  
      <endpoint address=""  
               binding="basicHttpBinding"   
               contract="Microsoft.ServiceModel.Samples.ICalculator" />  
   </service>  
</services>  
  
```  
  
 Le client ASMX communique avec le service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] à l'aide d'un proxy typé généré par l'utilitaire WSDL \(Web Services Description Language\) Wsdl.exe.Le proxy typé est contenu dans le fichier generatedClient.cs.L'utilitaire WSDL récupère les métadonnées pour le service spécifié et génère un proxy typé qui sera utilisé par un client pour communiquer.Par défaut, l'infrastructure n'expose pas de métadonnées.Pour exposer les métadonnées requises pour générer le proxy, vous devez ajouter un [\<serviceMetadata\>](../../../../docs/framework/configure-apps/file-schema/wcf/servicemetadata.md) et affecter à son attribut `httpGetEnabled` la valeur `True`, comme le montre la configuration suivante.  
  
```  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="CalculatorServiceBehavior">  
      <!-- Setting httpGetEnabled to True on the serviceMetadata  
           behavior exposes the service's wsdl at <base address>?wsdl :  
           http://localhost/servicemodelsamples/service.svc?wsdl -->  
      <serviceMetadata httpGetEnabled="True"/>  
      <serviceDebug includeExceptionDetailInFaults="False" />  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
 Exécutez la commande suivante à partir d'une invite de commandes dans le répertoire client pour générer le proxy typé.  
  
```  
wsdl /n:Microsoft.ServiceModel.Samples /o:generatedClient.cs /urlkey:CalculatorServiceAddress http://localhost/servicemodelsamples/service.svc?wsdl  
  
```  
  
 En utilisant le proxy typé généré, le client peut accéder à un point de terminaison de service donné en configurant l'adresse appropriée.Le client utilise un fichier de configuration \(App.config\) pour spécifier le point de terminaison avec lequel communiquer.  
  
```  
<appSettings>  
      <add key="CalculatorServiceAddress"   
      value="http://localhost/ServiceModelSamples/service.svc"/>  
</appSettings>  
  
```  
  
 L'implémentation cliente génère une instance du proxy typé pour commencer à communiquer avec le service.  
  
```  
// Create a client to the CalculatorService.  
using (CalculatorService client = new CalculatorService())  
{  
    // Call the Add service operation.  
    double value1 = 100.00D;  
    double value2 = 15.99D;  
    double result = client.Add(value1, value2);  
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Subtract service operation.  
    value1 = 145.00D;  
    value2 = 76.54D;  
    result = client.Subtract(value1, value2);  
    Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Multiply service operation.  
    value1 = 9.00D;  
    value2 = 81.25D;  
    result = client.Multiply(value1, value2);  
    Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Divide service operation.  
    value1 = 22.00D;  
    value2 = 7.00D;  
    result = client.Divide(value1, value2);  
    Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
  
}  
  
Console.WriteLine();  
Console.WriteLine("Press <ENTER> to terminate client.");  
Console.ReadLine();  
  
```  
  
 Lorsque vous exécutez l'exemple, les demandes et réponses d'opération s'affichent dans la fenêtre de console cliente.Appuyez sur ENTER dans la fenêtre du client pour arrêter le client.  
  
```  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
  
```  
  
### Pour configurer, générer et exécuter l'exemple  
  
1.  Assurez\-vous d'avoir effectué la procédure indiquée à la section [Procédure d'installation unique pour les exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2.  Pour générer l'édition C\# ou Visual Basic .NET de la solution, suivez les instructions indiquées dans [Génération des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3.  Pour exécuter l'exemple dans une configuration à un ou plusieurs ordinateurs, conformez\-vous aux instructions figurant dans la rubrique [Exécution des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
> [!NOTE]
>  [!INCLUDE[crabout](../../../../includes/crabout-md.md)] le passage et le retour de types de données complexes, consultez [Data Binding in a Windows Forms Client](../../../../docs/framework/wcf/samples/data-binding-in-a-windows-forms-client.md), [Data Binding in a Windows Presentation Foundation Client](../../../../docs/framework/wcf/samples/data-binding-in-a-wpf-client.md) et [Data Binding in an ASP.NET Client](../../../../docs/framework/wcf/samples/data-binding-in-an-aspnet-client.md)  
  
> [!IMPORTANT]
>  Les exemples peuvent déjà être installés sur votre ordinateur.Recherchez le répertoire \(par défaut\) suivant avant de continuer.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples`  
>   
>  Si ce répertoire n'existe pas, rendez\-vous sur la page \(éventuellement en anglais\) des [exemples Windows Communication Foundation \(WCF\) et Windows Workflow Foundation \(WF\) pour .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) pour télécharger tous les exemples [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] et [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Cet exemple se trouve dans le répertoire suivant.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples\WCF\Basic\Services\Interop\ASMX`  
  
## Voir aussi