---
title: "Проверкой подлинности шифрования и подразделов наследования"
author: rick-anderson
description: "В этом документе описаны сведения о реализации защиты данных ASP.NET Core подраздела наследования и шифрование с проверкой подлинности."
keywords: "Шифрование с проверкой подлинности ASP.NET Core, защиты данных, подраздела наследования"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 34bb58a3-5a9a-41e5-b090-08f75b4bbefa
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: 3eb27b8a6d04074662bf619a09fd867252624209
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="subkey-derivation-and-authenticated-encryption"></a>Проверкой подлинности шифрования и подразделов наследования

<a name="data-protection-implementation-subkey-derivation"></a>

Большинство ключей в кольце ключ будет содержать определенные виды энтропии и будет иметь алгоритмическое сведения о том, «режим CBC шифрования + HMAC проверки» или «шифрование GCM + проверки». В таких случаях мы называем внедренные энтропии материала основного (или км) для данного ключа и мы выполнения функции производного ключа для получения ключей, которые будут использоваться для фактических криптографических операций.

> [!NOTE]
> Ключи являются абстрактными, а пользовательская реализация может вести себя как показано ниже. Если ключ обеспечивает собственную реализацию `IAuthenticatedEncryptor` вместо того чтобы использовать один из встроенных фабрик, больше не применяется механизм, описанные в этом разделе.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Дополнительные данные, прошедшие проверку подлинности и подразделов наследования

`IAuthenticatedEncryptor` Интерфейс служит в качестве основной интерфейс для всех операций шифрования, прошедшего проверку подлинности. Его `Encrypt` метод принимает два буфера: обычный текст и additionalAuthenticatedData (AAD). Поток содержимого открытого текста без изменений вызов `IDataProtector.Protect`, но AAD создается системой и состоит из трех компонентов:

1. 32-разрядный magic заголовок 09 F0 C9 F0, определяющий этой версии системы защиты данных.

2. 128-разрядный идентификатор ключа.

3. Строка переменной длины, сформированным из цепочки цель создания `IDataProtector` выполняет эту операцию.

Так как AAD уникален для всех трех компонентов кортежа, мы можно использовать для формирования новых ключей из км вместо использования км сам во всех наших криптографических операций. При каждом вызове `IAuthenticatedEncryptor.Encrypt`, выполняется следующий процесс производного ключа:

(K_E, K_H) = SP800_108_CTR_HMACSHA512 (contextHeader K_M AAD, || keyModifier)

Здесь выполняется вызов Формирования NIST SP800-108 в режиме счетчика (см. [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), секунду 5.1) со следующими параметрами:

* Ключ производного ключа (KDK) = K_M

* PRF = HMACSHA512

* Метка = additionalAuthenticatedData

* контекст = contextHeader || keyModifier

Заголовок контекста имеет переменную длину и по сути служить отпечаток алгоритмов, для которых мы ваш класс порожден K_E и K_H. Модификатор ключей является строкой 128-разрядный формируется случайным образом при каждом вызове `Encrypt` и позволяет добиться с перегрузке вероятность, что KE и KH уникальны для этой операции шифрования конкретных протоколов проверки подлинности, даже если все другие входные данные для Формирования является константой.

Шифрование в режиме CBC + операций проверки HMAC | K_E | Длина ключа симметричный блочный шифр, и | K_H | — Это размер хэш-кода HMAC подпрограммы. Для шифрования GCM + операций проверки | K_H | = 0.

## <a name="cbc-mode-encryption--hmac-validation"></a>Шифрование в режиме CBC + HMAC проверки

После K_E создается посредством механизма выше, мы создать случайный вектор инициализации и запустить алгоритм симметричный блочный шифр, шифрование в виде обычного текста. Вектор инициализации и зашифрованного текста затем выполняются через подпрограмму HMAC, инициализируется с помощью ключа K_H для создания на компьютере MAC. Этот процесс и возвращаемого значения представлены графически ниже.

![Режим CBC процесс и вернуться](subkeyderivation/_static/cbcprocess.png)

*выходные данные: = keyModifier || IV || E_cbc (данные K_E iv) || HMAC (K_H, iv || E_cbc (данные K_E iv))*

> [!NOTE]
> `IDataProtector.Protect` Реализация будет [началу magic заголовка и идентификатора ключа](authenticated-encryption-details.md) вывод перед возвращением вызывающему объекту. Поскольку magic заголовка и идентификатора ключа неявно являются частью [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), и так как модификатор ключей передавались в качестве входных данных для Формирования, это означает проверку каждые однобайтовых последний возвращенный полезных данных по MAC.

## <a name="galoiscounter-mode-encryption--validation"></a>Режим Galois/счетчика шифрования + проверки

После K_E создается посредством механизма выше, мы формирования случайных nonce 96-разрядного и запустить алгоритм шифрования симметричных блок дешифруется открытого текста и создания тег аутентификации 128-разрядный.

![Процесс GCM режима и вернуться](subkeyderivation/_static/galoisprocess.png)

*выходные данные: = keyModifier || Nonce || E_gcm (K_E, nonce, данные) || authTag*

> [!NOTE]
> Несмотря на то, что GCM изначально поддерживает концепцию AAD, мы по-прежнему поступает AAD только для Формирования исходного, предусмотренной для его параметра AAD необходимо передать пустую строку в GCM. Причиной этого является двухступенчатой. Во-первых, [для поддержки гибкости](context-headers.md#data-protection-implementation-context-headers) мы никогда не будут K_M непосредственно как ключ шифрования. Кроме того GCM налагает очень строгие уникальность ее входными данными. Задает вероятность того, что процедура шифрования GCM когда-либо вызванный для двух или более различных входных данных с тем же (ключ, nonce) пары не должен превышать 2 ^ 32. Если мы будем устранять K_E невозможно выполнить больше, чем 2 ^ 32 операции шифрования, прежде чем мы запуска от 2 ^ ограничить -32. Это может показаться очень большое количество операций, но изменяемым веб-сервера можно выполнить запросы 4 млрд простое дней, в пределах обычный время существования для этих ключей. Чтобы оставаться совместимым 2 ^ предел вероятность-32, мы продолжаем использовать 128-разрядный модификатор ключей и 96-разрядного nonce, которая расширяет число операций можно использовать для любого заданного K_M радикально. Для упрощения разработки мы Формирования кода путь к общей папке между операциями CBC и GCM, а поскольку AAD уже считается в Формирования нет необходимости переслать его подпрограмма GCM.
