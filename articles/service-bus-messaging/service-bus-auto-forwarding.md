---
title: Encaminhamento automático de entidades de mensagens do barramento de serviço do Azure
description: Este artigo descreve como encadear uma fila ou assinatura do barramento de serviço do Azure para outra fila ou tópico.
ms.topic: article
ms.date: 01/20/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 80bef52d568130fa800a1da661f4867abb3df02c
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98678981"
---
# <a name="chaining-service-bus-entities-with-autoforwarding"></a>Encadeando entidades do Barramento de Serviço com o encaminhamento automático

O recurso *encaminhamento automático* do Barramento de Serviço permite encadear uma fila ou assinatura a outra fila ou outro tópico que faz parte do mesmo namespace. Quando o encaminhamento automático está habilitado, o Barramento de Serviço remove automaticamente as mensagens colocadas na primeira fila ou assinatura (origem) e as coloca na segunda fila ou no segundo tópico (destino). Ainda é possível enviar uma mensagem diretamente para a entidade de destino.

> [!NOTE]
> A camada básica do barramento de serviço não dá suporte ao recurso de encaminhamento automático. As camadas Standard e Premium oferecem suporte ao recurso. Para conferir as diferenças entre essas camadas, consulte [preços do Barramento de Serviço](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="using-autoforwarding"></a>Usar o encaminhamento automático

Você pode habilitar o encaminhamento automático configurando as propriedades [QueueDescription.ForwardTo][QueueDescription.ForwardTo] ou [SubscriptionDescription.ForwardTo][SubscriptionDescription.ForwardTo] nos objetos [QueueDescription][QueueDescription] ou [SubscriptionDescription][SubscriptionDescription] para a origem, como no exemplo a seguir:

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

A entidade de destino deverá existir no momento da criação da entidade de origem. Se a entidade de destino não existir, o Barramento de Serviço retornará uma exceção quando receber uma solicitação para criar a entidade de origem.

Você pode usar o encaminhamento automático para expandir um tópico individual. O Barramento de Serviço limita a [quantidade de assinaturas em determinado tópico](service-bus-quotas.md) em 2.000. Você pode acomodar outras assinaturas criando tópicos de segundo nível. Mesmo se o número de assinaturas que você tem não estiver limitado pelo Barramento de Serviço, a adição de um segundo nível de tópicos poderá melhorar a taxa de transferência geral do tópico.

![Diagrama de um cenário de encaminhamento automático mostrando uma mensagem processada por meio de um tópico Orders que pode ramificar para qualquer um dos três tópicos de pedidos de segundo nível.][0]

Você também pode usar o encaminhamento automático para separar os remetentes dos destinatários. Por exemplo, considere um sistema ERP composto por três módulos: processamento de pedidos, gerenciamento de estoque e gerenciamento de relacionamentos com o cliente. Cada um desses módulos gera mensagens que são enfileiradas em um tópico correspondente. Brenda e Pedro são representantes de vendas interessados em todas as mensagens relacionadas aos seus clientes. Para receber essas mensagens, Brenda e Pedro criaram, cada um, uma fila e assinatura pessoais em cada um dos tópicos ERP que encaminham automaticamente todas as mensagens para suas filas.

![Diagrama de um cenário de encaminhamento automático mostrando três módulos de processamento enviando mensagens por meio de três tópicos correspondentes para duas filas separadas.][1]

Se Brenda entrar de férias, sua fila pessoal, em vez do tópico ERP, ficará cheia. Nesse cenário, como um representante de vendas não recebeu nenhuma mensagem, nenhum dos tópicos ERP atingirá a cota.

> [!NOTE]
> Quando o encaminhamento automático é configurado, o valor de AutoDeleteOnIdle na **origem e no destino** é definido automaticamente como o valor máximo do tipo de dados.
> 
>   - No lado da origem, o encaminhamento automático atua como uma operação de recebimento. Portanto, a origem que tem a configuração de encaminhamento automático nunca é realmente "ociosa".
>   - No lado do destino, isso é feito para garantir que sempre haja um destino para encaminhar a mensagem.

## <a name="autoforwarding-considerations"></a>Considerações sobre o encaminhamento automático

Se a entidade de destino tiver acumulado mensagens demais e exceder a cota, ou se a entidade de destino estiver desabilitada, a entidade de origem adicionará as mensagens à sua [fila de mensagens mortas](service-bus-dead-letter-queues.md) até que haja espaço no destino (ou a entidade seja habilitada novamente). Essas mensagens continuam ativas na fila de mensagens mortas, portanto, você deve receber e processá-las explicitamente a partir da fila de mensagens mortas.

Ao encadear tópicos individuais a fim de obter um tópico composto com diversas assinaturas, recomendamos que você tenha uma quantidade moderada de assinaturas no tópico de primeiro nível e diversas assinaturas nos tópicos de segundo nível. Por exemplo, um tópico de primeiro nível com 20 assinaturas, cada uma delas encadeada com um tópico de segundo nível com 200 assinaturas, permite uma taxa de transferência superior do que um tópico de primeiro nível com 200 assinaturas, cada uma delas encadeada com um tópico de segundo nível com 20 assinaturas.

O Barramento de Serviço cobra uma operação por cada mensagem encaminhada. Por exemplo, o envio de uma mensagem a um tópico com 20 assinaturas, cada uma delas configurada para encaminhar automaticamente mensagens para outra fila ou outro tópico, receberá uma cobrança por 21 operações, caso todas as assinaturas de primeiro nível recebam uma cópia da mensagem.

Para criar uma assinatura encadeada a outra fila ou tópico, o criador da assinatura deve ter permissões de **Gerenciamento** na entidade de origem e de destino. O envio de mensagens para o tópico de origem exige apenas permissões para **Enviar** no tópico de origem.

Não crie uma cadeia que exceda 4 saltos. Mensagens que excedem 4 saltos são inativas.

## <a name="next-steps"></a>Próximas etapas

Para obter informações detalhadas sobre o encaminhamento automático, confira os seguintes tópicos de referência:

* [ForwardTo][QueueDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

Para saber mais sobre as melhorias de desempenho do Barramento de Serviço, consulte 

* [Práticas recomendadas para melhorias de desempenho usando o Sistema de Mensagens do Barramento de Serviço](service-bus-performance-improvements.md)
* [Entidades de mensagens particionadas][Partitioned messaging entities].

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
