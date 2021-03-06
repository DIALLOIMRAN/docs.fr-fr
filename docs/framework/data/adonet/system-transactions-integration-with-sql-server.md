---
title: "Int&#233;gration de System.Transactions &#224; SQL Server | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: b555544e-7abb-4814-859b-ab9cdd7d8716
caps.latest.revision: 6
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 6
---
# Int&#233;gration de System.Transactions &#224; SQL Server
La version 2.0 du [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] a introduit une nouvelle infrastructure de transactions à laquelle il est possible d'accéder via l'espace de noms <xref:System.Transactions>. Cette infrastructure expose des transactions de manière totalement intégrée au [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)], y compris à [!INCLUDE[vstecado](../../../../includes/vstecado-md.md)].  
  
 Outre les améliorations en termes de programmabilité, <xref:System.Transactions> et [!INCLUDE[vstecado](../../../../includes/vstecado-md.md)] peuvent collaborer pour coordonner les optimisations lorsque vous utilisez des transactions. Une transaction susceptible d'être promue est une transaction légère \(locale\) qui peut être promue automatiquement en une transaction entièrement distribuée en fonction des besoins.  
  
 À partir de la version 2.0 d'[!INCLUDE[vstecado](../../../../includes/vstecado-md.md)], <xref:System.Data.SqlClient> prend en charge des transactions pouvant être promues lorsque vous utilisez [!INCLUDE[ssNoVersion](../../../../includes/ssnoversion-md.md)]. Une transaction pouvant être promue n'invoque pas la charge supplémentaire d'une transaction distribuée à moins qu'elle ne soit requise. Les transactions pouvant être promues sont automatiques et ne requièrent aucune intervention du développeur.  
  
 Les transactions pouvant être promues sont disponibles uniquement lorsque vous utilisez le fournisseur de données [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] pour SQL Server \(`SqlClient`\) avec [!INCLUDE[ssNoVersion](../../../../includes/ssnoversion-md.md)].  
  
## Création de transactions pouvant être promues  
 Le fournisseur [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] pour SQL Server assure la prise en charge des transactions pouvant être promues, qui sont gérées à l'aide des classes dans l'espace de noms [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] de <xref:System.Transactions>. Les transactions pouvant être promues optimisent les transactions distribuées en différant la création d'une transaction distribuée jusqu'à ce qu'elle soit nécessaire. Si un seul gestionnaire de ressources est requis, aucune transaction distribuée n'a lieu.  
  
> [!NOTE]
>  Dans un scénario de niveau de confiance partiel, l'objet <xref:System.Transactions.DistributedTransactionPermission> est requis lorsqu'une transaction est promue en transaction distribuée.  
  
## Scénarios de transaction pouvant être promue  
 Les transactions distribuées consomment généralement une partie importante des ressources système, car elles sont gérées par Microsoft Distributed Transaction Coordinator \(MS DTC\), qui intègre tous les gestionnaires de ressources auxquels la transaction accède. Une transaction pouvant être promue est une forme particulière de transaction <xref:System.Transactions> qui délègue efficacement le travail à une transaction [!INCLUDE[ssNoVersion](../../../../includes/ssnoversion-md.md)] simple.<xref:System.Transactions>, <xref:System.Data.SqlClient> et [!INCLUDE[ssNoVersion](../../../../includes/ssnoversion-md.md)] coordonnent le travail de gestion de la transaction, en promouvant celle\-ci au niveau d'une transaction entièrement distribuée, si nécessaire.  
  
 L'avantage de l'utilisation de transactions pouvant être promues réside dans le fait que, quand une connexion est ouverte à l'aide d'une transaction <xref:System.Transactions.TransactionScope> active alors qu'aucune autre connexion n'est ouverte, la transaction est validée comme transaction légère, au lieu de générer la charge supplémentaire d'une transaction entièrement distribuée.  
  
### Mots clés des chaînes de connexion  
 La propriété <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A> prend en charge un mot clé, `Enlist`, qui indique si <xref:System.Data.SqlClient> détecte des contextes transactionnels et inscrit automatiquement la connexion dans une transaction distribuée. Si `Enlist=true`, la connexion est automatiquement inscrite dans le contexte de transaction actuel du thread qui s'ouvre. Si `Enlist=false`, la connexion `SqlClient` n'interagit pas avec une transaction distribuée. La valeur par défaut de `Enlist` est true. Si `Enlist` n'est pas spécifié dans la chaîne de connexion, la connexion est automatiquement inscrite dans une transaction distribuée détectée lors de l'ouverture de la connexion.  
  
 Les mots clés `Transaction Binding` d'une chaîne de connexion <xref:System.Data.SqlClient.SqlConnection> contrôlent l'association de la connexion à une transaction `System.Transactions` inscrite. Il est également possible d'utiliser la propriété <xref:System.Data.SqlClient.SqlConnectionStringBuilder.TransactionBinding%2A> d'un objet <xref:System.Data.SqlClient.SqlConnectionStringBuilder>.  
  
 Le tableau suivant décrit les valeurs possibles.  
  
|Mot clé|Description|  
|-------------|-----------------|  
|Implicit Unbind|Valeur par défaut. La connexion se détache de la transaction une fois cette dernière terminée et revient au mode de validation automatique.|  
|Explicit Unbind|La connexion reste attachée à la transaction jusqu'à la fermeture de cette dernière. La connexion échoue si la transaction associée n'est pas active ou ne correspond pas à la propriété <xref:System.Transactions.Transaction.Current%2A>.|  
  
## Utilisation de TransactionScope  
 La classe <xref:System.Transactions.TransactionScope> rend un bloc de code transactionnel en inscrivant implicitement des connexions dans une transaction distribuée. Vous devez appeler la méthode <xref:System.Transactions.TransactionScope.Complete%2A> à la fin du bloc <xref:System.Transactions.TransactionScope> avant de le quitter. Le fait de quitter le bloc invoque la méthode <xref:System.Transactions.TransactionScope.Dispose%2A>. Si une exception a été levée qui a pour effet que le code sorte de la portée, la transaction est considérée comme abandonnée.  
  
 Il est recommandé d'utiliser un bloc `using` pour s'assurer que la méthode <xref:System.Transactions.TransactionScope.Dispose%2A> est appelée sur l'objet <xref:System.Transactions.TransactionScope> en cas de sortie du bloc using. La non\-validation ou la non\-restauration des transactions en attente peut considérablement nuire aux performances, le délai d'attente par défaut de l'objet <xref:System.Transactions.TransactionScope> étant d'une minute. Si vous n'utilisez pas d'instruction `using`, vous devez effectuer tout le travail dans un bloc `Try` et appeler explicitement la méthode <xref:System.Transactions.TransactionScope.Dispose%2A> dans le bloc `Finally`.  
  
 Si une exception est levée dans l'objet <xref:System.Transactions.TransactionScope>, la transaction est marquée comme incohérente et est abandonnée. Elle sera annulée lors de la suppression du <xref:System.Transactions.TransactionScope>. Si aucune exception n'est levée, les transactions participantes sont validées.  
  
> [!NOTE]
>  La classe `TransactionScope` crée une transaction dont la propriété <xref:System.Transactions.Transaction.IsolationLevel%2A> a par défaut la valeur `Serializable`. En fonction de votre application, vous devez envisager d'abaisser le niveau d'isolation afin d'éviter une contention élevée dans votre application.  
  
> [!NOTE]
>  Il est recommandé de n'effectuer des mises à jour, des insertions et des suppressions que dans des transactions distribuées car ces opérations consomment des ressources de base de données importantes. Les instructions select risquent de verrouiller inutilement les ressources de base de données ; dans certains cas, vous pouvez être amené à utiliser des transactions pour effectuer des sélections. Tout travail autre qu'un travail de base de données doit être réalisé en dehors de la transaction, à moins qu'il n'implique d'autres gestionnaires de ressources. Bien qu'une exception dans la transaction empêche la validation de celle\-ci, la classe <xref:System.Transactions.TransactionScope> n'a aucune disposition pour annuler les modifications que votre code a apportées en dehors de la transaction proprement dite. Pour intervenir lors de l'annulation de la transaction, vous devez écrire votre propre implémentation de l'interface <xref:System.Transactions.IEnlistmentNotification> et vous inscrire explicitement dans la transaction.  
  
## Exemple  
 L'utilisation de l'espace de noms <xref:System.Transactions> exige que vous disposiez d'une référence à System.Transactions.dll.  
  
 La fonction suivante montre comment créer une transaction pouvant être promue en relation avec deux instances différentes de SQL Server, représentées par deux objets <xref:System.Data.SqlClient.SqlConnection> différents, enveloppés dans un bloc <xref:System.Transactions.TransactionScope>. Le code crée le bloc <xref:System.Transactions.TransactionScope> avec une instruction `using` et ouvre la première connexion qui l'inscrit automatiquement dans le <xref:System.Transactions.TransactionScope>. La transaction est initialement inscrite comme transaction légère, pas comme transaction entièrement distribuée. La seconde connexion est inscrite dans l'objet <xref:System.Transactions.TransactionScope> uniquement si la commande dans la première connexion ne lève pas une exception. Une fois la seconde connexion ouverte, la transaction est automatiquement promue en transaction entièrement distribuée. La méthode <xref:System.Transactions.TransactionScope.Complete%2A> est appelée, ce qui valide la transaction uniquement si aucune exception n'a été levée. Si une exception a été levée dans le bloc <xref:System.Transactions.TransactionScope>, la méthode `Complete` n'est pas appelée et la transaction distribuée est annulée lors de la suppression de l'objet <xref:System.Transactions.TransactionScope> à la fin de son bloc `using`.  
  
```csharp  
// This function takes arguments for the 2 connection strings and commands in order  
// to create a transaction involving two SQL Servers. It returns a value > 0 if the  
// transaction committed, 0 if the transaction rolled back. To test this code, you can   
// connect to two different databases on the same server by altering the connection string,  
// or to another RDBMS such as Oracle by altering the code in the connection2 code block.  
static public int CreateTransactionScope(  
    string connectString1, string connectString2,  
    string commandText1, string commandText2)  
{  
    // Initialize the return value to zero and create a StringWriter to display results.  
    int returnValue = 0;  
    System.IO.StringWriter writer = new System.IO.StringWriter();  
  
    // Create the TransactionScope in which to execute the commands, guaranteeing  
    // that both commands will commit or roll back as a single unit of work.  
    using (TransactionScope scope = new TransactionScope())  
    {  
        using (SqlConnection connection1 = new SqlConnection(connectString1))  
        {  
            try  
            {  
                // Opening the connection automatically enlists it in the   
                // TransactionScope as a lightweight transaction.  
                connection1.Open();  
  
                // Create the SqlCommand object and execute the first command.  
                SqlCommand command1 = new SqlCommand(commandText1, connection1);  
                returnValue = command1.ExecuteNonQuery();  
                writer.WriteLine("Rows to be affected by command1: {0}", returnValue);  
  
                // if you get here, this means that command1 succeeded. By nesting  
                // the using block for connection2 inside that of connection1, you  
                // conserve server and network resources by opening connection2   
                // only when there is a chance that the transaction can commit.     
                using (SqlConnection connection2 = new SqlConnection(connectString2))  
                    try  
                    {  
                        // The transaction is promoted to a full distributed  
                        // transaction when connection2 is opened.  
                        connection2.Open();  
  
                        // Execute the second command in the second database.  
                        returnValue = 0;  
                        SqlCommand command2 = new SqlCommand(commandText2, connection2);  
                        returnValue = command2.ExecuteNonQuery();  
                        writer.WriteLine("Rows to be affected by command2: {0}", returnValue);  
                    }  
                    catch (Exception ex)  
                    {  
                        // Display information that command2 failed.  
                        writer.WriteLine("returnValue for command2: {0}", returnValue);  
                        writer.WriteLine("Exception Message2: {0}", ex.Message);  
                    }  
            }  
            catch (Exception ex)  
            {  
                // Display information that command1 failed.  
                writer.WriteLine("returnValue for command1: {0}", returnValue);  
                writer.WriteLine("Exception Message1: {0}", ex.Message);  
            }  
        }  
  
        // If an exception has been thrown, Complete will not   
        // be called and the transaction is rolled back.  
        scope.Complete();  
    }  
  
    // The returnValue is greater than 0 if the transaction committed.  
    if (returnValue > 0)  
    {  
        writer.WriteLine("Transaction was committed.");  
    }  
    else  
    {  
        // You could write additional business logic here, notify the caller by  
        // throwing a TransactionAbortedException, or log the failure.  
        writer.WriteLine("Transaction rolled back.");  
    }  
  
    // Display messages.  
    Console.WriteLine(writer.ToString());  
  
    return returnValue;  
}  
```  
  
```vb  
' This function takes arguments for the 2 connection strings and commands in order  
' to create a transaction involving two SQL Servers. It returns a value > 0 if the  
' transaction committed, 0 if the transaction rolled back. To test this code, you can   
' connect to two different databases on the same server by altering the connection string,  
' or to another RDBMS such as Oracle by altering the code in the connection2 code block.  
Public Function CreateTransactionScope( _  
  ByVal connectString1 As String, ByVal connectString2 As String, _  
  ByVal commandText1 As String, ByVal commandText2 As String) As Integer  
  
    ' Initialize the return value to zero and create a StringWriter to display results.  
    Dim returnValue As Integer = 0  
    Dim writer As System.IO.StringWriter = New System.IO.StringWriter  
  
    ' Create the TransactionScope in which to execute the commands, guaranteeing  
    ' that both commands will commit or roll back as a single unit of work.  
    Using scope As New TransactionScope()  
        Using connection1 As New SqlConnection(connectString1)  
            Try  
                ' Opening the connection automatically enlists it in the   
                ' TransactionScope as a lightweight transaction.  
                connection1.Open()  
  
                ' Create the SqlCommand object and execute the first command.  
                Dim command1 As SqlCommand = New SqlCommand(commandText1, connection1)  
                returnValue = command1.ExecuteNonQuery()  
                writer.WriteLine("Rows to be affected by command1: {0}", returnValue)  
  
                ' If you get here, this means that command1 succeeded. By nesting  
                ' the Using block for connection2 inside that of connection1, you  
                ' conserve server and network resources by opening connection2   
                ' only when there is a chance that the transaction can commit.     
                Using connection2 As New SqlConnection(connectString2)  
                    Try  
                        ' The transaction is promoted to a full distributed  
                        ' transaction when connection2 is opened.  
                        connection2.Open()  
  
                        ' Execute the second command in the second database.  
                        returnValue = 0  
                        Dim command2 As SqlCommand = New SqlCommand(commandText2, connection2)  
                        returnValue = command2.ExecuteNonQuery()  
                        writer.WriteLine("Rows to be affected by command2: {0}", returnValue)  
  
                    Catch ex As Exception  
                        ' Display information that command2 failed.  
                        writer.WriteLine("returnValue for command2: {0}", returnValue)  
                        writer.WriteLine("Exception Message2: {0}", ex.Message)  
                    End Try  
                End Using  
  
            Catch ex As Exception  
                ' Display information that command1 failed.  
                writer.WriteLine("returnValue for command1: {0}", returnValue)  
                writer.WriteLine("Exception Message1: {0}", ex.Message)  
            End Try  
        End Using  
  
        ' If an exception has been thrown, Complete will   
        ' not be called and the transaction is rolled back.  
        scope.Complete()  
    End Using  
  
    ' The returnValue is greater than 0 if the transaction committed.  
    If returnValue > 0 Then  
        writer.WriteLine("Transaction was committed.")  
    Else  
        ' You could write additional business logic here, notify the caller by  
        ' throwing a TransactionAbortedException, or log the failure.  
       writer.WriteLine("Transaction rolled back.")  
     End If  
  
    ' Display messages.  
    Console.WriteLine(writer.ToString())  
  
    Return returnValue  
End Function  
```  
  
## Voir aussi  
 [Transactions et concurrence](../../../../docs/framework/data/adonet/transactions-and-concurrency.md)   
 [Fournisseurs managés ADO.NET et centre de développement DataSet](http://go.microsoft.com/fwlink/?LinkId=217917)