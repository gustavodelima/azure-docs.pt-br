---
title: Solucionar problemas de replicação entre regiões para Azure NetApp Files | Microsoft Docs
description: Descreve mensagens de erro e resoluções que podem ajudá-lo a solucionar problemas de replicação entre regiões para Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/18/2020
ms.author: b-juche
ms.openlocfilehash: b30ed0cca680013b85efe064d59fb7cb73d753d2
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/22/2020
ms.locfileid: "95239543"
---
# <a name="troubleshoot-cross-region-replication"></a>Solucionar problemas de replicação entre regiões

Este artigo descreve as mensagens de erro e as resoluções que podem ajudá-lo a solucionar problemas de replicação entre regiões para Azure NetApp Files. 

## <a name="errors-creating-replication"></a>Erros ao criar a replicação  

|     Mensagem de erro    |     Resolução    |
|-|-|
|     `Volume {0} cannot   be used as source because it is already in replication`    |     Você não pode criar uma replicação com um volume de origem que já esteja em um relacionamento de replicação de dados.    |
|     `Peered region   '{0}' is not accepted`    |     Você está tentando criar uma replicação entre regiões não emparelhadas.    |
|     `RemoteVolumeResource   '{0}' of wrong type '{1}'`    |     Valide se a ID do recurso remoto é uma ID de recurso de volume.    |

## <a name="errors-authorizing-volume"></a>Erros de autorização de volume  

|     Mensagem de erro    |     Resolução    |
|-|-|
|     `Missing value   for 'AuthorizeSourceReplication'`    |     O   `RemoteResourceID` está ausente ou é inválido na interface do usuário ou na solicitação de API (mensagem de erro de correção).    |
|     `Missing value   for 'RemoteVolumeResourceId'`    |     O   `RemoteResourceID` está ausente ou é inválido na interface do usuário ou na solicitação de API.    |
|     `Data   Protection volume not found for RemoteVolumeResourceId: {remoteResourceId}`    |     Validar se   `RemoteResourceID` está correto ou existe para o usuário.    |
|     `Remote volume   '{0}' is not configured for replication`    |     O volume de destino não é um volume de proteção de dados.    |
|     `Remote volume   '{0}' does not have source volume '{1}' as RemoteVolumeResourceId`    |     O volume de proteção de dados não tem esse volume de origem em sua ID de recurso remoto (a ID de origem errada foi inserida).    |
|     `The   destination volume replication creation failed (message: {0})`    |     Esse erro indica um erro de servidor. Entre em contato com suporte.    |

## <a name="errors-deleting-replication"></a>Erros ao excluir a replicação

|     Mensagem de erro    |     Resolução    |
|-|-|
|     `Replication   cannot be deleted, mirror state needs to be in status: Broken before deleting`    |     Valide se a replicação foi quebrada ou se ela não foi inicializada e está ociosa (inicialização com falha).    |
|     `Cannot delete   source replication`    |     Não é permitido excluir a replicação do lado da origem. Certifique-se de que você está excluindo a replicação do lado de destino.    |

## <a name="errors-deleting-volume"></a>Erros ao excluir o volume

|     Mensagem de erro    |     Resolução    |
|-|-|
| `Volume is a member of an active volume replication relationship`  |  Exclua a replicação antes de excluir o volume. Consulte [excluir replicações](cross-region-replication-delete.md). Esta operação requer que você interrompa o emparelhamento antes de excluir a replicação para o volume. |
| `Volume with replication cannot be deleted`  |  Exclua a replicação antes de excluir o volume. Consulte [excluir replicações](cross-region-replication-delete.md). Esta operação requer que você interrompa o emparelhamento antes de excluir a replicação para o volume. 

## <a name="errors-resyncing-volume"></a>Erros ao ressincronizar o volume

|     Mensagem de erro    |     Resolução    |
|-|-|
|     `Volume Replication is in invalid status: (Mirrored|Uninitialized) for operation: 'ResyncReplication'`     |     Valide se a replicação de volume está no estado "quebrada".    |

## <a name="errors-deleting-snapshot"></a>Erros ao excluir o instantâneo 

|     Mensagem de erro    |     Resolução    |
|-|-|
|     `Snapshot   cannot be deleted, parent volume is a Data Protection volume with a   replication object`    |     Valide se você desrompeu a replicação do volume se quiser excluir esse instantâneo.    |
|     `Cannot delete   volume replication generated snapshot`    |     A exclusão de instantâneos de linha de base de replicação não é permitida.    |

## <a name="next-steps"></a>Próximas etapas  

* [Replicação entre regiões](cross-region-replication-introduction.md)
* [Requisitos e considerações para usar a replicação entre regiões](cross-region-replication-requirements-considerations.md)
* [Criar replicação de volume](cross-region-replication-create-peering.md)
* [Exibir o status de integridade da relação de replicação](cross-region-replication-display-health-status.md)
* [Gerenciar recuperação de desastre](cross-region-replication-manage-disaster-recovery.md)
* [Solucionar problemas de replicação entre regiões](troubleshoot-cross-region-replication.md)
