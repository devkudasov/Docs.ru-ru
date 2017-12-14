---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: "Анимация элемента управления UpdatePanel (VB) | Документы Microsoft"
author: wenz
description: "Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления. Для содержимого..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d1056fc798e22254e94e5cad54436576a297f7d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="589cf-104">Анимация элемента управления UpdatePanel (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="589cf-104">Animating an UpdatePanel Control (VB)</span></span>
====================
<span data-ttu-id="589cf-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="589cf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="589cf-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="589cf-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="589cf-107">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="589cf-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="589cf-108">Для содержимого элемента управления UpdatePanel, существует специальные расширения, в большой степени основывается на платформе анимации: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="589cf-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="589cf-109">Этот учебник показывает, как для настройки таких анимации для элемента управления UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="589cf-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="589cf-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="589cf-110">Overview</span></span>

<span data-ttu-id="589cf-111">Элемент управления анимации в наборе элементов управления ASP.NET AJAX не только элемент управления, но всю платформу, позволяющую Добавление анимации в элемент управления.</span><span class="sxs-lookup"><span data-stu-id="589cf-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="589cf-112">Для содержимого `UpdatePanel`, специальные расширения существует, в большой степени основывается на платформе анимации: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="589cf-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="589cf-113">Этого учебника показано, как настроить такие анимацию для `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="589cf-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="589cf-114">Шаги</span><span class="sxs-lookup"><span data-stu-id="589cf-114">Steps</span></span>

<span data-ttu-id="589cf-115">Первым шагом является обычным образом, чтобы включить `ScriptManager` на странице, чтобы загрузить библиотеки ASP.NET AJAX и может использоваться набор элементов управления:</span><span class="sxs-lookup"><span data-stu-id="589cf-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="589cf-116">Анимация в этом сценарии будет применяться к ASP.NET `Wizard` веб-элемента управления, находящегося в `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="589cf-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="589cf-117">Недостаточно параметров для запуска обратных передач перечислены три шаги (произвольный).</span><span class="sxs-lookup"><span data-stu-id="589cf-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="589cf-118">Разметка, необходимые для `UpdatePanelAnimationExtender` очень похожа на разметку, используемую для управления `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="589cf-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="589cf-119">В `TargetControlID` мы предоставляем атрибут `ID` из `UpdatePanel` для анимации; в `UpdatePanelAnimationExtender` управления `<Animations>` элемент содержит XML-разметку для анимаций.</span><span class="sxs-lookup"><span data-stu-id="589cf-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="589cf-120">Однако есть одно различие: объем события и обработчики событий ограничен по сравнению с `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="589cf-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="589cf-121">Для `UpdatePanels`, только два из них существует:</span><span class="sxs-lookup"><span data-stu-id="589cf-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="589cf-122">`<OnUpdated>`При обновлении UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="589cf-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="589cf-123">`<OnUpdating>`Когда UpdatePanel начинается обновление</span><span class="sxs-lookup"><span data-stu-id="589cf-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="589cf-124">В этом случае новое содержимое `UpdatePanel` (после обратной передачи) должны появление.</span><span class="sxs-lookup"><span data-stu-id="589cf-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="589cf-125">Это необходимой разметки для этого:</span><span class="sxs-lookup"><span data-stu-id="589cf-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="589cf-126">Теперь каждый раз, когда происходит обратная передача в UpdatePanel, новое содержимое панели Плавное.</span><span class="sxs-lookup"><span data-stu-id="589cf-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


<span data-ttu-id="589cf-127">[![Следующий шаг мастера эффект постепенного увеличения](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="589cf-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="589cf-128">Следующий шаг мастера эффект постепенного увеличения ([Просмотр полноразмерное изображение](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="589cf-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="589cf-129">[Назад](changing-an-animation-using-client-side-code-vb.md)
[Вперед](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="589cf-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>